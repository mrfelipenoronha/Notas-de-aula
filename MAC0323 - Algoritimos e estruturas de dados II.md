# Bibliografia

- Algorithms IV in java - Sedgewick
---

# Aula 21/02 - Introdução

Nesta materia vamos usar Java e a biblioteca 'princeton.algs4' do livro Algorithms IV do Robert Sedgewick.

```java
import edu.princeton.cs.algs4.*;

public static increment(int[] a){
	// 'a' é um vetor de 0's e 1's
	// Transfora 0111 em 1000
	int k = a.length;
	int i = 0;                         // 1
	while (i < k && a[i] == 1){		   // 2
		a[i] = 0;					   // 3
		i = i + 1;                     // 4
	}
	if (i < k)						   // 5
		a[i] = 1;					   // 6
}
```
## Consumo de tempo:

Linha 1 = O(1)
Linha 2 = O(k)
Linha 3 = O(k)
Linha 4 = O(k)
Linha 5 = O(1)
Linha 6 = O(1)

Logo, o consumo de tempo total é O(k).

## Consumo de tempo para um numero N de chamadas
(começa com o vetor a preenchido por zeros)

Uma complexidade exagerada é O(n*k).
Agora, uma analise um pouco melhor:
- a[0] muda de estado n vezes
- a[1] muda de estado n/2 vezes
- a[2] muda de estado n/4 vezes
- ...
- a[n] muda de estado n/(2^n) vezes

Logo, o custo de mudança de estados de a[] é igual a
somatorio de i = 0, ate n, de 1/(2^i), que é igual a 2n.

Logo, o custo real desse programa é O(n).

## Custo amortizado
O custo amortizado de uma operação é o custo medio da
operação quando considerada uma sequencia de operações.
Usando o exemplo anterior, temos que o custo para N
operações é O(N), logo, o custo amortizado dessa
aplicação é O(1).

```java
import edu.princeton.cs.algs4.*;

import java.util.Iterator;

public class Stack<Item> implements Iterable<Item>{

	private Item[] a;
	private int n;

	public Stack(){
		// Class initializer - Constructor
		a = (Item[]) new Object[2];
		n = 0;
	}

	public boolean isEmpty(){
		return n == 0;
	}

	public int size(){
		return n;
	}

	private void resize(int cap){
		// Resize my stack into a size cap
		Item[] t = (Item[]) new Object[cap];
		for (int i = 0; i < n; i++){
			t[i] = a[i];
		}
		a = t;
	}

	public void push(Item item){
		// Insert item in stack
		if (n == a.length)
			resize(2*a.length); // If stack is full

		a[n++] = item;
	}

	public Item pop(){
		// Pop item in the stack
		Item item = a[n-1];
		a[n-1] = null; // avoid loitering
		// Loitering é a reutilização de memoria pelo Java
		n -= 1;

		// Keeping at least 1/4 of the stack occupied
		if (n > 0 && n == a.length/2)
			resize(a.length/2);

		return item;
	}

	public Iterator<Item> iterator(){
		return new RerverseArrayIterator();
	}

	private class RerverseArrayIterator implements Iterator<Item>{
		private int t = n;

		public boolean hasNext(){
			return t > 0;
		}

		public Item next(){
			return a[t--];
		}

		public void remove(){
			throw new UnsupportedOperationException();
		}
	}

	public static void main(String[] args){
		// Using stuff

		Stack<String> s = new Stack<String>();

		while (!StdIn.isEmpty()){
			String item = StdIn.readString();
			if (!item.equals("-")) s.push(item);
			else if (!s.isEmpty()) StdOut.println(s.pop() + " ");
		}

		StdOut.println("\nConteúdo usando while (...): ");
		StdOut.println("----");
		Iterator<String> it = s.iterator();

		while (it.hasNext())
			StdOut.println(it.next());

		StdOut.println("----\n");
		StdOut.println("\nConteúdo usando foreach statement: ");
		StdOut.println("----");

		for (String bla: s)
			StdOut.println(bla);

		StdOut.println("----\n");
		StdOut.println("(" + s.size() + " left on stack)");
	}
}
```

## Analise de custo para sequencia de operações push()

s0 -> s1 -> ... -> sm
si = estado da pilha apos o i-esimo push

Custo do push O(m) - Exagero
Custo da sequencia O(m²) == EXAGERO

Agora, uma analise mais cuidadosa
- 1 => a.lenght = 1; custo = 1
- 2 => a.lenght = 2; custo = 2
- 3 => a.lenght = 4; custo = 3
- 4 => a.lenght = 4; custo = 1
- 5 => a.lenght = 8; custo = 5
- 6 => a.lenght = 8; custo = 1

Logo, a soma desses custos vai ser igual a:
    m + sum(i = 0 -> k) 2^i = m + 2^(k+1)-1, onde k = log(m) ~= 3*m.

Assim, o consumo de tempo de m operações de push() é O(m).
Ja o custo de tempo amortizado sera O(1), ou seja, constante.

Esses custos são analogos para o operação de Pop() da stack.

---

# Aula 26/02

## Conjuntos disjuntos
Seja S = {s1, s2, ..., sn} uma coleção de conjuntos disjuntos, ou seja,
Si ^ Sj = 0/, para todo i e j.

## Coleção disjunta dinâmica
Conjuntos sao modificados ao longo do tempo.

## Tipos de union find

- Quick find: realiza o find em O(1) e union em O(n);
- Quick union: find em O(n) e union em O(1);
- Weighet UF: find em O(lg n) e union em O(lg n);

## Função log-estrela

ln*n é o menor k tal que lg(lg(lg(...k vezes...))) <= 1
- Com isso é possivel possivel implementar o union find com path compression,
fazendo union e find em O(lg*n), que é quase constante.

```Java
import edu.princeton.cs.algs4.*;

public class UnionFind {

	private int[] id;
	private int[] sz;
	private int count;

	public UnionFind(int n){
		count = n;
		id = new int[n];
		sz = new int[n];

		for (int i = 0; i < n; i++){
			sz[i] = 1;
			id[i] = i;
		}
	}

	public int count(){
		return count;
	}

	public boolean connected(int p, int q){
		return find(p) == find(q);
	}

	public int find(int p){
		while (p != id[p])
			id[p] = id[id[p]];
			p = id[p];
		return p;
	}

	public void union(int p, int q){
		p = find(p);
		q = find(q);

		if (p == q) return;

		if (sz[p] < sz[q]){
			id[p] = q;
			sz[q] += sz[p];
		}
		else {
			id[q] = p;
			sz[p] += sz[q];
		}

		count--;
	}
}

```
O UF realiza operações de Union() e de find() em O(1P).

---

# Aula 28/02

## Representação de arvores em vetores

Dado um numero **m** que representa o numero de nós na arvore. A matriz **a[]** irá representar a nossa arvore.

Dizemos que para qualque índice i de a[], temos que:
- 2i é o filho esquedo de i;
- 2i + 1 é o filho direito de i;
- i/2 é o pai de i.

Ademais, temos as seguintes convenções:
- O nó 1 não tem pai;
- i tem filho esquedo se 2i <= m;
- i tem filho direito se 2i + 1 <= m.

