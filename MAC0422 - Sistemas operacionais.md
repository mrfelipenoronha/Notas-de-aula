
# Indice

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [Indice](#indice)
- [MINIX](#minix)
- [Conceitos de sistemas operacionais](#conceitos-de-sistemas-operacionais)
   - [Processos](#processos)
   - [Arquivos](#arquivos)
   - [Pricipais chamadas de sistema do MINIX](#pricipais-chamadas-de-sistema-do-minix)
- [Race conditions e mutex (mutual exclusion)](#race-conditions-e-mutex-mutual-exclusion)
   - [Como resolver](#como-resolver)
      - [Solução 1: inibir interrupções](#solução-1-inibir-interrupções)
      - [Solução 2: implementar exclusão mutua por software](#solução-2-implementar-exclusão-mutua-por-software)
      - [Solução 3: exclusão mutua por hardware, test-and-set](#solução-3-exclusão-mutua-por-hardware-test-and-set)
   - [Semáforos](#semáforos)
      - [Sistemas produtor consumidor](#sistemas-produtor-consumidor)
   - [Monitories](#monitories)
- [Comunicação entre processos: envio de mensagens](#comunicação-entre-processos-envio-de-mensagens)

<!-- /MDTOC -->


Professor: Alan Durhan
Email: aland@usp.br

Bibliografia:
- Operating systems. Tanembaum (livro de apoio para minix)
- Sistemas operacionais. Silberschatz (livro de referencia)

Essas notas de aula são um misto entre coisas vistas em aula e outras fontes como os livros e notas de aulas de amiguinhos, como as [do Renato Ferreira](github.com/renatocf/MAC0422).

# MINIX

Sistema criado pelo Tanembaum na decada de 80. É um sistema ue tem muito semelhança com a versão 7 do unix e foi concebido com a ideia de ser usado nas universades, para o ensino sobre sistemas operacionais, sendo um software open source.

Nessa materia usaremos o sistema `minix`.

# Conceitos de sistemas operacionais

A interface entre o SO e os programas que o usuario usa é definida por um conjunto de *intruções extendidas* que o SO provem, são as **system calls**, que tendem a mudar de sistema para sistema, mas que possuem, em seu cerne, o mesmo conceito.

As chamadas de sistema do minix lidam com duas categorias: processos e sistema de arquivos.

## Processos

Um processo é basicamente um programa em execução. Associado a cada processo existe um **adress space**, uma lista de posições na memoria que o programa pode ler|escrever. Neste espaço fica todas as coisas coisas que serão utilizadas pelo programa, incluindo (pasmem), o proprio programa.

Um conceito muito interessante sobre processos é a **multiprogramação**, que é quando o SO decide *pausar* um processo e colocar outro para ser executado na CPU. Com isso, todas as informações acerca desse processo suspenso precisam ser salvas, para que depois elas possam ser recuperadas e o processo volte ser executado de onde ele parou. A estrutura que armazena essas informações é a **process table**, implementada em uma array ou lista ligada.

O processos podem invocar outros processos, um **processo filho**, e muita das vezes se faz a necessidade de realizar uma **interprocess comunication**. Cada processo tem uma identificação, **PID**. Matar um processo pai termina, tambem, os processos filhos.

Todas essas rotinas são realizadas por **system calls**

Um proceso pode se encotrar em 5 estados diferentes, são eles:

- Blocked: Esperando pelo resultado de uma chamada ao sistema;
- Ready: Pronto para rodar, mas não necessariamente rodando (outro programa de maior prioridade pode estar ocupando a CPU. Nesse estado, ele só precisa ocupar a CPU);
- Blocked Suspended: Se muitos programas estão rodando ao mesmo tempo, um programa pode ser suspenso, e ficar aguardando até que o sistema se desocupe. Nesse estado, ele ainda está aguardando por alguma chamada ao sistema;
- Ready Suspended: Quando o programa recebe sua requisição ao SO, fica Ready, mas ainda estará suspenso;
- Running: O programa ocupando a CPU.

## Arquivos

Chamadas do sistema são necessarias para criar/remover/ler/escrever arquivos de forma facil e eficiente. Se o acesso á um arquivo é requisitado existem 2 opções: O acesso foi permitido, nesse caso, o SO retorna um **file descriptor**; O acesso foi negado é um codigo de erro (-1) é retornado.

Para agrupar arquivos o minix usa **diretorios**, sendo que um diretorio pode conter arquivos ou outrso diretorios, criando assim uma hierarquia, o **sistema de arquivos**, que muitas vezes pode ser representaod como uma especie de arvore. Todo arquivo dentro de um diretorio pode ser especificado atravez de um **path name** que parte do **root directory**.

Podemos ainda, definir um **working directory**, onde todo processo executado dentro dele o enxerga como raiz.

Por ultimos, temos os **pipes**, que são *pseudoarquivos* usados para conectar dois processos distintos. Assim, basta que o processo A escreva em um pipe e que o processo B leia este arquivo para realizar uma conexão/comunicação.

## Pricipais chamadas de sistema do MINIX

- fork (única maneira de criar novos processos)
    - Gera duas cópias idênticas nas memórias separadas
    - Chamada volta zero para processo filho, e pid para o processo pai

- wait()
    - Processo pai aguarda o final de execução de um dos filhos

- execve(name, argv, envp)
    - A imagem do processo é substituída pelo conteúdo de um arquivo, passados argumentos e ambientes (que descreve o terminal, etc).
    - Essa é a maneira de um programa "mutar", e deixar de ser igual ao processo pai. A imagem vem de algum arquivo executável, que deve ser passado como argumento.

- exit()
    - Termina o processo e retorna o parâmetro como código de saída (o zero representa OK). O código único (0) é bom porque todos os outros podem ser usados como erro.

- brk()
    - Altera o tamanho da área de dados do parâmetro (processo)

- getpid()
    - Pede o ID do processo (para poder comunicar via fork).

- signal(sig,func)
    - Determina captura de um sinal pela função func (em geral, se não der para tratar, o processo é morto).

- kill(pid,signal)
    - Manda um sinal a um processo. Existe um sinal KILL que realmente mata um processo - mas, de forma geral, ele só funciona para processos filhos.

- alarm(seconds)
    - Agenda um envio do sinal SIGALARM, para que o processo seja avaliado depois de certo tempo. Usando signal, é possível dar tratamentos depois.

- pause()
    - Suspende o processo ate que este receba o próximo sinal.

- creat(name,mode)
    - Cria um novo arquivo e abre, ou abre e seta o tamanho para zero, retornando o descritor (como 'touch')

- open(file,how)
    - Abre um arquivo considerando o nome dele e se terá leitura/escrita. Devolve um descritor de arquivo.

- close(fd)
    - Fecha um arquivo aberto.
    - É importante para que os i-nodes sejam salvos, o que não ocorre se morrer.

- read(fd,buffer,nbytes)
    - Lê dados em um buffer, retorna bytes lidos.
    - Lê até nbytes, ou menos. Retorna o número de bytes realmente gravados.

- write(fd,buffer,nbytes)
    - Grava dados de um buffer e retorna o número de bytes gravados (porque poderia dar problemas).

- lseek(fd,offset,whence)
    - Muda o poteiro do arquivo, retona a nova posição para posição. Whente indica se o offset relativo à posição atual, início ou fim.

- stat(name,&buf),fstat(fd,&buf)
    - Lê e retorna o estado do arquivo

- dup(fd1)
    - Aloca um novo descritor de arquivos (número que descreve
      qual é o i-node que representa o arquivo) para o arquivo
      aberto. Dup cria um novo número para o mesmo i-node.
    - Dup costuma ser útil para SUBSTITUIR o arquivo apontado
      por aquele descritor específico, e depois retornar. É

- pipe(&fd[0])
    - Cria um pipe, retorna fd de leitura e escrita no vetor.

- ioctl(fd,request,argp)
    - Executa operações em arquivos especiais (em geral terminais)

- link(name1,name2)
    - Faz entrada name2 ser o mesmo i-node que name1

- unlink(name)
    - Remove uma entrada do diretório corrente

- mount(special,name,rwflag)
    - Monta um sistema de arquivos

- unmount(special)
    - Desmonta um sistema de arquivos

- sync()
    - Sincroniza todos os blocos de disco do cache

- chdir(name)
    - Muda o diretório corrente
    - O cd, do Bash, usa esse programa

- chroot(dirname)
    - Muda o diretório raiz (cuidado)
    - Funciona APENAS para a shell correspondente

- chmod(name,mode)
    - Muda os bits de proteção do arquivo

- uid = getuid()
    - Retorna o UID do usuário

- gid = getgid()
    - Retorna o GID do usuário.

- setuid()
    - Muda o UID do usuário.

- setgid()
    - Muda o GID do usuário.

- time(&seconds)
    - Retorna o tempo em segundos desde 1/1/1970.

- stime(tp)
    - Reinicializa o tempo desde 1/1/1970.

- utime(file,timep)
    - Seta o valor do último acesso do arquivo.

- times(bufer)
    - Retorna o tempo do usuário e de sistema.

# Race conditions e mutex (mutual exclusion)

Processos rotineiramente precisam se comunicar:
- Se processo quer ler arquivo, precisa enviar requisição ao sistema de arquivos
- Sistema de arquivos precisa se comunicar com driver
- Num esquema de pipes, a saída de um processo precisa ser comunicada a outro processo

Em muitos sistemas operacionais, quando processos trabalham juntos, eles compartilham algum armazenamento comum onde cada um pode ler e escrever. Normalmente memoria ou um arquivo.

Quando processos funcionam de maneira independente, isso pode criar uma condição de concorrência (ou “condição de corrida”), ou seja, processos diferentes dependem de informação compartilhada e resultado depende do escalonamento de processos.

## Como resolver

Regiões dos processos que podem gerar condições de concorrência são chamadas **regiões críticas**.

Dessa maneira, definimos 4 condições para a sessão critica:
- Só um processo deve entrar na região crítica de cada vez
- Não deve ser feita nenhuma hipótese sobre a velocidade relativa dos processos
- Nenhum processo executando fora de sua região crítica deve bloquear outro processo
- Nenhum processo deve esperar um tempo arbitráriamente longo para entrar na sua região crítica (adiamento indefinido)

### Solução 1: inibir interrupções

O hardware possui instrução especifica para inibir interrupções, SO usa quando o codigo do kernel esta executando. Porem, é uma solução para quando temos problemas (loops infinitos, Operações inválidas, Etc)

### Solução 2: implementar exclusão mutua por software

A primeira soção é o **algoritmo de dekker**. Esse algoritimo escolhe um processo como *favorito*, o que acaba com a chance dos processos ficarem competindo pelo uso da CPU.

```
Int p1querEntrar= 0;
Int p2querEntrar= 0;
Int favorito = 1;

# Processo 1
While (TRUE){
    p1querEntar = TRUE;
    while (p2querEntrar){
        if (favorito == 2){
            p1querEntrar = FALSE;
            while (favorito == 2) {};
            p1querEntrar = TRUE;
        }
    }; /*espera*/
    …..< região crítica>…..
    favorito = 2;
    p1querEntrar= FALSE;
    ….<resto do código>…
}

# Processo 2
While (TRUE){
    p2querEntar = TRUE;
    while (p1querEntrar){
        if (favorito == 1){
            p2querEntrar = FALSE;
            while (favorito == 1) {};
            p2querEntrar = TRUE;
        }
    }; /*espera*/
    …..< região crítica>…..
    favorito = 1;
    p2querEntrar= FALSE;
    ….<resto do código>…
}
```

Uma segunda solução é o **algoritmo de peterson**. Uma melhoria do algoritmo de Dekker, sendo mais sucinta.

```
Int p1querEntrar= 0;
Int p2querEntrar= 0;
Int favorito = 1;

# Processo 1
While (TRUE){
    p1querEntar = TRUE;
    favorito = 2;
    while (p2querEntrar && favorito == 2) {};
    …..< região crítica>…..
    p1querEntrar= FALSE;
    ….<resto do código>…
}

# Processo 2
While (TRUE){
    p2querEntar = TRUE;
    favorito = 1;
    while (p1querEntrar && favorito == 1) {};
    …..< região crítica>…..
    p2querEntrar= FALSE;
    ….<resto do código>…
}
```

### Solução 3: exclusão mutua por hardware, test-and-set

É uma instrução especial do hardware, que é atomica. Com ela, obtemos o seguinte resultado `test_and_set( a , b ) =➔ {a = b; b = TRUE; }`

Com isso, podemos ter a seguinte implementação de exclusão mutua

```
Int ativo = FALSE;

# Processo 1
While (TRUE){
    int p1_nao_pode_entrar = TRUE;
    while (p1_nao_pode_entrar){
        test_and_set(p1_nao_pode_entrar,
        ativo);
    };
    …..< região crítica>…..
    ativo = FALSE;
    ….<resto do código>…
}

# Processo 2
While (TRUE){
    int p2_nao_pode_entrar = TRUE;
    while (p2_nao_pode_entrar){
        test_and_set(p2_nao_pode_entrar,
        ativo);
    };
    …..< região crítica>…..
    ativo = FALSE;
    ….<resto do código>…
}
```

## Semáforos

Especie de *variavel*, que possui duas ações atomicas:

- P/down(semaforo)
    - Se valor é >0 subtrai 1 e continua
    - Senão se coloca em uma fila e fica em espera
- V/up(semaforo)
    - Se semaforo tem fila, libera o primeiro
    - Senao incrementa o valor em 1

Temos a variação de **semaforo binario**, que so assume valores em [0, 1].

Com isso, temos a facilitação da implementação da exclusão mutua em varios processos.

O codigo usando um semaforo esta a seguir

```
Semáforo mutex = 1;
While (TRUE){
    P(mutex);
    ....<região crítica>…..
    V(mutex);
    ….<região não crítica>….
    }
```

### Sistemas produtor consumidor

```
Semaforo_binario mutex; /*exclusão mútua*/
Semaforo_contador vazio= TAMBUFFER; /*controle buffer*/
Semaforo_contador cheio = 0; /*controle buffer */

# Produtor
While (TRUE){
    registro item_produzido;
    produz(&item_produzido);
    P(vazio);
    P(mutex);
    coloca_item(item_produzido);
    V(mutex);
    V(cheio);
}

# consumidor
While (TRUE){
    registro item_consumido;
    P(cheio);
    P(mutex);
    pega_item(&item_consumido);
    V(mutex);
    V(vazio);
    consome(item_consumido);
}
```

## Monitories

É algo implementado pelo compilador, uma especie de objeto. Nesse objeto, somente 1 processo pode entrar nele por vez, esse controle de entrada é feito por tecnincas de exlusão mutua.

Processo pode emitir um **WAIT**, que “sai” do monitor entre em uma fila associada à varíavel da operação WAIT, o processo é reativado pela operação SIGNAL. Processo pode emitir SIGNAL logo antes de sair do monitor, quem estiver esperando na fila entra em seguida (se houver).

```
Monitor ProdutorConsumidor {
    condition full, empty;
    int count;
    procedure coloca_item(item) {
        if (count == TAMBUFFER) {WAIT(full);} /* espera buffer ter espaço*/
        entra_item(item); /*altera buffer comum*/
        count + + ;
        if (count == 1) {SIGNAL(empty)};/*avisa que tem dado*/
    }
    procedure pega_item(&item) {
        if (count == 0) { WAIT(empty);} /*espera dado*/
        remove_item(&item); /*retira do buffer*/
        count - - ;
    if (count == TAMBUFFER – 1) { SIGNAL(full)} /*buffer não está mais cheio*/
    }
}

# Produtor
While (TRUE){
    registro item_produzido;
    produz(&item_produzido);
    coloca_item(item_produzido);
}

# Consumidor
While (TRUE){
    registro item_consumido;
    pega_item(&item_consumido);
    consome(item_consumido);
}
```

Vantagem: exclusão mútua implementada automáticamente, menos sujeito a erros.
Desvantagem: necessário apoio do compilador

# Comunicação entre processos: envio de mensagens

Podemos usar semaforos ou monitores para enviar mensagens entre processos, mas isso exige que eles compartilhem memoria.

Ademais, podemos fazer uma comunicação direta entre processos, com `Send(destino, mensagem); Receive(fonte, &mensagem)`.

As mensagens tem algumas caracteristicas/questões:

- Comunicação síncrona ou assíncrona?
    - Síncrona: como uma chamada de telefone. Quando um envia a mensagem, o outro já recebe.
    - Assíncrona: como uma carta. Quando um envia a mensagem, o outro pode receber depois de algum tempo.
- Confirmação
    - Mensagem de confirmação para certificação de entrega.
    - Retransmissão se certificação for muito demorada.
    - Mensagens duplicadas devem ser ignoradas (contar mensagens). Isso é necessário porque, se uma mensagem demorar muito, ela pode ser reenviada. Porém, no final, ambas podem chegar.
- Endereçamento
    - Serve para identificar se há comunicação entre máquinas.
    - processo@maquina, processo@maquina.dominio
- Autenticação: criptografia
- Eficiência quando na mesma máquina.

## Problema: Filosofos comilões

**COMPLETAR COM COISAS ATE O SLIDE 88**
# Escalonamento de processos
