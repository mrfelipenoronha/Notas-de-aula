# Aula 26/02

Bastar digitar "IRB + Enter" no terminal para começar a utiliza-lo
no modo iterativo.

## Variaveis

- Em Ruby, tudo é objeto, ou seja, tudo pertence á uma classe, por exemplo, numeros são da classe Fixn um;

- Strings são síbolos. Ou seja, possui um endereço de memoria imutavel;

- Não se declara variaveis em ruby, seu tipo é determinado na atribuição de valores. O tipo é do valor, não da varivel;

```ruby
x = 4
x.class
# > Fixnum
x = "oi"
x.class
# > String
```

- Qualquer varíavel inicializada com letra maiscula (ex: **Variavel**) é uma constante.

## Simbolos

```Ruby
sim = :felipe
puts sim.object_id #puts == Print
# > 47374285277960
```
- Um simbolo gera um Hash para a sua palavra.

## Arrays

```ruby
b = Array.new(12) # Cria uma array de 12 elementos, todos inicializados com null.
c = Array.new(12, 6) # Array populada com 6
d = Array.new(9) {|x| x*10}
 # > [0, 10, 20, 30, 40, 50, 60, 70, 80]

Array(1..6) # Imprime numeros em range
 # > [1, 2, 3, 4, 5, 6]

Array.[](1, 2, 3, 4, "oi")
 # > [1, 2, 3, 4, "oi"]

nomes = %[felipe carol ana julia]
 # criar uma lista {"felipe", "carol", "ana", "julia"}

 ```
- Uma array em Ruby pode conter mais de um tipo de dados.;

- O indice pode ser negativo, neste caso, vai ser contado a partir do final;

- Na array **d** o copilador leu da seguinte forma: "Crie uma array de 9 elementos, tomando x como argumento e aplicando a operação x*10".

## Estrututras de controle

```ruby
if 2 > 1
    p "oi"  # Printa "oi" caso 2 > 1
end

p "oi" if 2 > 1 # Outra maneira de escrever a mesma coisa

for i in 1..4 ; p i*8 ; end
```

## Funções

- Mesmo esquema das funções de python.

```ruby
def quadrado(x)
    return x*x
end
```

---
# AULA 28/02

## Orientação á objetos

As caracteristicas basicas de um objeto são:

- Em Ruby, tudo é um objet0;
- Um programa é uma coleção de objetos interagindo entre sí;
- Todo objeto tem um tipo. Em outras palavras, um objeto é uma instancia de uma classe;
- Objetos da mesma classe reagem do mesmo modo.

Um objeto tem 3 propriedades:

- Estado, descrito por um conjunto de variaveis;
- Comportamento, descrito por funções, chamadas de metodos;
- Identidade, indicada por uma instancia (endereço na memoria).

# Exemplo de classe
```ruby

class Carta

	def initialize(v, n) # Metodo construtor
		@naipe = n # É uma varivale dql instancia da classe
		@valor = v # Semelhante ao self.naipe
	end

	def to_s
		return "#{@valor} do #{@naipe}"
	end

	# getter, usa para quando quero retorna um valor da minha classe
	def naipe
		@naipe # por exemplo, variavel = obj.naipe
	end

	# setter, setar o valor da variavel
	def naipe = (n)
		@naipe = n
	end

	# Esse metodo cria todos o getters and setters para as vaeriaveis especificadas
	attr_accessor :naipe, :valor
	# Isso é chamado de metaprogração, pois é um metodo que cria outro metodo
end

```

## Outro exemplo
```ruby
class Baralho

	def initialize
		@cartas = [] # criando vetor
		for n in %[P C E O]
			for v in ["A"] + Array(2..10) + ["J", "Q", "K"] # Percorre todos esses valores
				@cartas << [Carta.new(v, n)]
			end
		end
	end
end
```

---
# Aula 07/03

# Comparable

É um modulo pre definido usado para realizar operações dentro de uma classe. No ruby temos o comparador total `<=>` que rotorna -1, 0 ou 1 se o valor da esquerda for menor, igual ou maior que o valor da direita. É basicamente uma função com o nome '<=>'.

