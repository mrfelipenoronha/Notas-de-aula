# MAC0425 - Inteligencia aritificial

## Marcelo Finger

- **Livro texto:** artificial intelligence, a modern approach 3ed

# Introdução

IA é o estudo e desenvolvimento de entidades autonomas capazes de perceber o ambiente e agir de maneira satisfatoria. Neste contexto, podemos definir um **agente inteligente**, para isso, definimos:

- Um conjunto de percepções `P`
- Um conjunto de ações `A`
- Um agente é uma `f: P* -> A` (função que tranforma uma sequencia de percepções em ações)
- Conjunto de estados do mundo `S`
- Função de desempenho `g: S* x A* -> ]-inf, +inf[`

Assim, um agente inteligente é uma `f que otimiza g`.

IA é: busca, representação de conhecimento, raciocionio logico, planejamento, raciocionio probabilistico, aprendizagem automatica e aspectos metodologicos, eticos e filosoficos.

# Buscas em grafos

Aqui, tentamos abtrair problema genericos para um grafo. Assim, temos os nós como estados e arcos como ações. Com isso, conseguimos tranformar o problema inicial em um problema computavel, em especial, o de **encontrar o caminho menos custoso de um nó inicial a um nó meta**.

Logo, podemos definir formalmente os problemas de busca. Onde, dados:

- Conjunto de estados `S` e de ações `A`
- Função de ações aplicaveis num estado `A(s) -> A' c= A`
- Modelo de transição `T(s, a) -> s'`
- Função de custo `g(a1,..., a2) -> ]-inf, +inf[`
- Estado inicial `s0 c S` e conjunto de estados meta `M c= S`

Queremos encontrar uma sequencia de alções que levam ao trajeto menos custoso do estado incial a um estado meta.

Para analisar o desempenho de um algoritmo de busca, definimos algumas coisas:

- `b`: grau de ramificação (media do grau de remificação da arvore)
- `d`: profunidade da solução mais curta
- `m`: profundidade maxima

## Busca cega

Só olho para o meu espaço de estados, não tendo nenhuma informação extra além do meu estado atual. Os algoritmos aqui diferem apenas na meneira de escolha dos nós.

### Busca em profundidade

Vou iniciar uma busca a partir do estado inicial. A cada passo, selecione um no `n` a partir do no atual, se `n.estado` for META, retorno a solução, caso contrario, realizo o mesmo procedimento a partir de `n`.

Caso haja empate, ou seja, nos com a mesma profundidade, eu posso usar algum criterio de desempate, por exemplo, pego o nó com menor custo.

A busca em profundidade pode não terminar se:

- espaço de estados for infiito;
- grau de ramificação for infinito;
- espaço de estados tiver ciclos.

Para evitar loops infinitos, podemos fazer uma **busca em profundidade com memoria**. Assim, vou lembrar os nos/estados que ja visitei, evitando de visita-los novamente.

Podemos tambem fazer uma **busca de profundidade limitada**, onde evitamos que a busca passe de uma certa profundidade.

Em termos de desempenho, temos que a busca em profundidade é:

- incompleta (nem sempre encontra solução)
- subotima (nem sempre encontra solução otima)
- Complexidade de tempo O(b^m)
- Complexidade de espaço O(b*m)

### Busca em largura

Usarei uma fila para fazer a busca. Realizo a busca no primeiro estado da fila, e adiciono os novos estados alcançados no fim da fila. É bom, pois me retorna a solução com menor custo.

Tambem podemos utilizar memoria para evitarmos passar no mesmo vertice mais de uma vez.

Em termos de desempenho, temos que a busca em largura é:

- completa para b finito (sempre encontra a solução se b for finito e ela existir)
- otima se o custo for uniforme
- Complexidade de tempo e espaço O(b^(d+1))

### Busca de custo uniforme

Tenta otimizar a solução, fazendo a busca a partir do nó com menor custo. Temos o seguinte pseudocodigo:

```pseudo
Inicie arvore com estado inicial
repita:
    selecione no não explorado n com menor custo g(n)
    se n for meta
        retorne n
    expanda n
```

## Busca informada