Em cada nivel 'p' os nós tem indices
	2^p, 2^p + 1, .. , 2^(p+1) - 1.    
O nivel de um nó com indice é log(i).

## Altura

A altura de um nó i é o maio caminho até a folha.  
A altura de um no i é chão(log(m/i)).

# Heaps

Um vetor a[1..m] é um **max-heap** se a[i/2] >= a[i], para todo i.

O coração de um algoritmo para a manipulação de um max-heap é uma função que recebe um vetor a[1..m] e um indice **p** e faz **a[p]** *descer* para sua posição correta, tal função chamaremos de sink().

```Java
import edu.princeton.cs.algs4.*;

public class MaxHeap {
	// Sort strings using heapsort. Will sort the first string passed as
	// paramether (args[0]) in the call of the application.

	public static void sort(String[] pq) {
		// Will transform the givem string in a max-heap

        int n = pq.length;
        for (int k = n/2; k >= 1; k--)
            sink(pq, k, n);
        while (n > 1) {
            swap(pq, 1, n--);
            sink(pq, 1, n);
        }
    }

    private static void sink(String[] pq, int k, int n) {
		// "Descend" character to right position

        while (2*k <= n) {
            int j = 2*k;
            if (j < n && less(pq, j, j+1)) j++;
            if (!less(pq, k, j)) break;
            swap(pq, k, j);
            k = j;
        }
    }

    private static boolean less(String[] pq, int i, int j) {
        return pq[i-1].compareTo(pq[j-1]) < 0;
    }

    private static void swap(Object[] pq, int i, int j) {
		// Swap two terms

        Object aux = pq[i-1];
        pq[i-1] = pq[j-1];
        pq[j-1] = aux;
    }

    private static void print(String[] a) {
		// Print actual String

        for (int i = 0; i < a.length; i++)
            StdOut.printf("%s", a[i]);
        StdOut.println();
    }

    public static void main(String[] args) {
		char[] c = args[0].toCharArray();
        String[] a = new String[args[0].length()];
		for (int i = 0; i < args[0].length(); i++)
			a[i] = String.valueOf(c[i]);
        MaxHeap.sort(a);
        print(a);
    }
}

```

O consumo de tempo da função sink() é O(lg m).

O consumo de tempo para a construção de um max-heap é O(nlgn), onde n é o numero de elementos.

O consumo de tempo amortizado é de O(n).

---

# Aula 07/03

## Ordenação por seleção

```java
public static void selecao(int n, Comparable [] v){
    int i, j, max;
    for (i = n; i > 1; i--){
        max = i;
        for (j = i-1; j >= 1; j--)
            if (!less(v[j], v[max]))
                max = j;
        exch(v[i], v[max]);
    }
}

private boolean less(Comparable a, Comparable b){
    return a.compareTo(b);
}
```

Na Ordenação por seleção temos as seguintes invariantes:
1. v[i+1 .. n] é crescente;
2. v[1 .. i] <= v[i+1 .. n];
3. v[1 .. i] é um maxHeap.

## HeapSort

A ideia do HeapSort é tranformar cada no da arvore em um MaxHeap, usando a funlção sink() da aula passada. Fazendo isso de forma recursiva, é possivel ordenar o vetor.

> implementação acima

O consumo de tempo é de aproximadamente O(n*logn).

## Filas com prioridade / Priority queue

Uma Priority Queu (PQ) é uma estrutura que manipula um conjunto de itens por meio de duas operações:
1. Inserção de um novo item;
2. Remoção de um item maximo.

Tal estrutura funciona com ambas implementações de Max/Min-Heap.

A API é a seguinte:

| Tipo | Nome | Descrição |
| --- | --- | --- |
|  | MaxPQ(int cap) | Cria uma PQ |
| void | Insert(Item v) | Insere v na PQ |
| Item | max() | Devolve o maximo elemento |
| Item | delMax() | Remove e devolve |
| boolean | isEmpty() | PQ esta vazia |
| int | size() | Tamanho da PQ |

```Java

public class MaxPQ<Item extends Comparable<Item>> implements Iterable<Item> {

    private Item[] pq; // Heap will fit in pq[1..n]
    private int n; // size

    public MaxPQ(int MAXN) {
        // Constructor Method

        pq = (Item[]) new Comparable<Item>[MAXN + 1];
    }

    public int size() {
        return n;
    }

    public boolean isEmpty() {
        return n == 0;
    }

    private booelan less(int i, int j) {
        // Used to itens itens in the struct

        Item itemI = pq[i];
        Item itemJ = pq[j];
        return itemI.compareTo(itemJ);
    }

    private void exch(int i, int j) {
        // Change itens with indexes i and j

        Item t = pq[i];
        pq[i] = pq[j];
        pq[j] = t;
    }

    private void sink(int p) {
        // Sink iten in the pq;

        while (2 * p <= n) {
            int f = 2 * p;
            if (f < n && less(f, f + 1))
                f++; // Biggest child
            if (!less(p, f))
                break; // Already a MaxHeap

            exch(p, f);
            p = f; // Sinking
        }
    }

    private void swim(int f) {
        // Inverse of Sink, get a child as up as possible in the PQ

        while (f > 1 && les(f / 2, f)) {
            exch(f / 2, f);
            f = f / 2;
        }
    }

    public void insert(Item v) {
        // Insert item into PQ

        pq[++n] = v;
        swim(n);
    }

    public Item delMax() {
        // Delete max element and returns it

        Item ret = pq[1];
        exch(1, n--);
        pq[n++] = null;
        sink(1);
        return ret;
    }

	public Iterator<Item> iterator() {
		return new HeapIterator();
	}

	private class HeapIterator implements Iterator<Item> {
		private MaxPQ<Item> copy;

		// Add all items to the copy of the heap
		// Will cost O(n), because it'll already be sorted
		public HeapIterator() {
			copy = new MaxPQ<Item>(size());
			for (int i = 1; i <= n; i++)
				copy.insert(pq[i]);
		}

		public boolean hasNext() {
			return !copy.isEmpty();
		}

		public Item next(){
			if (!hasNext())
				throw new java.util.NoSuchElementExcepetion();

			return copy.delMax();
		}

		public void remove() {
			throw new UnsupportedOperationException();
		}
	}
}

```

As operações são realizada em O(log n)

---

# Aula 12/03

## Mergeable Heaps

O leftist heap é uma implementação da PQ que permite a união (merge) de duas filas seja feita de forma eficiente.

Para isso, cada no X tem os seguintes campos:
1. esq[x]: filho esquerdo de x;
2. dir[x]: filho direito de x;
3. dist[x]: menor cumprimento de um caminho de x a null.

Uma arvore é esquerista se **dist[esq[x]] >= dist[dir[x]], para todo nó x**.

O caminho direitista de um no **x** é a sequencia {x, dir[x], dir[dir[x]], ..., null}
1. dcomp[x]: numero de nós no caminho direitista de x;
2. tam[x]: numero de nos da arvore de raiz x.

Se x é um no de uma arvore esquedista então dist[x] == dcomp[x].

### Heap esquerdista

H := arvore
raiz[H] := raiz de H
prior[x] := prioridade do no x
pai[x] := pai do no x

