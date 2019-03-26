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
