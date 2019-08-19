
# Indice

<!-- MDTOC maxdepth:6 firsth1:2 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [Introdução a lisp/racket](#introdução-a-lispracket)
- [Construindo nossa primeira linguagem, soma e multiplicação](#construindo-nossa-primeira-linguagem-soma-e-multiplicação)
- [Explicação sobre o parser](#explicação-sobre-o-parser)
- [Condicionais](#condicionais)
- [Criando executaveis de racket](#criando-executaveis-de-racket)
- [Nova linguagem, ExprC](#nova-linguagem-exprc)

<!-- /MDTOC -->

A bibliografia usada é [Programming Languages: Application and Interpretation](http://cs.brown.edu/courses/cs173/2012/book/).

A a linguagem usada na materia é **racket** e o interpretador **drracket**.

# Aula 06/08

## Introdução a lisp/racket

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

Podemos tambem construir objetos/classes com o `define-type`.

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
        )
    )
```

## Construindo nossa primeira linguagem, soma e multiplicação

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

# Aula 08/08

## Explicação sobre o parser

A ideia do parser e tranformar uma expressão da forma `(+ 2 3)` em uma expressão do tipo `(plusC (nunC 2) (nunC 3))`.
A entrada de um comando em racket (por exemplo *(+ 1 2 )*) é entendida como uma `s-expresion`.
Apos isso, verificamos se a expressao é apenas um numero, caso positivo retornamos o nunC desse numero. Caso contrario, a expressao é uma lista, convertemos a expressão em uma lista, e apos isso cada elemento da lista em um simbolo, dessa maneiras, tratamos os dois possiveis casos com `case`.

Agora podemos fazer operações da seguinte forma

`(interp (parse '(* (+ 1 2) (+ 6 8))))`

A seguir esta o codigo feito em Aula

```lisp
#lang plai-typed

#|
 | Incluindo troca de sinal: - unário, uminus
 | Mais açúcar?
 |#

(define-type ArithC
  [numC (n : number)]
  [plusC (l : ArithC) (r : ArithC)]
  [multC (l : ArithC) (r : ArithC)])

; Incluindo o sinal negativo
(define-type ArithS
  [numS    (n : number)]
  [plusS   (l : ArithS) (r : ArithS)]
  [bminusS (l : ArithS) (r : ArithS)]
  [uminusS (e : ArithS)]
  [multS   (l : ArithS) (r : ArithS)])

; Nova "desugar", ou quase
(define (desugar [as : ArithS]) : ArithC
  (type-case ArithS as
    [numS    (n)   (numC n)]
    [plusS   (l r) (plusC (desugar l) (desugar r))]
    [multS   (l r) (multC (desugar l) (desugar r))]
    [bminusS (l r) (plusC (desugar l) (multC (numC -1) (desugar r)))]
    ; tentativas para o - unário

    ; pode-se fazer (- 0 e)
    ;[uminusS (e)   (desugar (bminusS (numS 0) e))]
    ; esta solução é perigosa, pois estamos fazendo a recursão em no mesmo 'e'
    ; isto é, recursão "generativa", ou não estutural:
    ; o argumento da recursão é uma função, e não uma subparte, do argumento original

    ; Isto resoveria,mas coloca outro problema (qual?)
    ;[uminusS (e)   (bminusS (numS 0) (desugar e))]

    ; a solução (ainda bem que existe) também se fixa apenas nas primitivas
    [uminusS (e)   (multC (numC -1) (desugar e))]
    ))

; O interpretador é o mesmo, pois no final ainda temos ArithC
(define (interp [a : ArithC]) : number
  (type-case ArithC a
    [numC (n) n]
    [plusC (l r) (+ (interp l) (interp r))]
    [multC (l r) (* (interp l) (interp r))]))

double g = 2.0
int f = (double g)

; o parser muda mais um pouco
(define (parse [s : s-expression]) : ArithS
  (cond
    [(s-exp-number? s) (numS (s-exp->number s))]
    [(s-exp-list? s)
     (let ([sl (s-exp->list s)])
       (case (s-exp->symbol (first sl))
         [(+) (plusS (parse (second sl)) (parse (third sl)))]
         [(*) (multS (parse (second sl)) (parse (third sl)))]
         [(-) (bminusS (parse (second sl)) (parse (third sl)))]
         ; para o parser precisamos um sinal negativo...
         [(~) (uminusS (parse (second sl)))]
         [else (error 'parse "invalid list input")]))]
    [else (error 'parse "invalid input")]))

(test (interp (desugar (uminusS (numS 3) ))) -3)

(define (interpS [a : ArithS]) (interp (desugar a)))

(interpS (parse '(+ 5 (~ 3))))


```

---

# Aula 13/08

## Condicionais

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
    exercicio de casa é implementar a divisao

Com isso temos as seguintes tranfomações em uma expressão

| Processo (quem fez ela) | Expressão |
| --- | --- |
| Inicial | 3-2 ? 42 : 5+8 |
| pré compilador | `(if (- 3 2) (42) ( + 5 8)) |
| parser | (ifS (bminusS (nunS 3) (numS 2)) (nunS 42) ( plusS (nunS 5) (nunS 8))) |


---

# Aula 15/08

## Criando executaveis de racket

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

## Nova linguagem, ExprC

Agora vamos dar um proximo passo, vamos criar um tipo `ExprC`, que vai englobar tudo do `ArithC`. Agora nossa linguagem vai suportar funções.

As funções sao definidas da forma `[fdC (name : symbol) (arg : symbol) (body : ExprC)]` que pode ser lida como `def name(arg) { body }`. Com isso, no core da linguagem adicionamos a chamada de uma dessas funções é a *realização/aplicação* dessa mesma função.
