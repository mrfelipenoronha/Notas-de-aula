# Aula 12/03

- Interrupção: quando o processador coloca um processo como prioritario, pausando a execução de outros. Elas são extremamente rapidas.

- ISA: especie de API publica, que tanto RISC quanto CISC usam para se comunicar com o processador.x

---

# Aula 14/03

## PThreads intro

### Threads vs processo
Thread é o fluxo de execução de um processo. Um processo pode ter varias threads.

### Concorrencia vs Paralelismo
Concorrencia é lidar com varias coisas ao mesmo tempo, varios processos ao mesmo tempo. Paralelismo é fazer varias coisas ao mesmo tempo

### Operação atomica
Operação que acontece na CPU, que não pode ser iterrompida. Ela tem que entrar so uma vez na CPU para acontecer, não fazendo uso de muitos clocks.

### Taxinoma de Flynn
- SISD: Single instruction single data. Padrão, cada processo altera um unico dado;
- SIMD: Single instruction multiple data. Carregar multiplos dados para fazer uma operação, mesma instrução aplicada a varios dados;
- MISD: Multiple instruction single data. Operações para um unico dado, varias instruções aplicadas á um unico valor.
- MIMD: Multiple instruction multiple data.

### Race conditions
As threads podem ocorrer ao mesmo tempo ou em tempo diferentes. Com isso, podemos ter resultados que variam, depedendo da ordem da execução dos processos. Por exemplo, se você tem duas threads fazendo um calculo em uma variavel, elas podem pegar o valor da variavel com o resultado antigo, ou com resultados não sincronizados.

### Sessão critica
Trecho de codigo que deve ser executado inteiramente por uma thread, antes que qualquer coisa outra thread o comece a executar. É uma maneira de impedir esse erro da race condition. Para isso, usamos semafaros.

### Semaforos
Controle de acesso a um recurso. Conta o numero de processos que podem usar um recurso ao mesmo tempo sem que nenhum outro deixe de usar.

 - **MUTEX**: Semaforos que so permitem valores 0 e 1. So permite o acesso a uma thread por vez, ou seja, uma tarefa que roda num semafaro vai ser a unica rodando naquele momento, é meio que uma forma de deixar só uma parte da tarefa sequencial.

### Variaveis de condição
Faz com que uma thread espere ate que uma condição seja satisfeita.

### Barreiras
Quando uma thread entra em uma barreira ela não pode terminar ate que todas as outras threads tenham entrado. Uteis para sincronização.

### Dependencia de dados
So se pode paralelizar tarefas independetes.

### DeadLocks
Multiplos processos/threads bloqueados, esperando um pelo outro. Nenhum consegue terminar pois necessita dos dados dos outros.

## POSIX Threads
Para facilitar as coisas, o IEEE definiu um padrão para o uso de threads no UNIX, o PThreads. O PThreads tem a seguinte API:

| Chamada | Descrição |
| --- | --- |
|Pthread_create|Cria uma nova thread|
|Pthread_exit|Acaba a execução de uma thread|
|Pthread_join|Espera ate que uma thread acabe|
|Pthread_yieid|Libera a CPU para outra thread|
|Pthread_attr_init|Cria e inicializa atributos de uma thread|
|Pthread_attr_destroy|Remove os atributos de uma thread|

Uma thread vai ser inicilizada como um ponteiro do tipo `pthread_t`

---

# Aula 26/03

## Atômicidade - Programação com variaveis compartilhadas

> aula baseada no capitulo 2 do livro de Ben-Ari e Andrews

**Definição**: Um programa concorrente consiste em um numero finito de processos.
Cada processo é escrito usando um conjunto finito de instruções atômicas.

**Definição**: O estado de um programa é o conteúdo de suas variaveis em um dado momento.

**Varivel explicitas**: Variaveis declaradas no programa, faceis de serem vistas.

**Variaveis implicitas**: Registradores e pontos de controle (PC, IC).

Um **processo** executa um sequencia de instruções. Ou seja, processo é algo sequencial. Alem disso, um processo possui uma parte da memoria.   
Uma **instrução** é uma sequencia de uma ou mais instruções atomicas.   
**Instrução atomica** é uma ação indivisivel que examinam ou alteram o estado do programa.


A execução de um programa concorrente consiste de sequencias de instruções atomicas intercaladas. Ou seja, quando aplicações são executadas de forma paralelas eu posso definir/enxergar uma sequencia na qual as intruções atomicas foram executadas.

**História** é uma sequencia de execução das ações atomicas.

**Papel da sincronização => restringir o conjunto das historias possiveis a um conjunto desejavel.** Ou seja, cada processo roda de forma independete, para que eu não tenha historias desejadas eu preciso de algum mecanismo para sincronizar as execuções.

