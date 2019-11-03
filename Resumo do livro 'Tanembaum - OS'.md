# Resumo  do livro OPERATING SYSTEMS - DESIGN AND IMPLEMENTATION

Famoso livro do MINIX

# Conceitos de um sistema operacional

Aqui é feita uma breve introdução.

A ponte entre o sistema operacional e os programas de usurario é feita por um conjunto de *instruções extendidas*, conhecidas como **chamadas de sistema**. As chamadas de sistema apresentadas aqui usam como base as chamadas **POSIX**.

## Processos

Um processo é bsicamente um programa em execução, associado á um **adress space**, um posição na memoria de onde o programa pode ler e escrever. Além disso, temos alguns registradores associados, como o PC, ST e outros registradores de hardware.

Um olhar interessante para um processo é do ponto de vista de multiprogramação. De tempos em tempos a CPU decide parar de executar um processo e começar a executar outro. Quando um processo é suspenso dessa maneira, todas as informações acerca de sua execução devem ser salvas para que o processo possa voltar a ser executado depois, a partir de onde ele parou. Essas informações são guardadas na **process table**.

As chamadas mais importantes do gerenciador de processos (PM) são as que criam e destroem outros.

Toda pessoa/usuario tem um **UID** (user identification). Todo processo guarda o ID de quem o criou. Usuarios podem ser membros de grupos, cada grupo tem um **GID**. Um UID, chamado de **superuser** tem superpoderes.

## Arquivos

Antes de um arquivo ser lido ele deve ser aberto, e apos isso ele deve ser fechado. Para essas operaçẽs, temos chamadas de sistema encarregadas.

Um **diretorio** é uma maneira de agrupar arquivos. Dentro de um diretorio podemos ter arquivos e outros diretorios. Com isso, criamos uma hierarquia, o sistema de arquivos.

Todo arquivo pode ser acessado a partir de um **path name**, que é um caminhos a partir do **root directory**, tambem chamado de **absolute path**.

Podemos tambem definir um **working directory**, um diretorio em que um processo esta sendo executado, e, que, todo arquivos tera um **relative path** que tem tal diretorio como origem.

A proteção de um arquivo é feito por um codigo de proteção composto por 11 bits. Esse codigo é composto por **3-bits fields**: um para o dono, um para os membros do grupo do dono e outro para o resto, e 2 bits que serão discutidos depois. Esses 3 bits são conhecidos como **rwx bits** (read, write and execute permissions).

Esse conjunto de permissões é checado quando pedimos para abrir um arquivo. Caso acesso seja concedido, o SO nos retorna um **file descriptor**.

Temos tambem **special files**, que fazem com que I/O devices pareçam arquivos. Dois tipos existem:

- Block special files: Usado para devices que são uma coleção de blocos, tais como um disco. Assim, um programa pode acessar um dado bloco do dispositivo.

- Character special files: São usados para dispositivos que fazem a sua entrada e saida na forma de **stream**, como modens e mouses.

Os arquivos especiais são mantidos em `/dev`.

Por ultimo, temos um **pipe**, que é meio que um *pseudoarquivo* usado para conectar dois processos.

## A shell

É o interpretador de comandos do sistema operacional.


# Estrutura de sistemas operacionais

Vamos olhar o sistema operacional um pouco mais internamente agora.

## Sistemas monoliticos (A grande bagunça)

A estrutura aqui é justamente não ter estrutura. O sistema é escrito como uma colção de procedimentos, que se invocam sem nenhuma forma de controle. Para se construir um sistema dessa maneira primeiro se compila todos os procedimentos, depois, todos eles são ligados para forma o OS. Em termos de esconder informação, esse conceito meio que não existe, cada procedimento tem acesso á todo outro procedimento.

Todavia, nesse tipo de sistema, as chamadas de sistema são executadas se colocado parametros em locais bem definidos, como na pilha ou em registradores, e então executando uma *trap* especial chamada de **kernell call** ou **supervisor call**.

Essa operação tira o sistema de **modo usuario** e o coloca em controle do SO.

Podemos definir uma estrutura basica como sendo:

1. Um programa principal que invoca chamadas de sistema
2. Um conjunto de chamadas de sistema
3. Um conjunto de utilitarios que auxiliam as chamadas.

Muitas vezes chamamos de **serviços** os procedimentos que *entregam* as system calls.

## Sistemas em camadas

A generalização da estrutura anterior é organizr o sistema em camadas, cada uma sendo contruida em cima da anterior. Dijkstra fez o primeiro sistema utilizando essa ideia, com a seguinte estrutura:

Camada | Função
:---   | :---
5 | O operador
4 | Programas do usuario
3 | Administração de I/O
2 | Process comunication
1 | Administração de memoria
0 | Alocação de processos e multiprogramação

## Maquinas virtuais

Surgiu como opção para se ter um sistema com **multiusuarios** e **multiprogramação**. O coração do sistema é o **virtual machine monitor**, que é executada no hardware e realiza a multiprogramçaõ, provendo varias maquinas virtuais. Tais maquinas eram copias exatadas do hardware em que o VMM estava sendo executado (aprensentando kernel/user mode, I/O, interrupções, etc), ou seja, um prgrama que esta sendo executado nela parece que esta sendo executado diratemente na maquina, e não em cima de outro serviço (VMM). Com isso, cada VM podia rodar um SO diferente.

A solução de usar VM's se mostra muito util hoje em dia para a execução de software legado em hardwares novos, ou seja, programas que foram escritos para serem executados em hardware antigos e não apresentam compartiilidade com os novos.

## Modelo cliente-servidor

Uma tendencia nos SO modernos é mover o maximo de coisas possiveis para camadas mais externas ao SO, deixando um **minimal kernel**. A ideia é implementar a maioria das funções do SO no espaço de usuario. Para requisitar um serviço, tai como ler um arquivo, um processo de usuario (agora lido como **client process**) manda uma requisição para um **server process**, que realiza o trabalho e manda de volta a resposta.

Com isso, o kernel faz, basicamente, a comunicação entre os clientes e servidores. Dividindo o SO entre serviços (tais como *file service*, *process service*, *terminal service* ou *memory service*), fazemos com que eles sejam executados em espaço de usuario, e que, para acessarem diratemente o hardware, precisam pedir autorização ao kernel. Isso faz com que o sistema tenha mais robustez e maior tolerancia a falhas.

---

# Processos

## Introdução

Os computadores modernos podem fazer diversas coisas ao mesmo tempo, todavia, em um dado instante de tempo, a CPU estara rodando somente 1 programa, porém, ao decorrer de varios segundos, ela tera exeutado varios programas. Esse efeito, criado pelo SO, é o que alguns chamam de **pseudoparallelism**, que contrasta com o verdadeiro paralelismo implementado em um sistema com **multiprocessing**.

### O modelo do processo

Todos os softwares executaveis no computador, incluindo o proprio SO, são organizados em uma sequencia de processos. Um processo pode ser **definido como** um programa em execução, junto com seus contadores, registradores e variaveis. No ponto de vista do processo, cada um tem sua propria CPU, que o executa de maneira initerrupta. Do ponto de vista pratico, a CPU muda, de tempos em tempos, o processo que esta sendo executado por ela, á isso, damos o nome de **multiprogramação**.

De maneira geral, não ligamos para essa **mudança de contexto** que ocorre na CPU. Entretando, em procedimentos especificos, como a leitura de um segmento em disco, medidas podem ser tomadas para que o SO não tire aquele processo de execução.

### Criação de processos

SO's precisam de alguma maneira de ter certeza que todos seus processos essenciais existam. Em sistemas simples, é possivel fazer com que todos os processos que serão utilizados estejam prontos quando o sistema inicializar. Entretando, em sistemas de proposito geral, isso não é pratico, pois é necessario que processos sejam criados e finalizados durante o tempo em que os sistema estiver de pé.

4 eventos principais fazem com que um evento seja criado:

1. Inicialização do SO
2. Execução de um sistema de criação de processos
3. Requisição de usuario para a criação de novo processo
4. Iniciação de um **batch job**.

Durante a Inicialização do SO muitos processos são criados. Alguns desses processos realizam inteação com o usuario. Outros, funcionam em *background*, realizando tarefas especificas, os **daemons**.

Temos uma chamada de sistema que realiza a criação de um novo processo, `fork()`. Essa chamada cria uma copia exata do processo que a esta invocando, ou seja, cria um **processo filho**, que muitas vezes vai utilizar a chamada `execve()` para substituir sua *memory image*. Cada novo processo criado tem o seu proprio espaço de memoria.

### Finalização de processos

Normalmente, processos terminam por algum dos seguinte motivos:

1. Saida normal (voluntario): processo terminou o que tinha que ser feito
2. Saida por erro (voluntario): processo não pode terminar, por exemplo, um dos argumentos passados á ele estava vazio
3. Erro fatal (involutario): processo enfrentou algum erro de execução, como divisão por zero
4. Morto por outro processo (involutario): um processo foi cuzão e quis matar o coleguinha.

### Hierarquia de processos

Podemos representar os processos como uma arvore. Quando um processo realiza um `fork()` ele adiciona filhos ao ser nó.

Para exemplificar como arvores de processos são utilizadas vamos mostrar como o MINIX é inicializado. Dois processos especiais, **reincarnation server** e **init** estão na **boot image**. O RS é responsavel por reinicializar todos os drivers e servidores. Em contraste, o *init* é executa o script `/etc/rc`, que fala pro RS quais drivers e servidores devem ser inicilizados. Assim, todos essses novos processos criados serão filhos do RS, e caso, algum deles falhe, o RS sera notificado e realizará a reencarnação do mesmo. Esse mecanismo faz com que o MINIX seja toleravel á uma falha em algum driver ou servidor. Quando o *init* termina isso, ele executa o arquivo de configuração `/etc/ttytab` para saber qual terminal deve ser executado e realiza um *fork* no processo `getty`. Pronto, agora temos um terminal que é filho de *init*. Ou seja, todo novo processo criado pelo usuario sera filho de *init*.

### Estados dos processos

Mesmo que processos sejam unidades independentes, muitas vezes eles precisam se comunicar ou sincronizar com outros. Um processo pode se encontrar em 3 estados:

- Running: de fato, usando a CPU em um dado instante
- Ready: executavel, temporariamente parado para que outro processo seja executado
- Blocked: impedido de ser executado ate que algum evento externo aconteça

Podendo ter as seguintes transições:

