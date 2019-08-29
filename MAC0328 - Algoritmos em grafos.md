# Indice

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [Indice](#indice)   
- [Fontes de referencia para a matéria](#fontes-de-referencia-para-a-matéria)   
- [Definições básicas sobre grafos](#definições-básicas-sobre-grafos)   
   - [Arcos anti-paralelos e paralelos](#arcos-anti-paralelos-e-paralelos)   
   - [Leques e grau dos vertices](#leques-e-grau-dos-vertices)   
   - [Numero de arcos](#numero-de-arcos)   
   - [Subgrafos](#subgrafos)   
   - [Grafos não-dirigidos](#grafos-não-dirigidos)   
- [Estrutura de dados para um grafo](#estrutura-de-dados-para-um-grafo)   
   - [Matriz de adjacencia](#matriz-de-adjacencia)   
   - [Vetor de listas de adjacencia](#vetor-de-listas-de-adjacencia)   
- [Grafos aleatorios](#grafos-aleatorios)   
   - [Construtor 1](#construtor-1)   
   - [Construtor 2](#construtor-2)   
- [Caminhos e ciclos em grafos](#caminhos-e-ciclos-em-grafos)   
   - [Caminhos](#caminhos)   
   - [Ciclos](#ciclos)   
- [Grafos topologicos](#grafos-topologicos)   
   - [Definição](#definição)   
   - [Propriedades](#propriedades)   
   - [Variante antitopologica](#variante-antitopologica)   
- [Florestas radicadas](#florestas-radicadas)   
   - [Propriedades](#propriedades)   
   - [Terminologia](#terminologia)   
   - [Árvores radicadas](#árvores-radicadas)   
   - [Grafos bipartidos dirigidos](#grafos-bipartidos-dirigidos)   
- [Busca em profundidade (DFS)](#busca-em-profundidade-dfs)   
   - [Desempenho da busca DFS](#desempenho-da-busca-dfs)   
   - [Pré-ordem](#pré-ordem)   
   - [Florestas de busca](#florestas-de-busca)   
      - [Acima, abaixo, à esquerda e à direita](#acima-abaixo-à-esquerda-e-à-direita)   
   - [Arcos de retorno, de avanço, e cruzados](#arcos-de-retorno-de-avanço-e-cruzados)   
   - [Numeração pos-ordem](#numeração-pos-ordem)   
      - [Intervalos de vida](#intervalos-de-vida)   
   - [Ancestrais, descendentes, e primos](#ancestrais-descendentes-e-primos)   
- [Ciclos e DAGs](#ciclos-e-dags)   
   - [Detecção de ciclos](#detecção-de-ciclos)   
   - [Grafos topológicos versus dags](#grafos-topológicos-versus-dags)   
   - [Versão compactada do algoritmo](#versão-compactada-do-algoritmo)   
- [Caminhos minimos](#caminhos-minimos)   
   - [Arcos relaxados e potencial viável](#arcos-relaxados-e-potencial-viável)   
      - [Cota inferior e certificado de minimalidade](#cota-inferior-e-certificado-de-minimalidade)   
   - [Um algoritmo para dags](#um-algoritmo-para-dags)   
   - [Busca em largura (BFS)](#busca-em-largura-bfs)   
- [Componentes conexas](#componentes-conexas)   
   - [Grafos não-dirigidos conexos](#grafos-não-dirigidos-conexos)   
   - [Componentes conexas de grafos não-dirigidos](#componentes-conexas-de-grafos-não-dirigidos)   
   - [Cálculo das componentes conexas](#cálculo-das-componentes-conexas)   
   - [Circuitos e florestas](#circuitos-e-florestas)   
   - [Detecção de circuitos](#detecção-de-circuitos)   
   - [Florestas](#florestas)   
      - [Árvores](#árvores)   

<!-- /MDTOC -->

# Fontes de referencia para a matéria

- ["R. Sedgewick, Algorithms in C (part 5: Graph Algorithms)"](http://www.informit.com/store/algorithms-in-c-part-5-graph-algorithms-9780201316636)

- ["P. Feofiloff, Algoritmos para Grafos em C (via Sedgewick)"](https://www.ime.usp.br/~pf/algoritmos_para_grafos/)

# Definições básicas sobre grafos

Um  grafo é um par de conjuntos:  um conjunto de coisas conhecidas como vértices e um conjunto de coisas conhecidas como arcos. Cada arco é um par ordenado de vértices. O primeiro vértice do par é a ponta inicial do arco e o segundo é a ponta final.

Um **arco dirigido** é o arco que começa em um vértice e *termina* em outro. Um arco com ponta inicial em V e final em W é denotado por **`V-W`**.

W é *vizinho/adjacente* de V se existe o arco V-W.

O tamanho de um grafo com V vertices e A arestas é V+A.

Uma boa maneira de definir um grafo é exibir seu conjunto de arcos, como por exemplo:

    0-5 0-6 2-0 2-3 3-6 3-10

## Arcos anti-paralelos e paralelos

Dois arcos são anti-paralelos se um é V-W e o outro é W-V, para isso, damos o nome de **arco** (edge).

Dois arcos são paralelos (repetido) se a ponta inicial e final de um forem as mesmas do outro. Os grafos que lidaremos nesta materia não terão arcos paralelos.

## Leques e grau dos vertices

O **leque de saída** (fan-out) de um vértice num grafo é o conjunto de todos os arcos que saem do vértice.  O **leque de entrada** (fan-in) de um vértice é o conjunto de todos os arcos que entram no vértice.

O **grau de saída** (outdegree) de um vértice num grafo é o tamanho do seu leque de saída, ou seja, o número de arcos que saem do vértice.  O **grau de entrada** (indegree) é o tamanho do leque de entrada do vértice.

Uma **fonte** (source) é um vértice que tem grau de entrada nulo. Um **sorvedouro** (sink) é um vértice que tem grau de saída nulo.

Um vértice é **isolado** se seu grau de entrada e seu grau de saída são ambos nulos.  É claro que um grafo sem vértices isolados é completamente definido por seu conjunto de arcos.

## Numero de arcos

A soma dos graus de saída de todos os vértices de um grafo é igual ao número de arcos do grafo.  A soma dos graus de entrada de todos os vértices também é igual ao número de arcos. Segue daí que um grafo com V vértices tem no máximo

    V*(V−1)

arcos.

Um grafo é  **completo**  se todo par ordenado de vértices distintos é um arco. Um grafo completo com V vértices tem exatamente V*(V−1) arcos. Um **torneio** é qualquer grafo dotado da seguinte propriedade: para cada par V-W de vértices distintos, V-W é um arco ou W-V é um arco, mas não ambos. Um torneio com V vértices tem exatamente ½V(V−1) arcos.

## Subgrafos

Um subgrafo de um grafo G é um pedaço de G. O conjunto de vértices e o conjunto de arcos do pedaço de G devem ser coerentes. Assim, é melhor formular o conceito como uma relação entre dois grafos: Um grafo H é subgrafo de um grafo G se todo vértice de H é vértice de G e todo arco de H é arco de G. (Notação:  H ⊆ G.)

Alguns tipos de subgrafos merecem destaque. Um subgrafo H de um grafo G é

- **Gerador** (spanning) se contém todos os vértices de G;
- **Induzido** se todo arco de G com ambas as pontas em H também é arco de H;
- **Próprio** se for diferente de G (notação: H ⊂ G).

Um **supergrafo** é o contrário de um subgrafo: um grafo G é supergrafo de um grafo H se H for subgrafo de G.

## Grafos não-dirigidos

Um grafo é não-dirigido (undirected) se cada um de seus arcos é antiparalelo a algum outro arco: para cada arco  v-w,  o grafo também tem o arco  w-v.

Num grafo não-dirigido, a relação de adjacência é simétrica:  um vértice w é adjacente a um vértice v se e somente se v é adjacente a w.

Num grafo não-dirigido, o leque de um vértice v é o conjunto de arestas que incidem em v. O grau de v é o número de arestas no leque de v. É claro que o grau de um vértice é igual ao seu grau de entrada e também ao seu grau de saída.


# Estrutura de dados para um grafo

Como representar um grafo na linguagem C?

Os vertices dos nossos grafos serão representados por inteiros, para isso, faremos:

```C
/* Vértices de grafos são representados por objetos do tipo vertex. */

#define vertex int
```

Para representar os arcos demos dois metodos:

## Matriz de adjacencia

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

## Vetor de listas de adjacencia

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

# Grafos aleatorios

Para escolher um vertice aleatoriamente usamos o envolucro a seguir, que devolve um numero entre 0 e V-1:

```c
/* A função randV() devolve um vértice aleatório do grafo G. Vamos supor que G->V <= RAND_MAX. */

vertex randV( Graph G) {
   double r;
   r = rand( ) / (RAND_MAX + 1.0);
   return r * G->V;
}
```

## Construtor 1

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

## Construtor 2

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

# Caminhos e ciclos em grafos

## Caminhos

Um **passeio** (walk) em um grafo é uma sequência de vértices dotada da seguinte propriedade: se v e w são vértices consecutivos na sequência então v-w é um arco do grafo. Um **passeio é  fechado** (closed) se tem pelo menos dois arcos e seu primeiro vértice coincide com o último.

Um  **caminho** (path) em um grafo é um passeio **sem arcos repetidos**, ou seja, um passeio em que os arcos são todos diferentes entre si. Um caminho é simples  se não tem vértices repetidos.

A **origem** de um caminho é o seu primeiro vértice. O **término** é o seu último vértice.  Se um caminho tem origem s e término t, dizemos que  vai de s a t.

O **comprimento** (length) de um caminho é o número de arcos do caminho.  Se um caminho tem n vértices, seu comprimento é pelo menos n−1; se o caminho é simples, seu comprimento é exatamente n−1.

## Ciclos

**Ciclos** são estruturas muito importantes. São os ciclos que tornam grafos interessantes mas também complexos e difíceis de manipular.

Um **ciclo** (cycle) em um grafo é um caminho fechado. (Portanto, todo ciclo tem comprimento maior que 1 e não tem arcos repetidos.)  Dizemos que um arco v-w pertence a um dado ciclo (ou que o ciclo passa pelo arco) se o vértice w é o sucessor de v no ciclo.   Um ciclo é simples se não tem vértices repetidos exceto pelo último (que coincide com o primeiro).

É apropriado abusar um pouco do conceito de igualdade e tratar dois ciclos simples como iguais se eles têm o mesmo conjunto de arcos, ainda que tenham origens diferentes.  Por exemplo, trataremos como iguais os ciclos `6-2-7-3-6, 2-7-3-6-2, 7-3-6-2-7 e 3-6-2-7-3`.

Todos os arcos de um ciclo apontam no mesmo sentido — de um vértice do ciclo para o seu sucessor. Há quem goste de enfatizar esse fato dizendo ciclo dirigido no lugar de ciclo.

# Grafos topologicos

A definição de grafos topológicos exige que os vértices sejam numerados de uma certa maneira.  Assim, cada grafo é acompanhado de uma numeração (ranking) dos vértices, ou seja, uma atribuição de números inteiros aos vértices. Essa numeração dos vértices será representada por um vetor cujos índices são vértices e cujos elementos são números inteiros.

## Definição

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

## Propriedades

- Grafos topológicos não têm ciclos (a reciproca é verdadeira).
- Todo vértice de um grafo topológico é término de um caminho cuja origem é uma fonte.
- Todo vértice de um grafo topológico é origem de um caminho cujo término é um sorvedouro.

## Variante antitopologica

Uma numeração anti-topológica é uma numeração topológica ao contrário. Mais precisamente, uma numeração atopo[] dos vértices de um grafo é anti-topológica se  `atopo[v] > atopo[w]` para todo arco v-w.

# Florestas radicadas

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

## Propriedades

- Florestas radicadas não têm ciclos.
- Todo vértice de uma floresta radicada é término de um caminho que começa em uma fonte, e todo vértice é origem de um caminho que termina em um sorvedouro.
- Todo vértice de uma floresta radicada é término de um único caminho que começa numa fonte.
Segue daí imediatamente que, para quaisquer dois vértices v e z, existe no máximo um caminho de v a z. Ademais, se existe um caminho de v a z então não existe caminho de z a v.

## Terminologia

- As **fontes** de uma floresta radicada são chamadas raizes.
- Os **sorvedouros** de uma floresta radicada são chamados folhas.
- Cada vizinho de um vértice v é um **filho** de v.
- Todo vértice w de uma floresta radicada, exceto uma raiz, tem um pai.
- Um vértice u é **ancestral** de um vértice z se existe um caminho u a z.  Um ancestral de z é próprio se for diferente de z.
- Um vértice z é **descendente** de um vértice u se u é ancestral de z. Um descendente de u é próprio se for diferente de u.
- Um **primo** de um vértice u é qualquer vértice que não seja ancestral nem descendente de u.
- A **profundidade** de um vértice w numa floresta radicada é o comprimento do único caminho que começa em alguma raiz e termina em w.  A **altura** da floresta radicada é a profundidade de um vértice de profundidade máxima.

## Árvores radicadas

Uma árvore radicada (= branching) é uma floresta radicada que tem uma só raiz. A raiz é o vértice da árvore que tem o menor número em qualquer numeração topológica.

## Grafos bipartidos dirigidos

Um grafo é bipartido dirigido se tiver uma numeração topológica com apenas dois valores: 0 e 1. Em outras palavras, um grafo é bipartido dirigido se existe um vetor topo[] indexado pelos vértices tal que, para cada arco v-w, topo[v] vale 0 e topo[w] vale 1.


# Busca em profundidade (DFS)

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

## Desempenho da busca DFS

A função GRAPHdfs() examina o leque de saída de cada vértice uma só vez. Portanto, cada arco é examinado uma só vez. Assim, se o grafo tem V vértices e A arcos, GRAPHdfs() consome tempo proporcional a **O(V+A)**.

## Pré-ordem

A ordem em que a função GRAPHdfs() descobre os vértices do grafo é chamada pré-ordem (= preorder).  Para obter a permutação dos vértices em pré-ordem basta inverter o vetor pre[], ou seja, obter a order de descoberta dos vertices:

```c
vertex vv[1000];
for (vertex v = 0; v < G->V; ++v)
    vv[pre[v]] = v;
for (int i = 0; i < G->V; ++i)
    printf( "%d ", vv[i]);
```

Resumindo, dizemos que qualquer implementação do algoritmo de busca em profundidade descobre os vértices do grafo em pré-ordem.

## Florestas de busca

A busca em profundidade deixa uma espécie de rastro ao percorrer um grafo. Esse rastro é uma floresta radicada. Pode-se dizer que a floresta é um esqueleto do grafo.

O arco v-w  que dfsR() percorre para descobrir o vértice w é conhecido como **arco de floresta**. No fim da execução de GRAPHdfs(), o conjunto dos arcos de floresta define uma floresta radicada. Essa é a **floresta de busca em profundidade**, ou **floresta DFS**, construída pela função.

Uma floresta DFS contém todos os vértices do grafo e portanto é um subgrafo gerador.

Uma floresta DFS pode ser confortavelmente representada por um vetor de pais, digamos `pa[0..V-1]`: para cada vértice w, pa[w] é o pai de w na floresta. A seguinte função calcula o vetor de pais à medida que numera o grafo em pré-ordem:

```c
static int cnt;
int pre[1000];
vertex pa[1000];

/* A função GRAPHdfs() faz uma busca DFS no grafo G. Ela atribui um número de ordem pre[x] a cada vértice x (o k-ésimo vértice descoberto recebe o número de ordem k) e registra a correspondente floresta DFS no vetor de pais pa[].  (Código inspirado no programa 18.3 de Sedgewick.) */
void GRAPHdfs( Graph G)
{
   cnt = 0;
   for (vertex v = 0; v < G->V; ++v)
      pre[v] = -1;
   for (vertex v = 0; v < G->V; ++v)
      if (pre[v] == -1) {
         pa[v] = v; // v é uma raiz da floresta
         dfsR( G, v);
      }
}

/* A funçao dfsR() visita todos os vértices de G que podem ser alcançados a partir de v sem passar por vértices já descobertos. Todos esses vértices, e só esses, tornam-se descendentes de v na floresta radicada definida por pa[]. Se x é o k-ésimo vértice descoberto, a função atribui cnt+k a pre[x].  (O código supõe que G é representado por listas de adjacência.) */
static void dfsR( Graph G, vertex v)
{
   pre[v] = cnt++;
   for (link a = G->adj[v]; a != NULL; a = a->next) {
      vertex w = a->w;
      if (pre[w] == -1) {
         pa[w] = v; // v-w é arco da floresta
         dfsR( G, w);
      }
   }
}
```

### Acima, abaixo, à esquerda e à direita

Digamos que x e y são dois vértices da floresta DFS de um grafo. Como em qualquer floresta radicada, x pode ser ancestral, descendente, ou primo de y. (Um vértice é primo de outro se não for ancestral nem descendente do outro.) Além disso, em consequência da ordem em que os vizinhos de cada vértice são visitados durante a busca DFS, podemos distinguir um vértice mais velho de um vértice mais novo. Assim, se x e y são primos e pre[] é a numeração em pré-ordem associada à floresta então

- x é primo mais velho, ou esquerdo, de y  se  pre[x] < pre[y]  e
- x é primo mais novo, ou direito, de y  se  pre[x] > pre[y].
-
É claro que essas relações são mutuamente inversas: x é primo mais velho de y se e somente se y é primo mais novo de x.

**Essas relações genealógicas entre os vértices de uma floresta DFS poderiam também ser descritas por uma metáfora espacial: acima corresponde a ancestral, abaixo corresponde a descendente, à esquerda corresponde a primo mais velho e à direita corresponde a primo mais novo.**

## Arcos de retorno, de avanço, e cruzados

Dada uma floresta DFS de um grafo, cada arco v-w do grafo pertence a um de três tipos, conforme a posição relativa de v e w na floresta:

- se w é um descendente de v então v-w é de **avanço** (= forward arc = down arc);
- se w é um ancestral de v então v-w é de **retorno** (= back arc);
- se w é um primo de v então v-w é **cruzado** (= cross arc).

## Numeração pos-ordem

A busca em profundidade também pode numerar os vértices em pós-ordem. A combinação das numerações pos e pre é essencial para decidir eficientemente se um dado arco do grafo é de avanço, de retorno, ou cruzado em relação à floresta DFS.

Durante uma busca em profundidade, um vértice v *morre* depois que todos os seus vizinhos em G e todos os seus descendentes na floresta DFS forem visitados. A ordem em que os vértices morrem é conhecida como pós-ordem. Se o grafo for uma árvore radicada, a permutação dos vértices em pós-ordem pode ser descrita recursivamente:  para cada vizinho w da raiz, visite, em pós-ordem, a subárvore que tem raiz w; depois, visite a raiz

A implementação da busca em profundidade fica a seguir:

```c
static int cnt, pre[1000];
static int cntt, post[1000];
static vertex pa[1000];

void GRAPHdfs( Graph G) {
    cnt = cntt = 0;
    for (vertex v = 0; v < G->V; ++v)
        pre[v] = -1;
    for (vertex v = 0; v < G->V; ++v)
        if (pre[v] == -1) {
            pa[v] = v;
            dfsR( G, v);
        }
}

static void dfsR( Graph G, vertex v) {
    pre[v] = cnt++;
    for (link a = G->adj[v]; a != NULL; a = a->next)
        if (pre[a->w] == -1) {
            pa[a->w] = v;
            dfsR( G, a->w);
        }
    post[v] = cntt++; // numeração em pós-ordem
}
```

### Intervalos de vida

A encarnação da dfs() permanece em execução entre o instante em que `v` é descoberto e o instante em que `v` morre. Diremos que esse é o intervalo de vida (= lifespan) da encarnação. É claro que o início do intervalo corresponde a pre[v] e o fim do intervalo corresponde a post[v].

Em virtude do caráter recursivo de dfs(), a coleção dos intervalos de vida de todas as encarnações de dfs() é bem organizada no seguinte sentido: cada dois intervalos são disjuntos ou estão encaixados.

## Ancestrais, descendentes, e primos

As relações genealógicas (ancestral, descendente, primo) entre os vértices de uma floresta DFS podem ser formuladas em termos dos intervalos de vida das várias encarnações de dfs(). Um vértice x é ancestral de um vértice y se o intervalo de dfs(G,x) contém o intervalo de dfs(G,y); x é descendente de y se o intervalo de dfs(G,x) está contido no intervalo de dfs(G,y); x é primo esquerdo de y se o intervalo de dfs(G,x) vem antes do de dfs(G,y); x é primo direito de y se o intervalo de dfs(G,x) vem depois do de dfs(G,y).

Segue daí que a combinação das numerações em pré/pós-ordem permite decidir, em tempo constante, a relação entre dois vértices da floresta DFS: para quaisquer dois vértices x e y,

- **x é ancestral próprio de y** se e somente se  pre[x] < pre[y]  e  post[x] > post[y];
- **x é descendente próprio de y** se e somente se  pre[x] > pre[y]  e  post[x] < post[y];
- **x é primo esquerdo de y** se e somente se  pre[x] < pre[y]  e  post[x] < post[y];
- **x é primo direito de y** se e somente se  pre[x] > pre[y]  e  post[x] > post[y].

A combinação de pre[] e post[] também permite decidir, em tempo constante, a classe de cada arco em relação à floresta DFS: para qualquer arco v-w que não pertence à floresta,

- **v-w é de avanço** se e somente se  pre[v] < pre[w]  e  post[v] > post[w];
- **v-w é de retorno** se e somente se  pre[v] > pre[w]  e  post[v] < post[w];
- **v-w é cruzado** se e somente se  pre[v] > pre[w]  e  post[v] > post[w].

Note que se v-w é um arco cruzado então w é primo esquerdo de v. A seguinte tabela resume a caracterização dos arcos:

| pre | post | |
| --- | --- | --- |
| v () w | v () w | |
| < | > | floresta |
| < | > | avanço |
| > | < | retorno |
| > | > | cruzado |

Grafos não-dirigidos não têm arcos cruzados. Portanto, para qualquer arco v-w de um grafo não-dirigido,

v-w é de **avanço** se e somente se pre[v] < pre[w] e v-w é de **retorno** se e somente se pre[v] > pre[w].

# Ciclos e DAGs

Um grafo é topológico (ou seja, tem uma numeração topológica) se e somente se não tem ciclos.

## Detecção de ciclos

Um ciclo num grafo é um caminho fechado. Dizemos que um grafo é **acíclico** se não tem ciclo algum. A DFS oferece uma solução muito boa para detectar um ciclo em um grafo.

O algoritmo começa por fazer uma busca DFS no grafo. Depois, usa as numerações em pré- e pós-ordem produzidos pela busca para procurar um arco de retorno, pois todo arco de retorno faz parte de um ciclo.

```c
static int pre[1000], post[1000];

/* Esta função decide se o grafo G tem um ciclo. */
bool GRAPHhasCycle( Graph G) {

   GRAPHdfs( G); // calcula pre[] e post[]

   for (vertex v = 0; v < G->V; ++v) {
      for (link a = G->adj[v]; a != NULL; a = a->next) {
         vertex w = a->w;
         if (post[v] < post[w]) // v-w é de retorno
            return true;
      }
   }
   // post[v] > post[w] para todo arco v-w
   return false;
}
```

## Grafos topológicos versus dags

**Teorema**:  Um grafo é acíclico se e somente se tem uma numeração topológica.

Grafos acíclicos também são conhecidos como  **dags**  (= directed acyclic graphs).  O teorema pode ser parafraseado assim: dags e grafos topológicos são a mesma coisa.

## Versão compactada do algoritmo

Podemos acelerar ligeiramente a função GRAPHhasCycle() se interrompermos a busca DFS assim que um ciclo for encontrado.

```c
static int cnt, pre[1000];
static int cntt, post[1000];
/* Esta função decide se o grafo G tem um ciclo. */

bool GRAPHhasCycle( Graph G) {

   cnt = cntt = 0;
   for (vertex v = 0; v < G->V; ++v)
      pre[v] = post[v] = -1;
   for (vertex v = 0; v < G->V; ++v)
      if (pre[v] == -1)
         if (dfsRcycle( G, v))
            return true;

   return false;
}

/* A função dfsRcycle() devolve true se encontra um ciclo ao percorrer G a partir do vértice v e devolve false em caso contrário. */
static bool dfsRcycle( Graph G, vertex v) {

   pre[v] = cnt++;
   for (link a = G->adj[v]; a != NULL; a = a->next) {
      vertex w = a->w;
      if (pre[w] == -1) {
         if (dfsRcycle( G, w))
            return true;
      }
      else {
         if (post[w] == -1)
            return true; // base da recursão
      }
   }
   post[v] = cntt++;
   return false;
}
```

A complexidade desse algoritmo no pior caso é linear, O(V+A).

# Caminhos minimos

Um caminho C num grafo é mínimo se não existe outro caminho que tenha a mesma origem e o mesmo término que C mas comprimento menor que o de C.

**Problema do caminho mínimo:** Dados vértices s e t de um grafo G, encontrar um caminho mínimo em G que tenha origem s e término t.

## Arcos relaxados e potencial viável

Encontrar um caminho mínimo de um dado vértice s a um dado vértice t é relativamente fácil. Mais difícil é provar que o caminho é de fato mínimo.

Para provar que um dado caminho C é mínimo, precisamos mostrar que não existe caminho mais curto. Em outras palavras, precisamos mostrar que todo caminho com a mesma origem e o mesmo término que os de C é pelo menos tão comprido quanto C.  A fim de obter uma tal prova, vamos introduzir os conceitos de potencial e relaxação. Embora simples, esses conceitos são um tanto abstratos e por isso difíceis de assimilar.

Um **potencial** para um grafo G é uma numeração dos vértices de G, ou seja, um vetor que associa um número a cada vértice de G.  (Uma boa metáfora para o potencial de um vértice é sua altura acima de um hipotético chão. Por isso, usarei a inicial h de height para designar um potencial.)  Em relação a um potencial `h[]`, dizemos que um arco v-w está **tenso** se h[w]-h[v] > 1  e está **relaxado** se h[w]-h[v]  ≤ 1.

Um **potencial é viável** se deixa todos os arcos de G relaxados. Por exemplo, qualquer numeração anti-topológica é um potencial viável. Mas a recíproca não é verdadeira: o potencial constante é viável mas não é anti-topológico.

### Cota inferior e certificado de minimalidade

Suponha que `h[]` é um potencial viável em um grafo G. É fácil verificar que para qualquer caminho C de um vértice s a um vértice t tem-se

`h[s] + |C|  ≥  h[t] `, sendo |C| o comprimento de C.  Portanto, `|C| ≥ h[t] − h[s]`.

Em palavras: qualquer caminho de s a t é pelo menos tão comprido quanto a diferença de potencial entre t e s.

Assim, se h[ ] é um potencial viável então qualquer caminho M de s a t que tenha comprimento igual a  h[t] − h[s]  é mínimo! Ou seja, esse caminho tem o **certificado de minimalidade**.

## Um algoritmo para dags

Em grafos acíclicos, o problema do caminho mínimo é fácil. O algoritmo é baseado no método de programação dinâmica.

Como se sabe, um grafo é acíclico se e somente se tem uma numeração topológica. Por outro lado, numerações topológicas são equivalentes a permutações topológicas. Suponha então que temos uma permutação topológica vv[] dos vértices de nosso grafo.  O algoritmo processa os vértices em ordem topológica: primeiro vv[0], depois vv[1], depois vv[2], etc.

O algoritmo calcula, de uma só vez, todos os caminhos mínimos que têm origem s. O conjunto de todos esses caminhos pode ser representado por uma árvore radicada com raiz s que contém todos os vértices ao alcance de s, essa árvore de caminhos mínimos, será representada por um vetor de pais, digamos pa[]. As distâncias na árvore a partir do vértice s serão armazenadas num vetor dist[].

```c
#define Dag Graph

/* A função recebe um dag G, uma permutação topológica vv[] de G, e um
vértice s de G. A função armazena em pa[] o vetor de pais de uma árvore
de caminhos mínimos com raiz s. Se um vértice v não está ao alcance de s
, teremos pa[v] == -1. As distâncias a partir de s são armazenadas no
vetor dist[]. O espaço para os vetores pa[] e dist[] deve ser alocado
pelo usuário. */
void DAGshortestPaths( Dag G, vertex *vv, vertex s, vertex *pa, int *dist)
{
   const int INFINITY = G->V;
   for (vertex v = 0; v < G->V; ++v)
      pa[v] = -1, dist[v] = INFINITY;
   pa[s] = s, dist[s] = 0;

   for (int j = 0; j < G->V; ++j) {
      vertex v = vv[j];
      for (link a = G->adj[v]; a != NULL; a = a->next) {
         vertex w = a->w;
         if (dist[v] + 1 < dist[w]) {
            dist[w] = dist[v] + 1; // relaxação de v-w
            pa[w] = v;
         }
      }
   }
}
```

O **consumo de tempo** de `DAGshortestPaths()` é proporcional a  V + A no pior caso.

## Busca em largura (BFS)

A busca em largura começa por um vértice, digamos s, especificado pelo usuário. O algoritmo visita s, depois visita todos os vizinhos de s, depois todos os vizinhos dos vizinhos, e assim por diante.

O algoritmo numera os vértices, em sequência, na ordem em que eles são descobertos (ou seja, visitados pela primeira vez). Para fazer isso, o algoritmo usa uma fila (= queue) de vértices.  No começo de cada iteração, a fila contém vértices que já foram numerados mas têm vizinhos ainda não numerados.

A numeração dos vértices é registrada num vetor  num[]  indexado pelos vértices.  Se v é o k-ésimo vértice descoberto então num[v] vale k.

implementação:

```c
static int num[1000];

/* A função GRAPHbfs() implementa o algoritmo de busca em largura. Ela
visita todos os vértices do grafo G que estão ao alcance do vértice s e
registra num vetor num[] a ordem em que os vértices são descobertos.
Esta versão da função supõe que o grafo G é representado por listas de
adjacência.  (Código inspirado no programa 18.9 de Sedgewick.) */
void
GRAPHbfs( Graph G, vertex s) {
    int cnt = 0;
    for (vertex v = 0; v < G->V; ++v)
        num[v] = -1;
    QUEUEinit( G->V);
    num[s] = cnt++;
    QUEUEput( s);

    while (!QUEUEempty( )) {
        vertex v = QUEUEget( );
        for (link a = G->adj[v]; a != NULL; a = a->next)
            if (num[a->w] == -1) {
                num[a->w] = cnt++;
                QUEUEput( a->w);
            }
    }
    QUEUEfree( );
}
```

A busca em largura a partir de um vértice s constrói uma árvore radicada com raiz s:  cada arco v-w percorrido até um vértice w não numerado é acrescentado à árvore. Essa árvore radicada é conhecida como **árvore de busca em largura**, ou árvore BFS (= BFS tree).

# Componentes conexas

## Grafos não-dirigidos conexos

Um grafo não-dirigido é conexo se cada um de seus vértices está ao alcance de cada um dos demais.  Em outras palavras, um grafo não-dirigido é conexo se tem a seguinte propriedade: para cada par `s` `t` de seus vértices, existe um caminho com origem `s` e término `t`.

Um grafo não-dirigido é desconexo se não for conexo. Para caracterizar grafos não-dirigidos desconexos, precisamos introduzir duas definições: dizemos que um conjunto X de vértices é:

- **isolado** se nenhuma aresta tem uma ponta em X e outra fora de X e
- **trivial** se X for vazio ou contiver todos os vértices do grafo
- .
Agora podemos enunciar a caracterização: um grafo não-dirigido é desconexo se e somente se algum conjunto não-trivial de seus vértices é isolado.

## Componentes conexas de grafos não-dirigidos

Num grafo não-dirigido, a relação ao‑alcance‑de entre vértices é uma relação de equivalência, ou seja, uma relação reflexiva, simétrica e transitiva. Portanto, a relação ao‑alcance‑de impõe uma partição do conjunto de vértices em classes de equivalência: dois vértices s e t estão na mesma classe se e somente se t está ao alcance de s.

Uma **componente conexa** (connected component) de um grafo não-dirigido G é o subgrafo de G induzido por alguma das classes de equivalência da relação ao‑alcance‑de. Podemos resumir essa definição dizendo que uma componente de um grafo não-dirigido é um subgrafo conexo maximal do grafo.

**Problema das componentes conexas:** Encontrar as componentes conexas de um grafo não-dirigido.

## Cálculo das componentes conexas

Há muitas maneiras eficientes de calcular as componentes conexas de um grafo não-dirigido. A função UGRAPHcc() abaixo usa uma busca em profundidade.

Para identificar as componentes conexas, vamos atribuir um rótulo cc[v] a cada vértice v de tal modo que dois vértices tenham o mesmo rótulo se e somente se estiverem na mesma componente conexa.

```c
#define UGraph Graph

/* A função UGRAPHcc() devolve o número de componentes conexas do grafo
não-dirigido G. Além disso, armazena no vetor cc[] uma numeração dos
vértices tal que dois vértices v e w pertencem à mesma componente se e
somente se cc[v] == cc[w]. */
int UGRAPHcc( UGraph G, int *cc)
{
   int id = 0;
   for (vertex v = 0; v < G->V; ++v)
      cc[v] = -1;
   for (vertex v = 0; v < G->V; ++v)
      if (cc[v] == -1)
         dfsRcc( G, cc, v, id++);
   return id;
}

/* A função auxiliar dfsRcc() atribui o número id a todos os vértices
que estão na mesma componente conexa que v. A função supõe que o grafo
G é representado por listas de adjacência. */
static void dfsRcc( UGraph G, int *cc, vertex v, int id)
{
   cc[v] = id;
   for (link a = G->adj[v]; a != NULL; a = a->next)
      if (cc[a->w] == -1)
         dfsRcc( G, cc, a->w, id);
}
```
Depois de executar UGRAPHcc() uma só vez, é fácil verificar, em tempo constante, se dois vértices, digamos s e t, estão na mesma componente conexa:

```c
int UGRAPHconnect( UGraph G, vertex s, vertex t) {
   return cc[s] == cc[t];
}
```

## Circuitos e florestas

Um **circuito** (circuit) num grafo não-dirigido é um ciclo dotado da seguinte propriedade: se um arco `v-w` pertence ao ciclo então o arco antiparalelo `w-v` não pertence ao ciclo. Por exemplo, um ciclo da forma `a-b-c-d-a` é um circuito e um ciclo da forma `a-b-c-a-d-e-a` também é um circuito. Já um ciclo da forma `a-b-c-d-e-f-d-c-a` não é um circuito.

Num grafo não-dirigido, o inverso de um circuito também é um circuito.

Um circuito é **simples** se não tem vértices repetidos exceto o último.

É aceitável abusar da ideia de igualdade e dizer que dois circuitos simples são iguais se seus conjuntos de arestas são iguais.  Por exemplo, é aceitável dizer que circuitos `0-1-2-3-0`, `1-2-3-0-1`, `2-3-0-1-2` e `3-0-1-2-3` são todos iguais.

## Detecção de circuitos

**Problema**:  Decidir se um dado grafo não-dirigido tem um circuito. Essa formulação do problema pede uma resposta booleana — sim ou não.

Algoritmos eficientes de detecção de circuitos usam busca em profundidade. Em relação a qualquer floresta DFS, todo arco de retorno pertence a um ciclo simples. Esse ciclo é um circuito a menos que tenha comprimento 2. Essa observação leva a um algoritmo eficiente de detecção de circuitos. A ideia é fazer uma busca em profundidade para calcular uma numeração em pré-ordem e o correspondente vetor de pais e depois examinar os arcos um a um à procura de um arco de retorno que não seja antiparalelo a um arco da floresta:

```c
#define UGraph Graph
static int pre[1000];
static vertex pa[1000
];
/* Esta função decide se o grafo não-dirigido G tem algum circuito. */
bool UGRAPHhasCircuit( UGraph G)
{
   GRAPHdfs( G); // calcula pre[] e pa[]

   for (vertex v = 0; v < G->V; ++v) {
      for (link a = G->adj[v]; a != NULL; a = a->next) {
         vertex w = a->w;
         if (pre[v] > pre[w]) // v-w é de retorno
            if (w != pa[v]) return true;
      }
   }
   return false;
}
```

## Florestas

Uma floresta (forest) é um grafo não-dirigido sem circuitos.

**Definição**: Um grafo não-dirigido G é uma floresta se e somente se algum subgrafo gerador R de G tem a seguinte propriedade: R é uma floresta radicada e cada arco de G pertence a R ou é antiparalelo a um arco de R.

Uma **folha** (leaf) de uma floresta é qualquer vértice de grau 1. As palavras pai, filho e raiz, que usamos ao tratar de florestas radicadas, não fazem sentido em florestas.

### Árvores

Uma **árvore** (tree) é uma floresta conexa. Em outras palavras, uma árvore é um grafo não-dirigido conexo sem circuitos. Portanto, cada componente conexa de uma floresta é um árvore.

Duas propriedades fundamentais de árvores:

- Para cada par s t de vértices de uma árvore, existe um e um só caminho de s a t.
- Toda árvore com V vértices tem exatamente V−1 arestas.
