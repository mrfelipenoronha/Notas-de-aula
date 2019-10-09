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
   - [Grafos aresta-biconexos](#grafos-aresta-biconexos)   
      - [Pontes, florestas e abraços](#pontes-florestas-e-abraços)   
      - [Número de pré-ordem mínimo](#número-de-pré-ordem-mínimo)   
   - [Algoritmo de Tarjan para aresta-biconexão (TARZAN)](#algoritmo-de-tarjan-para-aresta-biconexão-tarzan)   
   - [Versão on-the-fly do algoritmo de Tarjan](#versão-on-the-fly-do-algoritmo-de-tarjan)   
   - [Componentes aresta-biconexas](#componentes-aresta-biconexas)   
      - [Floresta das componentes aresta-biconexas](#floresta-das-componentes-aresta-biconexas)   
      - [Problema das componentes aresta-biconexas](#problema-das-componentes-aresta-biconexas)   
      - [Componentes versus florestas DFS](#componentes-versus-florestas-dfs)   
      - [O algoritmo de Tarjan](#o-algoritmo-de-tarjan)   
      - [Implementação on-the-fly do algoritmo](#implementação-on-the-fly-do-algoritmo)   
- [Coloração de vertices](#coloração-de-vertices)   
   - [Uma heurística gulosa](#uma-heurística-gulosa)   
   - [Troca de cores em componentes bicoloridas](#troca-de-cores-em-componentes-bicoloridas)   
   - [Cliques](#cliques)   
   - [Bipartição do conjunto de vertices](#bipartição-do-conjunto-de-vertices)   
   - [Circuitos ímpares](#circuitos-ímpares)   
- [Emparelhamento](#emparelhamento)   
   - [Caminhos alternantes](#caminhos-alternantes)   
   - [Representação de emparelhamentos](#representação-de-emparelhamentos)   
   - [Cálculo da diferença simétrica](#cálculo-da-diferença-simétrica)   
   - [Algoritmo húngaro](#algoritmo-húngaro)   
- [Custo nos arcos e arestas](#custo-nos-arcos-e-arestas)   
   - [Listas de adjacência com custos](#listas-de-adjacência-com-custos)   

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

## Grafos aresta-biconexos

Um grafo não-dirigido é **aresta-biconexo** (edge-biconnected = 2-edge-connected) se é conexo e cada uma de suas arestas pertence a algum circuito.

Num grafo não-dirigido, qualquer aresta que não pertence a um circuito é conhecida como **ponte** (bridge). Portanto, um grafo não-dirigido é aresta-biconexo se e somente se é conexo e não tem pontes.

Pontes podem ser caracterizadas pela seguinte propriedade: uma aresta a de um grafo não-dirigido G é uma ponte se e somente se o grafo G−a tem mais componentes conexas que G. (Nesse sentido, grafos sem pontes são tolerantes a falhas.)

**Problema da aresta-biconexão**: Decidir se um grafo não-dirigido é aresta-biconexo.

A parte mais difícil do problema é decidir se o grafo é livre de pontes. Para fazer isso, basta verificar se o grafo continua conexo depois da remoção de qualquer aresta. Mas a aplicação inocente dessa ideia resulta em um algoritmo muito ineficiente.

Vamos mostrar o algoritmo de Tarjan, que resolve esse problema de forma eficiente. Mas para isso, precisamos ver algumas outras coisas antes.

### Pontes, florestas e abraços

É fácil entender que toda floresta DFS num grafo não-dirigido passa por todas as pontes do grafo. Mas é preciso formular essa propriedade com mais cuidado pois pontes são arestas enquanto florestas DFS são formadas por arcos:

Propriedade DFS das pontes: Depois de uma busca DFS num grafo não-dirigido, um dos dois arcos de cada ponte do grafo é um arco da floresta DFS.

Quais arcos da floresta DFS fazem parte de pontes? A seguinte definição dá um passo na direção da resposta. Dado um arco `u-v` da floresta DFS, dizemos que um arco `x-y` do grafo abraça `u-v` se as seguintes condições forem satisfeitas:

1. x é descendente de v na floresta,
2. y é ancestral de u na floresta e
3. x-y é diferente de v-u.

(Note que x pode ser igual a v e y pode ser igual a u.)  É óbvio que qualquer arco que abraça u-v é um arco de retorno e portanto fecha um circuito que passa por u-v. A recíproca é verdadeira, embora não seja tão óbvia.  Isso leva à seguinte caracterização:

**Lema do abraço**: Um arco u-v da floresta DFS faz parte de uma ponte se e somente se nenhum arco do grafo abraça u-v.

### Número de pré-ordem mínimo

Para que o lema do abraço possa ser usado de maneira eficiente, é preciso introduzir o conceito de número de **pré-ordem mínimo** (lowest preorder number) de um vértice. Digamos que pre[] é a numeração em pré-ordem produzida pela busca DFS no grafo. Para cada vértice v, a pré-ordem mínima de v é o número onde o mínimo é tomado sobre todos os arcos x-y que abraçam v. Esse número será denotado por `lo[v]`. É conveniente dizer que `lo[v] = pre[v]` nesse caso. Essa definição faz sentido pois se algum arco abraça v então lo[v] < pre[v].  Graças a essa definição, podemos dizer que lo[v] ≤ pre[v] para todo vértice v sem exceção.

Combinado com o lema do abraço, o vetor lo[] permite dizer que **um arco u-v da floresta DFS é uma ponte se e somente se lo[v] ≡ pre[v]**.

**Cálculo eficiente de lo[ ]:** O cálculo eficiente do vetor tem por base a seguinte fórmula recursiva, que exprime a componente v em função das componentes dos filhos de v. Para cada vértice v, lo[v] é o menor dos seguintes três números:

- min{lo[w]} com w filho(s) de v na floresta DFS;
- min{pre[y]} com y vizinho de v e ancestral próprio do pai de v;
- pre[v].
-
A fórmula vale porque qualquer arco x-y que abraça um filho w de v também abraça v, a menos que y seja igual a v. Para aplicar a fórmula, basta examinar os vértices de baixo para cima, começando com as folhas da floresta DFS e subindo em direção às raízes. Em outras palavras, basta examinar os vértices em pré-ordem inversa, pois nessa ordem todo vértice vem depois de seus filhos. Na verdade, podemos igualmente bem examinar os vértices em pós-ordem, pois também nessa ordem todo vértice aparece depois de seus filhos.  A função seguinte UGRAPHlo() faz exatamente isso:

```c
#define UGraph Graph
static int pre[1000];
static vertex pa[1000];
static int lo[1000];

/* Esta função faz uma busca DFS no grafo não-dirigido G e calcula o
lowest preorder number lo[] de cada vértice de G. Também calcula os
vetores pre[], post[] e pa[] correspondentes à busca DFS. */
void UGRAPHlo( UGraph G)
{
   vertex v, w;
   link a;
   GRAPHdfs( G); // calcula pre[], post[] e pa[]
   vertex vv[1000];
   for (v = 0; v < G->V; ++v)
      vv[post[v]] = v;
   // vv[0..V-1] é permutação em pós-ordem
   for (int i = 0; i < G->V; ++i) {
      v = vv[i];
      int min = pre[v];
      for (a = G->adj[v]; a != NULL; a = a->next) {
         w = a->w;
         if (pre[w] < pre[v]) { // (A)
            if (w != pa[v] && pre[w] < min)
               min = pre[w];
         } else { // (B)
            if (pa[w] == v && lo[w] < min)
               min = lo[w];
         }
      }
      lo[v] = min;
   }
}
```

## Algoritmo de Tarjan para aresta-biconexão (TARZAN)

Finalmente, podemos apresentar o algoritmo de Tarjan para o problema da aresta-biconexão.  Depois de usar UGRAPHlo() para calcular os vetores pre[] e lo[], o algoritmo verifica a existência de pontes aplicando o teste lo[v] ≡ pre[v] a todo vértice v.

```c
#define UGraph Graph
static int pre[1000], post[1000];
static vertex pa[1000];
static int lo[1000];

/* A função UGRAPHisEbc() decide se o grafo não-dirigido G é
aresta-biconexo. (A função implementa o algoritmo de Tarjan.) */
bool UGRAPHisEbc( UGraph G)
{
   vertex v;
   link a;
   UGRAPHlo( G); // calcula pre[], pa[] e lo[]
   for (v = 0; v < G->V; ++v) {
      if (lo[v] == pre[v] && pa[v] != v)
         return false; // pa[v]-v é ponte
   }
   int roots = 0;
   for (v = 0; v < G->V; ++v) {
      if (pa[v] == v) ++roots;
      if (roots > 1)
         return false; // G é desconexo
   }
   return true;
}
```

## Versão on-the-fly do algoritmo de Tarjan

```c
#define UGraph Graph
static int cnt, pre[1000];
static vertex pa[1000];
static int lo[1000];

/* A função UGRAPHisEbc() decide se o grafo não-dirigido G é
aresta-biconexo. (Esta é uma implementação on-the-fly do algoritmo de
Tarjan. Código inspirado no programa 18.7 de Sedgewick.) */
bool UGRAPHisEbc( UGraph G)
{
   vertex v;
   for (v = 0; v < G->V; ++v)
      pre[v] = -1;
   cnt = 0;
   pa[0] = 0;
   if (dfsRbridge( G, 0))
      return false; // G tem ponte
   for (v = 1; v < G->V; ++v)
      if (pre[v] == -1)
         return false; // G é desconexo
   return true;
}
/* A função dfsRbridge() calcula lo[v]. Devolve true se houver uma ponte na subfloresta DFS que tem raiz v. Senão, devolve false. */
static bool dfsRbridge( UGraph G, vertex v)
{
   link a;
   pre[v] = cnt++;
   int min = pre[v];
   for (a = G->adj[v]; a != NULL; a = a->next) {
      vertex w = a->w;
      if (pre[w] != -1) {
         // v-w é de retorno ou de avanço
         if (w != pa[v] && pre[w] < min)
            min = pre[w];
      } else {
         pa[w] = v;
         if (dfsRbridge( G, w)) // calcula lo[w]
            return true;
         if (lo[w] == pre[w]) // v-w é ponte
            return true;
         if (lo[w] < min)
            min = lo[w];
      }
   }
   lo[v] = min;
   return false;
}
```

## Componentes aresta-biconexas

Uma **componente aresta-biconexa** (edge-biconnected component = 2-edge-connected component) de um grafo não-dirigido G é um subgrafo aresta-biconexo maximal de G.

As seguinte propriedades das componentes aresta-biconexas de grafos não-dirigidos seguem imediatamente da definição:

1. todo vértice do grafo pertence a uma e uma só de suas componentes aresta-biconexas
1. se uma aresta do grafo tem ambas as pontas numa certa componente aresta-biconexa então essa aresta pertence a essa componente.

Portanto, as componentes aresta-biconexas determinam uma partição do conjunto de vértices do grafo e cada componente aresta-biconexa é induzida por um dos blocos da partição.

**Componentes versus pontes**: É claro que toda aresta de uma componente aresta-biconexa pertence a um circuito.  Reciprocamente, toda aresta do grafo que pertence a um circuito também pertence a alguma componente aresta-biconexa. Em outras palavras, uma aresta de um grafo é uma ponte se e somente se suas pontas estão em duas componentes aresta-biconexas distintas.

### Floresta das componentes aresta-biconexas

O grafo das componentes aresta-biconexas de G é o grafo não-dirigido `B(G)` definido assim:  os vértices de B(G) são as componentes aresta-biconexas de G e há uma aresta I–J em B(G) se e somente se I é diferente de J e alguma aresta de G tem uma ponta em I e outra em J. É claro que as arestas de B(G) correspondem às pontes de G. É fácil verificar que o grafo B(G) não tem circuitos, ou seja, que é uma floresta.

### Problema das componentes aresta-biconexas

**Encontrar todas as componentes aresta-biconexas de um grafo não-dirigido.**

A relação entre componentes aresta-biconexas e pontes mostra o seguinte: Se P é o conjunto de todas as pontes de um grafo G então cada componente conexa de G−P é uma componente aresta-biconexa de G e vice-versa.

### Componentes versus florestas DFS

Tarjan mostroU como a busca DFS pode ser usada para encontrar eficientemente as componentes aresta-biconexas de um grafo. O algoritmo de Tarjan é uma extensão do algoritmo que decide se um grafo é aresta-biconexo.

Antes de examinar o algoritmo, é necessário entender a relação entre florestas DFS e componentes aresta-biconexas.  Suponha que um grafo é submetido a uma busca DFS. A  cabeça  de uma componente aresta-biconexa é o primeiro vértice da componente a ser descoberto pela busca. Em outras palavras, a cabeça é o vértice da componente que tem o menor número de pré-ordem. Podemos dizer que a cabeça é a porta de entrada da componente.

A cabeça de uma componente aresta-biconexa é (1) uma raiz da floresta DFS ou (2) a ponta final de um arco da floresta DFS cuja ponta inicial está em outra componente. No caso (2), o arco da floresta que entra na componente faz parte de uma ponte do grafo.

Segue da definição da busca DFS que todos os vértices de uma componente aresta-biconexa descendem, na floresta DFS, da cabeça da componente.  Além disso, se c é a cabeça de uma componente aresta-biconexa C então, para qualquer vértice x de C, todos os vértices do caminho que vai de c a x na floresta DFS pertencem a C.  Essas duas propriedades podem ser resumidas assim:

**Propriedade DFS das componentes aresta-biconexas**:  Para qualquer floresta DFS de um grafo não-dirigido, a subloresta induzida pelo conjunto de vértices de qualquer componente aresta-biconexa do grafo é uma árvore radicada.

Se removermos os arcos da floresta DFS que entram nas cabeças de componentes aresta-biconexas, teremos uma coleção de árvores radicadas. De acordo com a propriedade acima, cada uma dessas árvores corresponde, exatamente, a uma componente aresta-biconexa do grafo.

Segue da propriedade que a cabeça de qualquer componente aresta-biconexa é o vértice da componente que tem o maior número de pós-ordem.

### O algoritmo de Tarjan

Considere a floresta DFS produzida por uma busca DFS num grafo não-dirigido. Digamos que `c0, c1, … , cn−1` são as cabeças das componentes aresta-biconexas do grafo. Suponha que essa lista de cabeças está em pós-ordem. Assim, c0 é a primeira cabeça a morrer — ou seja, a primeira a terminar de ser processada pela função dfsR() — e cn−1 é a última. Para cada i, seja Ci a componente aresta-biconexa cuja cabeça é ci. Não é difícil deduzir da propriedade DFS das componentes aresta-biconexas que C0 é induzida pelo conjunto de todos os descendentes de c0. Já C1 é induzida pelo conjunto de descendentes de c1 que não estão em C0. Da mesma forma, C2 é induzida pelo conjunto de descendentes de c2 que não estão em C0 ∪ C1.  Finalmente, Cn−1 é induzida pelo conjunto de descendentes de cn−1 que não estão em C0 ∪ … ∪ Cn−2.

Essas observações são a base do algoritmo de Tarjan. Para implementar o algoritmo, basta saber distinguir uma cabeça de um outro vértice qualquer. Como já observamos acima, toda cabeça é a ponta final de um arco de floresta que faz parte de uma ponte (exceto se a cabeça é uma raiz da floresta). Portanto, um vértice v é uma cabeça se e somente se

`lo[v] ≡ pre[v]`.


```c
static int pre[1000], post[1000];
static int cnt, cntt;
static vertex pa[1000];
static int lo[1000];
int ebcc[1000];

/* A função UGRAPHebcc() devolve o número (quantidade) de componentes
 aresta-biconexas do grafo não-dirigido G e armazena no vetor ebcc[] uma
 numeração dos vértices dotada da seguinte propriedade: ebcc[v] == ebcc[w]
 se e somente se v e w estão na mesma componente. */
int UGRAPHebcc( UGraph G)
{
   UGRAPHlo( G); // calcula lo[]
   // também calcula pre[], post[] e pa[]
   for (vertex v = 0; v < G->V; ++v)
      ebcc[v] = -1;
   vertex vv[1000];
   for (vertex v = 0; v < G->V; ++v)
      vv[post[v]] = v;
   // vv[0..V-1] é permutação em pós-ordem
   int id = 0;
   for (int j = 0; j < G->V; ++j) {
      vertex v = vv[j];
      if (lo[v] == pre[v]) // ebcc[v] == -1
         // v é cabeça de componente
         compR( G, v, id++);
   }
   return id;
}

/* Esta função faz ebcc[x] = id para todo vértice x que seja descendente
de v na floresta radicada representada por pa[] e tenha ebcc[x] == -1. */
static void compR( UGraph G, vertex v, int id)
{
   ebcc[v] = id;
   for (link a = G->adj[v]; a != NULL; a = a->next) {
      vertex w = a->w;
      if (pa[w] == v && ebcc[w] == -1)
         compR( G, w, id);
   }
}
```

### Implementação on-the-fly do algoritmo

Ao invocar UGRAPHlo(), a função UGRAPHebcc() examina o grafo todo duas vezes. Embora isso resulte em um algoritmo linear, é preferível obter o mesmo efeito examinando o grafo apenas uma só vez.  Para fazer isso, é preciso que as componentes aresta-biconexas sejam rotulados durante a busca DFS que calcula lo[] e não depois dela.  Não é óbvio como fazer isso, uma vez que a busca DFS não descobre uma componente aresta-biconexa completa de uma só vez: ela descobre alguns vértices de uma componente, depois alguns vértices de outra, depois alguns vértices de mais outra, até que finalmente volta a descobrir os vértices restantes da primeira componente.

Para lidar com essa dificuldade, será necessário armazenar temporariamente, em uma pilha, todos os vértices descobertos durante o intervalo de vida da cabeça da componente. Quando a cabeça morrer, os vértices da correspondente componente estarão todos na pilha.  A pilha pode ser compartilhado por várias componentes diferentes.  Quando alguma cabeça v morrer, todos os vértices da componente de v estarão armazenados acima de v na pilha, podendo então ser rotulados e removidos da pilha.  A função a seguir usa um vetor stack[0..t-1] para implementar a pilha de vértices.

```c
static int pre[1000], lo[1000];
static vertex pa[1000], stack[1000];
static int t, cnt, id;
int ebcc[1000];

/* A função UGRAPHebcc() devolve o número de componentes aresta-biconexas
 do grafo não-dirigido G e armazena no vetor ebcc[] uma numeração dos
 vértices tal que ebcc[v] == ebcc[w] se e somente se v e w estão na mesma
 componente. (A função implementa o algoritmo de Tarjan.) */
int UGRAPHebcc( UGraph G)
{
   for (vertex v = 0; v < G->V; ++v)
      pre[v] = -1;
   t = cnt = id = 0;
   for (vertex v = 0; v < G->V; ++v)
      if (pre[v] == -1) { // nova etapa
         pa[v] = v;
         dfsRebcc( G, v);
      }
   return id;
}

/* Para todo vértice u na pilha de vértices stack[0..t-1] tem-se
pre[u] != -1 e ebcc[u] == -1.  */
static void dfsRebcc( UGraph G, vertex v)
{
   pre[v] = cnt++;
   stack[t++] = v;
   lo[v] = pre[v];
   for (link a = G->adj[v]; a != NULL; a = a->next) {
      vertex w = a->w;
      if (pre[w] == -1) {
         pa[w] = v;
         dfsRebcc( G, w);
         if (lo[w] < lo[v])
            lo[v] = lo[w];
      } else {
         if (w != pa[v] && pre[w] < lo[v])
            lo[v] = pre[w];
      }
   }
   if (lo[v] == pre[v]) { // v é uma cabeça
      vertex u;
      do {
         u = stack[--t];
         ebcc[u] = id;
      } while (u != v);
      id++;
   }
}
```

# Coloração de vertices

Uma **coloração** dos vértices de um grafo não-dirigido é uma atribuição de cores aos vértices tal que cada vértice recebe uma e uma só cor. Portanto, uma coloração nada mais é que uma partição do conjunto de vértices. Uma coloração é válida se as duas pontas de cada aresta têm cores diferentes.

A expressão coloração com `k` cores não significa que todas as k cores são usadas; significa apenas que o número de cores não passa de k. Portanto, uma coloração válida com k−1 cores também é uma coloração válida com k cores. A propósito, dizemos que um grafo é k-colorível se tem uma coloração válida com k cores.

Encontrar colorações válidas com muitas cores é fácil. Encontrar colorações válidas com poucas cores é bem mais difícil.

**Problema da coloração mínima de vértices**: Dado um grafo não-dirigido G, encontrar uma coloração válida de G com o menor número de cores possível.

É fácil provar que um grafo é k-colorível: basta exibir uma coloração válida com k cores.  Mas como provar que um dado grafo não é k-colorível? Essa questão está na base da dificuldade do problema da coloração.

Um algoritmo rápido para o problema da coloração mínima ainda não foi descoberto. Suspeita-se mesmo que um tal algoritmo não existe.  Em vista disso, trataremos neste capítulo apenas de algumas heurísticas simples.

## Uma heurística gulosa

A seguinte heurística, conhecida como algoritmo de coloração sequencial, produz uma coloração válida de qualquer grafo. No início de cada iteração, temos uma coloração válida incompleta que usa as cores 0 1 2 ... k−1.  Cada iteração consiste no seguinte:

```
escolha um vértice incolor v
se alguma cor i não é usada por nenhum vizinho de v
    então atribua cor i a v
    senão atribua cor k a v e some 1 a k
```

Em geral, cada iteração tem vários candidatos para o papel de i, ou seja, tem mais de uma cor disponível para o vértice v.

```c
#define UGraph Graph

/* Esta função calcula uma coloração válida dos vértices do grafo
não-dirigido G e devolve o número de cores efetivamente usadas. A
coloração é armazenada no vetor color[]. */
int UGRAPHseqColoring( UGraph G, int *color)
{
   int k = 0;
   for (vertex v = 0; v < G->V; ++v) color[v] = -1;

   for (vertex v = 0; v < G->V; ++v) {
      bool disponivel[100];
      int i;
      for (i = 0; i < k; ++i) disponivel[i] = true;
      for (link a = G->adj[v]; a != NULL; a = a->next) {
         i = color[a->w];
         if (i != -1) disponivel[i] = false;
      }
      for (i = 0; i < k; ++i)
         if (disponivel[i]) break;
      if (i < k) color[v] = i;
      else color[v] = k++;
   }
   return k;
}
```

## Troca de cores em componentes bicoloridas

Para fugir do caráter guloso do algoritmo de coloração sequencial, podemos permitir que o algoritmo reveja decisões tomadas em iterações anteriores.

Suponha dada uma coloração válida incompleta do grafo não-dirigido G e um vértice v que ainda não foi colorido. Suponha que dispomos de k cores apenas e que cada uma das cores é usada na vizinhança de v. É possível alterar a coloração corrente de modo que alguma das cores saia da vizinhança de v e assim fique disponível para v?  Eis uma heurística da troca de cores em componentes bicoloridas que pode fazer isso.

Suponha, para simplificar, que a cor 0 aparece uma só vez na vizinhança de v.  Digamos que w é o vizinho de v que tem cor 0. Seja X o conjunto dos vértices que têm cor 0 ou 1 e considere o subgrafo não-dirigido G[X] induzido por X. Suponha que a componente conexa de G[X] que contém w não contém nenhum dos vizinhos de v que têm cor 1. Nesse caso, se intercambiarmos as cores 0 e 1 em X, a cor 0 ficará disponível para v!

Essa heurística pode, às vezes, reduzir o número de cores usado pelo algoritmo de coloração sequencial.

## Cliques

Uma clique num grafo não-dirigido é um conjunto de vértices adjacentes entre si.

É fácil encontrar uma clique pequena num grafo não-dirigido: cada vértice é uma clique de tamanho 1 e cada aresta corresponde a uma clique de tamanho 2. É bem mais difícil encontrar uma clique grande.

**Problema da clique máxima**: Encontrar uma clique de tamanho máximo num grafo não-dirigido.

Há uma relação simples entre cliques e coloração de vértices: se um grafo não-dirigido tem uma clique de tamanho q, então qualquer coloração válida precisa de pelo menos q cores.  Portanto, se um grafo não-dirigido tem uma clique de tamanho q e uma coloração válida com apenas q cores, a coloração é mínima e a clique é máxima.

## Bipartição do conjunto de vertices

Uma **bipartição**, ou bicoloração, de um grafo não-dirigido é uma coloração válida do grafo com duas cores.

**Problema da bipartição:** Decidir se um dado grafo não-dirigido é bipartido.

```c
#define UGraph Graph
int color[1000];

/* A função decide se o grafo não-dirigido G admite bipartição. Em caso
 afirmativo, a função atribui uma cor, 0 ou 1, a cada vértice de G de
 tal forma que toda aresta tenha pontas de cores diferentes. As cores
 dos vértices são armazenadas no vetor color[] indexado pelos vértices.
*/
bool UGRAPHtwoColor( UGraph G)
{
   for (vertex v = 0; v < G->V; ++v)
      color[v] = -1; // incolor
   for (vertex v = 0; v < G->V; ++v)
      if (color[v] == -1) // começa nova etapa
         if (dfsR2color( G, v, 0) == false)
            return false;
   return true;
}

/* Decide se existe uma bicoloração de G que atribui cor c ao vértice v e estende a bicoloração incompleta color[] à componente conexa de G que contém v. */
static bool dfsR2color( UGraph G, vertex v, int c)
{
   color[v] = c;
   for (link a = G->adj[v]; a != NULL; a = a->next) {
      vertex w = a->w;
      if (color[w] == -1) {
         if (dfsR2color( G, w, 1-c) == false)
            return false;
      }
      else { // v-w é de avanço ou de retorno
         if (color[w] == c) // base da recursão
            return false;
      }
   }
   return true;
}
```

## Circuitos ímpares

Um circuito num grafo não-dirigido é ímpar (= odd) se seu comprimento for um número ímpar.  Se um grafo tem um circuito ímpar, então é óbvio que não admite bipartição.  A recíproca é menos óbvia, como veremos a seguir.

Suponha que um grafo não-dirigido G não admite bipartição. Então a função UGRAPHtwoColor() acima devolve false quando recebe G. Portanto, dois vértices v e w de G têm a mesma cor em alguma execução da linha if (color[w] == c) do código.  Observe que o arco v-w é de avanço ou de retorno, uma vez que o grafo é não-dirigido. Suponha, primeiramente, que v-w é de retorno. Seja w-x-y-...-u-v o caminho de w a v na floresta DFS. O comprimento do caminho é par, uma vez que v e w têm a mesma cor e as cores dos vértices se alternam ao longo do caminho. Portanto, w-x-y-...-u-v-w é um circuito ímpar.  Suponha agora que v-w é de avanço. Então um raciocínio análogo ao anterior mostra que o arco w-v pertence a um circuito ímpar.  Conclusão: Se a função G não tem uma bicoloração então o grafo tem um circuito ímpar.

Essa discussão pode ser resumida no seguinte

**Teorema:** Um grafo não-dirigido é bipartido se e somente se não tem circuito ímpar.

# Emparelhamento

Um **emparelhamento** (matching) num grafo não-dirigido é um conjunto de arestas sem pontas em comum.  Em outras palavras, um emparelhamento é um conjunto M de arestas com a seguinte propriedade: o leque de cada vértice tem no máximo uma aresta de M.

**Problema:** Encontrar um emparelhamento máximo em um grafo não-dirigido.

## Caminhos alternantes

Dado um emparelhamento num grafo não-dirigido, como obter um emparelhamento maior?  A resposta passa pelo conceito de caminho alternante.

Suponha dado um emparelhamento M. Se uma aresta `v-w` pertence a M, dizemos que v e w estão **emparelhados ou casados**. Um vértice v é **solteiro** se o leque de v não contém arestas de M.

Um caminho é **alternante** (alternating) em relação a um emparelhamento M se for simples e se suas arestas estiverem alternadamente em M e fora de M. Um caminho alternante é **aumentador** (augmenting) se começa e termina num vértice solteiro e tem comprimento maior que 0.

Se M é um emparelhamento e P um caminho aumentador, vamos indicar por  M⊕P  o conjunto (M − EP) ∪ (EP − M),  sendo EP o conjunto de arestas de P.  O conjunto M⊕P, também conhecido como diferença simétrica de M e EP, pode ser descrito assim: tire de M todas arestas de P que estão em M e acrescente a M todas as arestas de P que não estão em M.  É fácil verificar que

- M⊕P  é um emparelhamento
- M⊕P é maior que M.

Segue daí a ideia de um algoritmo iterativo para o problema do emparelhamento máximo. Cada iteração do algoritmo começa com um emparelhamento M. Cada iteração encontra um caminho aumentador P. A iteração seguinte começa com M⊕P no papel de M.  Se não houver caminho aumentador, o processo para. Quando isso acontece, o emparelhamento M é máximo!

## Representação de emparelhamentos

Antes de pensar em implementar algoritmos, é preciso inventar uma boa maneira de representar um emparelhamento.

Usaremos um vetor `match[]` de vértices indexado por vértices para representar um emparelhamento. Se o emparelhamento casa v com w, teremos `match[v] ≡ w` e `match[w] ≡ v`. É como se match[] cuidasse de cada arco da aresta em separado. Se v é solteiro, adotaremos a convenção `match[v] ≡ -1`. É claro que vale a seguinte propriedade: se match[v] ≠ -1 então  match[match[v]] ≡ v.

## Cálculo da diferença simétrica

A representação de emparelhamentos que acabamos de adotar permite calcular M⊕P de maneira muito rápida. Dado um emparelhamento M, suponha que P é um caminho aumentador que começa num vértice s e termina num vértice t (é claro que s e t são solteiros). Suponha que o caminho é representado por um vetor pa[], para cada vértice w do caminho, pa[w] é o vértice anterior a w no caminho. Suponha também que pa[s] ≡ s. Então, se M é representado por um vetor match[], a seguinte função calcula M⊕P modificando match[] à medida que percorre P do fim para o começo.

```c
static vertex pa[1000];

/* Esta função executa a operação M⊕P sobre um emparelhamento M e um
caminho aumentador P. O emparelhamento é representado pelo vetor
match[0..V-1] e o caminho é representado por um vetor pa[0..V-1]. O
término de P é t. A origem de P não é dada explicitamente mas
caracterizada pela propriedade pa[s] == s. */
static void newMatching( vertex *match, vertex t)
{
   vertex x;
   do {
      x = pa[t];
      match[t] = x;
      match[x] = t;
      t = pa[x];
   } while (t != x);
}
```

Em cada iteração, a aresta t-x é acrescentada ao emparelhamento e a aresta x-pa[x] é retirada do emparelhamento:

## Algoritmo húngaro


A seguinte função implementa o algoritmo do emparelhamento máximo esboçado acima em um grafo bipartido. O coração da implementação é a função auxiliar augmentMatch().

```c
#define UGraph Graph

/* Esta função calcula um emparelhamento máximo M no grafo não-dirigido
bipartido G. A bipartição de G é dada pelo vetor bip[0..V-1], que tem
valores 0 e 1. A função devolve o tamanho de M e armazena uma
representação de M no vetor match[0..V-1]: para cada vértice v, match[v]
 é o vértice que M casa com v (ou -1 se v é solteiro). */
int UGRAPHbipMatch( UGraph G, int *bip, vertex *match) {
   for (vertex v = 0; v < G->V; ++v) match[v] = -1;
   int size = 0;
   while (augmentMatch( G, bip, match))
      size++;
   return size;
}
```

A missão da função booleana auxiliar augmentMatch() é encontrar um caminho aumentador para o emparelhamento representado por match[].  Para procurar por um caminho aumentador, a função faz uma busca em largura modificada. A busca começa, simultaneamente, em todos os vértices solteiros de cor 0 e anda dois passos de cada vez:

1. primeiro, percorre uma aresta que não está no emparelhamento;
2. depois, percorre uma aresta do emparelhamento.

A busca constrói uma floresta BFS alternante cujas raízes são os vértices solteiros de cor 0. A floresta é alternante porque todos os caminhos na floresta são alternantes. Se essa floresta atingir um vértice solteiro de cor 1 teremos encontrado um caminho aumentador.

```c
static vertex pa[1000];
static bool visited[1000];

/* Esta função recebe um grafo não-dirigido bipartido G, com bipartição
bip[0..V-1], e um emparelhamento M representado por match[0..V-1]. A
função procura calcular um emparelhamento maior que M. Se tiver sucesso,
 devolve true e modifica match[] de acordo. Se fracassar, devolve false
 sem alterar match[]. */
static bool augmentMatch( UGraph G, int *bip, vertex *match)
{
   for (vertex v = 0; v < G->V; ++v) visited[v] = false;
   QUEUEinit( G->V);
   for (vertex s = 0; s < G->V; ++s) {
      if (bip[s] == 0 && match[s] == -1) {
         visited[s] = true;
         pa[s] = s;
         QUEUEput( s);
      }
   }
   // a fila contém todos os vértices solteiros de cor 0

   while (!QUEUEempty( )) {
      // bip[v] == 0 para todo v na fila
      vertex v = QUEUEget( );
      for (link a = G->adj[v]; a != NULL; a = a->next) {
         vertex w = a->w; // bip[w] == 1
         if (!visited[w]) {
            visited[w] = true;
            pa[w] = v;
            if (match[w] == -1) { // caminho aumentador!
               newMatching( match, w);
               QUEUEfree( );
               return true;
            }
            vertex x = match[w]; // visited[x] == false
            visited[x] = true;
            pa[x] = w; // caminho ganhou segmento v-w-x
            QUEUEput( x);
         }
      }
   }

   QUEUEfree( );
   return false;
}
```

O consumo de tempo de UGRAPHbipMatch() é proporcional a V (V + E) no pior caso.


# Custo nos arcos e arestas

Dado um grafo G, suponha que temos um função c que associa um número **ca** com cada arco **a** de G. Diremos que **ca** é o custo do arco a. Vamos supor que os custos são números do tipo int, podendo ser positivos, negativos, ou nulos.

## Listas de adjacência com custos

Na representação por listas de adjacência, basta acrescentar aos nós node um campo cst para acomodar os custos dos arcos.

```c
typedef struct node *link;
struct node {
   vertex w;
   int cst;
   link next;
};
```

É claro que um terceiro argumento, correspondente ao custo, deverá ser acrescentado às funções NEWnode() e GRAPHinsertArc().    