- Running -> Blocked: processos bloquea para receber uma entrada.
- Running -> Ready: escalonador pega outro processo para usar a CPU
- Ready -> Running: escalonador escolhe este processo
- Blocked -> Ready: entrada se torna disponivel

### Implementação de processos

Para implementar processos, o SO utiliza uma *array* de **process tables**, que contem uma posição para cada processo. Essa tabela contem todas as informações acerca do processo.

No MINIX, comunicação entre processos, gerenciamento de memoria e gerenciamento de arquivos são feitos por diferentes modulos, fazendo com que a tabela de processos seja segmentada, cada segmento referente á um modulo.

### Threads

Muitas vezes é desejados que mais de uma tarefa seja executada dentro de um espaço de memoria, de um processo. Com isso, definimos uma **thread** como sendo um **lightweight process** que é executado dentro de um processo. Com isso, cada thread divide a sua execução dentro do tempo de execução de um processo.

Analogamente, precisamos definir uma **thread table**, que armzena informações acerca da execução de cada thread.

## Comunicação entre processos (IPC)

Temos 3 problemas aqui:

1. Como fazer com que 2 processos se comuniquem
2. Evitar que dois processos esbarrem um no caminho do outro enquanto estao se comunicando
3. Sequenciar a ordem em que cada processo deve ser executado

Note que todos esses problemas tambem se aplicam as threads.

### Race conditions

Ocorre quando processos compartilham algum recurso, como memoria, e competem para ver qual processo consegue ter acesso ao recurso primeiro, e, dessa forma, o resultado da execução dos processos pode variar de maneira indeterminada.

### Sessão critica

Para resolver o problema das *race conditions* é necessario ter **exclusão mutua**, uma forma de assegurar que se um processo esta usando algum recurso, somente ele vai estar usando o recurso.

A parte do programa do programa que pode vir gerar *race conditions* é chamada de **sessão critica**. Uma solução que aplica a exclusão mutua atende 4 condições:

- 2 processos não podem estar, simultaneamente, na sua sessão critica
- Nenhuma presunção pode ser feita acerca da velocidade ou quantidade de CPU's
- Nenhum processo fora de sua sessão critica pode bloquear outros processos
- Nenhum processo deve esperar para sempre para entrar na sua sessão critica

### Exclusão mutua com *Busy waiting*

Aqui vamos examinar soluções para exclusão mutua, onde, quando um processo esta ocupando realizando tarefas na sua sessão critica, nenhum outro processo vai atrapalhar.

#### Desabilitando interrupções

Uma alternativa simples é fazer com que cada processo desabilite todas as interrupções logo apos entrar em sua sessão critica e reabilite elas logo apos terminar. Dessa maneira, o processo monopoliza a CPU enquanto estiver executando sua SC.

Essa solução falha em dois aspectos:

- Não é muito sabio dar á um processo de usuario o podem de desabilitar interrupções, pois isso pode fazer com que o sistema trave para sempre
- Ela não se aplica muito bem á computadores com mais de uma CPU, pois nesse cenario, ainda poderiamos ter processos em CPU's diferentes competindo por recursos.

Porém, o kernel usa essa solução em alguma de suas rotinas (mas ele é o kernel então ele pode).

#### Travar variaveis

Considenre uma unica e compartilhada *lock variable*, inicializada com 0. Se um dado processo quer entrar na sessão critica ele testa a varivael

- Se a variavel for 0, ele entra na sessão critica e á coloca com valor 1
- Caso contrario ele espera até que o valor dela seja 0

Porém, a processo de `test and set` acaba sendo uma condição de corrida, ou seja, dois processos podem querer colocar o valor da variavel como 1 ao mesmo tempo.

#### Strict alternation

```c
turn = 0;

// Processo 1
while (TRUE) {
    while (turn != 0)
    critical_region( );
    turn = 1;
    noncritical_region( );
}

// Processo 2
while (TRUE) {
    while (turn != 1)
    critical_region( );
    turn = 0;
    noncritical_region( );
}
```

A solção acima, de esperar ate um certo valor de varival, é chamada de **busy waiting**, pois gasta CPU enquanto espera.

Porém a solução acima tem um problema: caso ele demore muito para executar a sua sessão **não** critica, ele vai impedir que o o outro processo execute a sua sessão critica novamente, violando a 3 regra.

Essa solução só é boa qunado dois processos ficam alternando execução, sem muita diferença entre tempos de exucução, como, por exemplo, num *spooler* de impressão.

#### Algoritmo de Peterson

```c

#define FALSE 0
#define TRUE 1
#define N 2 /* number of processes */
int turn;
int interested[N]; /* whose turn is it? */
/* all values initially 0 (FALSE) */

void enter_region(int process) /* process is 0 or 1 */
{
    int other = 1 − process; /* number of the other process */
    interested[process] = TRUE; /* show that you are interested */
    turn = process; /* set flag */
    while (turn == process && interested[other] == TRUE); /* null statement */
}

void leave_region(int process) /* process: who is leaving */
{
    interested[process] = FALSE; /* indicate departure from critical region */
}
```

#### Instrução TSL (Test and Set Lock)

Muitos processadores modernos tem a instrução `TSL RX,LOCK`, que funciona da seguinte maneira:

- Le a palavra `LOCK` na memoria e a coloca no registrador `RX`
- Coloca um valor positivo na palara `LOCK`

Essa instrução é atomica, definida por hardware. Para usar essa instrução, vamos usar uma variavel global LOCK. Quando ela tem o valor 0, qualquer processo pode seta-la para 1 e usar a memoria compartilhada (entrar na SC), e depois de terminar colocar o valor de LOCK para 0 novamente.

Podemos definir a seguinte rotina:

```assembly

enter_region:
    TSL REGISTER,LOCK   | copy LOCK to register and set LOCK to 1
    CMP REGISTER,#0     | was LOCK zero?
    JNE ENTER_REGION    | if it was non zero, LOCK was set, so loop
    RET                 | return to caller; critical region entered

leave_region:
    MOVE LOCK,#0        | store a 0 in LOCK
    RET                 | return to caller

```


### Sleep and Wakeup

Tanto a solução de Peterson quanto a TSL então corretas, porem, ambas aprensentam *busy wainting*, ou seja, ficam executando um loop ate poderem entrar na SC, o que é muito custoso e desperdiça recursos da CPU.

Vamos olhar algumas ideias que usam a comunicação entre processos para solucionar esse problema.

#### Problema do produtor-consumidor

Digamos que dois processos compartilham um certo *buffer*. Um processo (produtor) coloca coisas nesse buffer e o outro (consumidor) le as coisas do buffer. Um problema aparece quando o produtor quer colocar algo no buffer mas não tem mais espaço, assim, ele entra em modo *sleep* ate que o consumidor retire algo de la. Analogamente, temos o mesmo problema para o consumidor, que pode não ter nada para ser consumido no buffer e tem que esperar ate que o produtor insira algo la.

```c

#define N 100 // espaço no buffer
int count = 0; // itens no buffer

void produtor() {
    int item;

    while(1) {
        item = produce_item()
        if (count == N) sleep() // buffer esta cheio
        insert_item(item); // coloca item no buffer
        count++;
        if (count == 1) wakeup(consumer) // buffer estava vazio
    }
}

void consumidor() {
    int item;

    while(1) {
        if (count == 0) sleep(); // buffer vazio
        item  = remove_item(); // retira do buffer
        count--;
        if (count == N-1) wakeup(producer); // buffer estava cheio
        consume_item(item);
    }
}

```

Porém, aqui, ainda temos uma condição de corrida sob a variavel count.

### Semaforos

Alternativa criada para resolver o problema do produtor/consumidor. Um **semaforo** seria uma variavel que poderia ter o valor 0 (caso nenhum wakeup seja necessario) ou um valor positivo (dizendo quantos wakeups estao pendentes).

A ideia é que tenhamos duas operações `down` e `up` (generalização de sleep e wake up).

A operação **down** (P) checa se o valor do semaforo é maior do que 0, caso positivo decrementa 1 e continua a execução do processo. Caso negativo, o processo é colocado *para dormir* ate que haja algum valor positivo no semaforo. (note que as operações executadas pelo semaforo são atomicas, e por isso garantimos que há exclusão mutua).

A operação **up** (V) incrementa o valor do semaforo. Caso algum processo estivesse esperando para ser acordado, o sistema ativa essa processo para que ele possa ser executado, ou seja, para que ele possa fazer sua chamda de `down`. Note que, apos fazer a operação up, se algum processo estava adormecido, o valor do semaforo vai continuar sendo 0 apos a execução.

#### Resolvendo o problema do produtor consumidor

Para implementar as operações de up e down é necessario usar chamdas de sistema ou a função TSL.

Para o problema do produtor-consumidor, usaremos 3 semaforos:

- `full`: conta o numero de *slots* cheios. Iniciado com 0.
- `empty`: conta o numeor de *slots* vazios. Iniciado com o numero de *slots no buffer*.
- `mutex`: gerenciar se o produtor ou o consumidor esta acessando o buffer. Iniciado com 1.

Semaforos que são inicializados com 1 e usados por dois ou mais processos para garantir que somente 1 deles entre na sessão critica são chamados de **semaforos binarios**. Se um processo faz um *down* logo antes de entrar na SC e um *up* logo depois, temos exclusão mutua garantida.

A seguir, esta o problema de produtor consumidor resolvido com semaforos:

```c

#define N 100
typedef int semaphore;      // Semaforos são ints especiais
semaphore mutex = 1;        // controle acesso a SC
semaphore empty = N;        // slots vazios
semaphore full = 0;         // slots cheios

void producer(void) {
    int item;

    while (1) {
        item = produce_item();
        down(&empty);       // decrementa slot vazio
        down(&mutex);       // entra na regiao critica
        insert_item(item);  // coloca item novo no buffer
        up(&mutex);         // sai da regiao critica
        up(&full);          // incrementa slot cheio
    }
}

void consumer(void) {
    int item;

    while (TRUE) {
        down(&full);        // Decrementa slot vazio
        down(&mutex);       // entra na regiao critica
        item = remove_item( );
        up(&mutex);         // sai da regiao critica
        up(&empty);         // atualiza slot vazios
        consume_item(item); // faz algo com isso
    }
}
```

Ademis, podemos usar semaforoes para problemas de **sincronização**.

### Mutexes

Um **mutex** é basicamente um semaforo binario, ou seja, ele pode ter dois estados, `unlocked`(> 0) e `locked`(== 0).