O maior gargarlo da busca cega é o numero de estados/ações, que podem ser absurdamente grandes.

Usar heuristicas para tomar uma decisão. Novamente, podemos representar a busca como uma arvore. Podemos representar a fronteira da busca (franja da arvore) atravez de uma fila de prioridade, assim, vamos expendindo a fronteira utilizando os vertices com maior/menor prioridade. Note que, a aplicação da heuristica esta na menira em que ordenamos essa fila.

Não podemos considerar a busca de custo uniforma como uma busca informada, pois, a BCU sabe a sua distancia até a raiz, não tendo nenhuma informação sobre o qual perto estamos da posição _meta_. Já na busca informada, sabemos o quão perto estamos do estado _meta_, ou seja, fazendo escolhas pensando no futuro, e não no passado.

Para realizar a busca informada, precisamos definir uma **função heuristica** `h(n)`, que estima o custo de uma solução parcial (nó) a uma meta. Essa função possui apenas 2 propriedades:

- h(n) >= 0
- h(meta) == 0

### Busca de melhor escolha (gulosa)

Para cada nó, defino um valor `h(n)` para esse nó, algo relacionado com o estado _meta_, como, por exemplo, a distância entre o nó atual e o meta. Com isso, temos o seguinte pseudocodigo:

```pseudo
inicie arvore com estado inicial
repita:
    selecione o no n de menor heuristica h(n)
    expanda n
    se um filho de n for meta
        retorne filho
```

Em termos de desempenho, podemos perceber que:

- No melhor c;enario, leva o caminho menos custoso ao meta
- No pior caso, expande a arvore completa (como BP)
- Não é completa, pois pode entra em ciclo se não usar memorização e pode seguir uma caminho infinito
- Não é otima

### Busca A*

Combina busca de custo uniforme com custo de melhor escolha. Com isso, o valor do de nó atual é dado por `f(n) = g(n) + h(n)`, onde `g(n)` é o custo uniforme e `h(n)` é o custo de melhor escolha. Com isso, levamos em consideração o passado e o futuro.

A heuristica `h` é **admissivel se é otimista**, logo, temos que ter `0 <= h(n) <= g(meta) - g(n)` para todo estado n, onde `g` é o custo para sair do estado atual e chegar no estado n.

**Teorema:** A* é ótima quando _h_ é admissivel. Ou seja, se: A é nó meta **otimo**; B é nó meta **subotimo**; h é admissivel, então: A é selecionado antes de B.

A busca A* prioriza expansão em direção a meta, ou seja, um caminho reto ate ela.

Analisando o desempenho, temos que a busca A* é:

- Completa
- Otima
- Tempo e espaço dependem da heuristica

#### Consistencia

Se a heuristica é consistente no grafo, temos, por inplicação, que a heuristica é admissivel, logo, a busca será otima. A heuristica é consistente quando ela respeita a desigualdade triangular, isto é, `h(a)-h(c) <= custo(a para c)`, para todo arco _a-c_ do grafo.

#### Busca A* com memoria

Isso impede que eu repita nós.

```pseudo
inicie FRONTEIRA com nó n tal que n.ESTADO = Início e n.CUSTO = 0
inicie dicionário ALCANÇADOS vazio
repita enquanto FRONTEIRA não estiver vazia:
    selecione nó n de menor valor f(n)
    se n.ESTADO for estado-meta
        retorne solução correspondente
    para cada ação em Ações(n.ESTADO):
        suc = Sucessor(n.ESTADO, ação), custo = n.CUSTO + custo(n.ESTADO, ação):
        se suc ∉ ALCANÇADOS ou se ALCANÇADOS[suc] > custo:
            ALCANÇADOS[suc] = custo
            crie nó n’ com pai n, n’.ESTADO = suc e n’.CUSTO = custo e o adicione à FRONTEIRA
```

### Busca não deterministica

Aqui, os cenarios são mais parecidos com cenarios do mundo real, pois agora, não temos informações completas do ambiente. Não sei onde estou, não sei para onde vou, não sei se ficarei mais perto ou não do estado otimo e também não consigo saber se a minha mudança de estado teve sucesso ou não.

