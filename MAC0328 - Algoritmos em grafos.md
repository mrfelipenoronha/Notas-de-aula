---

# Aula 02/08

## Fontes de referencia para a matéria

- ["R. Sedgewick, Algorithms in C (part 5: Graph Algorithms)"](http://www.informit.com/store/algorithms-in-c-part-5-graph-algorithms-9780201316636)

- ["P. Feofiloff, Algoritmos para Grafos em C (via Sedgewick)"](https://www.ime.usp.br/~pf/algoritmos_para_grafos/)

## Definições básicas sobre grafos

Um  grafo é um par de conjuntos:  um conjunto de coisas conhecidas como vértices e um conjunto de coisas conhecidas como arcos. Cada arco é um par ordenado de vértices. O primeiro vértice do par é a ponta inicial do arco e o segundo é a ponta final.

Um **arco dirigido** é o arco que começa em um vértice e *termina* em outro. Um arco com ponta inicial em V e final em W é denotado por **`V-W`**.

W é *vizinho/adjacente* de V se existe o arco V-W.

O tamanho de um grafo com V vertices e A arestas é V+A.

Uma boa maneira de definir um grafo é exibir seu conjunto de arcos, como por exemplo:

    0-5 0-6 2-0 2-3 3-6 3-10

### Arcos anti-paralelos e paralelos

Dois arcos são anti-paralelos se um é V-W e o outro é W-V, para isso, damos o nome de **arco** (edge).

Dois arcos são paralelos (repetido) se a ponta inicial e final de um forem as mesmas do outro. Os grafos que lidaremos nesta materia não terão arcos paralelos.

### Leques e grau dos vertices

O **leque de saída** (fan-out) de um vértice num grafo é o conjunto de todos os arcos que saem do vértice.  O **leque de entrada** (fan-in) de um vértice é o conjunto de todos os arcos que entram no vértice.

O **grau de saída** (outdegree) de um vértice num grafo é o tamanho do seu leque de saída, ou seja, o número de arcos que saem do vértice.  O **grau de entrada** (indegree) é o tamanho do leque de entrada do vértice.

Uma **fonte** (source) é um vértice que tem grau de entrada nulo. Um **sorvedouro** (sink) é um vértice que tem grau de saída nulo.

Um vértice é **isolado** se seu grau de entrada e seu grau de saída são ambos nulos.  É claro que um grafo sem vértices isolados é completamente definido por seu conjunto de arcos.

### Numero de arcos

A soma dos graus de saída de todos os vértices de um grafo é igual ao número de arcos do grafo.  A soma dos graus de entrada de todos os vértices também é igual ao número de arcos. Segue daí que um grafo com V vértices tem no máximo

    V*(V−1)

arcos.

Um grafo é  **completo**  se todo par ordenado de vértices distintos é um arco. Um grafo completo com V vértices tem exatamente V*(V−1) arcos. Um **torneio** é qualquer grafo dotado da seguinte propriedade: para cada par V-W de vértices distintos, V-W é um arco ou W-V é um arco, mas não ambos. Um torneio com V vértices tem exatamente ½V(V−1) arcos.

### Subgrafos

Um subgrafo de um grafo G é um pedaço de G. O conjunto de vértices e o conjunto de arcos do pedaço de G devem ser coerentes. Assim, é melhor formular o conceito como uma relação entre dois grafos: Um grafo H é subgrafo de um grafo G se todo vértice de H é vértice de G e todo arco de H é arco de G. (Notação:  H ⊆ G.)

Alguns tipos de subgrafos merecem destaque. Um subgrafo H de um grafo G é

- **Gerador** (spanning) se contém todos os vértices de G;
- **Induzido** se todo arco de G com ambas as pontas em H também é arco de H;
- **Próprio** se for diferente de G (notação: H ⊂ G).

Um **supergrafo** é o contrário de um subgrafo: um grafo G é supergrafo de um grafo H se H for subgrafo de G.

### Grafos não-dirigidos

Um grafo é não-dirigido (undirected) se cada um de seus arcos é antiparalelo a algum outro arco: para cada arco  v-w,  o grafo também tem o arco  w-v.

Num grafo não-dirigido, a relação de adjacência é simétrica:  um vértice w é adjacente a um vértice v se e somente se v é adjacente a w.

Num grafo não-dirigido, o leque de um vértice v é o conjunto de arestas que incidem em v. O grau de v é o número de arestas no leque de v. É claro que o grau de um vértice é igual ao seu grau de entrada e também ao seu grau de saída.


## Estrutura de dados para um grafo

Como representar um grafo na linguagem C?

Os vertices dos nossos grafos serão representados por inteiros, para isso, faremos:

```C
/* Vértices de grafos são representados por objetos do tipo vertex. */

#define vertex int
```

Para representar os arcos demos dois metodos:

### Matriz de adjacencia

É um matriz de 0's e 1's, com as colunas e linhas idexadas pelos vertices.

Se `adj[][]` é uma tal matriz então, para cada vértice v e cada vértice w,

- adj[v][w] = 1   se v-w é um arco  e
- adj[v][w] = 0   em caso contrário.

Assim, a linha v da matriz adj[][] representa o leque de saída do vértice v e a coluna w da matriz representa o leque de entrada do vértice w.

Como nossos grafos não têm laços, os elementos da diagonal da matriz de adjacências são iguais a 0.

Se o grafo for não-dirigido, a matriz é simétrica: adj[v][w] ≡ adj[w][v].

Um grafo é representado por uma struct graph que contém a matriz de adjacências, o número de vértices, e o número de arcos do grafo:

```C

/* REPRESENTAÇÃO POR MATRIZ DE ADJACÊNCIAS: A estrutura graph representa um grafo. O campo adj é um ponteiro para a matriz de adjacências do grafo. O campo V contém o número de vértices e o campo A contém o número de arcos do grafo. */

struct graph {
   int V;
   int A;
   int **adj;
};

/* Um Graph é um ponteiro para um graph, ou seja, um Graph contém o endereço de um graph. */

typedef struct graph *Graph;
```
Seguem algumas ferramentas básicas para a construção e manipulação de grafos:

```C
/* REPRESENTAÇÃO POR MATRIZ DE ADJACÊNCIAS: A função GRAPHinit() constrói um grafo com vértices 0 1 .. V-1 e nenhum arco. */

Graph GRAPHinit( int V) {
   Graph G = malloc( sizeof *G);
   G->V = V;
   G->A = 0;
   G->adj = MATRIXint( V, V, 0);
   return G;
}

/* REPRESENTAÇÃO POR MATRIZ DE ADJACÊNCIAS: A função MATRIXint() aloca uma matriz com linhas 0..r-1 e colunas 0..c-1. Cada elemento da matriz recebe valor val. */

static int **MATRIXint( int r, int c, int val) {
   int **m = malloc( r * sizeof (int *));
   for (vertex i = 0; i < r; ++i)
      m[i] = malloc( c * sizeof (int));
   for (vertex i = 0; i < r; ++i)
      for (vertex j = 0; j < c; ++j)
         m[i][j] = val;
   return m;
}

/* REPRESENTAÇÃO POR MATRIZ DE ADJACÊNCIAS: A função GRAPHinsertArc() insere um arco v-w no grafo G. A função supõe que v e w são distintos, positivos e menores que G->V. Se o grafo já tem um arco v-w, a função não faz nada. */

void GRAPHinsertArc( Graph G, vertex v, vertex w) {
   if (G->adj[v][w] == 0) {
      G->adj[v][w] = 1;
      G->A++;
   }
}

/* REPRESENTAÇÃO POR MATRIZ DE ADJACÊNCIAS: A função GRAPHremoveArc() remove do grafo G o arco v-w. A função supõe que v e w são distintos, positivos e menores que G->V. Se não existe arco v-w, a função não faz nada. */

void GRAPHremoveArc( Graph G, vertex v, vertex w) {
   if (G->adj[v][w] == 1) {
      G->adj[v][w] = 0;
      G->A--;
   }
}

/* REPRESENTAÇÃO POR MATRIZ DE ADJACÊNCIAS: A função GRAPHshow() imprime, para cada vértice v do grafo G, em uma linha, todos os vértices adjacentes a v. */

void GRAPHshow( Graph G) {
   for (vertex v = 0; v < G->V; ++v) {
      printf( "%2d:", v);
      for (vertex w = 0; w < G->V; ++w)
         if (G->adj[v][w] == 1)
            printf( " %2d", w);
      printf( "\n");
   }
}

```
O espaço ocupado por uma matriz de adjacências é proporcional a V², sendo V o número de vértices do grafo.

### Vetor de listas de adjacencia

O vetor de listas de adjacência de um grafo tem uma lista encadeada (linked list) associada com cada vértice do grafo.  A lista associada com um vértice v contém todos os vizinhos de v.  Portanto, a lista do vértice v representa o leque de saída de v.

```C

/* REPRESENTAÇÃO POR LISTAS DE ADJACÊNCIA: A estrutura graph representa um grafo. O campo adj é um ponteiro para o vetor de listas de adjacência, o campo V contém o número de vértices e o campo A contém o número de arcos do grafo. */

struct graph {
   int V;
   int A;
   link *adj;
};

/* Um Graph é um ponteiro para um graph. */

typedef struct graph *Graph;

/* A lista de adjacência de um vértice v é composta por nós do tipo node. Cada nó da lista corresponde a um arco e contém um vizinho w de v e o endereço do nó seguinte da lista. Um link é um ponteiro para um node. */

typedef struct node *link;
struct node {
   vertex w;
   link next;
};

/* A função NEWnode() recebe um vértice w e o endereço next de um nó e devolve o endereço a de um novo nó tal que a->w == w e a->next == next. */

static link NEWnode( vertex w, link next) {
   link a = malloc( sizeof (struct node));
   a->w = w;
   a->next = next;
   return a;
}

```

Eis algumas funções básicas de construção e manipulação de grafos representados por listas de adjacência:

```C

/* REPRESENTAÇÃO POR LISTAS DE ADJACÊNCIA: A função GRAPHinit() constrói um grafo com vértices 0 1 .. V-1 e nenhum arco. */

Graph GRAPHinit( int V) {
   Graph G = malloc( sizeof *G);
   G->V = V;
   G->A = 0;
   G->adj = malloc( V * sizeof (link));
   for (vertex v = 0; v < V; ++v)
      G->adj[v] = NULL;
   return G;
}

/* REPRESENTAÇÃO POR LISTAS DE ADJACÊNCIA: A função GRAPHinsertArc() insere um arco v-w no grafo G. A função supõe que v e w são distintos, positivos e menores que G->V. Se o grafo já tem um arco v-w, a função não faz nada. */

void GRAPHinsertArc( Graph G, vertex v, vertex w) {
   for (link a = G->adj[v]; a != NULL; a = a->next)
      if (a->w == w) return;
   G->adj[v] = NEWnode( w, G->adj[v]);
   G->A++;
}

```
---

# Aula 05/08

## Grafos aleatorios

Para escolher um vertice aleatoriamente usamos o envolucro a seguir, que devolve um numero entre 0 e V-1:

```c
/* A função randV() devolve um vértice aleatório do grafo G. Vamos supor que G->V <= RAND_MAX. */

vertex randV( Graph G) {
   double r;
   r = rand( ) / (RAND_MAX + 1.0);
   return r * G->V;
}
```

### Construtor 1

A primeira função para construir um grafo aleatorios é a:

```c
Graph GRAPHrand1( int V, int A) {
   Graph G = GRAPHinit( V);
   while (G->A < A) {
      vertex v = randV( G);
      vertex w = randV( G);
      if (v != w)
         GRAPHinsertArc( G, v, w);
   }
   return G;
}
```

A função acima só deve ser invocada quando o numero A é bem menor que V*(V-1). Devido ao fato de ela tentar inserir um arco aleatório toda vez, caso o grafo gerado seja denso, vai demorar muito ate que todos os arcos possiveis sejam inseridos.

### Construtor 2

A segunda função para construir o grafo é dada a seguir:

```c
Graph GRAPHrand2( int V, int A) {
   double prob = (double) A / (V*(V-1));
   Graph G = GRAPHinit( V);
   for (vertex v = 0; v < V; ++v)
      for (vertex w = 0; w < V; ++w)
         if (v != w)
            if (rand( ) < prob*(RAND_MAX+1.0))
               GRAPHinsertArc( G, v, w);
   return G;
}
```

O codigo acima tem uma performance muito melhor quando o grafo a ser gerado é denso.

## Caminhos e ciclos em grafos

### Caminhos

Um **passeio** (walk) em um grafo é uma sequência de vértices dotada da seguinte propriedade: se v e w são vértices consecutivos na sequência então v-w é um arco do grafo. Um **passeio é  fechado** (closed) se tem pelo menos dois arcos e seu primeiro vértice coincide com o último.

Um  **caminho** (path) em um grafo é um passeio **sem arcos repetidos**, ou seja, um passeio em que os arcos são todos diferentes entre si. Um caminho é simples  se não tem vértices repetidos.

A **origem** de um caminho é o seu primeiro vértice. O **término** é o seu último vértice.  Se um caminho tem origem s e término t, dizemos que  vai de s a t.

O **comprimento** (length) de um caminho é o número de arcos do caminho.  Se um caminho tem n vértices, seu comprimento é pelo menos n−1; se o caminho é simples, seu comprimento é exatamente n−1.

### Ciclos

**Ciclos** são estruturas muito importantes. São os ciclos que tornam grafos interessantes mas também complexos e difíceis de manipular.

Um **ciclo** (cycle) em um grafo é um caminho fechado. (Portanto, todo ciclo tem comprimento maior que 1 e não tem arcos repetidos.)  Dizemos que um arco v-w pertence a um dado ciclo (ou que o ciclo passa pelo arco) se o vértice w é o sucessor de v no ciclo.   Um ciclo é simples se não tem vértices repetidos exceto pelo último (que coincide com o primeiro).

É apropriado abusar um pouco do conceito de igualdade e tratar dois ciclos simples como iguais se eles têm o mesmo conjunto de arcos, ainda que tenham origens diferentes.  Por exemplo, trataremos como iguais os ciclos `6-2-7-3-6, 2-7-3-6-2, 7-3-6-2-7 e 3-6-2-7-3`.

Todos os arcos de um ciclo apontam no mesmo sentido — de um vértice do ciclo para o seu sucessor. Há quem goste de enfatizar esse fato dizendo ciclo dirigido no lugar de ciclo.

## Grafos topologicos

A definição de grafos topológicos exige que os vértices sejam numerados de uma certa maneira.  Assim, cada grafo é acompanhado de uma numeração (ranking) dos vértices, ou seja, uma atribuição de números inteiros aos vértices. Essa numeração dos vértices será representada por um vetor cujos índices são vértices e cujos elementos são números inteiros.

### Definição

Um **grafo é topológico** se estiver acompanhado de uma numeração topológica dos seus vértices. Uma numeração, digamos `topo[]`, é topológica se

**topo[v] < topo[w]**

para todo arco v-w.

A maioria dos grafos não tem uma numeração topológica. Por outro lado, um grafo pode ter várias numerações topológicas diferentes. A seguinte função recebe uma numeração `topo[0..V-1]` dos vértices de um grafo G e decide se essa numeração é topológica:

```C
bool isTopoNumbering( Graph G, int topo[]) {
   for (vertex v = 0; v < G->V; ++v)
      for (link a = G->adj[v]; a != NULL; a = a->next)
         if (topo[v] >= topo[a->w])
            return false;
   return true;
}
```

### Propriedades

- Grafos topológicos não têm ciclos (a reciproca é verdadeira).
- Todo vértice de um grafo topológico é término de um caminho cuja origem é uma fonte.
- Todo vértice de um grafo topológico é origem de um caminho cujo término é um sorvedouro.

### Variante antitopologica

Uma numeração anti-topológica é uma numeração topológica ao contrário. Mais precisamente, uma numeração atopo[] dos vértices de um grafo é anti-topológica se  `atopo[v] > atopo[w]` para todo arco v-w.

---

# Aula 09/08

## Florestas radicadas

Uma **floresta radicada** (arvore) é um grafo topológico sem vértices com grau de entrada maior que 1.

A seguinte função recebe um grafo G e uma numeração `topo[0..V-1]` dos vértices de G e decide se essa numeração é topológica e se G é uma floresta radicada.

```c
bool isRootedForest( Graph G, int topo[]) {
   if (!isTopoNumbering( G, topo))
      return false;
   for (vertex v = 0; v < G->V; ++v)
      if (GRAPHindeg( G, v) > 1)
         return false;
   // ineficiente...
   return true;
}
```

### Propriedades

- Florestas radicadas não têm ciclos.
- Todo vértice de uma floresta radicada é término de um caminho que começa em uma fonte, e todo vértice é origem de um caminho que termina em um sorvedouro.
- Todo vértice de uma floresta radicada é término de um único caminho que começa numa fonte.
Segue daí imediatamente que, para quaisquer dois vértices v e z, existe no máximo um caminho de v a z. Ademais, se existe um caminho de v a z então não existe caminho de z a v.

### Terminologia

- As **fontes** de uma floresta radicada são chamadas raizes.
- Os **sorvedouros** de uma floresta radicada são chamados folhas.
- Cada vizinho de um vértice v é um **filho** de v.
- Todo vértice w de uma floresta radicada, exceto uma raiz, tem um pai.
- Um vértice u é **ancestral** de um vértice z se existe um caminho u a z.  Um ancestral de z é próprio se for diferente de z.
- Um vértice z é **descendente** de um vértice u se u é ancestral de z. Um descendente de u é próprio se for diferente de u.
- Um **primo** de um vértice u é qualquer vértice que não seja ancestral nem descendente de u.
- A **profundidade** de um vértice w numa floresta radicada é o comprimento do único caminho que começa em alguma raiz e termina em w.  A **altura** da floresta radicada é a profundidade de um vértice de profundidade máxima.

### Árvores radicadas

Uma árvore radicada (= branching) é uma floresta radicada que tem uma só raiz. A raiz é o vértice da árvore que tem o menor número em qualquer numeração topológica.

### Grafos bipartidos dirigidos

Um grafo é bipartido dirigido se tiver uma numeração topológica com apenas dois valores: 0 e 1. Em outras palavras, um grafo é bipartido dirigido se existe um vetor topo[] indexado pelos vértices tal que, para cada arco v-w, topo[v] vale 0 e topo[w] vale 1.


## Busca em profundidade (DFS)

O algoritmo de busca DFS visita todos os vértices e todos os arcos do grafo numa determinada ordem e atribui um número a cada vértice: o k-ésimo vértice descoberto recebe o número k. A numeração dos vértices é registrada em um vetor `pre[]` indexado pelos vértices, ou seja, o vetor pre[] guarda o momento em que o i-esimo vertice foi descoberto.

```c
static int cnt;
int pre[1000];

/* A função GRAPHdfs() faz uma busca em profundidade no grafo G. Ela atribui um número de ordem pre[x] a cada vértice x de modo que o k-ésimo vértice descoberto receba o número de ordem k.  (Código inspirado no programa 18.3 de Sedgewick.) */
void GRAPHdfs( Graph G) {
   cnt = 0;
   for (vertex v = 0; v < G->V; ++v)
      pre[v] = -1;
   for (vertex v = 0; v < G->V; ++v)
      if (pre[v] == -1)
         dfsR( G, v); // começa nova etapa
}

/* A funçao dfsR() visita todos os vértices de G que podem ser alcançados a partir do vértice v sem passar por vértices já descobertos. A função atribui cnt+k a pre[x] se x é o k-ésimo vértice descoberto. (O código supõe que G é representado por listas de adjacência.) */
static void dfsR( Graph G, vertex v) {
   pre[v] = cnt++;
   for (link a = G->adj[v]; a != NULL; a = a->next) {
      vertex w = a->w;
      if (pre[w] == -1)
         dfsR( G, w);
   }
}

```

### Desempenho da busca DFS

A função GRAPHdfs() examina o leque de saída de cada vértice uma só vez. Portanto, cada arco é examinado uma só vez. Assim, se o grafo tem V vértices e A arcos, GRAPHdfs() consome tempo proporcional a **O(V+A)**.

### Pré-ordem

A ordem em que a função GRAPHdfs() descobre os vértices do grafo é chamada pré-ordem (= preorder).  Para obter a permutação dos vértices em pré-ordem basta inverter o vetor pre[], ou seja, obter a order de descoberta dos vertices:

```c
vertex vv[1000];
for (vertex v = 0; v < G->V; ++v)
    vv[pre[v]] = v;
for (int i = 0; i < G->V; ++i)
    printf( "%d ", vv[i]);
```\

Resumindo, dizemos que qualquer implementação do algoritmo de busca em profundidade descobre os vértices do grafo em pré-ordem.
















---
~ by @carol
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