Quando um processo precisa acessar a SC ele chama `mutex_lock`. Se a mutex esta atualmente destravada, ou seja, a SC esta libre e pode ser usada, o processo pode entrar nela. Caso contrario, o processo aguarda ate que a SC critica fique libre e o processo que esta la dentro invoque `mutex_unlock`.

Se varios processos desejam esntra na mutex, um deles é escolhido de forma aleatoria para entrar.

A mutex é basicamente um inteiro modificado. Quando um processo invoca `down(mutex)` duas coisas podem acontecer:

- A mutex tem valor == 1: ou seja, o processo pode entrar nela. Em uma intrução atomica essa checagem e feita e o valor da mutex é setado para 0. Assim, outros processos sabem que existe uma processo na SC.
- A mutex tem valor == 0: isso quer dizer que algum processo esta na SC e que o processo atual deve aguardar ate sua vez.

Após sair da sessão critica, é necessario que o processo invoque `up(mutex)`, ou seja, que incremente o valor da mutex.

O peseudo-codigo pode ser definido assim:

```c

// codigo do down
if (mutex > 0) {
    mutex--;
    roda_processo();
}
else
    processo_espere;

// codigo do up
mutex++;

```

### Monitores

Proposta mais robusta que o semaforo, que visava oferecer um *pacote*, um objeto, com tudo já pronto, somente para o programador invodar e usar. Com isso, o monitor ia ter que ser algo implementado a nivel de compilador.

### Envio de mensagens

Esse metodo de comunicação entre processos tem 2 primitivas, `send(destination, &message)` e `receive(source, &message)`, com o utlimo podendo especificar se quer receber a mensagem de alguem especifico ou de qualquer fonte.

#### Problemas nos sistemas de envio de mensagens

O processo de troca de mensagens pode se tornar um pouco diferente dos casos anteriores. Por exemplo, quando a troca de mensagens é feita atravez de uma rede, existe o perigo de que tal mensagem seja perdida. Para superar este perigo o *receiver*, após receber a mensagem, manda uma mensagem de **acknowledgement** para o *sender*, avisando que tudo deu certo. Caso, apos um dado periodo de tempo, o sender não receba essa mensagem, ele retransmite a mensagem original.

Agora imagine que a mensagem de *acknowledgement* foi perdida, agora, o sender tera mandado duas copias iguais da mesma mensagem. É necessario, então, que o *receiver* saiba destinguir mensagens repetidas. Isso pode ser feito atribuindo uma sequencia de numeros para cada nova mensagem do sender, assim, caso o receiver receba duas mensagem com numeros iguais, ele sabe que recebeu uma mensagem repetida.

#### Problema produtor-consumidor no envio de mensagens

Vamos ver como solucionar esse problema usando o envio de mensagens e nenhuma memoria compartilhada. Vamos assumir que todas as mensagens tem o mesmo tamanho e que as mensagens enviadas e ainda não recebidas são colocadas, automaticamente, em um buffer. Seja `N` o numero de mensagens enviadas.

- O consumidor começa enviado N mensagens vazias para o produtor
- Quando o produtor precisa enviar uma mensagem, ele pega uma vazia, coloca a mensagem e a envia de volta para o consumidor.

Dessa maneira, o numero de mensagens no sistema permanece constante. O que faz com que a memoria alocada para este recurso tambem seja constante.

```c

#define N 100 // number of slots in the buffer

void producer(void) {
    int item;
    message m;

    while (TRUE) {
        item = produce_item( );     // gerando algo para colocar no buffer
        receive(consumer, &m);      // espera mensagem vazia chegar
        build_message(&m, item);
        send(consumer, &m);         // envia mensagem para o consumidr
    }
}

void consumer(void) {
    int item, i;
    message m;

    for (i = 0; i < N; i++)         // enviando mensagens vazias
        send(producer, &m);
    while (TRUE) {
        receive(producer, &m);      // espera ate receber uma mensagem
        item = extract_item(&m);
        send(producer, &m);         // manda mensagem vazia para o produtor
        consume_item(item);
    }
}
```

Agora, vamos olhar como as mensagens são endereçadas. A primeira maneira é fazer com que cada processo tenha um endereço e fazer com que as mensagens sejam enviadas paras esses endereços. Outra maneira é utilizar a estrutura de dados chamada **mailbox**, que guarda uma certa quantidade de mensagens. Assim, um processo pode enviar/receber mensagens de uma *mailbox*. Analogamente, caso um processo queira enviar uma mensagem para uma que esta cheia ele tem que espera, e caso ele queira pegar uma mensagem de uma que esta vazia ele tambem tem que esperar.

A maneira extremamente oposta á *mailbox* é chamada de **rendezvous**. Com ela, fazemos a comunicação ser direta, sem nenhum buffer. Assim, caso um processo A queira enviar uma mensagem para o precesso B, ele vai ficar bloqueado ate que o processo B faça a invocação de `receive`. O comportamento é analogo para o recebimento.

Essa é a *approach* que o MINIX usa para os processos internos. Para os processos de usuario, o MINIX usa  um *PIPE* como mailbox.

## Problemas classicos de comunicação entre processos

### Filosofos comilões

Cinco filosofos estão sentados numa mesa circular. Cada filosofp tem um prato de comida. Enter cada par de pratos existe um garfo e para comer, um filosofo precisa de 2 garfos.

A vida de um filosofo pode ser resumida em 2 ações, comer e pensar. Quando um filosofo esta com fome, ele tentar pegar os garfos a sua esquerda e a sua direita para comer. Depois disso, ele os coloca de volta na mesa.

A ideia é fazer um programa que resolva esse problema e que nunca fique preso, ou seja, que nunca sofra **starvation**.

Na pratica, muitos problemas desse tipo são resolvidos fazendo com que processos (filosofos) esperam quantidades aleatorias de tempo. Porém, vamos fazer uma solução definitiva e correta.

A primeira solução é a seguinte:

```c
#define N 5         // numero de filosofos
int mutex;          // controle de acesso a SC

void philosopher (int i) {  // numero do filosofo
    while (1) {
        think();
        down(mutex);        // entra na SC
        take_fork(i);
        take_fork((i+1)%N);
        eat();
        put_fork(i);
        put_fork((i+1)%N);
        up(mutex);          // sai da sessão critica
    }
}
```

O unico *problema* dessa solução é que somente 1 filosofo conseuge comer por vez.

### Problema dos leitores e escritores

Problema que tem uma analogia pratica em banco de dados. Quando se tem processos fazendo consultas e alterações no DB.

Nessa solução, o primeiro leitor á ter acesso ao banco faz `down` no semaforo `db`. Em seguida, os proximos leitores devem incrementar um contador `rc`. Quando um leitor terminar ele decrementa o contador e, caso ele seja o ultimo, da um `up` no semaforo, avisando que um escritor pode entrar.

```c

typedef int semaphore;
semaphore mutex = 1;
semaphore db = 1;
int rc = 0;                         // numero de processos lendo ou querendo ler

void reader (void) {
    while (TRUE) {
        down(&mutex);               // acesso exclusivo ao leitor
        rc++;                       // aumenta o contador de leitores
        if (rc == 1) down(&db);     // primeiro leitor avisa que nao pode escrever
        up(&mutex);                 // sai da SC
        read_data_base( );          // le
        down(&mutex);               // SC novamente
        rc--;                       // diminui # leitores
        if (rc == 0) up(&db);       // ultimo avisa que pode escrever
        up(&mutex);                 // sai da SC
        use_data_read( );
    }
}

void writer(void) {
    while (TRUE) {
        think_up_data( );           // sessao nao critica
        down(&db);                  // tenta obter acesso
        write_data_base( );         // faz mudança
        up(&db);                    // avisa que saiu
    }
}
```

O *problema* da solução acima é que ela favorece os leitores. Enquanto processos estiverem lendo nenhu escritor pode trabalhar no banco de dados.


## Escalonamento

Quando muitos processos estão prontos para executar, eles competem pelo uso da CPU. Para manejar isso, precisamos do **escalonador**, que vai executar um **algoritmo de escalonamento.**

### Comportamento dos processos

Quase todos os processos alternam sua execução entre computar algo e fazer alguma *request* de I/O (entra em estado bloqueado esperando o trabalho de outro dispositivo) . Algumas operações de I/O contam como computação, por exemplo, quando a CPU copia dados para a *video RAM* para atualizar o que esta sendo mostrado na tela.

Quando um processo gasta mais tempo computando algo, ele é **compute-bound**. Caso ele gaste mais tempo esperando I/O ele é **I/O-bound**. Com o aumento do poder de processamento das CPU's os processos tendem a se tornar mais IO-bound.

### Quando escalonar

Temos 2 situações em que o escalonamento pode acontecer:

1. Qaundo um processo termina
2. Quando o processos esta bloqueado por IO ou por algum semaforo

Temos outras 3 ocasiões em que o escalonamento tambem acontece:

3. quando um novo processo é criado.
4. quando uma interrupção para IO acontece (algum dispositivo de IO pode ter compleado seu trabalho e algum processo que estava esperando por isso pode rodar)
5. quando uma parada de relogio (clock interrupt) ocorre

O ultimo caso serve para decidir se um processo esta sendo executado por muito tempo. Um escalonamento **não-preeptivo** escolhe um processo para executar e o executa ate que ele seja bloquado ou libere a CPU. Um algoritmo **preemptivo** escolhe um processo e o excuta por um certo periodo de tempo, apos esse periodo o processo é suspenso e outro é executado em seu lugar.

### Categorias de escalonamentos

Em diferentes ambientes, diferentes tipos de escalonamentos são necessarios.

1. **Batch**: onde não existem usuarios esperando pela resposta. Reduz as trocas de contextos, assim, são escalonamentos não-preemptivos ou que preemptivos que possui um periodo de processamento muito alto.

2. **Iterativo**: Para lidar com varios usuarios, preeptivo.

3. **Tempo real**: Preeptivo, executa processos pequenos que sabem a hora de parar.

### Objetivos do escalonamento

- Todos os tipos
    - Fairness: Todos os processos tem uma fatia da CPU
    - Policy enforcement: a politica definida é aplicada
    - Balance: mantem todas as partes do sistema ocupadas

- Sistemas batch
    - Throughput: maximiza trabalhos por hora
    - Turnaround time: minimiza tempo entre submissão e termino
    - CPU utilization: mantem a CPU ocupada o tempo todo

