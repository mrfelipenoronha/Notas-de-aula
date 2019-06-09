Materia para P3, darei merge com o arquivo principal quando acabar o semestre.

---

# Aula 30/05

## Introdução á grafos

- Um **digrafo** é um grafo direcionado.
- Um **digrafo simetrico** é um grafo biderecionado.
- Um **arco** é uma *flecha* que liga dois vertices.
- O **grau de entrada de V** é o numero de arcos que chegam no vertice v.
- O **grau de saida** é o numero de arestas que partem do vertice v.
- Uma **aresta** é um arco biderecionado.
- Analogamente, podemos definir o **o grau de um vertice v** como o numero de arestas ligadas á ele.
- Em um grafo com arestas direcionadas (arcos), o numero maximo de arcos é dado por: n*(n-1).
- Em um grafo com arestas bidirecionadas, o numero maximo de arestas é dado por: n*(n-1)/2.
- Um grafo pode ser representado por uma lista de adjacencia.

A implemtação de um digrafo é dada a seguir:

```java
public class Digraph {
	private int V; // Numero de vértices
	private int E; // Numero de arestas
	private Bag<Integer>[] adj; // Lista de adjacencia
	private int[] indegree; // Grau de entrada de cada vertice

	public Digraph(int V) {
		this.V = V;
		this.E = 0;
		indegree = new int[V];
		adj = (Bag<Integer>[]) new Bag[V];
		for (int v = 0; v < V; v++)
			adj[v] = new Bag<Integer>();

	}

	public int V() { return V; }

	public int E() { return E; }

	// Insere um arco
	public void addEdge(int v, int w) {
		adj[v].add(w);
		indegree[w]++;
		E++;
	}

	// Retorna a lista de adjacência de v
	public Iterable<Integer> adj(int v) {
		return adj[v];
	}

	// Retorna o grau de saída de v
	public int outdegree(int v) {
		return adj[v].size();
	}

	// Retorna o grau de entrada de v
	public int indegree(int v) {
		return indegree[v];
	}

	// Retorna o sigrafo reverso
	public Digraph reverse() {

		Digraph reverse = new Digraph(V);

		for (int v = 0; v < V; v++)
			for (int w : adj(v))
				reverse.addEdge(w, v);

		return reverse;
	}
}

```

## Caminhos em grafos

### DFS

- Complexidade O(V + E)

```Java

```java

public class DFSpaths{

	private final int s;
	private boolean[] marked;

	public void DFSpaths(Digraph G,int s){

		marked = new boolean[G.V()];
		this.s = s;
		dfs(G, s);
	}

	// retorna true se há caminho de s a v
	public boolean hasPath(int v){
		return marked[v];
	}

	private void dfs(Digraph G, int v) {
		marked[v] = true;
		for (int w : G.adj(v)) {
			if (!marked[w])
			dfs(G, w);
		}
	}
}

```

### BFS

- Complexidade: O(V + E)

```java
public class BFSpaths{

	private final int s;
	private boolean[] marked;

	public void BFSpaths(Digraph G,int s){

		marked = new boolean[G.V()];
		this.s = s;
		bfs(G, s);
	}

	// retorna true se há caminho de s a v
	public boolean hasPath(int v){
		return marked[v];
	}