Um heap esquerdista H é uma arvore esquerdista que satisfaz:

	prior[pai[x]] >= prior[x]

para todo no x diferente da raiz.

Com tal estrutura, podemos realizar uma operação LeftistHeapMerge(), que tem complexidade **O(log m)**, onde m = tam(raiz[h1]) + tam(raiz[h2]).

Todas as operações e metodos implementados em um LeftistMaxPQ tem complexidade **O(lg n)**, onde n é o numero de itens na fila.


```Java
public class LeftistMaxPQ<Item extends Comparable<Item>> {

    private Node root;
    private int n;

    private class Node {
        private Item item;         
        private Node left, right;
        private int dist;          

        public Node(Item item, Node left, Node right, int dist) {
            this.item = item;
            this.left = left;
            this.right = right;
            this.dist = dist;
        }

        public String toString() {
            String s = "{Item: " + item + "(" + left + "),[" +
                right + "] " + dist + "}";
            return s;
        }
    }


    // Initializes an empty priority queue.
    public LeftistMaxPQ() {
    }

    public boolean isEmpty() {
        return root == null;
    }

    public int size() {
        return n;
    }

    public void insert(Item item) {
        Node s = new Node(item, null, null, 1);
        root = merge(root, s);
        n++;
    }

    public Item delMax() {
        Item max = root.item;
        root = merge(root.left, root.right);
        n--;
        return max;
    }

	// Transform 'this' in the union 'this + that'
    public void union(LeftistMaxPQ<Item> that) {
        if (that == null) return ;
        this.root = merge(this.root, that.root);
        this.n += that.n;
    }

    // Merge mantendo MaxLeftist
    private Node merge(Node r1, Node r2) {
        if (r1 == null) return r2;
        if (r2 == null) return r1;

        // r1 != null and r2 != null
        if (less(r1, r2)) {
            Node tmp = r1; r1 = r2; r2 = tmp;
        }
        // r1 points to bigger item
        if (r1.left == null) r1.left = r2;
        else {
            r1.right = merge(r1.right, r2);
            if (r1.left.dist < r1.right.dist) {
                // exchange left and right
                Node t = r1.left;
                r1.left = r1.right;
                r1.right = t;
            }
            r1.dist = r1.right.dist + 1;
        }

        return r1;
    }

    private boolean less(Node r, Node s) {
        return r.item.compareTo(s.item) < 0;
    }
}
```


## Binomial heaps/trees

Arvores binomiais são definidas recursivamente:
- Um no é a arvore binomial b0 de ordem 0.
- Para k = 1, 2, ..., a arvore bonomial bk de ordem k consiste de duas arvores bk-1 ligadas de tal forma que a raiz de uma é o filho mais á esquerda da raiz da outra.

Estrutura em uma arvore binomial bk de ordem k:
1. A raiz tem k filhos;
2. A arvore tem 2^k nós;
3. A arvore tem altura k;
4. Há exatamente (k|i) (= combinação) nós com profundidade i;
5. Os filhos da raiz são as avores binomiais bk-1, bk-2, bk-3, ...

**O grau maximo de qualquer nó em uma arvore binomial com n nós é lg(n).** Tal fato é fundamental para o consumo de tempo da binomial heap.

Os filhos de nó serão representados atravez de uma lista ligada ordenada pelos graus (= order) dos filhos.

### Binomial Heap

Uma binomial heap H é uma coleção de binomial trees que satisfaz as seguintes propriedades:
- Cada binomial tree em H é uma MinPQ (ou, tambem, uma MaxPQ), ou seja, o valor associado ao item de cada nó é menor ou igual (maior ou igual) ao valor associado de seus filhos.
- H possui no maximo uma binomial tree de cada ordem.

Assim, temos as seguintes operações e suas respectivas complexidades:

| Função | complexidade  |
| :------------- | :------------- |
| insert() | O(1) (amortizado) |
| delMin() | O(lgn) |
| change() | O(lgn) |
| union() | O(lgn) |

```Java
public class BinomialMinPQ<Key> implements Iterable<Key> {
    private Node head; // Head of the list of roots
    private final Comparator<Key> comp;	// Comparator over the keys

    //Represents a Node of a Binomial Tree
    private class Node {
        Key key; // Key contained by the Node
        int order;  // The order of the Binomial Tree rooted by this Node
        Node child, sibling; // Child and sibling of this Node
    }

    public BinomialMinPQ() {
        comp = new MyComparator();
    }

    public BinomialMinPQ(Comparator<Key> C) {
        comp = C;
    }

    public BinomialMinPQ(Key[] a) {
        comp = new MyComparator();
        for (Key k : a) insert(k);
    }

    public BinomialMinPQ(Comparator<Key> C, Key[] a) {
        comp = C;
        for (Key k : a) insert(k);
    }

    public boolean isEmpty() {
        return head == null;
    }


    public int size() {
        int result = 0, tmp;
        for (Node node = head; node != null; node = node.sibling) {
            if (node.order > 30) { throw new ArithmeticException("The number of elements cannot be evaluated, but the priority queue is still valid."); }
            tmp = 1 << node.order;
            result |= tmp;
        }
        return result;
    }

    public void insert(Key key) {
        Node x = new Node();
        x.key = key;
        x.order = 0;
        BinomialMinPQ<Key> H = new BinomialMinPQ<Key>(); //The Comparator oh the H heap is not used
        H.head = x;
        this.head = this.union(H).head;
    }

    public Key minKey() {
        if (isEmpty()) throw new NoSuchElementException("Priority queue is empty");
        Node min = head;
        Node current = head;
        while (current.sibling != null) {
            min = (greater(min.key, current.sibling.key)) ? current : min;
            current = current.sibling;
        }
        return min.key;
    }

    public Key delMin() {
        if(isEmpty()) throw new NoSuchElementException("Priority queue is empty");
        Node min = eraseMin();
        Node x = (min.child == null) ? min : min.child;
        if (min.child != null) {
            min.child = null;
            Node prevx = null, nextx = x.sibling;
            while (nextx != null) {
                x.sibling = prevx;
                prevx = x;
                x = nextx;nextx = nextx.sibling;
            }
            x.sibling = prevx;
            BinomialMinPQ<Key> H = new BinomialMinPQ<Key>();
            H.head = x;
            head = union(H).head;
        }
        return min.key;
    }

    public BinomialMinPQ<Key> union(BinomialMinPQ<Key> heap) {
        if (heap == null) throw new IllegalArgumentException("Cannot merge a Binomial Heap with null");
        this.head = merge(new Node(), this.head, heap.head).sibling;
        Node x = this.head;
        Node prevx = null, nextx = x.sibling;
        while (nextx != null) {
            if (x.order < nextx.order ||
                (nextx.sibling != null && nextx.sibling.order == x.order)) {
                prevx = x; x = nextx;
            } else if (greater(nextx.key, x.key)) {
                x.sibling = nextx.sibling;
                link(nextx, x);
            } else {
                if (prevx == null) { this.head = nextx; }
                else { prevx.sibling = nextx; }
                link(x, nextx);
                x = nextx;
            }
            nextx = x.sibling;
        }
        return this;
    }

    // Compares two keys
    private boolean greater(Key n, Key m) {
        if (n == null) return false;
        if (m == null) return true;
        return comp.compare(n, m) > 0;
    }

    // Assuming root1 holds a greater key than root2, root2 becomes the new root
    private void link(Node root1, Node root2) {
        root1.sibling = root2.child;
        root2.child = root1;
        root2.order++;
    }

    // Deletes and return the node containing the minimum key
    private Node eraseMin() {
        Node min = head;
        Node previous = null;
        Node current = head;

        while (current.sibling != null) {
            if (greater(min.key, current.sibling.key)) {
                previous = current;
                min = current.sibling;
            }
            current = current.sibling;
        }
        if (min == head) head = min.sibling;
        else previous.sibling = min.sibling;

        return min;
    }

    //Merges two root lists into one, there can be up to 2 Binomial Trees of same order
    private Node merge(Node h, Node x, Node y) {
        if (x == null && y == null) return h;
        else if (x == null) h.sibling = merge(y, null, y.sibling);
        else if (y == null) h.sibling = merge(x, x.sibling, null);
        else if (x.order < y.order) h.sibling = merge(x, x.sibling, y);
        else                        h.sibling = merge(y, x, y.sibling);
        return h;
    }

    public Iterator<Key> iterator() {
        return new MyIterator();
    }

    private class MyIterator implements Iterator<Key> {
        BinomialMinPQ<Key> data;

        // Constructor clones recursively the elements in the queue
        // It takes linear time
        public MyIterator() {
            data = new BinomialMinPQ<Key>(comp);
            data.head = clone(head, null);
        }

        private Node clone(Node x, Node parent) {
            if (x == null) return null;
            Node node = new Node();
            node.key = x.key;
            node.sibling = clone(x.sibling, parent);
            node.child = clone(x.child, node);
            return node;
        }

        public boolean hasNext() {
            return !data.isEmpty();
        }

        public Key next() {
            if (!hasNext()) throw new NoSuchElementException();
            return data.delMin();
        }

        public void remove() {
            throw new UnsupportedOperationException();
        }
    }

    // default Comparator
    private class MyComparator implements Comparator<Key> {
        @Override
        public int compare(Key key1, Key key2) {
            return ((Comparable<Key>) key1).compareTo(key2);
        }
    }

}
```