Aqui, as **ações não deterministicas** são afetadas por eventos exogenos (vento impediu de andar, perna do robo não girou completamente, dentre outros agentes externos a IA), assim como, podem ser afetada pelo corportamento de outros agentes ou pela observação do ambiente.

Um exemplo de agente que pode me afetar é quando estamos jogando o _jogo da velha_, pois, a minha ação é influenciada pelas ações do outro jogador. Podemos ter, tambem, _observabilidade parcial_, como no mundo de wumpus, onde só conseguimos ver as 8 celulas ao nosso redor.

#### Formalização

Para uma problema de busca com ações não deterministicas, dados:

- Conjunto de estados `S` e de ações `A`
- Função de ações aplicáveis num estado `A(s) → A’ ⊆ A`
- Modelo de transição `T(s,a) → S’ ⊆ S`
- NÃO TEMOS função de custo `solução → ]-∞,∞[`
- Estado inicial `s0 ∊ S` e Conjunto de estados metas `M ⊆ S`

Temos como objetivo: encontrar **plano contingente/politica** de ações que leva a estado **meta em qualquer execução**. Função π mapeando subconjunto de estados em ações. Descreve o comportamento do agente em parte do dominio.

#### Representação de funções

Para cada tipo de problema, precisamos de uma função que representa a politica a ser aplicada. Por exemplo: no jogo da velha precisamos de uma função mapeando o estado do tabuleiro para uma jogada que resulte no empate ou vitoria; No mundo de Wumpus precisamos de uma função mapeando o mundo observado a movimentações/ações seguras.

- **Arvore E/OU**: Aqui, o problema é representado por um espaço de soluções. Os nos da arvore alternam entre _nós OU_ (representam escolha de ação) e _nós E_ (representam resultado da açao), as folhas são os _estados-meta_. Problemas representados aqui são **P-espaço**, ou seja, apresentam um numero polinomial de estados, isso nos da uma ideia da complexidade, uma vez que os problemas _P-espaço_ são tão dificeis ou mais que os problemas em _NP-completo_.

Dessa maneira, para chegar a solução, podemos fazer uma busca no grafo E/OU, usando algum dos algoritmos vistos anteriormente.

**Determinização**: tentar traduzir o problema para um problema determinisco, e, assim, resolve-lo.

Um algoritmo para a busca num grafo E/OU é apresentado abaixo:

```pseudo
fun and_or_graph_search(problem) returns a conditional plan or failure
    or_search(problem.initial_state, problem, [])

fun or_search(state, problem, path) returns a conditional plan or failure
    if (problem.goal_test(state)) // verifica se é meta
        return empty_plan
    if (state is on path) // verifica se entrou em ciclo
        return failure
    for each action in problem.actions(state) do
        plan <- and_search(results(state, action), problem, [state|path])
        if (plan != failure) return [action|plan]
    return failure

fun and_search(states, problem, path) returns a conditional plan or failure
    for each s in states do
        plan <- or_search(s, problem, path)
        if (plan == failure)
            return failure
    return [if s1 than plan1 else if 2 than plan2 ...]
```

## Complexidade e busca

É muito fácil que nosso espaço espaço de busca fique exponencial, basta que cada estado tenha, em media, mais de uma escolha que pode ser feita. Vamos supor que queiramos apenas chegar ao _meta_. Logo, o caminho que usamos para chegar em tal estado é chamado _caminho testesmunha_, que vai da raiz ate o estado meta.

Quando queremos encontrar apenas 1 caminho na arvore de busca, e tal caminho tem tamanho polinomial, normalmente estamos tentando resolver problemas que estão em `NP` ou `NP-completo`.

Agora, por exemplo, em uma arvore E/OU. Para checar se todos os caminhos a partir de um _nó E_ vou gastar tempo exponencial. Porém, consigo checar muito rapidamente se uma caminho dado não é valido. Logo, aqui, temos um _contra exemplo testemunha_ que tem tamanho polinomial, e, logo, problemas desse tipo estão em `coNP`.

## Prolog

Linguem que usa logica para representar conhecimento.

