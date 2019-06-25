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

```
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

# Aula 11/06

## Ordenação por contagem

Recebe um vetor a[0....n-1], com elementos em {0...r-1};

```
int n = a.length;
int[] count = new int[r+1];

for (int i = 0; i < n; i++) {

	int c = a[i];
	count[c+1]++;
}

for (int i = 0; i < r; i++)
	count[i+1] += count[i];

for (int i = 0; i < n; i++) {
	int c = a[i];
	aux[count[c]++] = a[i];
}

for (int i = 0; i < n; i++)
	a[i] = aux[i];

```

Complexidade O(n + r)

## Ordenação digital (radix sort)

Nessa ordenção, vamos ordenando pelos digitos dos numeros.

Temos dois *sabores* que definem por onde começamos a ordenação

- LSD: Least significant digit. Começamos a ordenar pelo fim, ou seja, ordenamos os numeros em relação ao ultimo digito de cada um, depois ordenamos pelo penultimo, e assim por diante.
- MSD: Most significant digit. Começamos a ordenar pelo começo.

```
public class LSD {

    // LSD radix sort
    public static void sort(String[] a, int W) {
        int N = a.length;
        int R = 256;   // extend ASCII alphabet size
        String[] aux = new String[N];

        for (int d = W-1; d >= 0; d--) {
            // sort by key-indexed counting on dth character

            // compute frequency counts
            int[] count = new int[R+1];
            for (int i = 0; i < N; i++)
                count[a[i].charAt(d) + 1]++;

            // compute cumulates
            for (int r = 0; r < R; r++)
                count[r+1] += count[r];

            // move data
            for (int i = 0; i < N; i++)
                aux[count[a[i].charAt(d)]++] = a[i];

            // copy back
            for (int i = 0; i < N; i++)
                a[i] = aux[i];
        }
    }


    public static void main(String[] args) {
        String[] a = StdIn.readAllStrings();
        int N = a.length;
        boolean verbose = false;

        if (args.length > 0 && args[0].compareTo("-v") == 0) {
            verbose = true;
        }

        // check that strings have fixed length
        int W = a[0].length();
        for (int i = 0; i < N; i++)
            assert a[i].length() == W : "Strings must have fixed length";

        // sort the strings
        double start = System.currentTimeMillis()/1000.0;
        sort(a, W);
        double now = System.currentTimeMillis()/1000.0;

        // print results
        if (verbose) {
            for (int i = 0; i < N; i++)
                StdOut.println(a[i]);
        }
        StdOut.println(Math.round(now - start) + " seconds");
    }
}

```

```
public class MSD {
    private static final int R      = 256;   // extended ASCII alphabet size
    private static final int CUTOFF =  15;   // cutoff to insertion sort

    // sort array of strings
    public static void sort(String[] a) {
        int N = a.length;
        String[] aux = new String[N];
        sort(a, 0, N-1, 0, aux);
    }

    // return dth character of s, -1 if d = length of string
    private static int charAt(String s, int d) {
        assert d >= 0 && d <= s.length();
        if (d == s.length()) return -1;
        return s.charAt(d);
    }

    // sort from a[lo] to a[hi], starting at the dth character
    private static void sort(String[] a, int lo, int hi, int d, String[] aux) {

        // cutoff to insertion sort for small subarrays
        if (hi <= lo + CUTOFF) {
            insertion(a, lo, hi, d);
            return;
        }

        // compute frequency counts
        int[] count = new int[R+2];
        for (int i = lo; i <= hi; i++) {
            int c = charAt(a[i], d);
            count[c+2]++;
        }

        // transform counts to indicies
        for (int r = 0; r < R+1; r++)
            count[r+1] += count[r];

        // distribute
        for (int i = lo; i <= hi; i++) {
            int c = charAt(a[i], d);
            aux[count[c+1]++] = a[i];
        }

        // copy back
        for (int i = lo; i <= hi; i++)
            a[i] = aux[i - lo];


        // recursively sort for each character
        for (int r = 0; r < R; r++)
            sort(a, lo + count[r], lo + count[r+1] - 1, d+1, aux);
    }


