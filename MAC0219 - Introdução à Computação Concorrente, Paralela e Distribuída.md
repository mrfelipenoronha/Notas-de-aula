# Bibliografia usada

- Ben-Ari - Principles of Concurrent and Distributed Programming

- Andrews - Foundations of Multithreaded, Parallel, and Distributed Programming

---

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

---

# 02/04

## Locks e Barreiras (Cap 3)

**Problema da seção critica**: Nesse problema `n` processos executam repetidamente uma seção critica e, após, uma seção nao critica.

 - Protocolo de entrada: faz o codigo entrar na seção critica
 - Protocolo de saida: faz o codigo sair da seção critica

```
process CD [i = 1 to n]
    while (true)
        protocolo_de_entrada

        seção_critica()

        protocolo_de_saida

        seção_normal()
```

Suposição: um processo que entra em uma seção critica sai dela. Ou seja, o processo passa pelo protocolo de entrada, executa, sai pelo protocolo de saida e termina a execução.   

### Propriedades a serem respeitadas

 - Safety:

    1. Exclusão mutua - Garante que so haja um processo em dado momento.
    2. Ausencia de deadlocks - Se dois processos tentam entrar, ao menos um vai conseguir.
    3. Ausencia de atraso desnecessario - Se um processo esta tentando entrar um uma sessao critica e os outros estao em uma sessão nao critica, ele não é impedido de entrar.

 - Liveness

    1. Entrada garantida - Um processo que esta aguardando a entrada na SC, este irá entrar em algum momento.

Vamos começar com 2 processos de exemplo:

**1ª:**
```
int turn = 1

process CS1
    while (true)
        < await (turn == 1) >

        sessao_critica

        turn = 2

        Sessao_nao_critica()

process CS2
    while (true)
        < await (turn == 2) >

        sessao_critica

        turn = 1

        Sessao_nao_critica()
```
- [x] Exclusão mutua (Por causa do wait)
- [x] Ausencia de deadlock (Por causa do await)
- [ ] ATRASO - Problema causado por causa da sessão nao critica, pois esta pode demorar e fazer com que o outro processo tenha que esperar, sendo que ele poderia ser executado. Logo, esse codigo é perfeito para executar processos alternardos, cada um vai rodar uma vez, e depois o outro, e assim por diante, mas eles nao vao rodar em paralelo. Tal fato se deve á sessai Sessao_nao_critica, que pode ter a execução demorada e atrasar o reasign da variavel turn.

**2º:**
```
boolean in1 = in2 = false

process CS1
    while (true)
        < await (!in2) in1 = true>

        sessao_critica

        in1 = false

        Sessao_nao_critica()

process CS2
    while (true)
        < await (!in2) in2 = true>

        sessao_critica

        in2 = false

        Sessao_nao_critica()
```
- [x] Exclusão mutua - A função wait cuida disso, fazendo funcionar
- [x] Ausencia de DeadLocks - Manejado pela clausa de atribuição do await
- [x] Atraso - Agora o problema foi corrigido, pois logo apos um processo terminar a sessão critica ele possibilita que o outro seja iniciado, atravez do booleano
- [x] Entrada garantida - Sim, com justiça forte

### Como fazer para não usar os awaits

1. Primitiva de hardware
2. Algoritmos + tops

## Instruções do tipo test-and-set

```
boolean TS (boolean lock)
    < boolean inicial = lock
      lock = true
      return inicial >
```

Tal implementação é garantida de ser atomica em nivel de hardware.    
Podemos substituir o <await...> por `while(TS(lock)) sleep`. Porem, tal medida pode sobrecarregar o cache. A sução é fazer:
```
while (lock) sleep
while (TS(dlock))    
    while (lock)
        sleep
```

## Como implementar o await

A ideia para um await `< await(B) S>` é:

```
CSenter # Protocoloto de entrada

    while (!B)
        CSexit # Deixar algum outro processo funcionar
        sleep # Esperar
        CSenter # Fazer a entrada novamente

    S # Cheguei na condição e quero executar a instrução

CSexit # Protocolo de saida
```
---

# Aula 09/04

Algoritmos de sessão critica um pouco mais sofisticados

## Picket Algorithm
Ideia: cada processo pega um ticket para saber sua vez de usar a sessão critica.

Para isso, preciso de variveis compartilhadas:
1.  `number` - Usado para atribuir tickets aos processos, começa com 1.
2.  `next` - Qual é o processo que tem q ser executado agota, começa com 1.
3.  `turn[]` - Inicializado com zeros, fala qual o *ticket* do meu processo