- Sistemas iterativos
    - Response time: responde **requests** rapidamente
    - Proportionality: atende usuario

- Sistemas tempo-real
    - Meeting deadlines: evita perder dados
    - Predictability: evita degradação dos dados

### Escalonamentos em sistemas batch

Algoritimos usados por sistemas batch

#### first-come first-served

Algoritimo mais simples e não preemptivo. Processos usam a CPU na ordem em que a pedem, como uma fila simples, em que se aplica *ordem de chegada*.

#### Shortest job first

Esse metodo assume que os tempos de execução são conhecidod de antimão. Assim, os escalonador escolhe o processo que demora menos tempos para ser executado.

#### Shortest remaining time first

Similar ao de cima, so que nesse caso, o escalonador pega o proximo que esta mais proximo de ser executado.

#### Three-level scheduling

Quando um trabalho chega, ele é colocado numa fila de entrada no disco. O **admission scheduler** decide qual trabalho pode entrar no sistema (ele tenta fazer uma mix entre trabalhos IO-bound e compute-bound), o restante continua na fila esperando. Quando um trabalho entra no sistema um processo é criado para que ele possa ser executado na CPU.

Todavia, o numero de processos podem ser tão grande que todos eles não cabem na memoria principal. Com isso, alguns processos tem que ser armazenados no disco, quem faz isso é o **memory scheduler**. Com ele, podemos decidir o **grau de multiprogramação**, ou seja, a relação entre processos que ficam na memoria principal e no disco. Para tomar a decisão sobre quais processos manter em cada lugar, ele leva os seguintes fatores em consideração:

1. quanto tempo faz que tal processo mudou de lugar
2. quanto tempo de CPU tal processo teve
3. qual grande é tal processo
4. quão importante é tal proceso.

O proximo nivel de escalonamento é realizado pelo **CPU scheduler**, que pega o proximo processo pronto para ser executado, nele, qualquer algoritmo de escalonamento pode ser executado, preemptivo ou não.

### Escalonamento em sistemas iterativos

Note que, todos os metodos de escalonamento para sistemas batch tambem podem ser usados para sistemas iterativos.

#### round-robin

Nesse escalonamento, cada processo recebe uma quantidade de tempo, um **quantum**, na qual ele pode ser executado. Se o processo continua rodando ao fim do seu tempo, ele é interrompido e outro processo é executado em seu lugar. A unia coisa que o escalonador precisa fazer é manter uma lista de processos executaveis, quando o quantum de um processo acaba ele é colocado no fim da fila, com um novo quantum. Temos o seguinte desequilibrio:

1. um quantum pequeno faz com que a CPU gaste muito tempo realizando trocas de contexto
2. um quantum grande pode fazer com que requisições iterativas (coisa que o usuario faz e recebe) sofram atrasos.

Por padrão, definimos um quantum entre 20 e 50msec.

#### priority scheduling

Aqui, cada processo recebe um prioridade, e o processo com maior prioridade tem o direito de ser executado. Para previnir que processos sejam executados para sempre, o escalonador pode diminuir a prioridade do processo atual a cada *tick* do relogio. Ademais, podemos definir um quantum para cada processo, assim, quando o quanto do processo atual acabar, o proximo processo com a mesma prioridade é executado.

É conveniente agrupar processos de mesma prioridade e realizar escalonamento round-robin neles. É isso que o minix usa, com 16 classes de prioridade. Ele coloca como maior prioridade: drivers IO, servidores de memoria, sistema de arquivo e rede. Processos de usuario tem uma prioridade menor em realçao aos componentes do sistema.

### Escalonamento em sistemas de tempo real

Normalmente componentes extenos se comunicam com o computador, que deve criar uma resposta em um curto periodo de tempo. Tais sistemas são categorizados em 2 tipos:

- **hard real time**: existem prazos absolutos que devem ser respeitados
- **soft real time**: existem prazos mas da pra dar uma varzeada neles

## Visão geral de processos no MINIX

Ao contrario do UNIX, que tem um kernel monolitico, o MINIX é uma coleção de processos que se comunica um com o outro, atraves de mensagens. Essa estrutura torna o sistema mais flexivel e mutavel.

### Estrutura interna do MINIX

O MINIX é estrututurado em quatro camadas, que executam suas funções de forma bem definida.

O **kernel** esta a camada de baixo (1). Realiza o escalonamento de processos e a suas respectivas transições de estados. Além disso, lida com a comunicação entre processos, passando as mensagens. Ele tambem oferece o suporte para as interrupções de IO, que requer uso de um conjunto de instruções privilegiadas do processador, **kernel mode**. Nessa camada tambem temos a **clock task**, que gera sinais de tempo utilizados pelo kernel. Uma das principais atividades dessa camada é prover um conjunto de **kernel calls** para drivers e servidores acima, cuja implementação é feita pela **system task**. Entretando, mesmo que essas duas ultimas **tasks** sejam compiladas no mesmo espaço do kernel, elas são escolanadas como processos separados.

O kernel trata as 3 camadas acima dele da mesma maneira. Cada uma delas é limitadas por um conjunto de instruções pertencentes ao **user mode**, alem disso, elas não podem acessar porções de memoria fora de seus limites.

Processos podem ter privilegios especiais. O processos na camada 2 tem muitos privilegios, os da camada 3 alguns privilegios e os da camada 4 não tem nenhum privilegio especial.

A camada 2 possui os **devices drivers**, que basicamente são os mecanismos de fazer IO.

A camada 3 possui os **servers**, que produzem o ferramentario utilizados pelos procesos de usuario.

A camada 4 possui todos os processos de usuario.

Camada | O que acontece |
:--- | :--- |
4-user processes | init, user process |
3-server processes | process manager, file system, info server |
2-device drivers | disk driver, tty driver, ethernet driver |
1-kernel | kernel, clock task, system task |

É importante notar a diferença entre as **kernel calls** e as **POSIX system calls**. As kernel calls são funções em baixo nivel disponibilizadas pelo system task para permitir que drivers e servidores façam seu trabalho, como por exemplo, ler a porta IO de um dispositivo. Ja as POSIX system calls (read, fork, ...) são definidas pelo padrão POSIX e estão disponiveis para os programas de usuario da quarta camada.

### Gerenciamento de processos

Sabemos que um processo pode criar outro processo. Dito isso, todos os processos de usuarios são criados pelo processo **init**.

#### MINIX startup

Em muitos computadores com discos, temos uma hierarquia de **boot disk**, em que a imagem de inicialização é procurada no primeiro dispositivo definido. Um HD é dividido em partições, a primeira sessão possui um pequeno programa e a **tabela de partição** do disco, essas duas peças formam o **master boot record**. O pequeno programa é executado para ler a tabela de partições e selecionar a **active partition**, que possui um *bootstrap* em seu inicio, que executada a fim de achar e realizar a copia do programa **boot image**.

Esse programa contem: o kernel (junto com clock e system task), o process manager e o file system. Alem disso, pelo menos um driver de disco deve ser caregado, junto com alguns outros programas: reincarnation server, ram disk, console e init.

Todas as partes da boot image são programas separados, que são executados separadamente depois que o kernel, PM e FS são carregados. Isso torna o sistema tolerante a falhas de inicialização.

#### Inicialização da arvore de processos

**Init** é o primeiro processo de usuario e o ultimo a ser carregado pela boot image, é filho do reincarnation server e recebe o PID 1. O PM é inicializado antes e recebe PID 0. O init executa o script em `/etc/rc` que inicia drivers e servidores adicionais.

### Comunicação entee processos no MINIX

Temos 3 chamadas principais:

- send(dest, &message): manda mensagem para o processo dest
- receive(source, &message): recebe mensagem de uma fonte ou de qualquer lugar
- sendrec(src_dst, &message): manda mensagem e espera pela resposta, sobrescrevendo a original

O fluxo normal de mensagens é para baixo, e mensagens podem ser mandadas para processos na mesma camada ou em camadas adjacentes. Quando um processo manda mensagem para outro que não a esta esperando ele fica bloqueado ate que destinataria a receba.

Se um processo A tenta enviar para B e se bloqueia, e B tenta enviar para A e se bloqueia, temos um deadlock.

Temos outra função importante para a transmissão de mensagens, `notify(dest)`, que avisa para dest que algo importante aconteceu, mas não bloqueia o processo que invocou. A informação passada é minima, apenas o ID do processo invocador e um *time stamp*. É usada, por exemplo, pelo teclado para avisar que uma das teclas de função (F1...f12) foi apertada. Tal função é principalemente usadas por precessos de sistema, e como eles são poucos, cada processo possui um *bitmap* de notificações, assim, quando um processo A notifica um processo B, o bit relativo ao processo A é ligado no bitmap de notificações de B, tal bitmap fica no kernel.

### Escalonamento de processos do MINIX

O sistema de interrupção é o que faz a multiprogramação acontecer. Interrupções tambem são geradas por software, as **traps**. As operações de `send` e `receive` são traduzidas pela biblioteca do sistema como interrupções de software.

O minix implementa um sistema de **multilevel queues**, com 16 prioridades definidas. A fila mais baixa é a *IDLE*, que é executada quando não se tem nada mais pra fazer. Clock e system tasks são colocadas nas filas de maior prioridade. Ja os drivers e servidores ficam entre a prioridade dos processos de usuarios e a de clock task.

O quantum não é o mesmo para todos os processos. Os processos de usuario possuem um quantum menor, enquanto drivers e servidores recebem um quantum grande. Quando o quantum de um processo acaba ele é redifinido e colocado no fim da fila.

Se um processo que usou todo seu quantum tambem foi o ultimo a ser executado é sinal que temos um loop. Para resolver isso, o colocamos no final de uma fila de menor prioridade. Porem, se um processo acaba com seu quantum mas não impediu que outro processo fosse executado, ele pode ter sua prioridade aumentada.

## Implementação de processos no MINIX

NÃO RESUMI ESSA PARTE (PG 125-192)

## System task no MINIX

Uma consequencia de fazer grandes componentes do sistema fora do kernel é que elas serão proibidas de realizar IO, de manipilar tabelas do kernel etc. A solução para este problema é que o sistema ofereça um conjunto de serviços para os drivers e servidores. Tais serviçõs, que não são disponiveis para os processos de usuario, permitem realizar operações como se eles estivessem dentro do kernel.

Essas operações especiais são lidadas pela **system task**, que recebe essas requisições e as encaminha para o kernel.