### Recursão

Uma lista é simplemente um conjunto de elementos entre colchetes, como `[a,b,c,d,e]`.

## Introdução ao estudo computacional da lingaguem

Aqui, vamos ver maneiras formais de lidar com linguagem. Uma linguagem tem como elementos: sons, ritmos (prosodia), fonemas, palavras, sintagmas (frases), significados (semantica) e usos.

### A estrutura da frase

A palavra é a unidade basica de analise da frase. A cada palavra é atribuida uma catergoria morfo-sintatica, a classe gramatical. Temos como exemplos:

- Deteminante, conhecido como artigo;
- Nome, conhecido como substantivo;
- Verbo transitivo.

Um **sintagmas** são componentes formados pela combinação de palavras. É o que conhecemos por frases. Além disso, temos que categorias sintaticas podem ser formada por outra sequencia de otras categorias, assim, `c1, c2, ..., cn -> C`.

Juntando todas as regras temos uma **gramatica**, que gera e reconhece a frase. Com isso, conseguimos criar uma arvore sintatica, que sera composta por cada classe que gera cada outra classe, ate chegarmos na lasse mais superior que representa a raiz.

### Linguagem formais

Seja _V_ um conjunto não vazio de simbolos ou palavras. Uma linguagem _L_ sobre _V_ é um conjunto de cadeias formadas com os elementos de _V_. Se _r_ e _s_ são sequencias em _V*_, _rs_ representa a sequencia resultante de sua concatenação. _e_ é a sequencia vazia, para isso, _s: se = s = es_.

### Gramaticas formais

Uma gramatica _G_ é uma tupla `G = <V, T, P, S>`, onde

- _V_ é um conjunto não vazio, chamado vocabulario
- _T c V_ é um conjunto nao vazio de simbolos terminais
- _N = V-T_ é o conjunto de simbolos não terminais
- _S e N_ é o simbolo incial
- _P_ é um conjunto finito de regras da forma _a->b_

# Raciocionio probabilistico

Probabilidade é uma medida de incerteza. Diz como as coisas funcionam na media, ou seja, reflete coisas sobre uma população e não sobre individuos.

Uma **evidencia** se refere a parte observavel do mundo (sensores, sintomas). O **desconhecido** se refere a parte não observavel (evento futuros, comportamento do adversario).

Podemos fazer buscas sobre espaços probabilisticos, onde cada transição de estado tem uma probabilidade de acontecer.

## Probalidade discreta

Quando temos um espaço finito de resultados possiveis. O **espaço amostral** contem todos os resultados possiveis. Um **evento** é um subconjunto do EA. Uma **função de probabilidade** assossia um valor não negativo a um evento, é uma medida de incerteza sobre o evento. 

### Axiomas

- Normalização: a soma da probalidade de todos os eventos do espaço amostral é 1;
- Aditividade: evento disjuntos tem probabilidades aditivas. Se a interseção de A e B é vazia, então _P(A u B) = P(A) + P(B)_.

## Interpretação frequentista

Probalidade é a frequencia relativa no longo prazo de observações de um experimento. Porém isso tem varias limitações.

## Probabilidade condicional

Medida de incerteza assumindo que o estado do mundo é parcialmente conhecido. _P(A|B) = P(A inter B) / P(B)_ é lido como _probabilidade de A dado B_.

Temos que _P(A u B | C) = P(A|C) + P(B|C) -P(A inter B | C)_.

### Regra do produto

MUITO IMPORTANTE. Permite decompor probabilidade de eventos em probabilidades condicionais.

_P(A inter B) = P(A|B)P(B) = P(B|A)P(A)_.

## Regra de Bayes

_P(A|B) = P(B|A)P(A)/P(B)_.

## Arvore de probabilidades

Representação grafica do espaço de probabilidades. Cada nó representa um evento, raiz é o espaço amostral. Nós irmãos representam partições. Arcos são anotados com probabilidade condicional de eventos dados eventos ancestrais.

## Variavel aleatoria

Permite agrupar eventos similares. Uma variavel aleatoria _X_ permite mapear cada estado do espaço amostral para um valor real.