---

# Aula 14/03

## Hash Table (HT)

Uma tabela de simbolos é uma ADT(Abstract data type) que consiste em um conjunto de itens, sendo cada item um par `Key - value`, munido das operações:
- put(): insere um item na HT;
- get(): busca o valor associado a chave.   

Convenções sobre HTs:
- não há chaves repetidas;
- `null` nunca é usado como chave;
- `null` nunca é usado como valor.

API de uma HT:

| Tipo | Chamda | Descrição |
| --- | --- | --- |
|  | HT() | cria uma HT |
| void | put(Key key, Value val) | Insere (key, val) |
| Value | get(Key key) | Busca o valor associado a Key |
| void | delete(Key key) | remove o valor associado a chave |
| int | rank(Key key) | Numeoro de keys menores que key |
| boolean | isEmpty() | HT esta vazia? |
| boolean | contains(Key key) | A key esta na HT? |
| Iterable<Key> | keys() | Lista todas as chaves da HT |

Consumo de tempo:
- Durante a execução de get() ou put(), uma chave de HT é tocada quando comparada com key. Logo, o consumo de tempo é proporcional ao numero de chaves tocadas;
- O custo medio de uma busca bem sucedida é o quociente `c/n`, onde c é a soma dos custos das buscas e n é o numero total de chaves na tabela.

> Implementação usando lista ligada

---
# Aula 26/03

## Self-organizing lists

Metodos que reordena a lista, facilitando operações de consulta e inserção. Todas as ideias desse estilo utilizam a ideia de trazer os itens mais utilizados para a frente da lista ligada.

### Move to front
Ideia de mover um nó recem acessado para o inicio da lista. Mantendo os mais acessados no inicio da lista, o que diminui a complexidade.

## Lista ligada ordenada
As operações de get() ou set() são faceis de serem feitas numa lista ligada ordenada (LLO), porém, o que demora, é achar a posição em que elas devem ocorrer.   

Para contornar esse problema, vamos usar duas listas ligadas. A L0 vai conter todos os elementos, a L1 vai conter só uma parte dos elementos da L0, vamos usar a L1 como uma forma rapida de percorrer a lista ligada, logo, a L1 vai ser uma distribuição de partes dos elementos da L0.

> L0 -> 1 2 5 7 12 17 20 28 50 100 145 342 532   
> L1 -> 1       12          50             532

Logo, dessa maneira, o consumo de tempo sera <= |L1| + n/|L1|.
Para isso, temos que o valor que minimiza a complexidade é |L1| = sqrt(n).

Podemos generalizar e melhorar utilizando a ideia de raiz quadrada, atingindo resultados em lgN qundo melhoramos os niveis.

**Fato**: Existem lgN niveis.
**fato**: Em uma busca, visitamos no maximo 2 nós de cada nivel.
**Conclusão**: Numero de comparação <= 2lgN.

```java
public class SkipListST<Key, Value> {

	int MAX = 20; // Numero maximo de niveis
	int lgn; // Numero real de niveis

	Node first; // Head da LL
	int n; // Numero de elementos na lista

	// Estrutura da lista ligada com skip
	private class node{
		Key key;
		Value val;
		Node[] next;

		public Node(Key key, Value val, int levels){
			this.key = key;
			this.val = val;
			this.next = new Node[levels];
		}
	}

	public SkipListST() {
		first = new Node(null, null, MAX);
	}

	public int size() {
		return n;
	}

	public Value get(Key key) {

		Node p = first;

		// Loop para descer os niveis
		for (int k = lgn-1, k >= 0; k--) {
			p = rank(key, p, k);
			Node q = p.next[k];
			if (q != null && q.key.equals(key))
				return q.val;
		}

		// Caso não tenha achado nada
		return null;
	}

	private Node rank(Key key, Node start, int k) {

		Node p = start;
		Node q = p.next[k];

		while (q != null && q.key.compareTo(key) < 0){
			p = q;
			q = q.next[k];
		}

		return p;
	}

	private void put(Key key, Value val) {

		if (val == null){
			delete(key);
			return;
		}

		Node[] s = new Node[MAX];
		Node p = first;
		for (int k = lgn-1; k >= 0; k--){
			p = rank(key, p, k);
			Node q = p.next[k];
			if (q != null && q.key.equals(key)){
				q.val = val;
				return;
			}
			s[k] = p;
		}

		int levels = randLevel();
		Node novo = new Node(key, val, levels);

		for (int k = levels-1; k >= 0; k--){
			Node t = s[k].next[k];
			s[k].next[k] = novo;
			novo.next[k] = t;
		}

		n++;
	}

	private int randLevel(){

		int level = 0;
		int r = StdRandom.uniform((1<<(MAX-1)));

		while ((r&1) == 1){
			if (level == lgn){
				if (lgn == MAX)
					return MAX;
				return lgn+1;
			}

			level++;
			r >>= 1;
		}
		return level+1;
	}

	public void delete(Key key) {

		Node[] stop = new Node[MAX];
		Node p = first;

		for (int k = lgn-1; k >= 0; k--) {
			p = rank(key, p, k);
			stop[k] = p;
		}

		// Q aponta para o possivel no com o valor
		Node q = stop[0].next[0];

		// Se a chave nao esta na HT
		if (q == null || q.key.equals(key))
			return;

		int levels = q.next.length;

		for (int k = 0; k < levels; k++)
			stop[k].next[k] = q.next[k];

		for (int k = lgn-1; k >= 0 && first.next[k] == null; k--)
			lgn--;

		n--;
	}
}
```