No MINIX, as *system calls* realizadas pelos processos de usuarios são tranformadas em mensagens para os servidores. Os servidores então se comunicam entre eles e tambem com o kernel, atravez de mensagens, tais mensagens são recebidas pela systask. Vamos chamar esse ultimo tipo de mensagem de **kernel calls**.

Por exemplo: `fork()` é uma syscall que é encaminhada para o PM. O PM faz algum trabalho e então faz a chamada `sys_fork` para a system task, que então vai manipular o kernel.

A systask aceita 28 tipos de mensagens (kernel calls).

## Clock task no MINIX

Relogio é essencial para impedir que um processo monopolize a CPU. É executado no espaço do kernel é não pode ser acessada por um processo de usuario.

### Clock hardware

O relogio que realiza a interrupção é feito no hardware, usando o conhecimento de engenheiros. Mas é basicamente um contador que vai decrescendo. Quando chega á 0 ele faz um **clock tick**.

### Clock software

O que fazer com o tick gerado pelo hardware fica em cargo do clock driver. Esse driver basicamente cuida de guardar a hora do dia e de fornecer aparato para a CPU/escalonador.

# Input/Output

Uma das principais funções do istema operacional é controlar os dispositivos de entrada e saida, criando uma interface na qual eles possam se comunincar.

## Principios do hardware de IO

Neste livro, estamos interessados em olhar a parte de programação desses dispositivos, a qual esta intimamente ligada com o que ele faz.

### IO devices

Podemos dividir os dispositivos em duas categorias:

- **block devices**: armazena sua informação em blocos de tamanho fixo, cada um com seu proprio endereço. A propriedade essencial desses dispositivos é que existe a possiblida de ler e alterar tais blocos de maneira independente. Discos são os maiores representantes.
- **Character devices**: esse tipo de dispositivo entrega/recebe um fluxo de dados, não é endereçavel e não possui nenhuma operação de busca. Impressoras, mouses e etc.

O sistema de arquivos lida com abstrações de dispositivos de blocos, deicando a parte que faz a comunicação direta com o hardware para um software mais baixo, o **device driver**.

Os dispositivos variam bastante em relação a velocidade de transmissão dos dados, deixando em cargo do software lidar com essas diferenças.

### Device controllers

O componente eletronico de um dispositivo é chamado **device controller** ou **adapter**, esse controlador faz a ligação entre a parte mecanica e a digital do dispositivo, normalmente a programação é feita em uma linguagem de baixo nivel. Os dispositivos são normalmente conectados á placa mãe, e uma enterface entre ela e o dispositivo normalmente segue um padrão oficial ANSI, IEEE ou ISO.

Os SO se comunicam apenas com os controladores dos dispositivos. A maioria dos computadores utilizam o modelo de *bus*, que é basicamente uma unica linha de conexão entre os dispositivos e a CPU.

### Memory-mapped IO

Cada controlador possui alguns registradores que são utilizados para fazer a comunicação com a CPU, assim, o sistema operação consegue requisitar ações do dispositivo escrevendo nestes registradores.

Muitos dispositivos tambem possuem um **data buffer**, no qual o SO pode escrever coisas que serão feitas. Os monitores possuem uma VRAM que armazena oq sera exibido.

Para que a CPU se comunique com esses registradores e buffers temos duas alternativas. A primeira alternativa faz com que cada registrador de cada controlador possua uma **IO port number**, ou seja, um endereço em que se pode ler/escrever. A segunda opção é fazer que os registradores façam parte do espaço normal de memoria, chamamos isso de **memory mapped IO**, assim, quando uma operação é feita temos uma epecie de `if (isIO) else if (isNormalStuff)` que separa o que deve ser acessado da memoria e o que deve ser feitos em controladores.

### Interrupções

Registradores dos controladores possuem alguns **status bits** que podem ser testados para saber se uma requisição de dados ja foi completada ou se novos dados de um *input device* estão disponiveis.

A CPU pode ficar testando esses bits ate que o dispositivo esteja pronto para receber/enviar novos dados, esse processo é chamado de **polling** ou **busy waiting**. Tal processo não é muito bom, e so é recomendado para sistemas pequenos que não possuem muitos processos.

Muitos controladores utilizam interrupções para dizer a CPU que ele esta pronto para enviar/receber algo, para isso eles utilizam a linha especial de IRC (Interrpt ReQuest) do *bus system*, porem, o numero dessas linhas é limitado é podem haver conflitos entre dispositvos. Isso levou a industrua a desenvoler o **plug'n play**, na qual a BIOS decide qual dispositivo vai usar cada IRC, em tempo de boot.

## Principios de IO software

Vamos ver as coisas do ponto de vista do SO

### Objetivos do IO software

O principal conceito é o de **device independence**, ou seja, de que um programa possa acessar qualquer dispositivo, sem que isso tenha que ser definido de antemão. Um programa que le um arquivo deveria poder ler tal arquvio de um disquete, de um disco ou de um CD-ROM, sem ter que ser modificado para cada um. Deve ser resposabilidade do SO cuidar da diferenciação de cada um, de saber quais comandos podem/devem ser utilizados.

Alem disso, temos o conceito de **uniform naming**, o qual define que o nome de cada dispositivo deve ser simplesmente uma string ou numero. No UNIX e MINIX todo tipo de disco podem ser itegrado na hierarquia do sistema de arquivos.

Outro problema é o de **error handling**, que deve tentar ser tratado pelos niveis mais inferiores (controladores e drivers), so devendo ser passado para os niveis mais altos em ultimo caso.

Outra questão é a de tranferecias **sincronas** (blocking) vs **assincronas** (interrup driven). A maioria das operações IO fisicas são assincronas, a CPU começa a tranferencia e vai fazer outra coisa. Programas de usuario funcionam de modo mais simples caso a tranferecias seja feita atravez de bloqueio, por exemplo, quando um programa é avisado que recebeu algum dado de algum dispositivo ele é suspenso ate que toda essa infromação esteja disponivel em um buffer.

Outros dois problemas são **beffering**, informação tem que ser guardada temporiareamente ate ser usada, e **compartilhabilidade**, se mais de um usuario pode ter acesso ao device.

O software de IO é organizado em 4 camadas:

- User-level I/O software
- Device-independent operating system software
- Device drivers
- Interrupt handlers
- Hardware

### Interrupt handlers

Um driver que começa uma operação IO fica bloqueado ate que essa operação seja concluida, quando essa operação termina uma interrupção acontece. Quando a interrupção é feita, o *handler* faz oq for preciso para que tudo ocorra adequadamente, então ele pode desbloquar o driver que o invocou. Esse modelo funciona bem se os drivers forem tratados como processos.

### Device drivers

Cada dispositivo precisa de uma API especial para que possa ser controlado, chamamos isso de **device driver**, é como se fosse uma biblioteca que oferece uma abstração acerca do dispositivo, para aplicações que as usarão.  Um driver controla um unico dispositivo ou uma familia de dispositivos muito semelhantes.

Na maioria dos sistemas, esses drivers fazem parte do kernel, por motivos de performance. No MINIX, os drivers são separados e executados como processos de usuarios, por motivos de confiabilidadde.

A maioria dos SO definem uma interface padrão na qual dispositivos de bloco e caracteres devem suportar, essa interface consiste em um conjunto de operações que o SO pode fazer uso.

Depois que o driver recebe uma comando, ele checa se os parametros passados estão corretos. Apos tal checagem, ele traduz esses comandos para algo que faça sentido para o controlador, ou seja, começa a escrever nos registradores do controlador. Depois disso, duas coisas podem acontecer: o driver tem que esperar ate que o controlador faça algo, então ele se bloqueia e espera ate que uma interrupção aconteça e o desbloqueie; a operação termina sem *delay* e o driver não precisa ser bloquado.

Em qualquer um dos casos, os driver checa se a operação foi feita corretamente e da um *feedback* para a aplicação no nivel acima.

O papel do driver tambem pode ser inicializar o dispositivo no *startup* do SO, lidar com o plug'n play ou realizar logs.

### Device-independent IO software

Uma grande parcela dos softwares de dispositivos IO não dependendem diretamente do dispositivo. No MINIX, a maioria desse software faz parte do file system. A função desse tipo de software é realizar operações que são comuns em todos os dispositivos e assim prover uma interface uniforme para as aplicações de usuario.

A seguir, vamos olhar algumas resposabilidades do *device independent software*.

#### Interface uniforma para drivers de dispositivos

Um dos desafios é fazer com que os dispositivos e drivers sigam um padrão, assim, o SO não precisa ser modificado cada vez que um novo dispositivo for conectado.

Outro aspecto dessa interface uniforme é a maneira em que os dispositvos são nomeados. O *device independent software* cuida de mapear um nome para o dispositivo itself. No UNIX e no MINIX, um dispositivo, por exemplo, `/dev/disk0` especifica unicamente o i-node para um arquivo especial. Nesse arquivo temos duas infromações:

- **major device number**: Usado para localizar o driver apropriado, como se fosse sua identificação;
- **minor device number**: Passado para o driver para esficificar a unidade em que se quer fazer a operação.

Temos a questão de segurança, como garantir que um usuario não acesse dispositivos que ele não tem permissão de acessar? No UNIX e no MINIX os dispositivos aparecem como abjetos no sistema de arquivos, o que significa que as regras padrão de proteção (famoso *rwx bits*) são tambem aplicada aos devices.

#### Buffering

O *device independent software* tambem cuida do buffering apra dispostivos de bloco e de caracteres. Os dispostivos de bloco podem escrever/ler um bloco de uma vez so, assim, caso uma aplicação escreva só metade de um bloco, o SO normalmente vai guardar essa informação em um buffer e esperar a outra metade, para mandar so uma requisição pro device. Ja nos dispositivos de caracteres, podemos ter que esperar ate que os dados fornecidos por eles possam ser usados.

#### Error reporting

Erros são as coisas mais comuns em IO. Muitos erros são especificos dos dispositivos, então quem cuida deles é o driver. Porém, o restante dos erros é passado para o sistema, e quem cuida disso é um *device independent software*, tomando a atitude necessaria.

#### Allocating and releasing dedicated devices

Alguns dispositvos so podem ser usados por um processo, assim, o SO tem que decidir quem pode realizar ações no dispositivo em um dado momento.

#### Device independent block size

