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
			id[i] = 1;
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

# Aula 14/03

## Hash Table (HT)

Uma tabela de simbolos é uma ADT(Abstract data type) que consiste em um conjunto de itens, sendo cada item um par `Key - value`, munido das operações:
- put(): insere um item na HT;
- get(): busca o valor associado a chave.   

Convenções sobre HTs:
- não há chaves repetidas;
- `null` nunca é usado como chave;
- `null` nunca é usado como valor.

```java
public class ArrayST<Key, Value> {
    private int CAP = 8;
    private Key[] keys;
    private Value[] values;
    private int n;

    public ArrayST(){
        this.(CAP);
    }

    public ArrayST(int CAP){
        keys = (Key[]) new Object[cap];
        values = (Value[]) new Object[cap];

    }

    public Value get(Key key){
        int i = rank(key);
        if (i < n && key.equals(keys[i]))
            return vals[i];
        return null
    }


}
```
