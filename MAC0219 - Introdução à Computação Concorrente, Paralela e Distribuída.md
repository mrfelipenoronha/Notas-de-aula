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
Operação que acontece na CPU, que não pode ser iterrompida.

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
Controle de acesso a um recurso. Conta o numero de processos que podem usar um recursoao mesmo tempo sem que nenhum outro deixe de usar.

 - **MUTEX**: Semaforos que so permitem valores 0 e 1. So permite o acesso a uma thread por vez, ou seja, uma tarefa que roda num semafaro vai ser a unica rodando naquele momento, é meio que uma forma de deixar só uma parte da tarefa sequencial.

### Variaveis de condição
Faz com que uma thread espere ate que uma condição seja satisfeita.

### Barreiras
Quando uma thread entra em uma barreira ela não pode terminar ate que todas as outras threads tenham entrado. Uteis para sincronização.

### Dependencia de dados
So se pode paralelizar tarefas independetes.

---
