
# Indice

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Introdução a lisp/racket](#introduo-a-lispracket)
- [Construindo nossa primeira linguagem, soma e multiplicação](#construindo-nossa-primeira-linguagem-soma-e-multiplicao)
- [Explicação sobre o parser](#explicao-sobre-o-parser)

<!-- /TOC -->

A bibliografia usada é [Programming Languages: Application and Interpretation](http://cs.brown.edu/courses/cs173/2012/book/).

A a linguagem usada na materia é **racket** e o interpretador **drracket**.

# Aula 06/08

## Introdução a lisp/racket

Usamos a chamada `#lang racket` para definir a linguagem usada no script.

Quando colocamos `'` antes de alguma expressão tranformamos ela em um simbolo, ou seja, é so uma lista de valores sem nenhuma operação a ser realizada.

Exemplos de execução no interpretador

```
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

```
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

```
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

```
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