    // return dth character of s, -1 if d = length of string
    private static void insertion(String[] a, int lo, int hi, int d) {
        for (int i = lo; i <= hi; i++)
            for (int j = i; j > lo && less(a[j], a[j-1], d); j--)
                exch(a, j, j-1);
    }

    // exchange a[i] and a[j]
    private static void exch(String[] a, int i, int j) {
        String temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }

    // is v less than w, starting at character d
    // DEPRECATED BECAUSE OF SLOW SUBSTRING EXTRACTION IN JAVA 7
    // private static boolean less(String v, String w, int d) {
    //    assert v.substring(0, d).equals(w.substring(0, d));
    //    return v.substring(d).compareTo(w.substring(d)) < 0;
    // }

    // is v less than w, starting at character d
    private static boolean less(String v, String w, int d) {
        assert v.substring(0, d).equals(w.substring(0, d));
        for (int i = d; i < Math.min(v.length(), w.length()); i++) {
            if (v.charAt(i) < w.charAt(i)) return true;
            if (v.charAt(i) > w.charAt(i)) return false;
        }
        return v.length() < w.length();
    }


    public static void main(String[] args) {
        String[] a = StdIn.readAllStrings();
        int N = a.length;
        boolean verbose = false;

        if (args.length > 0 && args[0].compareTo("-v") == 0) {
            verbose = true;
        }
        double start = System.currentTimeMillis()/1000.0;        
        sort(a);
        double now = System.currentTimeMillis()/1000.0;

        // print results
        if (verbose) {
            for (int i = 0; i < N; i++)
                StdOut.println(a[i]);
        }
        StdOut.println(Math.round(now - start) + " seconds");
    }
}