Um *device independent software* cuida de fazer com que os dispositivos tenham o mesmo tamaho de blocos para os softwares de alto nivel. Por exemplo, dois discos podem ter setores de tamanho diferentes, e o software faz eles parecerem que tem o mesmo tamanho de setor. A mesma coisa se aplica para dispositvos de caracteres, que podem transmitir diferentes quantidades de informações por vez.

### User space IO software

Uma porção do software de IO consiste de bibliotecas que são utilizadas por programas de usuario, por exemplo, a chamada `write(fd, buffer, nbytes)`, é uma syscall que implementa uma operação de IO.

Porem, nem todos software de user-space-IO consiste de bibliotecas. Um exemplo é o sistema de **spooling**, que é uma maneira de lidar com dispositivos IO em um sistema de multiprogramação. Vamos analisar o caso de uma impressora. Para imprimir, é necessario escrever num arquivo especial, que então é lido e impresso. Porem, não podemos permitir que os processos acessem diretamente esse arquivo especial. O que fazemos é criar um **spooling directory**, assim, quando um processo quer imprimir algo, basta que ele coloque o arquivo a ser impresso neste diretorio. Criamos tambem um **deamon**, que vai ser o unico processo com permissão á modificar o arquivo especial da impressora, a resposabilidade do deamon vai ser a seguinte: ele vai ler todos os arquvios que estao no spooling directory e cuidar para que eles sejam impressos na ordem.

## Deadlocks

Vamos estudar os deadlocks no contexto do SO

### Recursos

Deadlocks podem acontecer quando processos tem acesso exclusivo á dispositivos, arquvios e etc. Vamos nos referir á qualquer um desses objetos como **recurso**.

Recursos podem ser preemptivos (pode ser retirado do processo que o tem sem nehum efeito colateral) ou não-preemptivos (contrario), sendo o segundo o tipo mais comum para se ocorrer um deadlock, já que se temos um deadlock em um recurso preeptivo basta realoca-lo para um dos processos envolvidos. Existem 3 passos para se usar um recurso:

1. pedir o recurso
2. usar o recurso
3. liberar o recursos

### Principios de deadlocks

Definição: **A set of processes is deadlocked if each process in the set is waiting for an event that only another process in the set can cause.**

#### Condições para deadlocks

Precisamos de 4 condições para um deadlock acontecer:

- Exclusão mutua: Cada recurso ou esta em uso por exatamente um processo ou esta livre
- Hold and wait: Processos que ja possuem recursos podem pedir mais recursos
- Não preepção: Nenhum recurso dado á um processo pode ser tirado dele á força, ou seja, um recurso alocado so fica livre se o processo libera-lo
- Circular wait: Precisa existir uma fila circular, onde cada processo espera u recurso do membro a frete

#### Modelagem de deadlock

Podemos ilustrar um deadlock como um grafo dirigido, onde:

- Vertices redondos são processos
- Vertices quadrados são recursos
- Um arco recurso->processo significa que o recurso foi requisitado, concedido e esta em posse do processo
- Um arco processo->recurso diz que os processo esta bloquado esperando por tal recurso

Um deadlock é simplesmente um ciclo.

### Algoritmo do avestruz

Maneira mais simples de lidar com deadlocks, fingir que não existe nenhum deadlock. Um exemplo muito comum de deadlocks que poderiam acontecer mais são muito raros são os que podem acontecer por tamanho de tabelas (tabela de processos ou de arquivos). Imagine que a tabela de processos tenha so 100 posições livres, imagine tambem que temos 10 processos rodando e cada um vai dar fork em outros 12. Quando cada processo tiver dado fork em 9 processos a nossa tabela estara cheia, e todos os processos estarao em deadlock. Mas devemos mesmo nos preocupar com esse caso??

### Detection and recovery

Com essa tecnica para lidar com deadlocks, mantemos o grafo de alocação de recursos, assim, toda vez que um processo pede/libera um recurso, atualizamos o grafo. Caso haja algum ciclo no grafo (deadlock) simplesmente matamos um dos processos que o compoem, caso o ciclo continue, matamos outro processo, repetindo isso ate que o ciclo deixe de existir.

Isso é normalmente feito em sistemas BATCH, quando matar e reinicializar um processo é aceitavel.

### Deadlock prevention

Essa solução proprõe estabeler restrições nos processos para impedir os deadlocks, ou seja, impedir uma das 4 condições acima de aconterem.

- Impedindo exclusão mutua. Poderiamos fazer com que nenhum recurso fosse alocado para somente um processo, ou seja, poderamos fazer *spooling* para todos os recursos. Isso não funciona muito bem.

- Impedindo hold and wait. Poderiamos impedir que um processo peça recursos no meio de sua execução e fazer com que ele declare todos os recursos que vai utilizar logo de inicio, assim poderiamos fazer um escalonamento previo, mas isso não funciona quando temos processos não deterministicos. Uma outra tentativa para acabar com essa condição é fazer que o processo libere temporiareamente todos os recursos alocados por ele ao pedir novos recursos.

- Impedir a não-preepção. Tirar os recursos, mesmo que isso não gere resultados satifatorios.

- Impedir a fila circular. Tentar achar uma numeração topologica para a execução do grafo.

### Deadlock avoidance

Agora vamos ver jeitos de impedir deadlocks com uma alocação sabia e segura de recursos, considerando que vamos ter algumas informações de cada processo.

#### Algoritimo do banqueiro

Definimos um estado como a quantidade de cada recurso que um processo tem e quer, assim como a quantidade livre de cada recurso. Assim podemos ter uma tabela, onde cada linha é um processo e cada coluna representa um recurso, alem de um vetor dizendo os recursos disponiveis.

Um estado é **seguro** se podemos executar os processos em uma ordem tal que tudo seja satisfeito e respeitado.

Assim, podemos ter um processo P1 que tem 2 recursos A e 1 recuros B, e que ainda precisa de 4 do recurso B. Ademais podemos dizer que existem 3 recursos A disponiveis e 8 recursos B disponivel. Esse estado seria seguro, pois P pode ser executado.

A execução do algoritmo fica a seguinte:

1. Procura por uma linha/processo P ainda não executado, tal que a qauntidade exigida de cada recurso seja menor que a disponivel. Caso esse processo não exista, temos um deadlock.
2. Executo o processo P e acrescento os recursos que estavam sendo usados por ele nos recursos disponiveis.
3. Repito 1 e 2 ate que todos os processos sejam executados.

## Visão geral de IO no MINIX

### Interrupt handlers e acesso IO no MINIX

Muitos drivers de dispostivos inicializam o dispositivo de IO e então se bloqueiam, esperando ate que uma mensagem chegue, essa mensagem é enviada pelo **interrupt handler**.

Temos 4 diferentes niveis de acesso que um *user-space device driver* pode querer.

- Um driver pode precisar de acesso a memoria que nao esta em seu espaço. Por exemplo, o driver de memoria ram
- Um driver pode precisar ler/escrever nas portas de IO, uma operação que so o kernel pode fazer
- Um driver pode ter que responder a uma interrupção prevista.
- Um driver pode ter que responder a uma interrupção não prevista

Todos esses casos são suportados por kernel calls providas pelo systask.

### Drivers de dispositvos no MINIX

Para cada classe de dispositivos IO temos um device driver separado, que são processos. Tais drivers se comunicam com o sistema de arquivos atravez do sistema padrão de envio de mensagens.

No MINIX, para um processo ler um arquivo, ele manda uma mensagem para o processo do sistema de arquivos. O FS entao manda uma mensagem para o *disk driver*. o DD então usa kernel calls para pedir ao systask que faça a verdadeira IO e copiar os dados entre os processos.

Todos os drivers do MINIX funcionam da mesma maneira: recebendo mensagens. Para os dispotivos de bloco, temos, normalmente, os seguintes parametros na mensagem:

- m_type : operação requisitada
- DEVICE : minor device number
- PROC_NR : processo requisitando IO
- COUNT : contagem de bytes
- POSITION : posição no dispositivo

No UNIX, todos os processos tem 2 partes, uma parte do kernel e outra do user-space. Logo, os *device drivers* são apenas procedimentos no kernel que são invocados pela *kernel-space part* do processo.

### Device-independent IO software in MINIX

O processo do file system contem todo o codigo de *device independent IO*. Alem de lidar com a interface entre drivers, buffering e alocação de blocos, o FS tambem lida com a segurança e manutenção dos i-nodes, diretorios e _mounted file systems_.

### User-level IO software in MINIX

O MINIX tem todas os procedimentos de biblioteca requeridos pelo padrao POSIX. O *spooler deamon* é o **LPD**, que coloca um arquivo no spool de impressão atravez do comando `lp`.

### Deadlock handling in MINIX

O MINIX segue a mesma filosofia do UNIX com respeito ao tratamento de deadlocks: ele so ignora.

## Dispositivos de blocos no MINIX

Vamos discutir o RAM disk, hard disk e floppy disk.

### Visão geral de *block device drivers* no MINIX

O MINIX sempre tem 2 *block device drivers* compilados no sistema: o RAM disk driver e hard/floppy disk driver. O driver para cada dispositivo é compilado separadamente, mas uma biblioteca comum de codigo fonte é compartilhada entre eles.

Um driver contem uma função com um loop infinito, que fica checando se uma mensagem chegou.

Temos 6 operações que podem ser requisitadas á qualquer device driver, elas ficam no campos **m_type** da mensagem:

| operação | Descrição
| --- | ---- |
| OPEN | Verifica se o dispositivo esta acessivel |
| CLOSE | Verifica se não há nada em algum buffer para ser feito no dispositivo |
| READ | Pega um bloco do device e coloca na memoria |
| WRITE | Pega um blcoo da memoria e coloca no device |
| IOCTL | Usado para pegar/mudar informações acerca do device |

### Common block device driver software

Definições que são utilizadas por todos os driver ficam em `drivers/libdriver/driver.h`.

## RAM disks

Vamos estudar em detalhes o memory driver, que é usado para permitir acesso a qualquer parte da memoria.

### RAM disk hardware e software

O RAM disk tem basicamente duas funções: escreve um bloco e lê um bloco.

Em um sistema que tem suporte a **mounted files systems** temos um **dispositivo raiz** que esta sempre presente na mesma localização. Assim, sistemas de arquivos removiveis podem ser montados na arvore de arquivos, formando um sistema de arquivos integrado. Assim o usuario não precisa se preocupar em qual dispositivo esta cada arquivo.

## Discos

Todos os computadores modernos, execetos sistemas embarcados, possuem *disk drivers*.

