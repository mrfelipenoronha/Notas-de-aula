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