Exclusão mutua => Criar sessões que parecem atomicas.

Condição de sincronização => atrasar uma ação ate que seja valida uma condição.


**Definição**: O estado de um algoritmo concorrente corrresponde aos estados das variveis e os respectivos ponteiros de controle dos processos.

**Definição**: Dados dois estados s1 e s2, existe uma transição entre s1 e s2 se alguma das intruções apontadas pelos ponteiros de controle muda de s1 para s2.

Um **diagrama de estados** é uma representação de todos os possiveis estados da aplicação, a cada passo.

## Mini-EP
Encontre o máximo de um vetor com inteiros positivos. Tranforme o codigo sequencial para um paralelo.
```
int m = 0
for [i = 0 to n-1]
    if (a[i] > m)
        m = a[i]
```
1. Transformar para um `for` atomico, que faz tudo de uma vez
2. Transformar o if e a atribuição em uma estrutura atomica (sessao critica <>), porem isso faz com que todo mundo entra na sessão critica
3. Criar um novo if para reverificar, almentando a eficação da sessão critica.

```
int m = 0
forall [i = 0 to n-1]   
    if (a[i] > m)
        <if (a[i] > m)
            m = a[i]>
```

A tarefa do mini-ep é descobrir qual a quantidade ótima de if's a serem colocados. Sendo que o ultimo if é atomico (esta na sessão critica). A ideia é minimizar o numero de acessos a sessão critica.

---

# Aula 28/03

## Ações atomicas e comando await

Duas formas de obtermos "ordem" corretas:
- Blocos atomicos - **exclusão mutua**
- Atraso de edxecução ate que algo seja valido - **condição de sincronização**

## Referencia critica

**Definição**: Referencia para uma variavel modifica por outro processo concorrente.

Se estes dois processos concorrentes não há referencia criticas a execução parece atomica.

## Propriedade 'no maximo uma vez'

Uma atribuição `x = e` satisfaz a propriedade se uma das seguintes opções forem satisfeitas:
1. `e` contem no maximo uma referencia critica, e `x` não é lido por outro processo;
2. `e` não contem referencias criticas, e nesse caso `x` pode ser lido.

**Motivação**: ter execução que pareçam atomicas com apenas uma referencia não é possivel saber quando ocorre a atualização.

**Primitiva de sincronização**:  <await(B) S>
 - Enquanto `B` não for verdade, a execução espera - condição de atraso;
 - Se `B` for verdadeiro, `S` é executado - sequencia de comandos.

Note que `S` vai ser uma expressão atomica

## Exemplos
1. < await (s > 0) s = s-1>
2. < await (coount > 0)>


**Importante**: Se a condição B respeita a propriedade no maximo uma vez, `<await(B)` é equivalente á `while not B`.

## Produtor / consumidor

Variaveis `p` e `c` que controlam os itens produzidos e consumos.

Sincronização ->  c <= p <= c+1

```
int buff p = 0, c = 0;

process Producer {

    int a[n];

    while (p < n){

        < await (p == c) >
        buff = a[p];
        p++;
    }
}

process Consumer {

    int b[n];

    while (c < n) {

        < await (p > c) > // Isso pode ser trocado por um while
        b[c] = buff;
        c++;
    }    
}

// Neste caso, os valores do vetor a[] vão ser tranferidos para o vetor b[] atravez do buff.
// As clausulas de <await> garante que essa tranferencia vai ser feita de forma correta.
```

## Propriedades de Safety / Liveness

- **Safety**: não acontece nada de errado durante.
- **Liveness**: terminaçao chega ao estado final.

Se eu tenho um programa sequencia, temos as seguintes definições:
1. Safety == estado final correto do programa.
2. Liveness == Programa chega ao estado final.  

Se eu tenho um programa em paralelo, as definições são as seguintes:
1. Safety == Exclusão mutua e ausencia de deadlocks.
2. Liveness == Um processo entrara em algum momento na seção critica, alem de uma mensagem chegar ao seu destino em algum momento.

## Justiça (Fairness)
Garantia de que todos os procesos podem prosseguir.

- **Justiça incondicional**: toda ação atomica passiva de execução é executada em algum momento.

```
boolean continue = true, try = false;

// Uma thread
concorrente {
    while (continue){
        try = true;
        try = false;
    }
}

// Outra thread
< await (try) continue = false >

```

- **Justiça fraca**: Cada ação condicionamal atomica é executada em algum momento se a condição fica e *permanece* verdadeira.

- **Justiça forte**: É incondicional. Toda ação condicional atomica *executavel* é executada em algum momento, assumindo que a condição é frequentemente verdadeira. Porem, JUSTIÇA FORTE É IMPRATICAVEL.



> Complementar com explicações do livro pg (PEGAR PG E LIVRO