```

---

# Aula 13/06

## Busca de substring - KMP

Dada uma string `pat`(tamanho m) e uma string `txt`(tamananho n), encontrar ocorrencia de `pat` em `txt`.

- Approach força bruta:

```
int search (String pat, String txt) {

	int i, n = txt.length();
	int j, m = pat.length();

	for (i = 0; i <= n-m; i++) {
		for (j = 0; j < m; j++) {

			if (txt.charAt(i) != pat.charAt(j))
				break

			if (j == m)
				return i;

	return n;
}
```

A implemtação utilizando força bruta tem complexidade de O(m(n-m)) ~ O(nm)


Agora, vamos aplicar uma ideia que nos da um complexidade que é somente proporcional á n.

Para chegar nisso, vamos, antes, fazer que o indice `j` seja *resetado* quando uma diferença é encontrada.

```
int search (String pat, String txt) {

	int i, n = txt.length();
	int j, m = pat.length();

	for (i = 0; i <= n-m; i++) {

		if (txt.charAt(i) == charAt(j))
			j++;

		else {
			i -= j; // Retrocesso
			j = 0;
		}

	return n;
}

```

Porém, a implementação acima desperdiça comparações que ja fizemos. Agora, a nossa ideia é olha o nosso proprio padrão, ate o indice `j`, e a partir disso, conseguir saber para onde eu irei mover o `j` para tentar criar correspondencia. Dessa maneira, eu nao *reseto* o indice `j`, eu so descolo ele algumas posições atras, aproveitando as comparações que eu ja fiz. Explicando melhor, a ideia é a seguinte:

Queremos encontrar o maior `k` tal que:
1. `pat[0...k-1] == pat[j-k-1...j-1]`
2. `pat[k] == txt[i]`

Ou seja, encontrar o maior prefixo de `pat` que seja igual ao prefixo de `pat` e que permita correspondencia com `txt`.

Podemos tambem, pre construir uma tabela com os `k`'s, isso elimina o tempo de encontra-lo durante a iteração. Essa tabela é a `dfa[alfabeto.size][m]`.

**Exemplo para tabela**
pat = ABABAC
dfa[3][6]

| |A|B|A|B|A|C|
|---|---|---|---|---|---|---|
|A|1|1|3|1|5|1|
|B|0|2|0|4|0|4|
|C|0|0|0|0|0|6|

```
int search (String pat, String txt) {

	int i, n = txt.length();
	int j, m = pat.length();

	for (i = 0; i < n && j < m ; i++)
		j = dfa[charAt(i)][j];

		if (j == m) return i;

	return n;
}
```

Dessa maneira, nosso algoritmo tem complexidade O(n), sem contar a criação da tabela `dfa[][]`.

Nosso problema se volta para a construção da tabela `dfa[][]` (deterministic finite automata)

A ideia é foda e a aula do Coelho foi incrivel, impossivel transcrever, apenas leia o codigo a seguir:

```
// Esse codigo é basicamente um pseudo-codigo, nao esta em nenhuma linguagem

void kmp (String pat) {

	int m = pat.length()
	dfa = new int[r][m] // r = tamanho do alfabeto (255 para ASCII extendido)

	dfa[0][0] = 1
	for (j = 1, x = 0; j < m; j++)

		for (c = 0; c < r; c++)
			dfa[c][j] = dfa[c][x]
			dfa[ pat[j] ][j] = j+1
			x = dfa[ pat[j] ][ c ]
}

```

---

# Aula 18/06

## Expressões regulares

Dado uma expressão regular `regex` de strings e uma string `txt`, encontrar uma (todas) ocorrencias de padrões `pat` em `regex` em `txt`

### Definindo expressão regular

Uma string `re` sobre Sigma (linguagem) é uma expressão regular se é:

 - vazia, denotamos por `e`
 - apenas um simbolo
 - (se) - entre parenteses
 - concatenação de `re1` e `re2`
 - disjunção (re1|re2)
 - fecho re*

### Exemplos

- `A(B|C)D` == ABD ou ACD
- `A*|(AB*B(C|A))*` == `e`, A, AAA, AAAAA, ... , ABBA, ABCBC

## Abreviaturas

- `.` = Curinga, aceita qualquer caracter
- `+` = Qualque quantidade de repetições, mas tem que ser maior que 1. A+ == A, AA, ...
- `?` = Pode repetir 0 ou 1 vezes. B? = e, B
- `{r}` = Faz r repretições.
- `[set]` = Qualquer coisa no conjunto set. [ABC]* = todas as strings com ABC.

## Teorema kleene

Para toda `regex` existe uma **dfa** que reconhece as strings representadas por `regex`.

## Plano

1. Criar uma `dfa` a partir da `regex`
2. Usar a `dfa` como no KMP

### Problema

O automato (dfa) pode ter um numero exponencial de estados. Para isso, vamos usar um `nfa`, que deixa de ser determinisco, e pode deixar coisas em aberto.

## Implemtação

```

boolean recongnizes (String txt) {

	int i, n = txt.length();
	DFSpaths dfs = new DFSpaths(G, 0);
	Bag<Integer> pc = new Bag<Integer>;
 	for (int v = 0; v < G.V(); v++)
		if (dfs.hasPath(v))
			pc.add(v);

	for (i = 0; i < n; i++) {
		Bag<Integer> match = new Bag<Integer>;
		for (int v : pc) {
			if (v == m) continue;
			if (re[v] == txt.charAt(i) || re[v] == '.')
				match.add(v+1);
		}

		dfs = new DFSpaths(G, match);
		pc = new Bag<Integer>;
		for (int v = 0; v < G.V(); v++)
		if (dfs.hasPath(v))
			pc.add(v);
	}

	for (int v : pc)
		if (v == m) return true;

	return false;
}


public class NFA (String regex) {

	re = regex.toCharArray();
	m = re.length;
	stack<Integer> ops = new stack<Integer>;
	G = new Digraph(m+1);

	for (int i = 0; i < m; i++) {
		int lp = i;

		if (re[i] == '(' || re[i] == '|')
			ops.push(i);

		else if (re[i] == ')') {
			int  or = ops.pop();
			if (re[or] == '|') {
				lp = ops.pop();
				G.addEdge(lp, or+1);
				G.addEdge(or, i);
			}
			else
				lp = or;
		}

		if (i < m-1 && re[i+1] == '*') {
			G.addEdge(lp, i+1);
			G.addEdge(i+1, lp);
		}

		if (re[i] == '(' || re[i] == ')' || re[i] == '*')
			G.addEdge(i, i+1);
	}
}



```