### RAID

Acronomo de **redundant array of independent disks**. Uma maneira de implementar paralelismo no disk IO. Nesse sistema, temos que varios discos são conectados em paralelos e gerenciados por um **RAID controller**, assim, a sessão 0 fica com o disco 0, a sessão 1 fica com o disco 1 e assim por diante. Quanto escrevemos um arquivo, cada parte dele sera escrita em um disco diferente, de maneira simultanea.

Ao ler um arquivo, o driver fica resposavel por colocar os pedaçoes em order para formar o arquivo correto, chamamos isso de **striping**.

Poderiamos implemetar o *RAID level 1* e fazer com que aja 100% de redundancia, assim, caso um disco falhe, temos outro.

## Terminals

NÂO RESUMI

# Memory management (MM)

Nos computadores temos uma **hierarquia de memoria**. A memoria cache é a mais rapida, mas a mais escassa. A memoria RAM é rapida e consiguimos ter alguns gbs dela. E armazenamos no disco centanas de GB que são acessados e forma lenta.

O **memory manager** cuida dessa hierarquia. Em muitos sistemas (mas não no MINIX), ele faz parte do kenrel.

## Basic memory management

Podemos dividir os sistemas de MM em 2 tipos:

- sistemas que movem processos entre a memoria principal e o disco durante a execução, **swapping and paging**
- sistemas que não fazem o que esta acima

### Monoprogramming without Swapping or Paging

O jeito mais simples de MM é executar apenas um programa/processo por vez, fazendo com que a memoria seja compartilhada entre esse programa e o SO. Temos 3 esquemas principais de fazer isso:

- Colocar o SO no inicio da RAM e o programa no que sobrar. Usado por mainframes antigamente
- Colocar o SO na memoria ROM e o programa na RAM. Usado em alguns sistemas embarcados
- Colocar o SO e o programa na RAM e os device drivers na ROM. Usado no inicio da computação, a parte que ficava na ROM era chamada de **BIOS**.

### Multiprogramming with Fixed Partitions

O jeito mais facil de fazer MM quando temos multi processos é simplesmente dividir a memoria em *n* partições, com tamanhos possivelmente diferetes. Uma solução pra a criação dessas partições seria criar varias delas, com tamanhos difentes, assim que o sistema é iniciado, chamado **MFT**, Multiprogramming with a Fixed number.

Nesse modelo apresentado acima, as partições possuem tamanho fixo. Um jeito de alocar uma partição para um processo seria colocar ele em uma fila na menor partição que pode armazena-lo. Outra alternativa seria termos uma fila unica, onde um proceso pode ser colocado na primeira partição livre que tiver espaço para ele. Em ambos os casos temos desperdicio de espaço.

### Relocation and protection

Com a solução acima, sabemos que diferentes processos vão estar em diferentes regiões da memoria. Como fazer com que programas/bibliotecas linkados a esse processo saibam o endereço e consigam acessar os dados de maneira correta? Ademais, como garantir que um processo não acesse o espaço de memoria de outro?

A solução para isso foi fazer com que a maquina tivesse mais dois registradores, **base** e **limit**. Quando um processo é escalonado, o registrador base recebe o inicio do endereço de memoria desse processo, e o limit recebe o final. Assim, bibliotecas conseguem se limitar a aquele espaço de memoria usando algebra simples, e a segurança é garantida vendo se o acesso foi entre esses dois limites.

## Swapping

Nos sistemas modernos, nem todos os processos cabem na memoria principal no mesmo momento, logo, alguns processos precisam ser guardados em disco e trazidos para a memoria quando necessarios.

A estrategia mais simples para lidar com isso é o **swapping**, que implica transferirmos o processo inteiro entre a memoria principal e o disco. Para esse metodo, um sistema de **partição dinamica**, onde as partições não tem um tamanho fixo desde o inicio do sistema, seus tamanhos vão sendo alterados durante a execução.

O fato acima, sem duvida, aumenta a eficiencia da memoria, causando menos desperdicio. Outra coisa que podemos fazer é a **memory compaction**, fazendo com que todas a partições fiquem *juntinhas*, mas isso não é geralemnte feito pois usa muita CPU.

Quando temos processos que podem aumentar o tamanho de memoria alocada durante sua execução, é uma boa pratica, ao trazer o processo do disco para a memoria, deixar um espaço extra entre esse processo e outro, para que, se tal processo deseje aumentar de tamanho, ele não tenha que esperar.

### Memory management with bitmaps

Uma necessidade é saber, de maneira rapida, quais partes da memoria estão livres. Em um **bitmap** cada bit ira representar uma porção de memoria, 0 esta vazio e 1 esta ocupada. O problema é decidir o tamanho da porção representada. Se 1 bit representar uma porção muito pequena, nosso bitmap vai ficar muito grande. Se 1 bit representar uma porção muito grande, podemos perder precisão.

Quando se deseja trazer um processo que tem tamanho de *k-porções*, o MM vai procurar no bitmap por uma sequencia de *k* zeros, posições vazias da memoria que podem armazenar o processo.

### Memory management with linked lists

Uma outra maneira de resolver o problema das partes da memoria que estão livres é usar uma lista ligada. Tal lista tem os nos **process (P)** e **hole (H)**. Cada nó especifica o inicio de uma região na memoria, o seu tamanho e o proximo item da lista. Um exemplo de tal lista seria:

`P[0, 5, ->] -> H[5, 1, ->] -> P[6, 10, -]`

No exemplo acima, a fila esta ordenada por order de inicio de endereço. Essa ordenação, alem de ser facil de ser mantida, facilita o processo de alocar ou desalocar memoria. Com isso, podemos ter os seguintes algoritmos para alocação de memoria:

- **first fit**: percorre a lista ligada desde o começo ate achar uma posição vazia que tem capacidade para receber o novo processo. O hole é quebrado em 2 novos nos, um processo e um novo buraco (menor). É rapido pois acaba assim que acha
- **next fit**: mesma coisa que o de cima, mas começa a busca a partir do ultimo ponto de parada. É pior.
- **best fit**: percorre a lista inteira e tenta achar o menor buraco que pode armazenar o processo. Além de demorar mais, não apresenta ganho no proveito de memoria, pois deixa pedaços muito pequenos vazios, que não poderão ser usados por outros processos.
- **worst fit**: acha o maior buraco, para tentar solucionar o problema do de cima. Mas tambem não é bom.
- **quick**: cria um vetor com varios tamanhos de buracos, uma posição desse vetor vai conter a cabeça de uma lista ligada que vai armazenar todos os buracos com aquele dado tamanho. É muito rapido para achar um novo buraco, mas extremamente ruim para desalocar.

## Virtual memory

Em um certo momento começaram a aparecer programas tão grandes que eles não cabiam na memoria. A solução adotada em dividir o programa em pedaços, chamados **overlays**. Dessa maneira, a primeira overlay começava a ser executada, depois outra, ate que o programa terminasse. As overlays eram mantidas no disco e trazidas de maneira dinamica para a memoria, pelo SO. Quem dividia o programa em pedaços era o proprio programador.

O trabalho do programador de dividir o programa chegou ao fim ao a **memoria virtual**. Com ela, o disco pode ser visto como a extensão da memoria RAM, e o programa e divido entre esses dois recursos, com a parte mais usada ficando na memoria principal.

### Paging

Tecnica usada por sistemas de memoria virtual. Em qualquer computador existe conjunto de endereços de memoria que programas podem gerar. Por exemplo, a operação `MOV REG,1000` move o conteudo do endereço de memoria 1000 para REG.

Esses endereços gerados pelos programas são os **virtual addresses** e formam o **virtual address space**. Em computadores sem memoria virtual, esse endereço é traduzido diretamente para a memoria RAM. Quando usamos a memoria virtual, esse endereço vai para o **MMU (memory management unity)**, que mapeia esse endereço para as duas opções de memoria fisica (RAM e disco).

O espaço da memoria virtual é dividido em **paginas**, e suas representações na memoria fisica são chamadas **page frames**, ambas tem o mesmo tamanho. Quando tranferencias são realizadas entre a RAM e o disco sempre se tranfere uma pagina inteira.

O numero de _paginas_ é sempre maior que o numero de _page frames_, então, existe um **present/absent bit**, presente no hardware, que diz se aquela esta presente ou não na memoria fisica.

Quando um programa tenta acessar uma pagina que o MMU não mapeou o MMU faz com que a CPU faça uma *trap* no SO, chamada de **page fault**. O SO então pega uma page frame que não foi muito usada, coloca ela de volta no disco e muda o mapeamento para que a pagina que ainda não mapeada se refencie para essa nova page frame livre. Dai então a trap e retirada e a operação reinciada.

Para realizar esse mapeamento, o MMU usa a **page table**, assim, os primeiros indices de um endereço passado para o MMU representam um indice na tabela. Esse indice, ao ser acessado, possui o *p/a bit* e o endereço para o page frame. O restante dos bits do enreço de entrada é chamado de **offset** e é passado para complementar o endereço final.

Um exemplo, podemos dividir o endereço de entrada em X+K (XXXXKKKKKKK). Acessamos o indice X da tabela e pegamos o valor Y. O endereço que sai do MMU é o Y+K (YYKKKKKKK).

### Page tables

O jeito mais simples de mapearmos memoria virtual para memoria fisica é fazer o que descrevemos acima. O endereço virtual é dividido em um **vitual page number (high order bits)** e um **offset (low order bits)**. Por exemplo, com endereços de 16-bits e uma pagina de 4KB (a pagina precisa ter o tamanho de 2^|offset|. Aqui é 4096 == 2^12), poderiamos usar os 4 primeiros bits do endereço como VPN para uma das 16 paginas e outros 12bits especificariam o offset (0 ate 4095) na pagina selecionada.

Temos dois principais desafios: a tabela de paginas pode ser grande e o mapeamento precisa ser rapido. O jeito mais simples de montar a tabela é fazer uma tabela unica. Porem, a seguir, veremos dois metodos mais robustos.

#### Multilevel page tables

Para nos livrarmos do problema de precisarmos ter a _page table_ (PT) completa  na memoria, podemos criar mais um nivel. Assim, um _virtual adress_ de 32bits pode ser dividido em **10bit PT1 adress**, um **10bit PT2 adress** e um **12bit offseet**. Assim usamos o endereço de PT1 para acessar PT2, e PT2 para acessar a posição na memoria fisica. Note que todas as possiveis PT2 não precisam ficar na memoria principal.