```ruby
class SizeMatters

    include Comparable # Incluindo comparador

    def <=>(other) # Metodo comparador
        str.size <=> other.str.size
    end

    def initialize(str)
        @str = str
    end

    def inspect
        @str
    end
end

s1 = SizeMatters.new("Z")
s2 = SizeMatters.new("YY")
s3 = SizeMatters.new("WWW")
s4 = SizeMatters.new("FFFF")

s1 < s2
 # => True

s2.between?(s1, s3)
 # => True

[ s1, s2, s3, s4].sort
 # => ["FFFF", "Z", "YY", "WWW"]

```

# Exceções e tratamento de erros

Normalmente uma exceção é associada á algum erro. Caso algo que não deveria ocorrer
aconteça, uma exceção pode ser levantada. As exceções são classes e muitas delas ja
estão implementadas no ruby.

Marca-se o inicio do bloco com `begin` e a região de tratamento com `rescue`.
Como capturar exceções:
* `begin` seguido pelo codigo protegido;
* `rescue =>` seguido pelo tratamento da exceção;
* `else` tratamento para ações nao especificadas;
* `ensure` seguido pelo codigo que deve ser executado sempre, com erro ou não;
* `end`.

```Ruby
tentativas = 0
begin
    puts "Entre com um numero negativo"
    x = gets.chomp.to_i # chomps elimina \n no final
    raise "Eu disse negativo" if x >= 0
    puts "Muito bem"

    rescue => e
        puts $!
        puts "Voce digitou #{x}"
        retry if (tentativas += 1) < 4
end
```

Neste exemplo acima, `rescue` é algo que ele vai executar somente se um erro acontecer,
ou seja, quando o `raise` é invocado a proxima linha a ser executada vai ser o rescue.

## Throw e Catch

Tem um processamento muito mais rapido do que as exceções. O `catch` anota um bloco
com um nome que sera terminar se o `throw` para aquele bloco for executado. É um pulo
direto para o bloco acima na fila de execução.

```Ruby
def Entrada(prompt)
    print prompt
    l = gets.chomp
    throw :refaz if l == "!"
    return 1
end

catch :refaz do
    nome = Entrada 'Nome: '
    idade = Entrada 'Idade: '
    sexo = Entrada 'Seco: '
    puts "Registro completo"
end
```
---
# Aula 12/03

## Metaprogramação
Como Ruby é uma linguage de script interpretada, é possivel extender o codigo em tempo de execução, e isso damos o nome de metaprogração.

A rigor, toda chamada de um metodo é uma mensagem ao objeto dizendo a ele o que deve ser feito. Isso aparece de forma bem explicita em ruby:

```ruby
class C
    def grite
        puts 'AAAAAAAAAAAAAAAAAAAAA'
    end
end

obj = C.new
obj.grite
obj.send 'grite'
obj.send :grite
g = "grite"
obj.send(g)

# Todas essas ultimas linhas, essencialmente, fazem a mesma coisa
```

### Objetos abertos
Quando uma instancia é criada, ela ganha uma classe adicional so para ela. Essa nova classe é inserida na hierarquia entre a instancia e a classe mãe, é chamada de **singleton**.
Podemos alterar essa classe, fazendo com que uma instancia ganhe novos metodos:
```ruby
class A
end

a = A.new
b = A.new

def b.oi
    puts "oiiiiiiiii"
end

a.oi # Da erro
b.oi # Executa a classe criada
```

Uma outra caracteristica é permitir que um bloco seja executado dentro do escopo de uma instancia.

```Ruby
class A
    def initialize
        @um = 666
    end

    def um
        @um
    end
end

a = A.new
a.instance_eval { puts 2*um }
a.instance_eval { puts 2*@um }
a.instance_eval " puts 'oiii' "
a.instance_eval "def uia ; puts 'caramba' ; end"
a.uia
```
Aqui, o `instance_eval` serve como um injetor de codigo para dentro da classe,
podendo imprimir variaveis privadas e ate mesmo criar novos metodos, como o `uia`.

---
# Aula 14/03

## Computação na nuvem

Computação integrada por varios computadores, que realizam um processamento remoto das operações.

- Pontos positivos: comunicação, escalabilidade, elasticidade e disponibilidade.
- Uso aglomerado de computadores, ao inves de maquinas grandes
- Varias maquinas pequenas são mais baratas que varias maquinas grandes

## Testes

Codigo legado: Pré-existente, originário de implementações antigas. Em geral, significa que é bem sucessido.   
Codigo belo: Código de longa duração e facil de evoluir.

O maior problema é quando o codigo fica obsoleto de forma rapida e inexperada.   
A qualidade de um codigo depende de dois aspectos: atender ao cliente e facilidade de criação e manutenção.