Para entrar na sessão critica o processo CSi faz primeiro `turn[i] = number; number++`.
O processo CSi então espera ate que next seja igual ao seu numero para entrar na SC ao sair incrementa o next.

```
int number = 1, next = 1, turn[1:n] = ([n]0)
process CS [i = 1 to n]
    while (true)
        < turn[i] = number, number++ >  // Fetch and add
        while (turn[i] != next) sleep
        sessão_critica()
        next++
        sessão_não_critica()
```

### Instrução FETCH-AND-ADD
Intrução implementada a nivel de hardware que realiza a operação de forma atomica.

```
FA (var, icr)
    <int tmp = var, var += icr, return tmp>
```

Logo, podemos substituir `< turn[i] = number, number++ >` para `turn[i] = FA(number, 1)`

## The Bakery Algorithm

**Objetivo:** aprensetar um algoritmo que não precisa de instruções especiais.    
**Ideia:** Os processos verificam entre si que é o proximo.

```
int turn[1:n] = ([n]0)  // Inicializando com 0
process CS[i=1 to n]
    while ()
        < turn[i] = max(turn[1:n]) + 1 >

        for [j = 1 to n such that j != i]

            // Enquanto existirem processos que querem usar a sessão critica que
            // Chegaram antes do meu
            while (turn[j] == 0 or turn[i] < turn[j])
                sleep

        sessão_critica()

        turn[i] = 0     // Falando que não quero mais usar sessão_critica

        sessão_não_critica()
```

---

# Aula 11/04

## OpenMP

O OpenMP é uma especificação que vai implementar o multthreading. Vai usar certas ferramentas de acordo com o copilador e o SO. Por exemplo, ao usar OpenMP copilado no GCC e rodando no linux, o pragma vai fazer uma implementação do PThreads muito mais otimizada.

Procurar **OpenMP cheatsheet** para ver todas as coisas da especificação.

- `#pragma omp parallel for`: uma anotação que reliza um tipo de metaprogramação no codigo.
- Com `num_threads(2)` a gente pode especificar o numero de threads que vamos usar.
- `#pragma omp critical(NOME_DA_SC) {}`: reliza uma sessão critica com o codigo entre parenteses.
- Para compilar usando o OpenMP, é preciso usar a flag `-fopenmp`.
- Quando eu executo meu programa com mais threads doq meu computador possui isso é chamado 'green-threads', que são uma especie de pseudothreads.
- Uma operação de redução é quando reunimos o resultado de operações de varias threads em torno de um resultado unico e final. No caso do codigo abaixo, o `acc += ` não vai adicionar diretamente na variavel acc definida anteriomente, e sim e variaveis criadas pelo openmp que vão ser depois unidas, em uma forma de divide and conquer.

```
double acc = 0.0;

#pragma omp parallel for private(i) reduction(+:acc)
for (i = 0; i < n; i++){
    double x = (i*2)/2;
    acc += sqrt(1 - (x*x)) * i;
}

return acc;
```

No codigo acima, o openmp dividiu o trabalho em partes iguais, e não em partes dinamicas. Ou seja, um processo pode terminar antes de outros, dividindo de forma dinamica, a divisão de trabalho não é feita de forma igual e os trabalhos podem ter diferentes tamanhos.

---

# Aula 23/04

## CUDA

Uma GPU é uma unidade de processamento com um numero muito grande de threads, porem, tem um clock mais lento do que uma CPU normal. Logo, uma GPU se torna ideal para o processamento paralelo, pois ela pode realizar milhões de calculos em paralelo.

**CUDA** é uma extensão de C/C++, com APIs e extensões especificas para o processamento usando placas da nvidia.     
Por definição, quando fazendo coisas em CUDA, chamamos a CPU de host e a gpu de device.

A ideia é que um codigo seja executado tanto na CPU quanto na GPU. Com a parte sequencial no host e a parte paralela no device. Na seguinte ordem:
- Copiar os dados da memoria da cpu para a memoria da gpu
- Executar o codigo copiado na gpu
- Copiar os dados da memoria da gpu para a memoria da cpu

`nvcc` é o copilador usado, com a extensão `.cu`.

Existem identificadores para mostrar onde as coisas vão ser executadas. São eles `__global__`, `__host__` e `__device__`. Onde a `__global__` diz que tal função vai ser chamada do host e executada no device.

> Olhar slides da aula de cuda dos monitores
