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
   - [Heurística de Brélaz](#heurística-de-brélaz)   
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
- [Árvores geradoras de grafos não-dirigidos](#árvores-geradoras-de-grafos-não-dirigidos)   
   - [Cortes](#cortes)   
   - [Duas propriedades de substituição de arestas](#duas-propriedades-de-substituição-de-arestas)   
      - [Primeira propriedade](#primeira-propriedade)   
      - [Segunda propriedade](#segunda-propriedade)   
   - [Árvore geradora mínima (MST)](#árvore-geradora-mínima-mst)   
      - [Critério de minimalidade baseado em circuitos](#critério-de-minimalidade-baseado-em-circuitos)   
      - [Critério de minimalidade baseado em cortes](#critério-de-minimalidade-baseado-em-cortes)   
- [Algoritmo de Prim](#algoritmo-de-prim)   
   - [O algoritmo](#o-algoritmo)   
   - [Implementações eficientes](#implementações-eficientes)   
   - [Implementação](#implementação)   
- [Algoritmo de Kruskal](#algoritmo-de-kruskal)   
   - [Implementação](#implementação)   
- [Caminhos baratos](#caminhos-baratos)   
   - [Caminhos de custo mínimo](#caminhos-de-custo-mínimo)   
   - [Potencial viável e relaxação](#potencial-viável-e-relaxação)   
   - [Árvore de caminhos mínimos (SPT)](#árvore-de-caminhos-mínimos-spt)   
   - [Caminhos de custo mínimo em DAGs](#caminhos-de-custo-mínimo-em-dags)   
   - [Algoritmo de Dijkstra](#algoritmo-de-dijkstra)   
      - [O algoritmo](#o-algoritmo)   
      - [Prova de correção do algoritmo](#prova-de-correção-do-algoritmo)   
      - [Implementação](#implementação)   
- [Fluxo em redes](#fluxo-em-redes)   
   - [Fluxo](#fluxo)   
   - [Fluxo versus coleção de caminhos](#fluxo-versus-coleção-de-caminhos)   
   - [O problema do fluxo máximo](#o-problema-do-fluxo-máximo)   
   - [O algoritmo de ford-fulkerson](#o-algoritmo-de-ford-fulkerson)   
      - [Pseudocaminhos](#pseudocaminhos)   
      - [Pseudocaminhos aumentadores](#pseudocaminhos-aumentadores)   
      - [Capacidade residual de um pseudocaminho](#capacidade-residual-de-um-pseudocaminho)   
      - [O algoritmo de Ford-Fulkerson](#o-algoritmo-de-ford-fulkerson)   
      - [Desempenho do algoritmo](#desempenho-do-algoritmo)   
   - [Teorema: Fluxo maximo e corte minimo](#teorema-fluxo-maximo-e-corte-minimo)   
      - [Saldo de fluxo num conjunto de vértices](#saldo-de-fluxo-num-conjunto-de-vértices)   
      - [Cortes e sua capacidade](#cortes-e-sua-capacidade)   
      - [Cortes limitam fluxo](#cortes-limitam-fluxo)   
      - [Análise do algoritmo de Ford-Fulkerson](#análise-do-algoritmo-de-ford-fulkerson)   
   - [Implementação do algoritmo de Ford-Fulkerson](#implementação-do-algoritmo-de-ford-fulkerson)   
      - [Estrutura de dados: nós expandidos](#estrutura-de-dados-nós-expandidos)   
      - [Arcos artificiais](#arcos-artificiais)   
      - [Caminhos aumentadores e capacidades residuais](#caminhos-aumentadores-e-capacidades-residuais)   
      - [Camada externa da implementação do algoritmo](#camada-externa-da-implementação-do-algoritmo)   
   - [Cálculo de um caminho aumentador mínimo](#cálculo-de-um-caminho-aumentador-mínimo)   
      - [Número de iterações do algoritmo do fluxo máximo](#número-de-iterações-do-algoritmo-do-fluxo-máximo)   
   - [Caminho aumentador de máxima capacidade residua](#caminho-aumentador-de-máxima-capacidade-residua)   
      - [Número de iterações do algoritmo de fluxo máximo](#número-de-iterações-do-algoritmo-de-fluxo-máximo)   

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

## Heurística de Brélaz

Esta é uma variante do algoritmo de coloração sequencial. Suponha dada uma coloração válida incompleta de um grafo não-dirigido G e suponha que o conjunto de cores é 0 1 2 ... k-1.  Em cada iteração, escolha um vértice incolor v que seja adjacente ao maior número de cores. Em caso de empate, escolha o vértice que tiver maior grau.  Aplique a v a menor cor que seja diferente das cores usadas nos vizinhos de v.

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

# Árvores geradoras de grafos não-dirigidos

Uma subárvore de um grafo não-dirigido G é **geradora** (spanning) se contém todos os vértices de G. Como árvores são conexas, todo grafo não-dirigido dotado de árvore geradora é conexo. Reciprocamente, todo grafo não-dirigido conexo tem (pelo menos) uma árvore geradora. Toda árvore geradora de um grafo não-dirigido com V vértices tem exatamente V−1 arestas.

É fácil calcular uma árvore geradora de um grafo não-dirigido conexo: a busca em profundidade e a busca em largura fazem isso. Mais precisamente, qualquer das duas buscas calcula uma árvore radicada que contém um dos dois arcos de cada aresta de uma árvore geradora do grafo.

## Cortes

Um **corte** é o conjunto de arestas que liga alguma parte do grafo ao resto. Para uma definição mais formal, começamos com o conceito de **leque**. O leque de um conjunto X de vértices de um grafo não-dirigido é o conjunto de todas as arestas que têm uma ponta em X e outra no complemento de X. (O complemento de X é o conjunto X de todos os vértices do grafo que não estão em X.) Se X é trivial, o leque de X é vazio.

Um corte (cut) é o leque de algum conjunto não-trivial X de vértices.

## Duas propriedades de substituição de arestas

A manipulação de árvores geradoras de grafos não-dirigidos usa duas operações básicas: uma acrescenta uma aresta a uma árvore geradora e assim cria um circuito; outra remove uma aresta da árvore geradora e assim define um corte. Essas operações dão origem a duas propriedades de substituição de arestas (= exchange properties).

### Primeira propriedade

Seja T uma árvore geradora de um grafo não-dirigido G. Para qualquer aresta **e** de G, vamos denotar por  T+e  o grafo que se obtém quando e é inserida em T. Para qualquer aresta t de T, vamos denotar por  T-t  o grafo que se obtém quando t é removida de T.

**Propriedade dos circuitos** (ou Propriedade insere-remove): Seja T uma árvore geradora de um grafo não-dirigido G. Para qualquer aresta e de G que não esteja em T, o grafo  T + e tem um único circuito. Para qualquer aresta t desse circuito, o grafo  `T + e − t` é uma árvore geradora de G.

O único circuito em T+e é conhecido como **circuito fundamental de e relativo a T**.

### Segunda propriedade

Seja T uma árvore geradora de um grafo não-dirigido G. Para toda aresta t de T, as duas componentes conexas de T − t definem um corte em G: trata-se do conjunto de todas as arestas de G que têm uma ponta em uma das componentes conexas de T − t e outra ponta na outra componente conexa. Diremos que esse é o corte determinado por T − t ou corte fundamental de t relativo a T.

**Propriedade dos cortes** (ou Propriedade remove-insere):  Seja T uma árvore geradora de um grafo não-dirigido G. Para qualquer aresta t de T e qualquer aresta e do corte determinado por T − t, o grafo não-dirigido `T − t + e` é uma árvore geradora de G.

## Árvore geradora mínima (MST)

Seja G um grafo não-dirigido com custos nas arestas. Os custos podem ser positivos ou negativos. O custo de um subgrafo não-dirigido T de G é a soma dos custos das arestas de T.

### Critério de minimalidade baseado em circuitos

A propriedade dos circuitos (propriedade insere-remove) leva ao seguinte critério de minimalidade:

**Critério de minimalidade baseado em circuitos**: Uma árvore geradora T de um grafo não-dirigido com custos nas arestas é uma MST se e somente se toda aresta e fora de T tem custo máximo no circuito fundamental de e relativo a T.

Se denotarmos por ca o custo de uma aresta a, podemos formular o critério assim: T é uma MST se e somente se ce ≥ ct para cada aresta e fora de T e qualquer aresta t do circuito fundamental de e.

### Critério de minimalidade baseado em cortes

A propriedade dos cortes (propriedade remove-insere) leva ao seguinte critério de minimalidade:

**Critério de minimalidade baseado em cortes:** Uma árvore geradora T de um grafo não-dirigido com custos nas arestas é uma MST se e somente se cada aresta t de T tem custo mínimo no corte fundamental de t relativo a T.

Se denotarmos por ca o custo de uma aresta a, podemos formular o critério assim: T é uma MST se e somente se ct ≤ ce para cada aresta t de T e qualquer aresta e do corte fundamental de t.

# Algoritmo de Prim

Acha MST

## O algoritmo

Dado um grafo não-dirigido conexo G com custos nas arestas, o algoritmo de Prim cultiva uma subárvore de G até que ela se torne geradora. No fim do processo, a árvore é uma MST.

Suponha que T é uma subárvore de G. A **franja** (fringe) de T é o conjunto de todas as arestas de G que têm uma ponta em T e outra fora de T.

Podemos agora descrever o algoritmo de maneira precisa. Cada iteração começa com uma subárvore T. No início da primeira iteração, T consiste em um único vértice.  O processo iterativo consiste no seguinte: enquanto a franja de T não estiver vazia,

1. escolha uma aresta da franja que tenha custo mínimo,
2. seja x-y a aresta escolhida, com x em T,
3. acrescente a aresta x-y e o vértice y a T.

Como se vê, o algoritmo tem caráter guloso: em cada iteração, abocanha a aresta mais barata da franja sem se preocupar com o efeito global, a longo prazo, dessa escolha.

## Implementações eficientes

A implementação ingênua do algoritmo de Prim é lenta e ineficiente porque cada iteração recalcula toda a franja da árvore, mesmo sabendo que a franja mudou pouco desde a iteração anterior. Para obter uma implementação mais eficiente, é preciso começar cada iteração com a franja pronta e atualizá-la no fim da iteração. Mas é difícil fazer isso se a franja for tratada como uma simples lista de arestas; é preciso inventar uma representação mais eficiente e tomar algumas decisões de projeto adicionais.

A **fronteira** de uma árvore T é o conjunto de todos os vértices do grafo que não pertencem a T mas são vizinhos de vértices de T. O preço de um vértice w da fronteira de T é o custo de uma aresta de custo mínimo dentre as que estão na franja de T e incidem em w. Se a aresta da franja que determina o preço de w é v-w, diremos que v é o **gancho** de w.

Podemos agora reescrever o algoritmo de Prim em termos de preços e ganchos. Cada iteração começa com uma árvore T e com os preços e ganchos dos vértices que estão na fronteira de T. O processo iterativo consiste no seguinte:  enquanto a franja de T não estiver vazia,

1. escolha um vértice y de preço mínimo na fronteira de T,
2. seja x um gancho de y,
3. acrescente o arco x-y e o vértice y a T ,
4. atualize os preços e ganchos fora de T.

Para armazenar os preços dos vértices usaremos um vetor `preco[]` indexado pelos vértices. Os ganchos poderiam ser armazenados num vetor alocado especialmente para esse fim, mas é melhor armazená-los na parte ociosa do vetor de pais de T, ou seja, nas posições do vetor pa[] indexadas pelos vértices da fronteira de T. Com isso, os elementos de pa[] terão a seguinte interpretação: se v está em T então pa[v] é o pai de v, se v está na fronteira de T então pa[v] é o gancho de v, e nos demais casos pa[v] está indefinido. Poderíamos dizer que os elementos de pa[] indexados pelos vértices da fronteira **são pais provisórios**, estando sujeitos a alterações nas próximas iterações.

## Implementação

A implementação mantém os vértices da fronteira em ordem crescente de preços (ou quase isso), para que não seja preciso procurar o vértice mais barato. Cada iteração começa assim:

- o vetor característico tree[] do conjunto de vértices da árvore T (tree[i] == 1 se i esta em T),
- um vetor preco[] que contém o preço de cada vértice da fronteira de T,
- um vetor pa[] que contém os pais dos vértices de T e os ganchos dos vértices da fronteira de T.

Mas os vértices que não pertencem a T são mantidos numa fila priorizada (priority queue).

```c
#define UGraph Graph

/* Recebe um grafo não-dirigido conexo G com custos arbitrários nas
arestas e calcula uma MST de G. A função trata a MST como uma árvore
radicada com raiz 0 e armazena a árvore no vetor de pais pa[]. */
void UGRAPHmstP2( UGraph G, vertex *pa)
{
   bool tree[1000];
   int preco[1000];

   // inicialização:
   for (vertex v = 1; v < G->V; ++v)
      pa[v] = -1, tree[v] = false, preco[v] = INFINITY;
   pa[0] = 0, tree[0] = true;
   for (link a = G->adj[0]; a != NULL; a = a->next)
      pa[a->w] = 0, preco[a->w] = a->cst;

   PQinit( G->V);
   for (vertex v = 1; v < G->V; ++v)
      PQinsert( v, preco);

   while (!PQempty( )) {
      vertex y = PQdelmin( preco);
      if (preco[y] == INFINITY) break;
      // atualização dos preços:
      tree[y] = true;
      for (link a = G->adj[y]; a != NULL; a = a->next)
         if (!tree[a->w] && a->cst < preco[a->w]) {
            preco[a->w] = a->cst;
            PQdec( a->w, preco);
            pa[a->w] = y;
         }
   }
   PQfree( );
}
```

**Desempenho**: A implementação clássica da fila priorizada usa uma estrutura de heap.  Com essa implementação da fila, a função UGRAPHmstP2() consome tempo proporcional a `(V+E) log V` no pior caso, sendo V o número de vértices e E o número de arestas de G. Como G é conexo, temos E ≥ V−1 e portanto o consumo de tempo é proporcional a

**E log V**

no pior caso.  Portanto, UGRAPHmstP2() é apenas um pouco pior que linear.  Podemos dizer que UGRAPHmstP2() é linearítmica.

# Algoritmo de Kruskal

O algoritmo de Kruskal faz crescer uma floresta geradora até que ela se torne conexa. A floresta cresce de modo a satisfazer o critério de minimalidade de MSTs baseado em circuitos.

Uma **subfloresta** de um grafo não-dirigido G é qualquer floresta que seja subgrafo não-dirigido de G. Uma **floresta geradora** de G é qualquer subfloresta que tenha o mesmo conjunto de vértices que G.

Uma aresta a de G é **externa** a uma floresta geradora F se a não pertence a F e o grafo  F + a  é uma floresta, ou seja, um grafo sem circuitos. Portanto, uma aresta é externa a F se tem uma ponta em uma componente conexa de F e outra ponta em outra componente.

Cada iteração do algoritmo de Kruskal começa com uma floresta geradora  F  de G. O processo iterativo é muito simples: enquanto existe alguma aresta externa,

1. escolha uma aresta externa que tenha custo mínimo;
2. seja a a aresta escolhida, acrescente a a F.

No início da primeira iteração, cada componente conexa da floresta F tem apenas um vértice.  No fim do processo iterativo, F é conexa, uma vez que G é conexo e não há arestas externas a F.

## Implementação

Basta colocar as arestas em ordem crescente de custo e então examinar cada aresta uma só vez. Será preciso recorrer à estrutura union-find, que usa um vetor de chefes mais flexível.

```c
/* A função UGRAPHmstK1() recebe um grafo não-dirigido conexo G com custos
arbitrários nas arestas e calcula uma MST de G.  O grafo é dado por suas
listas de adjacência. A função armazena as arestas da MST no vetor mst[0..V-1],
 alocado pelo usuário. */
void UGRAPHmstK1( UGraph G, edge mst[]) {
   edge e[500000];

   UGRAPHedges( G, e);
   int E = G->A/2;
   sort( e, 0, E-1);

   UFinit( G->V);
   int k = 0;
   for (int i = 0; k < G->V-1; ++i) {
      vertex v0 = UFfind( e[i].v);
      vertex w0 = UFfind( e[i].w);
      if (v0 != w0) {
         UFunion( v0, w0);
         mst[k++] = e[i];
      }
   }
}

/* Esta função armazena as arestas do grafo não-dirigido G no vetor e[0..E-1]. */
static void UGRAPHedges( UGraph G, edge e[])
{
   int i = 0;
   for (vertex v = 0; v < G->V; ++v)
      for (link a = G->adj[v]; a != NULL; a = a->next)
         if (v < a->w)
            e[i++] = EDGE( v, a->w, a->cst);
}
```

**Consumo de tempo**: `(E + V) log V`.

# Caminhos baratos

## Caminhos de custo mínimo

Num grafo com custos nos arcos, o custo de um caminho é a soma dos custos dos arcos do caminho.

Um caminho B é mais barato que um caminho C se tiver custo menor que o de C. Dizemos que um caminho C  tem **custo mínimo** se nenhum caminho com a mesma origem e o mesmo término de C é mais barato que C.

Problema do caminho mínimo sob custos positivos:  Dados vértices s e t de um grafo com custos positivos nos arcos, encontrar um caminho mínimo de s a t. Se não houver caminho algum de s a t, o problema não tem solução.

A **distância** de um vértice s a um vértice t é o custo de um caminho mínimo com origem s e término t.

## Potencial viável e relaxação

Para provar que um dado caminho C tem custo mínimo, precisamos mostrar que qualquer caminho com a mesma origem e o mesmo término que os de C é pelo menos tão caro quanto C.

Dado um grafo G com custos positivos nos arcos, um **potencial** é um vetor de números indexado pelos vértices de G. Dizemos que um arco v-w está relaxado em relação a um potencial h[] se

`h[v] + cvw  ≥  h[w]`,

sendo cvw o custo do arco.  Dizemos que um potencial h[ ] é viável se todos os arcos do grafo estão relaxados em relação a h[ ].

Se s é um vértice de G e d[v] é a distância de s a v para cada v então d[ ] é um potencial viável.

**Certificado de minimalidade**: Num grafo G com custos positivos nos arcos, como é possível provar que o custo de um dado caminho é mínimo? Suponha que h[ ] é um potencial viável em G. É fácil verificar que para qualquer caminho C de um vértice s a um vértice t tem-se

`h[s] + cC  ≥  h[t]`,

sendo cC o custo do caminho C. Portanto, cC ≥ h[t] − h[s]. Conclusão: qualquer caminho de s a t é pelo menos tão caro quanto a diferença de potencial entre t e s.

Segue daí que qualquer caminho C de s a t que satisfaz a igualdade `cC = h[t] − h[s]` tem custo mínimo! Nessas condições, h[ ] torna-se um certificado de minimalidade do caminho C.

## Árvore de caminhos mínimos (SPT)

Uma árvore de caminhos mínimos, ou SPT (shortest paths tree), é uma árvore radicada T em G, com raiz s, tal que

- todo vértice ao alcance de s em G pertence a T e
- todo caminho em T que começa em s é um caminho mínimo em G.

**Problema da SPT sob custos positivos**: Dado um vértice s de um grafo com custos positivos nos arcos, encontrar uma SPT com raiz s no grafo.

Dada uma árvore radicada T, como podemos verificar se T é uma SPT? Digamos que s é a raiz de T. Para cada vértice v de T, seja x[v] o custo do único caminho de s a v em T. Para cada vértice z fora de T, faça x[z] = M, onde M é um número maior que o custo de qualquer caminho simples em G.  Se o vetor x[ ] deixa todos os arcos de G relaxados, então T é uma SPT (veja o certificado de minimalidade acima).  Senão, a desigualdade triangular é violada e portanto T não é uma SPT.

## Caminhos de custo mínimo em DAGs

**Problema do caminho mínimo em dags**: Dada uma permutação topológica e um vértice s de um grafo G com custos positivos nos arcos, encontrar uma SPT com raiz s em G.

A função DAGspt() abaixo produz uma SPT com raiz s e um vetor das distâncias em G a partir de s.  A distância de s a vértices fora do alcance de s será representada pela constante INFINITY, de valor maior que o custo de qualquer caminho. A função recebe uma permutação topológica vv[0..V-1] de G e processa os vértices em ordem: primeiro vv[0], depois vv[1], depois vv[2], etc.

```c
#define Dag Graph

/* A função recebe um dag G com custos nos arcos, uma permutação
topológica vv[] de G, e um vértice s de G. A função armazena no vetor de
pais pa[] uma SPT com raiz s. Se um vértice v não está ao alcance de s,
teremos pa[v] == -1. As distâncias a partir de s são armazenadas no vetor
dist[]. Os vetores pa[0..V-1] e dist[0..V-1] devem ser alocados pelo usuário */
void DAGspt( Dag G, vertex *vv, vertex s, vertex *pa, int *dist) {
   for (vertex v = 0; v < G->V; ++v)
      pa[v] = -1, dist[v] = INFINITY;

   pa[s] = s, dist[s] = 0;
   for (int j = 0; j < G->V; ++j) {
      vertex v = vv[j];
      if (dist[v] == INFINITY) continue;
      for (link a = G->adj[v]; a != NULL; a = a->next) {
         if (dist[v] + a->cst < dist[a->w]) {
            dist[a->w] = dist[v] + a->cst;
            pa[a->w] = v;
         }
      }
   }
}
```

Para verificar que a função DAGspt() está correta, basta observar os seguintes invariantes: no início de cada iteração,

1. para cada vértice v, se pa[v] ≠ -1 então dist[v] é o custo de algum caminho de s a v;
2. todo arco x-y tal que x = vv[i] para algum i < j está relaxado em relação a dist[].


O consumo de tempo de DAGspt() é proporcional a  V + A  no pior caso.

## Algoritmo de Dijkstra

### O algoritmo

Dado um grafo G com custos positivos nos arcos, o algoritmo de Dijkstra faz crescer em G uma subárvore radicada a partir do vértice s até que ela englobe todos os vértices que estão ao alcance de s. Ao final, a subárvore torna-se uma SPT.

Podemos agora descrever o algoritmo de Dijkstra.  O algoritmo é iterativo. Cada iteração começa com uma árvore radicada T com raiz s.  No começo da primeira iteração, s é o único vértice de T e dist[s] vale 0. O processo iterativo consiste no seguinte: enquanto a franja de T não estiver vazia,

- escolha, na franja de T, um arco x-y que minimize dist[x] + cxy ,
- acrescente o arco x-y e o vértice y a T ,
- faça dist[y] = dist[x] + cxy .

### Prova de correção do algoritmo

Para provar que o algoritmo de Dijkstra resolve o problema da SPT em qualquer grafo G com custos positivos nos arcos é preciso observar que as seguintes propriedades são invariantes: no início de cada iteração,

- para cada vértice v de T, dist[v] é o custo do único caminho de s a v em T,
- todos os arcos de G que têm ambas as pontas em T estão relaxados em relação a dist[ ].

### Implementação

```c
/* Recebe um grafo G com custos positivos nos arcos e um vértice s.
Armazena no vetor de pais pa[] uma SPT de G com raiz s. Se um vértice v
não está ao alcance de s, pa[v] valerá -1. As distâncias a partir de s
são armazenadas no vetor dist[]. */

void GRAPHsptD2( Graph G, vertex s, vertex *pa, int *dist)
{
   bool tree[1000];
   // inicialização:
   for (vertex v = 0; v < G->V; ++v)
      pa[v] = -1, tree[v] = false, dist[v] = INFINITY;
   pa[s] = s, tree[s] = true, dist[s] = 0;
   for (link a = G->adj[s]; a != NULL; a = a->next)
      pa[a->w] = s, dist[a->w] = a->cst;
   PQinit( G->V);
   for (vertex v = 0; v < G->V; ++v)
      if (v != s) PQinsert( v, dist);

   while (!PQempty( )) {
      vertex y = PQdelmin( dist);
      if (dist[y] == INFINITY) break;
      tree[y] = true;
      // atualização de dist[] e pa[]:
      for (link a = G->adj[y]; a != NULL; a = a->next) {
         if (true[a->w]) continue;
         if (dist[y] + a->cst < dist[a->w]) {
            dist[a->w] = dist[y] + a->cst;
            PQdec( a->w, dist);
            pa[a->w] = y;
         }
      }
   }
   PQfree( );
}
```

# Fluxo em redes

## Fluxo

Suponha dada uma tabela `f` que associa um número positivo para cada arco de um grafo. O número que f associa com um arco v-w será denotado por **fvw** e chamado fluxo no arco v-w.

Para qualquer vértice v do grafo, o **fluxo que entra** em v (inflow) é a soma dos fluxos nos arcos que entram em v. O **fluxo que sai** de v (outflow) é a soma dos fluxos nos arcos que saem de v. O **saldo de fluxo** (net flow) em v é a diferença `out(v) − in(v)` entre o fluxo que sai de v e o fluxo que entra em v.

Um **fluxo** (flow) num grafo com vértice inicial s e vértice final t é uma tabela f que atribui números positivos aos arcos do grafo de tal modo que

- o saldo de f é nulo em todo vértice diferente de s e de t (tudo que entra sai)
- o saldo de f em s é positivo.

A **intensidade** (flow value) de um fluxo f é o saldo de f no vértice inicial s.

Como representar um fluxo?  Se o grafo é representado por listas de adjacência, poderíamos acrescentar um campo aos nós que representam os arcos. Se o grafo é representado por uma matriz de adjacências, podemos usar uma matriz f[][] definida da maneira óbvia: se v-w é um arco, então f[v][w] é o fluxo no arco; senão, f[v][w] é 0

## Fluxo versus coleção de caminhos

As seguintes ideias produzem uma imagem mental intuitiva de um fluxo e assim ajudam a compreender melhor o conceito.

Todo fluxo por ser representado por uma coleção de caminhos. Cada caminho da coleção é simples, começa em s, termina em t, e conduz uma certa quantidade de fluxo. A soma das quantidades de fluxo conduzidas pelos vários caminhos é igual à intensidade do fluxo original.

## O problema do fluxo máximo

Uma **grafo capacitado** é um grafo com números positivos associados aos arcos. O número associado a um arco é conhecido como capacidade do arco. A capacidade de um arco v-w será denotada por cvw. Um grafo capacitado com vértice inicial e vértice final é às vezes chamado **rede** (network).

Suponha que f é um fluxo num grafo capacitado com vértice inicial e vértice final. Dizemos que f respeita as capacidades se `fvw  ≤  cvw` para cada arco v-w.

**Problema do fluxo máximo (maximum flow problem)**: Dado um grafo capacitado com vértice inicial e vértice final, encontrar um fluxo de intensidade máxima dentre os que respeitam as capacidades dos arcos.

## O algoritmo de ford-fulkerson

### Pseudocaminhos

Um **pseudocaminho** num grafo é uma sequência de vértices dotada da seguinte propriedade: para cada par vw de vértices consecutivos, v–w é um arco do grafo ou w–v é um arco do grafo.

No primeiro caso, dizemos que v–w é um **arco direto** (forward arc) do pseudocaminho. No segundo caso, dizemos que w–v é um **arco reverso** (backward arc) do pseudocaminho. Um pseudocaminho sem arcos reversos é um caminho.

### Pseudocaminhos aumentadores

Suponha dado um grafo capacitado e um fluxo que respeita as capacidades dos arcos. Dizemos que um arco v–w está cheio se o fluxo no arco é igual à capacidade do arco. Dizemos que um arco v–w está vazio se o fluxo no arco é nulo.

Em relação a esse fluxo, um pseudocaminho é **aumentador** (augmenting) se vai do vértice inicial ao final do grafo e tem as seguintes propriedades:

- nenhum arco direto do pseudocaminho está cheio e
- nenhum arco reverso do pseudocaminho está vazio.

Dado um pseudocaminho aumentador relativo a um fluxo f, a operação de enviar ε unidades de fluxo ao longo do pseudocaminho consiste em calcular um novo fluxo da seguinte maneira:

para cada arco direto v–w do pseudocaminho, faça  fvw = fvw + ε, ou seja, some ε ao fluxo no arco, e
para cada arco reverso w–v do pseudocaminho, faça  fwv = fwv − ε,  ou seja, subtraia ε do fluxo no arco.
É fácil verificar que o resultado dessa operação é um fluxo.  Ademais, a operação acrescenta ε à intensidade do fluxo.

### Capacidade residual de um pseudocaminho

Suponha dado um grafo capacitado com vértice inicial e vértice final. Seja f um fluxo no grafo que respeita a capacidade c. Suponha dado um pseudocaminho aumentador relativo a f. A capacidade residual de um arco direto v–w do pseudocaminho é a diferença `cvw − fvw`, sendo cvw a capacidade e fvw o fluxo no arco v–w. A capacidade residual de um arco reverso w–v do pseudocaminho é fwv .

A capacidade residual do pseudocaminho todo é a menor das capacidades residuais dos seus arcos.

Digamos que δ é a capacidade residual de um pseudocaminho aumentador P e  ε  é um número entre 0 e δ.  Se enviarmos ε unidades de fluxo ao longo de P, produziremos um novo fluxo que respeita as capacidades dos arcos. A intensidade do novo fluxo será ε unidades maior que a intensidade do fluxo original.

### O algoritmo de Ford-Fulkerson

O algoritmo de Ford-Fulkerson, também conhecido como algoritmo dos pseudocaminhos aumentadores, resolve o problema do fluxo máximo. Cada iteração começa com um fluxo f que respeita as capacidades dos arcos. A primeira iteração começa com o fluxo nulo. O processo iterativo consiste no seguinte: enquanto existe pseudocaminho aumentador,

- encontre um pseudocaminho aumentador P,
- calcule a capacidade residual δ de P,
- envie δ unidades de fluxo ao longo de P (e atualize f).

### Desempenho do algoritmo

O consumo de tempo do algoritmo de Ford-Fulkersonal é proporcional ao número de iterações. Tudo se reduz, portanto, à seguinte questão: Quantos pseudocaminhos aumentadores são necessários para produzir um fluxo máximo a partir do fluxo nulo?

Número de pseudocaminhos aumentadores: Se todos os arcos do grafo têm capacidade inteira e menor que uma constante M então o número de pseudocaminhos aumentadores necessário para atingir um fluxo máximo é menor que VM, sendo V o número de vértices do grafo.

Essa estimativa do número de pseudocaminhos aumentadores não é excessivamente pessimista: existem grafos capacitados em que o número de pseudocaminhos aumentadores chega muito perto de VM.


Como o número de pseudocaminhos aumentadores depende das capacidades dos arcos, o algoritmo não pode ser considerado satisfatório: a mera multiplicação de todas as capacidades por 100, por exemplo, pode multiplicar o consumo de tempo por 100. Para obter um desempenho que não dependa das capacidades será preciso escolher os pseudocaminhos aumentadores de maneira mais cuidadosa.

## Teorema: Fluxo maximo e corte minimo

### Saldo de fluxo num conjunto de vértices

O fluxo que sai de um conjunto S é a soma dos fluxos nos arcos que saem de S e o fluxo que entra em S é a soma dos fluxos nos arcos que entram em S. O saldo de fluxo em S é a diferença `outflow(S) − inflow(S)` entre o fluxo que sai de S e o fluxo que entra em S.

### Cortes e sua capacidade

Dado um grafo com vértice inicial s e vértice final t, dizemos que um conjunto de vértices separa s de t se contém s mas não contém t. Um **corte** (cut) no grafo é qualquer conjunto de arcos que tenha a forma `C' ∪ C''`, sendo C' o leque de saída e C'' o leque de entrada de algum conjunto S de vértices que separa s de t. Diremos que o conjunto S é a margem superior do corte e o complemento Ŝ de S é a margem inferior do corte.

Um arco de um corte é **direto** se pertence ao leque de saída da margem superior do corte (ou seja, se vai da margem superior para a margem inferior) e é **reverso** se pertence ao leque de entrada da margem superior do corte (ou seja, se vai da margem inferior para a margem superior).

Num grafo capacitado, a **capacidade de um corte** é a soma das capacidades dos arcos diretos do corte. A expressão **corte mínimo** é uma abreviatura de corte de capacidade mínima.

### Cortes limitam fluxo

A capacidade de qualquer corte limita a intensidade de qualquer fluxo que respeita as capacidades.

**Delimitação superior dos fluxos**: Em qualquer grafo capacitado com vértice inicial e vértice final, a intensidade de qualquer fluxo que respeita as capacidades é menor que ou igual à capacidade de qualquer corte.

Essa propriedade pode ser resumida pela expressão intensidade `(fluxo) ≤ capacidade (corte)`. Ela decorre da propriedade dos saldos, como mostramos a seguir.

**Prova**: Seja f um fluxo que respeita as capacidades, C um corte, e S a margem superior do corte. Pela propriedade dos saldos, a intensidade de f é igual ao saldo de f em S. Esse saldo é outflow(S) − inflow(S) e portanto não passa de outflow(S). Como f respeita as capacidades, outflow(S) não passa da capacidade do corte C.

**Uma consequência importante da delimitação**: Se um fluxo f respeita as capacidades e tem intensidade igual à capacidade de algum corte então f é um fluxo de intensidade máxima. Um tal corte é, portanto, um certificado da maximalidade do fluxo.

### Análise do algoritmo de Ford-Fulkerson

Quando discutimos o algoritmo de Ford-Fulkerson no capítulo de mesmo nome, omitimos a prova de sua correção. Para mostrar que o algoritmo de fato resolve o problema do fluxo máximo, basta provar o seguinte fato:  se um dado fluxo f não tem pseudocaminho aumentador então f é um fluxo de intensidade máxima. Segue a prova desse fato:

**Prova**:  Suspenda, temporariamente, a cláusula da definição de pseudocaminho aumentador que exige que o pseudocaminho termine em t. Seja S o conjunto de todos os vértices que são término de um pseudocaminho aumentador. É claro que s está em S e o vértice t está fora de S pois, por hipótese, não existe pseudocaminho aumentador que termine em t. Considere o corte C cuja margem superior é S. Todos os arcos diretos de C estão cheios e todos os arcos reversos estão vazios. Portanto, o saldo de f em S é igual à capacidade de C.

De acordo com a propriedade dos saldos, o saldo de f em S é igual à intensidade de f. Por outro lado, conforme a delimitação superior dos fluxos, não existe fluxo de intensidade maior que a capacidade de C. Logo, nosso fluxo f tem intensidade máxima!

Essa análise do algoritmo de Ford-Fulkerson prova o seguinte teorema:

**Teorema do fluxo máximo e corte mínimo (Maxflow-mincut theorem)**:  Em qualquer grafo capacitado com vértice inicial e vértice final, a intensidade de um fluxo máximo é igual à capacidade de um corte mínimo.

O algoritmo de Ford-Fulkerson mostra também um importante adendo ao teorema: se as capacidades dos arcos forem números inteiros então existe um fluxo máximo tal que o fluxo em cada arco é um número inteiro.

## Implementação do algoritmo de Ford-Fulkerson

### Estrutura de dados: nós expandidos

Adicionando a eles alguns campos auxiliares. Assim, além dos campos usuais w e next, teremos

- um campo `cap` para a capacidade do arco que o nó representa,
- um campo `flow` para o fluxo no arco,
- campos `type` e `twin`, cujo papel será discutido adiante.

Um nó com esses campos será chamado nó expandido.

```c
typedef struct node *link;
struct node { // nó expandido
   vertex w;
   int cap;
   int flow;
   int type;
   link twin;
   link next;
};
```

### Arcos artificiais

É difícil procurar pseudocaminhos num grafo por causa dos arcos reversos dos pseudocaminhos. Para enfrentar essa dificuldade, vamos acrescentar alguns arcos auxiliares ao nosso grafo.

Para cada arco v-w, acrescentaremos ao grafo um novo arco w-v, antiparalelo a v-w.

```c
b = malloc( sizeof (struct node));
b->w = v;
b->twin = a;
a->twin = b;
b->next = G->adj[w];
G->adj[w] = b;
```

Usaremos o campo twin para que a possa ser facilmente localizado a partir de b e vice-versa.  O campo  type indicará o tipo do arco: +1 se o arco for original e -1 se for artificial:

```c
a->type = +1;
b->type = -1;
```

O novo grafo, depois da adição dos arcos artificiais, será chamado grafo expandido.

No grafo expandido, cada arco artificial terá capacidade zero e o fluxo em cada arco artificial será o negativo do fluxo no correspondente arco original.

```c
b->cap = 0;
b->flow = -a->flow;
```

Como flow é positivo nos arcos originais, a desigualdade flow ≤ cap valerá em todo arco artificial.

### Caminhos aumentadores e capacidades residuais

Com a introdução dos arcos artificiais, os pseudocaminhos do grafo original podem ser representados por caminhos tradicionais no grafo expandido: os arcos artificiais de um caminho no grafo expandido representam os arcos reversos do correspondente pseudocaminho no grafo original.

Suponha que o grafo original tem um certo fluxo. Então a capacidade residual de um arco, seja ele original ou artificial, é a diferença `cap − flow`.

Como o fluxo é positivo nos arcos originais, a capacidade residual dos arcos artificiais é positiva. Se o fluxo respeita as capacidades dos arcos originais, a capacidade residual dos arcos originais também é positiva.

A capacidade residual de um caminho é a menor das capacidades residuais de seus arcos. Um caminho de s a t no grafo expandido é aumentador se tiver capacidade residual estritamente positiva.

### Camada externa da implementação do algoritmo

A função GRAPHmaxflow() abaixo implementa o algoritmo de Ford-Fulkerson.  Ela calcula um fluxo máximo num grafo capacitado G com vértice inicial s e vértice final t. A função GRAPHmaxflow() tem três fases:

- a primeira fase expande o grafo acrescentando arcos artificias e preenchendo os campos type e twin dos nós;
- a segunda fase calcula um fluxo máximo e registra o fluxo nos campos flow dos nós;
-  terceira fase elimina os arcos artificias criados durante a primeira fase e assim restaura a estrutura original de G.

```c
/* Esta função devolve a intensidade de um fluxo máximo no grafo capacitado
 G com vértice inicial s e vértice final t. O fluxo é armazenado nos campos
 flow dos nós das listas de adjacência. O código supõe que G tem no máximo
 1000 vértices. */
int GRAPHmaxflow( Graph G, vertex s, vertex t) {
   vertex pa[1000];
   link parc[1000]; // parent-arc
   Expand( G);
   for (vertex v = 0; v < G->V; ++v)
      for (link a = G->adj[v]; a != NULL; a = a->next)
         a->flow = 0;

   int intensity = 0;
   while (true) {
      int delta = AugmPath( G, s, t, pa, parc);
      if (delta == 0) break;
      for (vertex w = t; w != s; w = pa[w]) {
         link a = parc[w];
         a->flow += delta;
         a->twin->flow -= delta;
      }
      intensity += delta;
   }

   Contract( G);
   return intensity;
}

/* Acrescenta os arcos artificiais ao grafo G e preenche os campos type e
twin de todos os nós das listas adj[]. */
void Expand( Graph G) {
   for (vertex v = 0; v < G->V; ++v)
      for (link a = G->adj[v]; a != NULL; a = a->next)
         a->type = +1;
   for (vertex v = 0; v < G->V; ++v)
      for (link a = G->adj[v]; a != NULL; a = a->next)
         if (a->type == +1) {
            link b = malloc( sizeof (struct node));
            b->w = v;
            b->cap = 0;
            b->type = -1;
            b->twin = a;
            a->twin = b;
            b->next = G->adj[a->w];
            G->adj[a->w] = b;
         }
}

/* A função Contract() desfaz o efeito de Expand(), removendo todos os
 arcos artificiais do grafo. */
void Contract( Graph G) {
   for (vertex v = 0; v < G->V; ++v) {
      link a = G->adj[v];
      while (a != NULL && a->type == -1) {
         link b = a;
         G->adj[v] = a = a->next;
         free( b);
      }
   }
}
```

Parte substancial do código de GRAPHmaxflow() está escondida na função AugmPath(), que tem a missão de encontrar um caminho aumentador (= augmenting path). Duas diferentes implementações da função AugmPath() são discutidas nos proximos topicos.

## Cálculo de um caminho aumentador mínimo

Alguns arcos de G são originais enquanto outros são artificiais, mas a função ignora essa distinção e toma conhecimento apenas da capacidade residual cap - flow de cada arco. A função limita-se a calcular um caminho de s a t que tenha capacidade residual estritamente positiva e seja o mais curto possível sob essa restrição. O caminho é armazenado no par de vetores pa[] e parc[].

A função ShrtAugmPath() é uma mera adaptação da função GRAPHbfs() de busca em largura.

```c
/* Esta é a versão ShrtAugmPath() da função AugmPath(). */
#define ShrtAugmPath AugmPath

/* Esta função recebe um grafo capacitado G e um fluxo registrado nos campos
 flow dos nós das listas de adjacência. Calcula um caminho de comprimento mínimo
 de s a t dentre os que têm capacidade residual estritamente positiva e armazena
 o caminho nos vetores pa[] e parc[]. Devolve a capacidade residual do caminho;
 se nenhum caminho de s a t tem capacidade residual estritamente positiva,
  devolve 0. A função supõe que todos os arcos têm capacidade menor que uma constante M. */
int ShrtAugmPath( Graph G, vertex s, vertex t, vertex pa[], link parc[])
{
   vertex v;
   int visited[1000];
   for (v = 0; v < G->V; ++v) visited[v] = -1;
   QUEUEinit( G->V);
   visited[s] = 0;
   QUEUEput( s);

   while (!QUEUEempty( )) {
      v = QUEUEget( );
      if (v == t) break;
      for (link a = G->adj[v]; a != NULL; a = a->next) {
         vertex w = a->w;
         int residual = a->cap - a->flow;
         if (visited[w] == -1 && residual > 0) {
            visited[w] = 0;
            pa[w] = v; parc[w] = a;
            QUEUEput( w);
         }
      }
   }

   if (visited[t] == -1) return 0;
   int delta = M;
   for (vertex w = t; w != s; w = pa[w]) {
      link a = parc[w];
      int residual = a->cap - a->flow;
      if (residual < delta) delta = residual;
   }
   QUEUEfree( );
   return delta;
}
```

### Número de iterações do algoritmo do fluxo máximo

Edmonds e Karp mostraram, em 1972, que o consumo de tempo dessa implementação não depende das capacidades dos arcos.

Número de caminhos aumentadores: O número de caminhos aumentadores usados pela combinação de GRAPHmaxflow() com ShrtAugmPath() nunca é maior que `VA/2`, sendo V o número de vértices e A o número de arcos originais.

## Caminho aumentador de máxima capacidade residua

Alguns arcos de G são originais enquanto outros são artificiais, mas a função ignora essa distinção e toma conhecimento apenas da capacidade residual cap - flow de cada arco. A função limita-se a calcular um caminho de s a t que tenha a maior capacidade residual possível.

A função MaxCapAugmPath() é uma variante do algoritmo de Dijkstra. No início de cada iteração, para cada vértice x fora da árvore radicada definida por pa[] e parc[], o número cprd[x] é a capacidade residual de um caminho de capacidade residual máxima dentre os que começam em s, terminam em x, e têm um só vértice (o último) fora da árvore radicada.

```c
/* Esta é a versão MaxCapAugmPath() da função AugmPath(). */
#define MaxCapAugmPath AugmPath

/* Recebe um grafo capacitado G e um fluxo registrado nos campos flow dos nós
das listas de adjacência. Calcula um caminho de capacidade residual máxima de
s a t e armazena o caminho nos vetores pa[] e parc[]. Devolve a capacidade
residual do caminho ou devolve 0 se tal caminho não existe. A função supõe
que todas as capacidades são menores que uma constante M. */
int MaxCapAugmPath( Graph G, vertex s, vertex t, vertex pa[], link parc[])
{
   int cprd[1000];
   PQinit( G->V);
   for (vertex x = 0; x < G->V; ++x)
      cprd[x] = 0, PQinsert( x, cprd);
   cprd[s] = M;
   PQinc( s, cprd);

   while (!PQempty( )) {
      vertex x = PQdelmax( cprd);
      if (cprd[x] == 0 || x == t) break;
      for (link a = G->adj[x]; a != NULL; a = a->next) {
         int residual = a->cap - a->flow;
         if (residual > cprd[x]) residual = cprd[x];
         vertex y = a->w;
         if (residual > cprd[y]) {
            cprd[y] = residual;
            pa[y] = x, parc[y] = a;
            PQinc( y, cprd);
         }
      }
   }
   PQfree( );
   return cprd[t];
}
```

### Número de iterações do algoritmo de fluxo máximo

O consumo de tempo da função GRAPHmaxflow() é proporcional ao número de iterações e portanto ao número de caminhos aumentadores necessários para atingir o fluxo máximo.  Quantos caminhos são necessários, no pior caso?

Número de caminhos aumentadores:  O número de caminhos aumentadores usados pela função GRAPHmaxflow() junto com MaxCapAugmPath() nunca é maior que `2 A log M`, sendo A o número de arcos originais e M a maior das capacidades.

# Algoritmo de Bellman-Ford

Dado um vértice s de um grafo com custos arbitrários (positivos e negativos) nos arcos, encontrar uma árvore de caminhos mínimos com raiz s no grafo.

## Instrodução

Algoritmos de busca só sabem procurar passeios, pois não podem evitar a repetição de arcos. Um grafo que tem um ciclo negativo tem passeios de custo arbitrariamente pequeno, tão negativo quanto se queira. Assim, o algoritmo de busca é levado a repetir arcos ad æternum.

Se um grafo não tem ciclos negativos ao alcance de s, todos os caminhos de custo mínimo com origem s são simples. Além disso, a coleção de todos os caminhos mínimos com origem s pode ser representada por uma SPT (árvore de caminhos mínimos) com raiz s.

## Versão preliminar do algoritmo de Bellman-Ford

O algoritmo de Bellman-Ford tenta resolver o problema dos caminhos de custo mínimo. Se o grafo tiver um ciclo negativo ao alcance de s, o algoritmo anuncia a existência de um tal ciclo e desiste de encontrar uma SPT; caso contrário, o algoritmo calcula uma SPT com raiz s.

A ideia é simples: o algoritmo relaxa os arcos do grafo repetidamente. Cada arco é relaxado várias vezes mas não precisa ser relaxado mais que V vezes, sendo V o número de vértices do grafo.  Eis uma implementação preliminar do algoritmo:

```c

/* Recebe um grafo G com custos arbitrários nos arcos e um vértice s e decide se
existe algum ciclo negativo ao alcance de s. Se tal ciclo não existe, armazena em
pa[] o vetor de pais de uma SPT de G com raiz s. Se algum vértice v não está ao
alcance de s em G, teremos pa[v] == -1. As distâncias a partir de s são
armazenadas no vetor dist[]. */
bool GRAPHsptBF0( Graph G, vertex s, vertex *pa, int *dist)
{
   const int INFINITY = INT_MAX;
   for (vertex v = 0; v < G->V; ++v)
      pa[v] = -1, dist[v] = INFINITY;
   pa[s] = s, dist[s] = 0;

   for (int k = 0; k < G->V-1; ++k) {
      for (vertex v = 0; v < G->V; ++v) {
         if (dist[v] == INFINITY) continue; // Evita overflow
         for (link a = G->adj[v]; a != NULL; a = a->next) {
            if (dist[v] + a->c < dist[a->w]) {
               dist[a->w] = dist[v] + a->c; // relaxação
               pa[a->w] = v;
            }
         }
      }
   }

   for (vertex v = 0; v < G->V; ++v) {
      if (dist[v] == INFINITY) continue;
      for (link a = G->adj[v]; a != NULL; a = a->next)
         if (dist[v] + a->c < dist[a->w])
            return true; // ciclo negativo
   } // todos os arcos estão relaxado
   return false; // pa[] define uma SPT
}

```

A função GRAPHsptBF0() consome V(V+A) unidades de tempo no pior caso.

## Algoritmo de Bellman-Ford com fila

A versão preliminar do algoritmo de Bellman-Ford examina muitos arcos antes que o arco esteja em condições de ser relaxado. A versão seguinte do algoritmo examina os arcos numa ordem mais racional: um arco arco v-w é examinado somente depois que o valor de dist[v] tiver diminuído.  Para impor essa ordem, basta manter os vértices numa fila e inserir um vértice w na fila toda vez que algum algum arco da forma v-w for relaxado.

Se um ciclo negativo estiver ao alcance de s, o processamento continuará para sempre a menos que seja explicitamente interrompido. Mas essa interrupção não deve ser feita antes que todos os passeios com origem s sejam examinados.  Para implementar essa parada forçada, a fila de vértices é dividida em V-1 blocos e cada bloco é separado do seguinte por alguma marca. Adotaremos o pseudovértice V para fazer o papel da marca de separação entre blocos.

Para que a fila de vértices não fique excessivamente longa, um vértice será inserido na fila somente se já não estiver presente no bloco corrente, ou seja, depois da última ocorrência do separador V.

```c
/* Recebe um grafo G com custos arbitrários nos arcos e um vértice s e decide se 
existe algum ciclo negativo ao alcance de s. Se tal ciclo não existe, armazena em
pa[] o vetor de pais de uma SPT de G com raiz s. Se alum vértice v não está ao
alcance de s em G teremos pa[v] == -1. As distâncias a partir de s são armazenadas
no vetor dist[]. */
bool GRAPHsptBF1( Graph G, vertex s, vertex *pa, int *dist)
{
   const int INFINITY = INT_MAX;
   QUEUEinit( G->A);
   bool *onqueue = mallocc( G->V * sizeof (bool));
   for (vertex u = 0; u < G->V; ++u)
      pa[u] = -1, dist[u] = INFINITY, onqueue[u] = false;
   pa[s] = s, dist[s] = 0;
   QUEUEput( s);
   onqueue[s] = true;
   int V = G->V; // pseudovértice
   QUEUEput( V); // sentinela
   int k = 1;

   while (true) { // !QUEUEempty( )
      vertex v = QUEUEget( );
      if (v < V) {
         for (link a = G->adj[v]; a != NULL; a = a->next) {
            if (dist[v] + a->c < dist[a->w]) {
               dist[a->w] = dist[v] + a->c; // relaxação
               pa[a->w] = v;
               if (onqueue[a->w] == false) {
                  QUEUEput( a->w);
                  onqueue[a->w] = true;
               }
            }
         }
      } else { // A
         if (QUEUEempty( )) return false; // B
         if (k++ > G->V-1) return true; // C
         QUEUEput( V); // sentinela
         for (vertex u = 0; u < G->V; ++u)
            onqueue[u] = false;
      }
   }

   free( onqueue);
}
```