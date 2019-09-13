
 Indice

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [Introdução a lisp/racket](#introdução-a-lispracket)
- [Parsing](#parsing)
- [Interpretando](#interpretando)
- [Construindo nossa primeira linguagem, soma e multiplicação](#construindo-nossa-primeira-linguagem-soma-e-multiplicação)
   - [Explicação sobre o parser](#explicação-sobre-o-parser)
- [Sugar, yes please](#sugar-yes-please)
- [Condicionais](#condicionais)
- [Criando executaveis de racket](#criando-executaveis-de-racket)
- [Nova linguagem, ExprC](#nova-linguagem-exprc)
- [Escopo dinamico](#escopo-dinamico)
- [O problema da definição de função](#o-problema-da-definição-de-função)
- [Closure](#closure)
   - [Exemplo em python](#exemplo-em-python)
- [Boxes](#boxes)
- [Trivias/piadas que o Gubi contou em aula](#triviaspiadas-que-o-gubi-contou-em-aula)

<!-- /MDTOC -->

A bibliografia usada é [Programming Languages: Application and Interpretation](http://cs.brown.edu/courses/cs173/2012/book/).

A a linguagem usada na materia é **racket** e o interpretador **drracket**.

# Introdução a lisp/racket

Usamos a chamada `#lang racket` para definir a linguagem usada no script.

Quando colocamos `'` antes de alguma expressão tranformamos ela em um simbolo, ou seja, é so uma lista de valores sem nenhuma operação a ser realizada.

Exemplos de execução no interpretador

```lisp
> (/ 12 5.)
2.4

> (+ 1 2 3) # Lista com operação a ser realizada, soma
6

> (car '(4 22 1 4)) # Retorna o primeiro elemento da lista
4

> (cdr '(4 22 1 4)) # Retora o restante
'(22 1 4)
```

Agora vamos fazer alguns exemplos com `#lang plai-typed` a linguagem utilizada no livro texto

```lisp
# função com o nome 'f3x' que recebe um numero 'n' e retorna 3*n

> (define (f3x [n : number]) : number
    (* 3 n))

> (f3x 14)
42

> (define (fun_oi [n : number][s : string]) : number
    (begin
        (display s)
        (/ 5 n)
    ))

> (fun_oi 52 "dot")
dot52/5

# Implementação de fatorial
> (define (! [n : number]): number
    (if (< n 2) 1 (* n (! (- n 1)))))
```

Podemos tambem construir *objetos/classes* com o `define-type`.

Podemos fazer uma especie de switch com o `type-case` que tem como sintaxe `<tipo base> <expressão a ser computada>`, assim pode-se fazer `type-case number n` que vai fazer switch de acordo com o valor de n.

```lisp
# Tipo poligono
>   (define-type poli
        [quad (lado : number)]
        [triang (base : number) (altura : number)]
    )

# Definindo um triangulo isoceles
>   (define iso (triang 3 2))
>   (define bob (quad 5))

# Função para calcular area de algum desses objetos
>   (define (area [p : poli]) : poli
        (type-case poli p
            [quad (l) (* l l)]
            [triang (l h) (/ (* l h) 2)]
        ))
```

# Parsing

**Parsing** é a ideia de tranformar uma entrada de um programa em algo mais estruturado, que sera melhor aproveitado pela aplicação. Uma maneira intuitiva de representar dados é atravez de uma arvore, na qual a aplicação pode ir processando de forma recursiva.

Por exemplo, a entrada `23 + 5 - 6` pode ser *parseada* para a expressão `(+ 23 (- 5 6))`, que seria facilmente interprertada em Racket.

Podemos usar o mecanismo de *quotation* do racket que nos permite lidar com a entrada atravez de uma lista, a chamada `'<expr>` cria um simbolo, uma **s-expression**, que pode ser facilmente ser lida como uma lista, podendo ter outras listas dentro. Cada elemento unario de uma s-expression é um **symbol**.

# Interpretando

Queremos achar uma maneira de interpretar livremente expressões aritmeticas. Para isso, vamos definir funções que mapeiam expressões aritmeticas em numeros.

Podemos ter coisas do tipo `(+ (- 2 3) (* 4 2))`, para isso, nosso interpretador deve lidar recursivamente com cada lado da expressão.

# Construindo nossa primeira linguagem, soma e multiplicação

Abaixo segue o script da linguagem construida

```lisp
#lang plai-typed

;; classe para representar lista, como arvore de calculadora polonesa
(define-type ArithC
    [numC (n : number)] ;; numeros da caluladora
    [plusC (l : ArithC) (r : ArithC)] ;; operação de soma de duas subarvores
    [multC (l : ArithC) (r : ArithC)] ;; operação de multiplicação
)

;; interpretador, calcula o resultado de uma expressão
;; (multC (numC 3) (numC 4)) tem que retornar 12
(define (interp [a : ArithC]) : number
    (type-case ArithC a
        [numC (n) n]
        [plusC (l r) (+ (interp l) (interp r))]
        [multC (l r) (* (interp l) (interp r))]
    )
)

:: parser
(define (parse [s : s-expression]) : ArithC
    (cond
        [(s-exp-number? s) (numC (s-exp->number s))]
        [(s-exp-list? s)
            (let ([sl (s-exp->list s)])
                (case (s-exp->symbol (first sl))
                    [(+) (plusC (parse (second sl)) (parse (third sl)))]
                    [(*) (multC (parse (second sl)) (parse (third sl)))]
                    [else (error 'parse "invalid list input")]))]
        [else (error 'parse "invalid input")]))

```

---

## Explicação sobre o parser

A ideia do parser e tranformar uma expressão da forma `(+ 2 3)` em uma expressão do tipo `(plusC (nunC 2) (nunC 3))`.
A entrada de um comando em racket (por exemplo *(+ 1 2 )*) é entendida como uma `s-expresion`.
Apos isso, verificamos se a expressao é apenas um numero, caso positivo retornamos o nunC desse numero. Caso contrario, a expressao é uma lista, convertemos a expressão em uma lista, e apos isso cada elemento da lista em um simbolo, dessa maneiras, tratamos os dois possiveis casos com `case`.

Agora podemos fazer operações da seguinte forma

```lispracket
> (parse '(+ (* 1 2) (+ 2 3)))
- ArithC
(plusC
 (multC (numC 1) (numC 2))
 (plusC (numC 2) (numC 3)))
```

# Sugar, yes please

A nossa linguagem foi contruida de forma bem dura e direta. As funções existentes na aplicação acima são suficientes para que possamos extender, e muito, nossa linguagem. Para isso, vamos definir novas funções que chamam as funções existentes. Á isso, damos o nome de **açucar sintatico**.

Para isso, criaremos um novo tipo, o `ArithS`, que tera a seguinte cara

```lisp
(define-type ArithS
  [numS (n : number)]
  [plusS (l : ArithS) (r : ArithS)]
  [multS (l : ArithS) (r : ArithS)])
  [bminusS (l : ArithS) (r : ArithS)]
  [uminusS (e : ArithS)])
```

Essa versão *açucarada* implementa a subtração e a inversão. Agora, devemos implementar uma função que faz o **desugar**, ou seja, mapeia as expressões de ArithS para as **funções core** de ArithC.

```lisp
(define (desugar [as : ArithS]) : ArithC
  (type-case ArithS as
    [numS (n) (numC n)]
    [plusS (l r) (plusC (desugar l) (desugar r))]
    [multS (l r) (multC (desugar l) (desugar r))]
    [bminusS (l r) (plusC (desugar l) (multC (numC -1) (desugar r)))]
    [uminusS (e) (desugar (bminusS (numS 0) e))]))
```

# Condicionais

Agora, na nossa linguagem, vamos colocar um condicional. Ela vai ter a seguinte sintaxe: `if (e1, e2, e3)`, checa se a condição 1 é verdadeira, caso positivo retorna e2, caso negativo retorna e3.
Nosso primeiro obstaculo é a representação de verdadeiro e falso. Para isso usaremos 1 como verdadeiro e 0 para falso.

```lisp

(define-type ArithC
<...>
[ifC (condição : ArithC) (sim : ArithC) (nao : ArithC)]
)

(define-type ArithS
<...>
[ifS (c : ArithS) (s : ArithS) (n : ArithS)]
)

; desaçucarando
(define (desugar [as : ArithS]) : ArithC
    (type-case ArithS as
        <...>
        [ifS (c s n) (ifC (desugar c) (desugar s) (desugar n))]
))

: interpretador precisa reconhecer a novidade
(define (interp [a : ArithC]) : number
    (type-case ArithC a
        <...>
        [ifC (c s n) (if (zero? (interp c)) (interp n) (interp s))]
))

: parser atualizdo
(define (parse [s : s-expression]) : ArithS
  (cond
    [(s-exp-number? s) (numS (s-exp->number s))]
    [(s-exp-list? s)
     (let ([sl (s-exp->list s)])
       (case (s-exp->symbol (first sl))
         <...>
         [(if) (ifS (pase (second sl)) (parse (third sl)) (pase (fourth sl)))]
       <...>

: funcional adicional para ver se um numeor é negativo
(define (neg? n) (< n 0))

```

Com isso temos as seguintes tranfomações em uma expressão

| Processo (quem fez ela) | Expressão |
| --- | --- |
| Inicial | 3-2 ? 42 : 5+8 |
| pré compilador | `(if (- 3 2) (42) ( + 5 8)) |
| parser | (ifS (bminusS (nunS 3) (numS 2)) (nunS 42) ( plusS (nunS 5) (nunS 8))) |


---

# Criando executaveis de racket

Para isso usamos o **raco**, um compilador de racket. Dessa forma podemos definir um makefile para criar executaveis/binarios de racket.

Esse seria um exemplo para o makefile para um arquivo `direiro.rkt`

```makefile

udo: mcalc direto

mcalc: mcalc.tab.o lex.yy.o main.o
        gcc -o $@ $^  -lfl

mcalc.tab.o: mcalc.y
        bison -d mcalc.y
        gcc -c mcalc.tab.c

lex.yy.o: mcalc.l
        flex mcalc.l
        gcc -c lex.yy.c

direto: direto.rkt
        raco exe $<

clean:
        rm -f *.o lex.yy.c mcalc.tab.c mcalc.tab.h direto *~

```

# Nova linguagem, ExprC

Agora vamos dar um proximo passo, vamos criar um tipo `ExprC`, que vai englobar tudo do `ArithC`. Agora nossa linguagem vai suportar funções.

As funções sao definidas da forma `[fdC (name : symbol) (arg : symbol) (body : ExprC)]` que pode ser lida como `def name(arg) { body }`. Com isso, no core da linguagem adicionamos a chamada de uma dessas funções é a *realização/aplicação* dessa mesma função.

Dessa forma, a chamada `appC` recebe um nome e um valor, ou seja, o nome da função que ele deve chamar e o(s) valores que devem ser passados como argumento. Dessa forma, precisamos substituir no `body`, todas as ocorrencias de `arg` para o `valor` passado.

Por exemplo, a seguinte chamada de função em c `f(5*2 + 4)` seria algo da forma `(appC 'f (plucC (multC (nunC 5) (nunC 2)) (numC 4)))` na nossa linguagem, quando recebemos o argumento assim, na forma bruta de uma expessão, e lidamos com ele depois, chamamos de **lazy**. Ao inves disso, podemos interpretar o valor assim que ele é passado, e chamar a função ja com o valor final calculado, para o exemplo acima, ficaria `(appC 'f (nunC 14))`, á isso, damos o nome de **eager**.

Na nossa solução, tanto o metodo lazy quanto o eager podem ser uteis, dependendo do contexto. Para isso, vamos usar uma hashtable, onde os valores/expressões vao ser colocados para serem substituidos durante a interpretação do programa.

# Escopo dinamico

Especie de definição de variavel global, qundo associamos valores/simbolos fora do escopo de chamada de função. Por exemplo, nessa chamada de fuções, temos o seguinte resultado:

```lisp

; funções definidas anteriormente
(fdC 'f1 'x (appC 'f2 (nunC 4)))
(fdC 'f2 'y (plucC (idC 'x) (idC 'y)))

```

A chamada `(appC 'f1 (nunC 3))`
| resultados em lisp | em uma linguagem normal |
| --- | --- |
| (appC 'f1 (nunC 3)) | f1(3) |
| (fdc-arg fd) -> 'x | |
| (fdc-body) -> (appC 'f2 (nunC 4)) | f2(4) |
| env(bind x -> (nunC 3)) | |
| env(bind y -> (nunC 4))  | |

Isso retorna 7. Pois uma funcão ira usar o enviroment extendido pela outra, logo, na chamada final, teremos valores associados a `'x` e a `'y`.

Ou seja, o escopo dinamico faz necessario que uma função tenha que olhar todo o historico de execução, para descobrir todos os valores associados no decorrer da execução. **Isso é ruim pois pode gerar confusões e ambiguidades.**


Para arrumar isso em nosso projeto, basta que não o enviroment não seja passado em cada chamada de função, ou seja, cada função vai ter o seu proprio escopo de variaveis.

# O problema da definição de função

Na nossa linguagem temos o seguinte problema: não conseguimos definir funções dentro da linguagem de forma facil. Para isso, vamos mecher no *core* da linguagem, definindo um novo operador para função. Alem disso, nosso interpretador vai retorna um novo tipo `Value`, que pode tanto ser o body de uma função ou um numero.

# Closure

Ideia para substitui o escopo dinamico, no qual uma função, ao ser chamada, captura o enviroment do momento em que ela foi chamada, e passsa a usar ele, porem, essa **closure** vai ser usada apenas naquela função, toda modificação de varivale feito dentro de uma closure fica dentro dela. No exemplo abaixo, a chamada de `let` possui uma closure do enviromment definido anteriormente

```
(defvar a 3)            ; a becomes 3.

(let ((a 10))           ; a rebound to 10.
  (print (+ a 6)))      ; 16 is printed.

(print a)               ; 3 is printed.

```

## Exemplo em python

```
def g(x):
    def f(y):
        print(x+y)
    return f

> g
function

> oi = g("Oi, como vai ")

> oi
function

> oi("Felipe")
Oi, como vai Felipe
```

Toda vez que eu faço uma nova instancia de `g(x)` essa instancia vai ter um enviromment diferente, o atual, que no caso, é composto pelo valor de x.

# Boxes

Implementação primitiva de ponteiros. Ou seja, fazemos um simbolo `x` apontar para um *local* (box), essa associação simolo-local é um **bind**. A box possui um valor.

Com isso, definimos nossa **store** como a memoria, ou seja, o conjunto de boxes. E deixamos as associações em cargo do enviroment.



# Trivias/piadas que o Gubi contou em aula

- 20 minutos de uma aula foram dedicados a historia de **Srinivasa Ramanujan**, um matematico genial.
- professor disse que as vezes ele olha pra sala e ve um grande **WTF** vindo dos nossos rostos, foi engraçado.
- O cara escreveu o programa todo na lousa e chamou o prof pra ver, a lousa tava vazia pq ne, era um codigo em **whitespace**.
- Aprendemos que o Fabio Porchat coda em lisp, vimos o video dele entrando em uma aula do Gubi.
- Tinha um filme super bobo que o cara era jogador de basquete e ele não ia poder jogar pq ia repetir numa materia. Dai o professor deu uma prova com a pergunta "diga-me oq vc sabe sobre socrates" e o cara respondeu "nada", dai ele foi aprovado na materia pq ele realmente não sabia nada.