#### Estrutura de uma entrada na page table

O principal campo é o **page frame number**, ate porque esse é todo o objetivo de termos PTs. Em seguida tempos o **present/absent bit** (1-entrada é valida e pode ser usada | 0-a pagina virtual não esta atualmente na memoria). Temos o **protection bit** (rwx). Temos um **modified/referenced bit**, que avisa se aquela pagina foi modificada, pois isso significa que sua versão no disco deve ser modificada. O ultimo bit avisa se o cache deve ou não ser habilitado.

### TLB - Translation lookaside buffers

Foi percebido que os processos costumam fazer uma grande quantidade de referencias para uma pequena quantidade de paginas. Sabendo disso, foi criado um novo buffer no hardware que serve como um _cache_ para a memoria virtual, onde acessos podem ser feitos de forma mais rapidas. É uma especie de mapeamento direto, onde a tabela possui pouquissimas entradas.

## Page replacement algorithms

Quando uma _page fault_ acontece o sistema operacional precisa escolher uma pagina pra ser retirada da memoria e assim conseguir espaço para a pagina que precisa ser acessada. Se a pagina escolhida para ser retirada foi modificada enquanto estava na memoria, sua versão no disco precisa ser atualizada. Caso contrario, basta sobrecreve-la.

### Algoritmo otimo para page replacement

O algoritmo otimo é facil de ser descrito e impossivel de ser implementado. O algoritmo funciona da seguinte maneira:

Quando uma _page fault_ acontece, algum conjunto de paginas estão na memoria. Poderiamos entiquetar cada uma dessas paginas com o numero de execuções que irão acontecer em seguida que não usarão tal pagina. O algoritmo perfeito, removeria a pagina que vai demorar mais tempo ate ser usada, ou seja, a pagina com a maior etiqueta.

### The Not Recently Used Page Replacement Algorithm

A maioria dos computadores com memoria virtual possuem 2 bits associados a cada pagina. O bit `R` é ligado toda vez que uma pagina é referenciada (lida ou escrita) e `M` é ligado toda vez que a pagina é modificada. Se o hardware não possui esses bits, é possivel que eles sejam emulados pelo SO.

O algoritmo que utiliza os bits R e M é descrito a seguir:

Quando um processo é inicializado, todos os bits RM de suas paginas são setados como 0. Periodicamente o bit R e resetado para distiguirmos os processos que foram referenciados mais recentemente. Quando uma _page fault_ acontece, o SO inspeciona todas as peginas e as classifica em 4 classes, de acordo com os bits RM:

- classe 0: não referenciado, não modificado
- classe 1: não referenciado, modificado
- classe 2: referenciado, não modificado
- classe 3: referenciado, modificado

O algoritmo então, escolhe uma pagina aleatoria, da classe mais baixa (mais proxima de 0) não vazia. O algoritmo não é top, mas é consideravelmente eficiente e de facil implementação.

### The First-In, First-Out (FIFO) Page Replacement Algorithm

O SO mantem uma lista de todas as paginas na memoria, onde a pegina na frente da lista é a mais velha e a que esta no fim da fila é a mais nova. Quando uma _page fault acontece_ a pagina mais velha é removida para dar espaço. Facil de implementar mas não muito bom.

### The Second Chance Page Replacement Algorithm

Uma alternativa para a algoritmo FIFO, que evita que paginas muito acessadas sejam retiradas da memoria é dada a seguir. Da mesma maneira acima, queremos remover o processo mais antigo, porem, checamos se o seu bit R (diz se o processo foi referenciado recentemente) esta ligado. Caso positivo, esse bit é desligado e a pagina é colocada no fim da fila, como se tivesse acabado de entrar, e então a busca continua. Caso negativo, basta retirarmos essa pagina velha que esta com o bit R desligado.

Desligarmos o bit R quando colocamos uma pagina de volta na fila garante que o algoritmo vai acabar.

### The Clock Page Replacement Algorithm

Similar ao algoritmo acima, porem, a implementação utiliza uma lista circular e um ponteiro aponta a pagina mais velha.

### The Least Recently Used (LRU) Page Replacement Algorithm

É a melhor aproximação do algoritmo otimo. Esse algoritmo consiste na observação de que paginas que foram muito acessadas nas ultimas instruções provavelmente vão continuar sendo muito acessadas. E como consequencia, as paginas pouco acessadas vão continuar assim. A ideia se resume em: quando uma _page fault_ acontece, jogamos fora a pagina que não foi usada por mais tempo.

Esse algoritmo é muito custoso, pois toda vez que uma pagina é referenciada, precisamos atualizar o seu tempo de ultimo acesso, e para fazermos isso precisamos percorrer a lista.

Entretanto, podemos fazer implementações eficientes com auxilio do hardware.

A primeira solução é equipar um contador C no hardware, que é incrementado toda vez que uma instrução é feita. Assim, quando uma pagina é referenciada, basta guardarmos nela o valor atual de C. Assim, basta descobrirmos a pagina com o menor valor de C e retirarmos ela.

Outra maneira é a seguinte: se temos N page frames, vamos manter uma matriz NxN. Quando uma page frame K é referenciada, colocamos todos os bits da k-esima linha como 1 e todos os bits da k-esima coluna como 0. Em qualquer instante, a linha com o menor valor binario é a menos usada.

### Simulando LRU em software

Muitas maquinas não possuem os artefatos acima. Devido a isso, precisamos recorrer para alternativas em software.

Uma das soluções é o algoritmo **NFU (not frequently used**). Cada pagina tem um contador, inicizalidao com 0. A cada _clock interrupt_ o SO passa por todas as paginas e incrementa R no contador. Com isso, retiramos da memoria a pagina com o menor contador.

## Design issues for paging systems

### The working set model

Processos são iniciados com nenhuma de suas paginas na memoria. Conforme ele vai sendo executado, tudo o que ele precisa vai indo para a memoria e cada vez menos _page faults_ vão acontecendo. Isso é chamado de **demand paging**, pois as paginas são carregadas _on demand_.

Muitos processos aprensentam **locality of reference**, ou seja, durante alguma fase de execução o processo vai referenciar apenas uma pequena porção de suas paginas.

O conjunto de paginas que estão sendo utilizadas por um processo são chamadas de **working set**. Se todo o working set esta na memoria, poucas page fault irão acontecer. Se a memoria for muito pequena e não armazenar grande parte do working set, vão acontecer muitas page faults, quando isso acontece com muita frequencia, dizemos que o processo esta **thrashing**.

Num sistema de multiprogramação, o principal problema surge quando trazemos um processo de volta para a memoria principal. Com esse ato, muitas _page fault_ irão acontecer. Com isso, muitos sistemas de paginamento trazem de volta algumas paginas associadas ao processo, quando este esta sendo carregado na memoria. Isso evita _demand paging_ e é chamado de **prepagin/working set model**.

### Local versus global allocation policies

Acima, quando discutimos algoritmos de alocação de memoria, pulamos um detalhe: devemos retirar uma pagina do mesmo processo ou de qualquer processo no sistema?

Dizemos que o algoritmo de _page replacement_ é **local** se ele so remove paginas do mesmo processo. Isso corresponde a alocar um tamanho fixo de memoria para cada processo.

Já um algoritmo **global** pode retirar uma pagina de qualquer outro processo. Isso faz com que a quantidade de memoria usada por cada processo passe a ser dinamica.

Em geral, algoritmos globais funcionam melhor.

### Virtual memory interface

Podemos fazer com que o programador tenha acesso ao MMU. Dessa meneira pode se fazer com que processos compartilhem regiões de memoria. Com isso, alem das aplicações basicas, pode se implementar um sistema rapido para passagem de mensagens.

## Segmentation

Ideia para que um processo organize seu espaço de memoria. Assim, ele pode ter segmentos, que é constituido por uma sequencia continua de endereços, que podem variar de tamanho de maneira independente. Essa estruta é definida e esta visivel para o programador. É como se estivessemos criando _pseudo virtual adresses_ dentro da execução do programa.

Organizar a memoria em segmentos tambem facilita com que haja compartilhamento de memoria entre processos.

Um segmento tambem pode apresentar uma proteção especifica, ou seja, podemos dizer que um segmento so pode ser executado e que não pode ser modificado. Isso não faz sentido de ser feito na memoria virtual, uma vez que o programador não tem ciencia de como as paginas estão sendo criadas e mantidas.

Comparação entre paging e segmentação

| Consideração | Paging | Segmentation |
| --- | --- | --- |
| Need the programmer be aware that this technique is being used? | NO | YES |
| How many linear address spaces are there? | 1 | MANY  |
| Can the total address space exceed the size of physical memory? | YES | YES |
| Can procedures and data be distinguished and separately protected?  | NO | YES |
| Can tables whose size fluctuates be accommodated easily? | NO | YES |
| Is sharing of procedures between users facilitated? | NO | YES |
| Why was this technique invented? | To get a large linear address space without having to buy more physical memory | To allow programs and data to be broken up into logically independent address spaces and to aid sharing and protection |

### Implementation of pure segmentation

A implementação de segmentos difere da implementação de paginação na seguinte maneira: paginas tem tamanho fixo, segmentos não. Com a aumento/diminuição dos segmentos, vamos criando buracos na memoria, esse fenomeno é chamado de **checkerboarding** ou **external fragmentation**. A solução para isso é realizar compactação.

### Segmentation with Paging: The Intel Pentium

O pentiu pode ser setado pelo SO para usar so segmentação, so paginação ou ambos. O coração disso esta em duas tabelas, **LDT (local descriptor table)** e **GDT (global descriptor table)**. Cada programa tem sua propria LDT, mas so existe uma GDT compartilhada por todos os programas. A LDT descreve os segmentos locais de cada programa e a GDT descreve segmentos do sistema em si.

# File systems

Quando um processo so armazena informação na memoria temos 3 problemas:

- O espaço na memoria é limitado, não se pode guardar arquivos muito grandes
- Quando o processo acaba, toda a informação é perdida
- As vezes é necessario que multiplos processos tenham acesso a informação

A solução para esses problemas é transformar essas informações em **arquivos** e guarda-las nos discos ou em algum dispositivo externo.

Chamos de **file system (FS)** a parte do sistema operacional que gerencia tais arquivos.

## Arquivos

### File naming

Arquivos são um mecanismo de abstração, uma maneira de ler/escrever informações no disco.