Existem dois tipos de testes que fazemos para assegurar tais coisas:
1. Verificação: ver se a coisa foi contruida corretamente;
2. Validação: ver se a coisa certa foi contruida.

### Test Driven Development (TDD)
Desenvolvimento guiado por testes. Serve para unidade e modulo: garante que o codigo não possua erros. Faça os testes, corrija ou implementa novas funcionalidades.

### Behaviour Drive Desing (DDD)
Projeto guiado por comportamento. Serve para integração.

### Produtividade
- Clareza via concisão (abstração e alto nivel);
- Sintese (geração automatica de codigo a partir de uma descrição sintetica);
- Reuso (padrões de projeto, orientação a objetos, DRY-Dont repeat yourself);
- Ferramentas de automação.

## TCP/IP - Arquitetura cliente-servidor

    Browser    <-->    Iternet     <-->     Site
                                              |
                            Web server / App Server / Database

A internet, em geral, tem uma arquitetura cliente-servidor, que é fundamentalmente um esquema pedido-resposta. Este tipo de arquitetura pode prover um serviço especializado, pois, o servidor pode ser especializado.    
Podemos tambem ter uma arquitetura p2p, em que um cliente se comunica com outro cliente.

- **IP** é o 'internet protocol', um identificador para cada rede.   
- **TCP** é o 'transmission control protocol', que valida os IPs e controla as transmições e controla as chamadas. Pode possuir varias portas, fazendo possivel a comunicação com varios IPs e objetos diferentes.   
- **DNS** é um sistema de mapeamento de um endereço (blalba.com) para um IP (x.x.x.xx).   
- **HTTP** é um protoclo para a tranferencia de informação pela internet. Representa uma conversa entre o cliente e o servidor, enviando requisições e recebendo respostas.   

---

# Aula 26/03

> Continuando a aula passada

### Cookies
Servidor usando a maquina para salvar coisas, deixando a conexão mais rapida. Porem, isso pode não ser seguro, uma vez que sites podem querer guardar arquivos maliciosos no computador do cliente.   
O Ruby consegue lidar de uma forma muito inteligente e automatica com os cookies.

### Arquitetura do Projeto
1. No projeto, o cliete vai mandar uma requisição pela rede.
2. O servidor vai receber essa requisição e manda-la para um controlador.
3. O controlador vai ver qual função deve ser chamada.
4. O modelo vai gerir os dados armazenados, pegar a função e executada, além de passar os dados para visualição.
5. Na visualização vai ser mostrado para o cliente o que ocorreu.

## Model-view controller
Nome do tipo de arquitetura comentado anteriormente. Vale lembrar que o `controller` se comunica com o `view`, e vice-versa. O `view` tambem se comunica com o `model` e vice-versa, e, finalente, o `controller` se comunica com o `model`.

### Exercicio invertido - Desing REST para um loja online de sucos
- GET: Consulta dados
- POST: Cria novo item
- PUT: Atualizar
- DELETE: Deleta item

Configurando `routers.rb`:

```Ruby
get '/pastries/:flavor' => 'pastries#eat',
    :as => 'eat_dessert'
```

---

# Aula 28/03

## HTML e Ruby

Arquivos salvos com a extensão '.html.erb' são codigos em HTML que rodam partes de codigos em Ruby.
Tais funções rodam atravez das tags `<%= ` e `%>`

`rake db:create` : Cria um banco de dados
`rail server` : Servidos de rais

Uma rota vai ser algo que recebe a URL e a direciona para a aplicação correspondente.   
Rodando `bin/rails g controller Manda index` no diretorio do projeto criar uma rota padrão.

Todos os controladores ficam em `app/views/manda`, nessa pasta ficam os apelidos, caminhos e controlodares para cada metodo, no nosso caso, que o controlador chama manda, temos que acessar a subpasta manda.      
`manda#index` se traduz como "No cotrolador manda vou chamar o metodo index"

Criando um controlador 'apelidos':
`bin/rails g controller Apelidos`

A chamda de `resources :apelidos` no arquivo 'routes.rb' criar automaticamente os arquivos e rotas necessarias, com atribuições e funcões padroes, cabendo ao programador criar o metodo para cada uma no controller.   

No arquivo 'apelidos_controller.rb' devem ser criado todos os metodos


> Olhar no PACA como construir ambiente rails localmente e ver os exemplos   
> Ver livro dessa parte
