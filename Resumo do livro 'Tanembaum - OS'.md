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