---

# Aula 28/03

## Arvore binaria

São estruturas com celular/nós, que possuem 3 capos:
1. Conteudo;
2. Filho esquedo;
3. Filho direito;
4. Pai.

Da mesma forma, uma arvore binaria possui uma raiz.   
A profundidade de um nó é a distancia dele ate a raiz.   
A altura de uma arvore é a maior altura.

## Maneiras de percorrer una BT

- In-order traversal: **ERD**. Visita subarvore esquerda, visita raiz, visita subarvore direita.

- Pre-order traversal: **RED**.

- Pos-order traversal: **EDR**

Para essas travessias, vamos criar um iterador para cada uma.

Se a arvore for constituida de operadores numeros e possuir numeros nas folhas, temos duas possibilidades:
1. Percorrer a arvore in-order vai nos devolver a expressão da forma comum (2 * (8 + 7) / 9);
2. Percorrer a arvore pos-order vai nos devolver a notação posfixa, que é muito util para calculos usando pilhas e que são resolvidos de maneira linear.

```java
public class BT {

	private Node r;

	// Estrutura para cada nó da arvore
	private class Node {

		private Item item;
		private Node left, right;
		private Node parent;

		public Node (Item item, Node left, Node right, Node parent){
			this.item = item;
			this,left = left;
			this.right = right;
			this.parent = parent;
		}

		// Vendo se um node é uma folha
		public booelan isLeaf() {
			return this.left == null && this.right == null;
		}
	}

	// Metodo iterador para travessia in-order
	public Iterable<Item> inOrder() {

		Queue<Item> q = new Queue<Item>();
		inOrder(r, q);
		return q;
	}

	// Função que realiza a travessia in-order recursivamente
	private void inOrder(Node r, Queue<Item> q) {

		if (r != null) {
			inOrder(r.left, q);
			q.enqueue(r.item);
			inOrder(r.right, q);
		}
	}

	// Função que realiza a travessia in-order de forma iterativa
	private void inOrderIterative(Node r, Queue<Item> q) {

		// Pilha auxiliar dos nos percorridos
		Stack<Node> s = new Stack<Node>();

		while (r != null || !s.isEmpty()) {

			// Tenho filho esquerdo, ou seja, tenho que visitar ele
			if (r != null) {
				s.push(r);
				r = r.left;
			}

			// Tenha terminado uma subarvore esquerda, agora volto para uma raiz
			else {
				r = s.pop();
				q.enqueue(r.item);
				r = r.right;
			}
		}
	}

	// Retorna o sucessor in-order do node 'p', com 'p' != null
	private Node sucessor(Node p) {

		// A ideia é ir para direita e efundar, caso eu tenha filho direito
		if (p.right != null) {
			Node q = p.right;
			while (q.left != null) q = q.left;
			return q;
		}

		// Caso contrario, eu tenho que voltar para o pai esquerdo mais proximo
		while (p.parent != null && p.parent.right == p)
			p = p.parent;

		return p.parent;
	}

	// Escreve a arvore de um jeito escroto
	private static void writeBT(Node x) {

		// Caso tenha chego em um no vazio
		if (x == null) {
			StdOut.print(1);
			return;
		}

		// Printa 0 caso exista aquele nó
		StdOut.print(0);
		writeBT(x.left);
		writeBT(x.right);
	}

	// Vai ler uma arvore do jeito escroto, devolvendo um no com a raiz dela
	private static Node readBT() {

		// Caso a leitura seja == 1
		if (StdIn.reaInt())
			return null;

		// Criando novo nó, mas sem o parent
		return new Node(666, readBT(), readBT(), null);

		// Aparentemente isso é uma parada muito top e util
	}

	// Metodo iterador para travessia pre-order
	public Iterable<Item> preOrder() {

		Queue<Item> q = new Queue<Item>;
		preOrder(r, q);
		return q;
	}

	// Função que realiza a travessia pre-order
	private void preOrder(Node r, Queue<Item> q) {

		if (r != null) {
			q.enqueue(r.item);
			preOrder(r.left, q);
			preOrder(r.right, q);
		}
	}

	// Metodo iterador para travessia pos-order
	public Iterable<Item> posOrder() {

		Queue<Item> q = new Queue<Item>;
		posOrder(r, q);
		return q;
	}

	// Função que realiza a travessia pos-order
	private void posOrder(Node r, Queue<Item> q) {

		if (r != null) {
			posOrder(r.left, q);
			posOrder(r.right, q);
			q.enqueue(r.item);
		}
	}
}
```

## Doubling method

Metodo para estimar o consumo de tempo de um determinado código.  
**Hipotese**: O consumo de tempo do programa T(n) = a*(n^b).   
**Consequencia**: Quando n cresce linearmente T(2n)/T(n) ~ 2^b. T(2n) = a(2n)² = 4an² = 4T(n).

Esse metodo pode ser descrito da seguinte maneira:
- Comece com um n moderado;
- Meça e registe o consumo de tempo;
- Dobre o valor de n;
- Repita enquanto for possivel;
- Verifique que a razão de tempos consecutivos se aproxima 2^b;
- Prediga e extrapole.

## Problema 3-sum
Dados n numeros inteiros, quantos trios que soma 0?

### Solução força bruta
```Java
for (int i = 0; i < n; i++)
	for (int j = 0; j < n; j++)
		for (int k = 0; k < k; k++)
			if (a[i] + a[j] + a[k] == 0)
				ans++;
```

---

# Aula 04/04

## Arvores binarias de busca (BST)

Arvore utilizada para representar uma tabela de simbolos. Ou seja, utiliza chaves, no esquema de uma arvore binaria.

- Todas as chaves a esquerda de um no são menores que ele
- Todas as chaves a direita de um no são maiores que ele
- O numero de arvores que podem ser construidas a partir de um vetor ordenado com N numeros é: comb(2n n)(1/n+1)
- O comprimento interno de uma BT é a soma das profundidades dos seus nos

### Teoria da informação
Não existe algoritmo que implemente get() e put() em tempo O(lg(lgn)).    
Assim como, todo algoritmo de ordenação baseado em comparação, tem complexidade proporcional a O(n*logn) no pior caso.