	private void bfs(Digraph G, int v) {

		Queue<Integer> q= new Queue<Integer>();
		marked[v] = true;
		q.enqueue(s);

		while (!q.isEmpty()) {
			int v = q.dequeue();
			for (int w : G.adj(v))
				if (!marked[w]) {
					edgeTo[w] = v;
					marked[w] = true;
					q.enqueue(w);
				}
		}
}

```
---

# Aula 03/06

## Cortes

Um corte é uma bipartição do conjunto de vertices. Um arco pertence/atravessa um corte (S,T) se tiver uma ponta em S e outra em T.
Um corte é u, *st-corte* se s esta em S e t esta em T.

Para demonstrarmos que **não existe** um caminho de s a t basta exibirmos um st-corte em que todo arco no corte tem ponta inicial em T e ponta final em S.

## Arcos de arborescencia

São os arcos v-w que a dfs() percorre para visitar w pela primeira vez.
Conjunto de arborescências é a floresta da busca em profundidade

## Arcos de retorno

v-w é arco de retorno se w é ancestral de v.

## Arcos descendente

v-w é descendente se w é descendente de v, mas não é filho.

## Ciclos

---

# Aula 06/06

## Problema

Dado `G` e `s` determine a distancia de `s` aos demais vertices do digrafo. Nesta solução, fazemos com que a distancia entre quaisquer dois vertices é 1.

Guaradamos em um vetor `edgeTo[]` para podermos reconstruir a arvore de caminho de caminho minimo.

```java

public class BFSpaths {

	int INF; // Definindo um infinto
	int s;
	int[] distTo;
	int[] edgeTo; // Para conseguirmos reproduzir o caminho depois

	public BFSpaths (Digraph G, int s) {
		INF = G.V();
		distTo = new int[G.V()];
		edgeTo = new int[G.v()];
		this.s = s;
		for (int v = 0; v < G.V(); v++)
			distTo[v] = INF;
		bfs(G, s);
	}

	private void bfs(Digraph G, int s) {

		// BFS padrão
		Queue<Integer> q = new Queue<>();
		q.enqueue(s);

		while (!q.isEmpty()) {

			int v = q.dequeue();
			for (int w : G.adj(v)) {

				int d = dist[v] + 1;
				// Caso ainda não tenha sido setada
				if (distTo[w] == INF) {
					edgeTo[w] = v;
					distTo[w] = d;
					q.enqueue(w);
				}
			}
		}
	}
}

```

Para isso, vale O(V + E).

## Dijkstra

Aqui, vamos tratar um caso mais geral, quando as distancias entre vertices podem assumir qualquer valor não negativo.

Para isso, usamos a mesma ideia do caso acima, porem, a ordem que visitamos os vertices muda. Fazemos com que o proximo vertice visitado seja o vertice mais proximo (menor distancia) da origem que ainda não foi visitado, ou seja, propagamos as visitas na forma de uma *shockwave*.

```java

public class DijkstraPaths {

	double INF;
	int s;
	double[] distTo;
	arc[] edgeTo; // Para conseguirmos reproduzir o caminho depois

	public Dijkstra (EWDigraph G, int s) {

		INF = Double.Positivy_INFINITY;
		distTo = new double[G.V()];
		edgeTo = new arc[G.v()];
		this.s = s;
		for (int v = 0; v < G.V(); v++)
			distTo[v] = INF;
		distTo[s] = 0;
		dijkstra(G, s);
	}

	private void dijkstra(EWDigraph G, int s) {

		// Fila de prioridade com o index (peso) sendo os custo ate um
		// vertice e o valor associado sendo o vertice em si
		IndexMinPQ<Double> pq = new IndexMinPQ<>();
		pq.insert(s, distTo[s])

		while (!q.isEmpty()) {

			int v = q.dequeue();
			for (arc e : G.adj(v)) {

				int w = e.to();
				int d = dist[v] + e.weight();
				// Caso ainda não tenha sido setada
				if (distTo[w] > d) {
					edgeTo[w] = e;
					distTo[w] = d;

					if (pq.contains(w))
						pq.decreaseKey(w, d);

					else
						pq.insert(w, d);
				}
			}
		}
	}
}

```

A complexidade do algoritmo do algoritmo de dijkstra é de O(E * logV)

## Menor caminho em um DAG

Em um DAG (direct acyclic graph) podemos achar o caminho minimo usando a ordenação topologica. Ademais, podemos ter arestas com pesos negativos.

Para isso, percorremos o grafo na ordem que a ordação topologica definiu, com isso, eu consigo definir todas as menores distancias entre o vertice atual e os filhos.

---
