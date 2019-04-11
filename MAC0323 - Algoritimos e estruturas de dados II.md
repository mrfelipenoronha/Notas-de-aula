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