```java

public class BST<Key extends.... Values> {

	private Node r; // Raiz

	private class Node {

		Key key;
		Value val

		Node left, right;

		// Contrutor do no
		public Node(Key key, Value val) {
			this.key = key;
			this.val = val;
		}
	}

	// Retorna o valor de um item com a chave key
	public Value get(Key key) {

		Node x = get(r, Key); // Chamando metodo privado recursivo
		if (x == null)
			return null;

		return x.val;
	}

	// Acha uma chave de forma recursiva
	private Value get(Node x, Key key) {

		if (x == null)
			return null;

		int cmp = key.compareTo(x.key);

		if (cmp < 0) // A chave é menor
			return get(x.left, key);

		if (cmp > 0) // A chave é maior
			return get(x.right, key);

		// Achei o no correto
		return x;
	}

	// Dado um valor val, insere na arvore
	public void put(Key key, Value val) {

		r = put (r, key, val);
	}

	// Metodo privado para a inserção
	private Node put(Node x, Key key, Value val) {

		// Criando um novo no para a arvore
		if (x == null)
			return new Node(key, val);

		int cmp = key.compareTo(x.key);

		// Descendo na arvore
		if (cmp < 0)
			x.left = put(x.left, key, val);

		else if (cmp < 0)
			x.right = put(x.right, key, val);

		// Caso eu esteja na chave, logo, altero o valor
		else
			x.val = val;

		return x;
	}

	// Retorna o valor da menor chave
	public Value min() {

		if (r == null)
			return null;

		return min(r).val;
	}  

	private Node min(Node x){

		if (x == null)
			return null;

		if (x.left == null)
			return x;

		return min(x).left;
	}

	// Deleta o elemento com chave minima
	public void deleteMin() {

		if (r == null)
			return;

		r = deleteMin(r);
	}

	private Node deleteMin(Node x) {

		if (x == null)
			return null;

		if (x.left == null)
			return x.right;

		x.left = deleteMin(x.left);
		return x;
	}

	// Deleta uma dada chave da arvore
	public void delete(Key key) {

		if (r == null)
			return null;

		r = delete(r, key);
	}

	private Node delete(Node x, Key key) {

		if (x == null) return null;

		int cmp = key.compareTo(x.key);

		if (cmp < 0)
		 	x.left = delete(x.left, key);

		else if (cmp > 0)
			x.right = delete(x.right, key);

		// Chego no no que quero deletar
		else {

			// X nao tem algum dos filhos
			if (x.right == null) return x.left;
			if (x.left == null) return x.right;

			// Caso ele tenha os dois filhos
			Node t = x;
			// Achando o menor valor a direita
			x = min(t.right);
			// Reatribuindo conexão
			x.right = deleteMin(t.right);
			// Subarvore esquerda antiga
			x.left = t.left;
		}

		// Retornando novo no
		return x;
	}
}

```

---

# Aula 09/04

## Arvores binarias de busca ótima
Quando voce consegue otimizar a relação entre profundidade de um no e a sua probabilidade de ser a resposta, ou seja, minimizar o numero de operações ate chegar naquele nó, levando em conta a probabilidade dele de ser escolhido.

Chamaremos `p[]` a probabilidade de uma chave ser buscada.   
Chamaremos `prf[]` a profundidade de um dado no.

queremos minimiazar a expressão:

`p[0](prf[0]+1) + p[1](prf[1]+1) + ... + p[n-1](prf[n-1]+1)`

### **Como achar a arvore otima?**

Usaremos a propriedade de que toda subarvore de uma arvore otima é otima.   
A ideia é criar um algoritmo de divide and conquer, que começa a montar a arvore de baixo pra cima, sempre deixando o no com maior probabilidade como raiz.

## Arvores 2-3 / B-tree

São arvores perfeitamente balanceadas. Ou seja, todos os links para NULL estão no mesmo nivel.

Nesta arvore podemos ter nos *normais* e nos duplos. Um no duplo possui as seguintes propriedades:
1. Ele tem dois números, x e y.
2. Sua subarvore esquerda tem numeros menores que X.
3. Sua subarvore direita tem numeros maiores que Y.
4. Existe uma subarvore central, que possui numeros entre x e y.

Fato: se h é a altura da arvore, entaõ (2^(h+1))-1 <= n <= ((3^(h+1))-1), onde n é o numero de nos.
Logo, h <= log2(n+1)

Operação de inserção: a ideia é sempre colocar o no embaixo, criando espaço para ele caber e manter a propriedade da arvore. Podemos fazer isso tranfromando um no normal para um no duplo ou *espatifando* um no duplo em uma *mini-arvore*, fazedno com que a raiz dessa subarvore tente entrar em algum lugar.

A implementação dessa arvore se apoia na implementação da arvore binarias.

## BST rubro-negra / Red-black tree

Uma BST rubro negra é uma BST com links que sao negros ou rubros.

- Links **rubros** são links esquerdos.
- Nenhum no é incidente a dois links rubros.
- balanceamento negro perfeito: todo caminho da raiz ate um link null tem o mesmo numero de links negros.

---

# Aula 11/04

## Continuação arvores 2-3 e Rubro-negras...

```java

// 'defines' em Java
private static final boolean RED = true;
private static final boolean BLACK = false;

private class Node {
	Key key;
	Value val;
	Node left, right;
	int n;
	boolean cor;

	Node(Key key, Value val){
		this.key = key;
		this.val = val;
	}
}

public Value get(Key key) {

	Node x = get(r. key);
	if (x == null)
		return null;

	return x.val;
}

// Implementação igual ao da arvore binaria		
private Node get(Node x, Key key) {

	if (x == null)
		return null;

	int cmp = key.compareTo(x.key);

	if (cmp < 0) return get(x.left, key);
	if (cmp > 0) return get(x.right, key);
	return x;
}

Private boolean isRed(Node x) {
	if (x == null) return false;
	return x.color == RED;
}

private void rotateLeft(Node h) {

	Node x = h.right;
	h.right = x.left;
	x.left = h;
	x.color = h.color;
	h.color = RED;
	x.n = h.n;
	h.n = 1 + size(h.left) + size(h.right);
	return x;
}

private void rotateRight(Node h) {

	Node x = h.left;
	h.left = x.right;
	x.right = h;
	x.color = h.color;
	h.color = RED;
	x.n = h.n;
	h.n = 1 + size(h.left) + size(h.right);
	return x;
}

private void flipColor(Node h) {
	h.color = !h.color;
	h.left.color = !h.left.color;
	h.right.color = !h.right.color;
}

private Node balance(Node h) {
	if (isRed(h.right) && !isRed(h.left))
		h = rotateLeft(h);

	if (isRed(h.left) && isRed(h.left.left))
		h = rotateRight(h);

	if (isRed(h.left) && isRed(h.right))
		flipColor(h);

	return h;
}

public void put(Key key, Value val) {
	r = put(r, key, val);
	r.color = BLACK;
}

private Node put(Node h, Key key, Value val) {

	if (h == null)
		return new Node(key, val, 1, RED);

	int cmp = Key.compareTo(h.key);

	if (cmp < 0)
		h.left = put(h.left, key, val);

	else if (cmp > 0)
		h.right = put(h.right, key, val);

	else 	
		h.val = val;

	h = balance(h);
	return h;
}

```

----

# Aula 23/04

## Hash Table

Aqui, temos uma implementação simples de uma tabela de simbolos:

```Java

public class DirectAddressST<Value> {

	private Value[] vals;

	public DirectAddressST(int n){
		vals = (Value[]) new Object(n);
	}

	public Value get(Key key){
		return vals[key];
	}

	public void put(Key key, Value val){
		vals[key] = val;
	}

	public void delete(Key key){
		vals[key] = null;
	}
}

```

Agora, queremos fazer uma hashtable generica que realiza as funções get(), put() e delete() tem tempo medio O(1).
Para isso, usamos uma função de hashing.

```Java

private int hash(String key){
	int h = 0;
	for (int i = 0; i < key.length(); i++)
		h = (31*h + key.charAt(i))%mod;
}
```

---

# Aula 25/04

# Separating chaining
Tecnica utilizada para evitar colisões, diminuindo/condansando a HT, em cada celula da HT temos uma symble table move to front.

## Fator de carga / Load factor

- **Alpha = n/m**

É a relação de elementos em cada posição da HT, ou seja, o numero de elemetos em cada ST. Precisamos definir um alpha que seja bom para nos, caso ele aumente mais do que queremos, aumentamos a HT.

Desse modo, na HT, temos que as operações possuiem complexidade media de O(alpha+1).

```java
public SepChainHashST<Key, Value> {
	int m; // Tamanho da tabela / posições
	int n; // Numero de chaves

	ST<Key, Value>[] st;

	public SepChainHashST() {

		this.(997);
	}

	public SepChainHashST(int m) {

		this.m = m;
		st = (ST[]) new ST[m];
		for (int i = 0; i < m; i++)
			st[i] = new ST<Key, Value>[]; // Tabela de simbolo em cada posicao
	}

	private int hash(Key key) {
		// O java tem um metodo hashCode em todo objeto, que devolve um inteiro entre [-2³¹, 2³²]
		return (key.hashCode()&0x7fffffff)%m; // Deixando o numero positivo e pegando o modulo
	}

	public Value get(Key key) {

		int h = hash(key);
		return st[h].get(key);
	}

	public void put(Key key, Value val) {

		if (n >= 10*m) resize(2*m); // Vendo o load factor, que neste caso é 10
		int h = hash(key);
		if (!st[h].contains(key)) n++;
		st[h].put(key, val);
	}

	private void resize(int C) {
		SepChainHashST<Key, Value> t;
		t = new SepChainHashST<Key, Value>(C);
		for (int i = 0; i < m; i++)
			for (Key key : st[i])
				t.put(key, st[i].get(key)); // Populando nova HT

		this.m = C;
		this.st = t.st;
	}
}

```

---

# Aula 30/04

## Hipotese de hashing uniforme

- Ideia: função de hashing deve parecer aleatoria, ou seja, a probabilidade de uma chave ser escolhida é de 1/m.

Na media, as listas da tabela de simbolo tem n/m chaves.

### Consumo de tempo

- Busca malsucedida, na media o consumo de tempo é O(1 + n/m)
- Busca bem sucessida, na meida, o consumo é o(1 + n/m)

Como n/m == alpha, temos uma complexidade constante.

## Open adressing (OA)

Ideia de inserir N chaves em uma HT de tamanho M > N, confiando em espaços vazios na HT para evitar colisões.

### Linear probing
Maneira mais simples de realizar OA. Quando existe uma colisão eu simplesmente procuro a proxima posição vazia na HS. Aqui, temos as seguintes possibilidades:

- Chave igual a chave de procura: search hit
- Posição vazia, null na posição index - search miss
- Chave diferente da chave procurada - tento proxima entrada

Dessa maneira, fico pesquisando ate que alguma das duas primeiras possibilidades aconteça.

```Java

public class linHashST<Key, Value> {
	int n;
	int m;
	Key[] keys;
	Value[] vals;

	public linHashST() {
		this.(100); // Capacidade inicial
	}

	public linHashST(int cap) {
		m = cap;
		keys = (Key[]) new Object[m];
		vals = (Value[]) new Object[m];
	}

	public Value get(Key ket) {
		for (int h = hash(key); key[h] != null; h = (h+1)%m)
			if (keys[h].equals(key))
				return val[h];
		return null;
	}

	public void put(Key key, Val val) {

		// Asserting for open adressing
		if (n >= m/2)
			resize(2*m);

		int h = hash(key);
		// Checkignif already is in HT
		for (; keys[h] != null; h = (h+1)%m)
			if (keys[h].equals[key]) {
				vals[h] = val;
				return;
			}

		// Assigning new pos
		this.keys[h] = key;
		this.vals[h] = val;
		this.n++;
	}

	private void resize(int cap) {

		linHashST<Key, Value> t;
		t = new linHashST(cap);

		for (int h = 0; h < m; h++)
			if (keys[h] != null)
				t.put(keys[h], vals[h]);

		this.keys = t.keys;
		this.vals = t.vals;
		this.m = t.m;
	}

	public void delete(Key key) {

		if (!contais(key))
			return;

		int h = hash(key);
		// Finding boi
		while (!key.equal(key[h]))
			h = (h+1)%m;

		// Deleting value
		keys[h] = null;
		vals[h] = null;

		// Aqui, definimos a politica de realizar o rehash() de todo mundo
		// que vem depois dele, todo mundo no cluster dele.
		h = (h+1)%m;
		while (keys[h] != null) {
			keysT = keys[h];
			valsT = vals[h];
			keys[h] = null;
			vals[h] = null;
			n--;
			put(keysT, valsT);
			h = (h+1)%m;
		}
	}
}

```

#### complexidade

- Busca bem-sucedida: O(1/2(1 + ( 1/(1-alpha) )))
- Busca mak-sucedida: O(1/2(1 + ( 1/(1-alpha)^2 )))

### Quadratic probing

### Double hashing

---

# Aula 02/05

## Tries

Tabela de simbolos especilizada em chaves que são estrings, as chaves ficam armazenadas como caminhos em uma arvore, fazendo um nó auxiliar para armazenar a informação desejada.

Definimos que um alfabeto tem **R** caracteres.

```Java

class True<Value> {

	Node r; // Root
	int n; // Number of keys

	class Node {
		Value val;
		Node[] next = new Node[R];
	}

	public Value get (String key) {
		Node x = get(r, key, 0);
		if (x == null) return null;
		return x.val;
	}

	private Node get (Node x, String key, int i) {
		if (x == null) return null;
		if (i == key.lenght()) return x;
		char c = key.charAt(i);
		return get(x.next[c]), key, i+1);
	}

	public void put (String key, Value val) {
		r = put(r, key, val, 0);
	}

	private void put (Node x, String key, Value val, int i) {
		if (x == null)
			x = new Node();

		if (i == key,length()) {
			if (x.val == null) n++;
			v.val = val;
			return;
		}

		char c = key.charAt(i);
		x.next[c] = put(x.next[c], key, val, i+1);
		return x;
	}

	public void delete (String key) {
		r = delete(r, key, 0);
	}

	private void delete (Node x, String key, int i) {
		if (x == null) return null;
		if (i == key.length()) x.val = null;
		else {
			char c = key.charAt(i);
			x.next[c] = delete(x.next[c], key, i+1);
		}

		// Cleaning
		if (x.val != null) return x;
		for (char c = 0; c < r; c++)
			if (x.next[c] != null)
				return x;

		return null;
	}

	public Iterable<String> keysWithPrefix(String pre) {
		Queue<String> q = new Queue<String> ();

		Node x = get(r, pre, 0);
		collect(x, pre, q);
		return q;
	}

	private void colect (Node x, String pre, Queue<String> q) {
		if (x == null) return;
		if (x.val != null)
			q.enqueue(pre);

		for (char c = 0; c < r; c++)
			collect (x.next[c], pre+c, 1);
	}

	public Iterable<String> Keys() {
		return keysWithPrefix("");
	}

	public String longestPrefixOf(String s) {
		int max = -1;
		Node x = r;

		for (int d = 0; x != null; d++) {
			if (x.val != null) max = d;
			if (d == s.length()) break;
			x = x.next[s.charAt(d)];
		}

		if (max == -1)
			return null;

		return s.substring(0, max);
	}

	public Iterable<String> keyThatMatch (String pat) {
		Queue<String> q = new Queue<String>;
		collect(r, "", pat, q);
		return q;
	}

	private void collect (Node x, String pre, String pat, Queue<> q) {
		if (x == null) return;
		if (pre.length() == pat.length() && x.val != null)
		 	q.enque(pre);
		if (pre.length() == pat.length())
			return;

		char c_next = pat.charAt(pre.length());
		for (int c = 0; c < r; c++)
			if (c_next != '.' || c_next == c)
				collect(x.next[c], pre+c, pat, q);

	}
}

```

---

# Aula 07/05

## Data compression

Reprensenta um arquivo grande em um arquivo pequeno, de maneira lossless.

**Taxa de compressao**: (#Bits apos compressao)/(#Bits antes da compressao)

**Fato**: nenhum algoritmo pode garantir taza de compressão estritamente menor que 1 para todo e qualquer fluxo de bits.

Vamos escrever um exemplo que comprime os dados de para um sequencia de DNA

```Java
public static void copress() {

	Alphabet DNA = new Alphabet("ACGT"); // Like a hash table

	int w = DNA.lgR();
	String s = BinaryStdIn.readString();
	int n = s.length();

	BinaryStdOut.write(n);

	for (int i = 0; i < n; i++) {
		int d = DNA.toIndex(s.charAt(i));
		BinaryStdOut.write(d, w);
	}

	BinaryStdOut.close();
}
```

---

# Aula 09/05

## Algoritmo de Huffman

Transform bytes (8 bits) em codigos. Chamaremos de R o numero de bytes diferentes, tranformando cada byte em um codigo menor, que utiliza menos bits.

Para isso, usamos uma tabela de simbolos, criando uma especie de dicionario para os codigos.

**Ideia do algoritimo**: Usar codigos curtos (com menos bits) para os caracteres (bytes) que aparecem com mais frequencia. Deixando o codigo mais longo para os bytes mais raros.

Precisamos tambem, construir uma tabela st[], que armazena a conversão dos bytes em codigos. Tal tabela **precisa ser livre de prefixos** para que a decodificação seja unica para cada cadeia codificada. A tabela inversa, que armazena a conversão dos codigos em bytes, vai ser um trie bianria.

**Proposição**: O algoritmo de Huffman é o mais FODA dentre todos os algoritimos com tabelas libres de prefixos.


```java


// Nó para a Trie
Node implements Comparable<Node> {
	char ch;
	int freq;
	Node left, right;

	Node (char ch, int freq, Node left, Node right) {
		this.ch = ch;
		this.freq = freq;
		this.left = left;
		this.right = right;
	}

	boolean isLeaf() {
		return (this.left == null && this.right == null);
	}

	int compareTo (Node that) {
		return this.freq - that.freq;
	}
}

// Construindo trie (bits (codigo) -> byte (caractere))
Node buildTrie(int[] freq) {
	MinPQ<Node> pq new MinPQ<Node>();

	// Construindo folhas da trie
	for (char c= 0; c < r; c++)
		if (freq[c] > 0)
			pq.insert(new Node(c, freq[c], null, null) );

	// Agregando folhas
	while (pq.size() > 1) {
		Node x = pq.delMin();
		Node y = pq.delMin();

		Node parent = new Node ('\0', x.freq+y.freq, x, y);
		pq.insert(parent);
	}

	return pq.delMin();
}

// Constroi st[] (Byte (caractere) -> bits (codigo))
String[] buildCode (Node root) {
	String st[] = new String[r];
	buildCode(st, root, "");
	return st;
}

void buildCode(String[] st, Node x, Strins s) {

	if (x.isLeaf) {
		st[x.ch] = s;
		return;
	}

	buildCode(st, x.left, s+0);
	buildCode(st, x.right, s+1);
}

// Printando trie na saida
void writeTrie(Node x) {
	if (x.isLeaf()) {
		BSO.write(true);
		BSO.write(x.ch);
		return;
	}

	BSO.write(false);
	writeTrie(x.left);
	writeTrie(x.right);
}

Node readTrie() {

	if (BSI.readBoolean()) {
		char c = BSI.readChar();
		return new Node (c, 0, null, null);
	}

	return new Node ('\0', 0, readTrie(), readTrie());
}

void compress() {

	String s = BinaryStdIn.readString(); // Binario (01010101101010)
	char input = s.toCharArray(); // Caracteres - ASCI extendido (A,B,*,!,..)

	int[] freq = new int[r];
	for (int i = 0; i < input; i++)
		freq[input[i]]++;

	Node root = buildTrie(freq); // Criando a trie
	String[] st = buildCode(root); // Criando ST
	writeTrie(root); // Escrevendo a trie na saida

	BinaryStdOut.write(input.length);

	// Percorrendo todos os bytes
	for (int i = 0; i < input.lenght; i++) {

		String code = st[input[i]]; // Posição correspondente ao caractere

		// Escrevendo o codigo na saida
		for (int j = 0; j < code.lenth(); j++) {

			if (code.charAt(j) == '0')
				BinaryStdOut.write(false);
			else
				BinaryStdOut.write(true);
		}
	}
}

void expand () {

	Node root = readTrie();
	int n = BinaryStdIn.readInt(); // Quantidade de codigos comprimidos

	for (int i = 0; i < n; i++) {

		Node x = root; // Inicializando com raiz da trie

		// Enquanto estou lendo codigo novo e ele nao acabou
		while (!x.isLeaf) {
			if (BinaryStdIn.readBoolean())
				x = x.right;
			else
				x = x.left;
		}

		// Escrevendo o byte correspondente ao codigo lido
		BinaryStdOut.write(x.ch);
	}

	BinaryStdOut.close();
}

```
---

# Aula 16/05

## Algoritmo de Lempel, Zif & Welch

LZW é um metodo de dicionarios, onde o codigo tem tamanho fixo e strings sao de tamanho variado.

```Java

public static void compress() {

	string input = BinaryStdIn.readString();
	TST<Integer> st = new TST<Integer>();
	for (int i = 0; i < R; i++) 	
		st.put("" + (char)i, i);

	int code = R+1;

	while (input.length() > 0) {
		string s = st.longestPrefixOf(input);
	}

}

```

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
