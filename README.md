# V Documentação

[Tradução da documentação original](https://github.com/vlang/v/blob/master/doc/docs.md)

### Introdução

**V** é uma linguagem de programação compilada estaticamente tipada projetada para construir software sustentável.

É semelhante ao Go e seu design também foi influenciado por Oberon, Rust, Swift, Kotlin e Python.

**V** é uma linguagem muito simples. Analisar esta documentação levará cerca de uma hora e, ao final dela, você terá aprendido praticamente toda a linguagem.

A linguagem promove a escrita de código simples e claro com abstração mínima.

Apesar de ser simples, **V** dá ao desenvolvedor muito poder. Tudo o que você pode fazer em outras linguagens, você pode fazer em **V**.

### Instalar da fonte

A principal forma de obter o melhor e mais recente **V** é instalá-lo a partir do código-fonte . É fácil e geralmente leva apenas alguns segundos .

### Linux, macOS, FreeBSD, etc:

Você precisa git, e um compilador C como tcc, gccou clang, e make:

```shell
git clone https://github.com/vlang/v
cd v
make
```

### Windows

Você precisa git, e um compilador C como tcc, gcc, clang ou msvc:

```shell
git clone https://github.com/vlang/v
cd v
make.bat -tcc
```

NB: Você também pode passar um dos -gcc, -msvc, -clangao make.batinvés, se você preferir usar um compilador C diferente, mas -tcc é pequeno, rápido e fácil de instalar (V vai baixar um binário pré-construídos automaticamente).

Recomenda-se adicionar esta pasta ao PATH de suas variáveis de ambiente. Isso pode ser feito com o comando ```v.exe symlink```.

### Android

A execução de aplicativos gráficos V no Android também é possível via [vab](https://github.com/vlang/vab).

V Dependências do Android: V , Java JDK > = 8, Android SDK + NDK .

1. Instale dependências (veja [vab](https://github.com/vlang/vab))
2. Conecte seu dispositivo Android
3. Run:

```shell
git clone https://github.com/vlang/vab && cd vab && v vab.v
./vab --device auto run /path/to/v/examples/sokol/particles
```

Para obter mais detalhes e solução de problemas, visite o [repositório vab GitHub](https://github.com/vlang/vab).

### Índice

- Olá Mundo
- Executando uma pasta de projeto
- Comentários
- Funções
	- Retornando vários valores
- Visibilidade do símbolo
- Variáveis
- Tipos
	- Strings
	- Números
	- Arrays
	- Matrizes de tamanho fixo
	- Mapas
- Importações de módulos
- Declarações e expressões
	- If
	- In operator
	- For loop
	- Math
	- Defer
- Structs
	- Estruturas embutidas
	- Valores de campo padrão
	- Sintaxe literal curta de estrutura
	- Modificadores de acesso
	- Métodos
- Unions
- Funções 2
	- Funções puras por padrão
	- Argumentos mutáveis
	- Número variável de argumentos
	- Funções anônimas e de alta ordem
- Referências
- Constantes
- Funções integradas
- Impressão de tipos personalizados
- Módulos
- Tipos 2
	- Interfaces
	- Enums
	- Tipos de soma
	- Tipos de opção / resultado e tratamento de erros
- Genéricos
- Simultaneidade
	- Gerando Tarefas Simultâneas
	- Canais
	- Objetos Compartilhados
- Decodificando JSON
- Testando
- Gerenciamento de memória
- ORM
- Escrevendo documentação
- Ferramentas
	- v fmt
	- Profiling
- Tópicos Avançados
	- Código inseguro de memória
	- Structs com campos de referência
	- sizeof e __offsetof
	- Chamando C de V
	- Depuração de código C gerado
	- Compilação condicional
	- Pseudo variáveis em tempo de compilação
	- Reflexão em tempo de compilação
	- Sobrecarga limitada do operador
	- Inline assembly
	- Traduzindo C para V
	- Recarregando código quente
	- Compilação cruzada
	- Scripts de shell de plataforma cruzada em V
	- Atributos
	- Goto
- Apêndices
	- Palavras-chave
	- Operadores

### Olá Mundo

```vlang
fn main() {
	println('hello world')
}
```

Salve este trecho em um arquivo chamado ```hello.v```. Agora fazer: ```v run hello.v```.

>Isso pressupõe que você tenha vinculado simbolicamente seu V com v symlink, conforme descrito aqui . Se ainda não o fez, você deve digitar o caminho para V manualmente.

Parabéns - você acabou de escrever e executar seu primeiro programa em V!

Você pode compilar um programa sem execução com ```v hello.v```. Consulte ```v help``` para todos os comandos suportados.

No exemplo acima, você pode ver que as funções são declaradas com a ```fn palavra-chave```. O tipo de retorno é especificado após o nome da função. Nesse caso main, não retorna nada, portanto, não há tipo de retorno.

Como em muitas outras linguagens (como C, Go e Rust), main é o ponto de entrada do seu programa.

println é uma das poucas funções integradas. Ele imprime o valor passado para a saída padrão.

A declaração ```fn main()``` pode ser ignorada em programas de um arquivo. Isso é útil ao escrever pequenos programas, "scripts" ou apenas aprender o idioma. Para resumir, ```fn main()``` será ignorado neste tutorial.

Isso significa que um programa "hello world" em V é tão simples quanto

```vlang
println('hello world')
```

### Executando uma pasta de projeto com vários arquivos

Suponha que você tenha uma pasta com vários arquivos ```.v```, onde um deles contém sua função ```main()``` e os outros arquivos têm outras funções auxiliares. Eles podem ser organizados por tópico, mas ainda não estruturados o suficiente para serem seus próprios módulos reutilizáveis separados, e você deseja compilá-los todos em um programa.

Em outras linguagens, você teria que usar includes ou um sistema de construção para enumerar todos os arquivos, compilá-los separadamente para arquivos objeto e, em seguida, vinculá-los em um executável final.

No V, entretanto, você pode compilar e executar toda a pasta de arquivos ```.v``` juntos, usando apenas ```v run .```. Passar parâmetros também funciona, então você pode fazer: v run . --yourparam some_other_stuff

O exemplo acima irá primeiro compilar seus arquivos em um único programa (com o nome de sua pasta/projeto) e, em seguida, irá executar o programa --yourparam some_other_stuff transmitido a ele como parâmetros CLI.

Seu programa pode então usar os parâmetros CLI como este:

```vlang
import os

println(os.args)
```

NB: após uma execução bem-sucedida, V irá deletar o executável gerado. Se você quiser mantê-lo, use ```v -keepc run .```, ou apenas compile manualmente com ```v .```.

NB: qualquer sinalizador do compilador V deve ser passado antes do runcomando. Tudo após o arquivo/pasta de origem, será passado para o programa como está - não será processado por V.

### Comentários

```vlang
// Este é um comentário de uma única linha.
/*
 Este é um comentário de várias linhas.
	/* Pode ser aninhado. */
*/
```

### Funções

```vlang
fn main() {
	println(add(77, 33))
	println(sub(100, 50))
}

fn add(x int, y int) int {
	return x + y
}

fn sub(x int, y int) int {
	return x - y
}
```

Novamente, o tipo vem depois do nome do argumento.

Assim como em **Go** e **C**, as funções não podem ser sobrecarregadas. Isso simplifica o código e melhora a capacidade de manutenção e leitura.

Funções podem ser usadas antes de sua declaração: add e sub são declarados após main, mas ainda podem ser chamados de main. Isso é verdadeiro para todas as declarações em V e elimina a necessidade de arquivos de cabeçalho ou de pensar sobre a ordem dos arquivos e declarações.

### Retornando vários valores

```vlang
fn foo() (int, int) {
	return 2, 3
}

a, b := foo()
println(a) // 2
println(b) // 3
c, _ := foo() // ignore os valores usando `_`
```

### Visibilidade do símbolo

As funções são privadas (não exportadas) por padrão. Para permitir que outros módulos os usem, inclua pub. O mesmo se aplica a constantes e tipos.

Nota: pub só pode ser usado a partir de um módulo nomeado. Para obter informações sobre como criar um módulo, consulte Módulos.

### Variáveis

```vlang
name := 'Bob'
age := 20
large_number := i64(9999999999)
println(name)
println(age)
println(large_number)
```

As variáveis são declaradas e inicializadas com ```:=```. Essa é a única maneira de declarar variáveis em V. Isso significa que as variáveis sempre têm um valor inicial.

O tipo da variável é inferido do valor do lado direito. Para escolher um tipo diferente, use a conversão de tipo: a expressão T(v) converte o valor v para o tipo T.

Ao contrário da maioria das outras linguagens, V só permite definir variáveis em funções. Variáveis globais (nível de módulo) não são permitidas. Não há estado global em V (consulte Funções puras por padrão para obter detalhes).

Para consistência em diferentes bases de código, todos os nomes de variáveis e funções devem usar o estilo snake_case, ao contrário dos nomes de tipo, que devem usar PascalCase.

### Variáveis mutáveis

```vlang
mut age := 20
println(age)
age = 21
println(age)
```

Para alterar o valor da variável, use ```=```. Em V, as variáveis são imutáveis por padrão. Para poder alterar o valor da variável, você deve declará-la com ```mut```.

Tente compilar o programa acima após remover o ```mut``` da primeira linha.

### Inicialização vs atribuição

Observe a diferença (importante) entre ```:=``` e ```=```. ```:=``` é usado para declarar e inicializar, ```=``` é usado para atribuir.

```vlang
fn main() {
	age = 21
}
```

Este código não será compilado, porque a variável idade não foi declarada. Todas as variáveis precisam ser declaradas em V.

```vlang
fn main() {
	age := 21
}
```

Os valores de várias variáveis podem ser alterados em uma linha. Desta forma, seus valores podem ser trocados sem uma variável intermediária.

```vlang
mut a := 0
mut b := 1
println('$a, $b') // 0, 1
a, b = b, a
println('$a, $b') // 1, 0
```

### Erros de declaração

No modo de desenvolvimento, o compilador irá avisá-lo de que você não usou a variável (você receberá um aviso de "variável não usada"). No modo de produção (habilitado passando o sinalizador ```-prod``` para v - ```v -prod foo.v```), ele não compilará (como em Go).

```vlang
fn main() {
	a := 10
	if true {
		a := 20 // erro: redefinição de `a`
	}
	// aviso: variável não utilizada `a`
}
```

Ao contrário da maioria das linguagens, o sombreamento variável não é permitido. Declarar uma variável com um nome que já é usado em um escopo pai causará um erro de compilação.

No entanto, você pode criar sombra nos módulos importados, pois isso é muito útil em algumas situações:

```vlang
import ui
import gg

fn draw(ctx &gg.Context) {
	gg := ctx.parent.get_ui().gg
	gg.draw_rect(10, 10, 100, 50)
}
```

### Tipos

#### Tipos primitivos

```vlang
bool

string

i8    i16  int  i64      i128 (em breve)
byte  u16  u32  u64      u128 (em breve)

rune // representa um ponto de código Unicode

f32 f64

byteptr, voidptr, charptr, size_t // estes são usados principalmente para interoperabilidade C

any // semelhante ao void * de C e à interface de Go {}
```

Observe que, ao contrário de C e Go, int é sempre um número inteiro de 32 bits.

Há uma exceção à regra de que todos os operadores em V devem ter valores do mesmo tipo em ambos os lados. Um pequeno tipo primitivo de um lado pode ser promovido automaticamente se se ajustar completamente ao intervalo de dados do tipo do outro lado. Estas são as possibilidades permitidas:

```vlang
   i8 → i16 → int → i64
                  ↘     ↘
                    f32 → f64
                  ↗     ↗
 byte → u16 → u32 → u64 ⬎
      ↘     ↘     ↘      ptr
   i8 → i16 → int → i64 ⬏
```

Um valor ```int```, por exemplo, pode ser promovido automaticamente para ```f64``` ou ```i64```, mas não para ```u32```. (```u32``` significaria perda do sinal para valores negativos). A promoção de ```int``` para ```f32```, no entanto, atualmente é feita automaticamente (mas pode levar à perda de precisão para valores grandes).

Literais como ```123``` ou ```4.56``` são tratados de maneira especial. Eles não levam a promoções de tipo, no entanto, o padrão é ```int``` e ```f64``` respectivamente, quando seu tipo precisa ser decidido:

```vlang
u := u16(12)
v := 13 + u    // v é do tipo `u16` - sem promoção
x := f32(45.6)
y := x + 3.14  // x é do tipo `f32` - sem promoção
a := 75        // a é do tipo `int` - padrão para int literal
b := 14.7      // b é do tipo `f64` - padrão para literal flutuante
c := u + a     // c é do tipo `int` - promoção automática do valor de` u`
d := b + x     // d é do tipo `f64` - promoção automática do valor de` x`
```

#### Strings

```vlang
name := 'Bob'
println(name.len)
println(name[0]) // a indexação dá um byte B
println(name[1..3]) // fatiar dá uma string 'ob'
windows_newline := '\r\n' // escapar caracteres especiais como em C
assert windows_newline.len == 2
```

Em V, uma string é um array de bytes somente leitura. Os dados da string são codificados usando UTF-8. Os valores da string são imutáveis. Você não pode modificar elementos:

```vlang
mut s := 'hello 🌎'
s[0] = `H` // não permitido
```

>erro: não é possível atribuir a s[i], pois as strings V são imutáveis

Observe que a indexação de uma string produzirá um byte, não uma runa. Os índices correspondem a bytes na string, não a pontos de código Unicode. Literais de caracteres têm tipo runa. Para denotá-los, use `

```ṽlang
rocket := `🚀`
assert 'aloha!'[0] == `a`
```

As aspas simples e duplas podem ser usadas para denotar strings. Para consistência, vfmt converte aspas duplas em aspas simples, a menos que a string contenha um caractere de aspas simples.

Para strings brutas, prefixe r. Strings brutos não são escapados:

```vlang
s := r'hello\nworld'
println(s) // "hello\nworld"
```

Strings podem ser facilmente convertidos em inteiros:

```vlang
s := '42'
n := s.int() // 42
```

#### Interpolação de string

A sintaxe de interpolação básica é bastante simples - use ```$``` antes do nome de uma variável. A variável será convertida em uma string e incorporada ao literal:

```vlang
name := 'Bob'
println('Hello, $name!') // Hello, Bob!
```

Também funciona com os campos: **'age = $user.age'**. Se você precisar de expressões mais complexas, use **${}**: **'can register = ${user.age > 13}'**.

Especificadores de formato semelhantes aos de **printf()** do **C** também são suportados. **f, g, x, etc**. são opcionais e especificam o formato de saída. O compilador cuida do tamanho do armazenamento, portanto, não há **hd** ou **llu**.

```vlang
x := 123.4567
println('x = ${x:4.2f}')
println('[${x:10}]') // pad com espaços à esquerda => [   123.457]
println('[${int(x):-10}]') // pad com espaços à direita => [123       ]
println('[${int(x):010}]') // preencher com zeros à esquerda => [0000000123]
```

#### Operadores de string

```vlang
name := 'Bob'
bobby := name + 'by' // + é usado para concatenar strings
println(bobby) // "Bobby"
mut s := 'hello '
s += 'world' // `+=` é usado para anexar a uma string
println(s) // "hello world"
```

Todos os operadores em V devem ter valores do mesmo tipo em ambos os lados. Você não pode concatenar um inteiro com uma string:

```vlang
age := 10
println('age = ' + age) // não permitido
```

>erro: infix expr: não é possível usar int (expressão correta) como string

Temos que converter a idade em uma string:

```vlang
age := 11
println('age = ' + age.str())
```

ou use interpolação de string (preferencial):

```vlang
age := 12
println('age = $age')
```

#### Números

```vlang
a := 123
```

Isso atribuirá o valor de `123` a `a`. Por padrão, `a` terá o tipo `int`.

Você também pode usar notação hexadecimal, binária ou octal para literais inteiros:

```vlang
a := 0x7B
b := 0b01111011
c := 0o173
```

Todos eles receberão o mesmo valor, 123. Todos eles terão o tipo int, independentemente da notação usada.

V também suporta a escrita de números com `_` como separador:

```vlang
num := 1_000_000 // igual a 1000000
three := 0b0_11 // igual a 0b11
float_num := 3_122.55 // igual a 3122.55
hexa := 0xF_F // igual a 255
oct := 0o17_3 // igual a 0o173
```

Se quiser um tipo diferente de número inteiro, você pode usar a conversão:

```vlang
a := i64(123)
b := byte(42)
c := i16(12345)
```

A atribuição de números de ponto flutuante funciona da mesma maneira:

```vlang
f := 1.0
f1 := f64(3.14)
f2 := f32(3.14)
```

Se você não especificar o tipo explicitamente, por padrão, os literais flutuantes terão o tipo f64.

#### Arrays

```vlang
mut nums := [1, 2, 3]
println(nums) // "[1, 2, 3]"
println(nums[1]) // "2"
nums[1] = 5
println(nums) // "[1, 5, 3]"
println(nums.len) // "3"
nums = [] // A matriz agora está vazia
println(nums.len) // "0"
// Declare uma matriz vazia:
users := []int{}
```

O tipo de uma matriz é determinado pelo primeiro elemento:

- [1, 2, 3] é uma matriz de ints ([] int)
- ['a', 'b'] é uma matriz de strings ([] string).

O usuário pode especificar explicitamente o tipo do primeiro elemento: [byte (16), 32, 64, 128]. As matrizes V são homogêneas (todos os elementos devem ter o mesmo tipo). Isso significa que código como [1, 'a'] não será compilado.

O campo .len retorna o comprimento da matriz. Observe que é um campo somente leitura e não pode ser modificado pelo usuário. Os campos exportados são somente leitura por padrão em V. Consulte modificadores de acesso.

**Operações de array**

```vlang
mut nums := [1, 2, 3]
nums << 4
println(nums) // "[1, 2, 3, 4]"
// append array
nums << [5, 6, 7]
println(nums) // "[1, 2, 3, 4, 5, 6, 7]"
mut names := ['John']
names << 'Peter'
names << 'Sam'
// names << 10  <-- Isso não vai compilar. `names` é um array de strings.
println(names.len) // "3"
println('Alex' in names) // "false"
```

`<<` é um operador que anexa um valor ao final do array. Ele também pode anexar um array inteiro.

val no array retorna true se o array contiver val. Veja no operador.

**Inicializando propriedades de array**

Durante a inicialização, você pode especificar a capacidade do array (cap), seu comprimento inicial (len) e o elemento padrão (init):

```vlang
arr := []int{len: 5, init: -1}
// `[-1, -1, -1, -1, -1]`
```

Definir a capacidade melhora o desempenho das inserções, pois reduz o número de realocações necessárias:

```vlang
mut numbers := []int{cap: 1000}
println(numbers.len) // 0
// Agora, os elementos anexados não serão realocados
for i in 0 .. 1000 {
	numbers << i
}
```

Nota: O código acima usa um intervalo para declaração.


**Métodos de array**

Todos os arrays podem ser facilmente impressos com `println(arr)` e convertidos em uma string com `s := arr.str()`.

A cópia dos dados da matriz é feita com `.clone()`:

```vlang
nums := [1, 2, 3]
nums_copy := nums.clone()
```

As matrizes podem ser filtradas e mapeadas com eficiência com os métodos `.filter()` e `.map()`:

```vlang
nums := [1, 2, 3, 4, 5, 6]
even := nums.filter(it % 2 == 0)
println(even) // [2, 4, 6]
// filtro pode aceitar funções anônimas
even_fn := nums.filter(fn (x int) bool {
	return x % 2 == 0
})
println(even_fn)
words := ['hello', 'world']
upper := words.map(it.to_upper())
println(upper) // ['HELLO', 'WORLD']
// o mapa também pode aceitar funções anônimas
upper_fn := words.map(fn (w string) string {
	return w.to_upper()
})
println(upper_fn) // ['HELLO', 'WORLD']
```

**it** é uma variável embutida que se refere ao elemento atualmente sendo processado nos métodos de **filtro/mapa**.

Além disso, **.any()** e **.all()** podem ser usados para testar convenientemente os elementos que satisfazem uma condição.

```vlang
nums := [1, 2, 3]
println(nums.any(it == 2)) // true
println(nums.all(it >= 2)) // false
```

**Arrays multidimensionais**

Os arrays podem ter mais de uma dimensão.

Exemplo de array 2D:

```vlang
mut a := [][]int{len: 2, init: []int{len: 3}}
a[0][1] = 2
println(a) // [[0, 2, 0], [0, 0, 0]]
```

Exemplo de array 3D:

```vlang
mut a := [][][]int{len: 2, init: [][]int{len: 3, init: []int{len: 2}}}
a[0][1][1] = 2
println(a) // [[[0, 0], [0, 2], [0, 0]], [[0, 0],
```

**Classificando array**

A classificação de matrizes de todos os tipos é muito simples e intuitiva. As variáveis especiais **a** e **b** são usadas ao fornecer uma condição de classificação personalizada.

```vlang
mut numbers := [1, 3, 2]
numbers.sort() // 1, 2, 3
numbers.sort(a > b) // 3, 2, 1


struct User {
	age  int
	name string
}

mut users := [User{21, 'Bob'}, User{20, 'Zarkon'}, User{25, 'Alice'}]
users.sort(a.age < b.age) // classificar pelo campo User.age int
users.sort(a.name > b.name) // classificação reversa pelo campo de string User.name
```

**Fatias de array**

As fatias são arrays parciais. Eles representam cada elemento entre dois índices separados por um operador `..` O índice do lado direito deve ser maior ou igual ao índice do lado esquerdo.

Se um índice do lado direito estiver ausente, será considerado o comprimento do array. Se um índice do lado esquerdo estiver ausente, será considerado 0.

```vlang
nums := [0, 10, 20, 30, 40]
println(nums[1..4]) // [10, 20, 30]
println(nums[..4]) // [0, 10, 20, 30]
println(nums[1..]) // [10, 20, 30, 40]
```

Todas as operações de array podem ser realizadas em fatias. As fatias podem ser colocadas em um array do mesmo tipo.

```vlang
array_1 := [3, 5, 4, 7, 6]
mut array_2 := [0, 1]
array_2 << array_1[..3]
println(array_2) // [0, 1, 3, 5, 4]
```

#### Arrays de tamanho fixo

V também oferece suporte a arrays com tamanho fixo. Ao contrário dos arrays comuns, seu comprimento é constante. Você não pode anexar elementos a eles, nem reduzi-los. Você só pode modificar seus elementos no local.

No entanto, o acesso aos elementos de arrays de tamanho fixo é mais eficiente, eles precisam de menos memória do que arrays comuns e, ao contrário de arrays comuns, seus dados estão na pilha, então você pode querer usá-los como buffers se não quiser uma pilha adicional alocações.

A maioria dos métodos é definida para funcionar em arrays comuns, não em arrays de tamanho fixo. Você pode converter uma array de tamanho fixo em um array comum com fatiamento:

```vlang
mut fnums := [3]int{} // fnums é uma array de tamanho fixo com 3 elementos.
fnums[0] = 1
fnums[1] = 10
fnums[2] = 100
println(fnums) // => [1, 10, 100]
println(typeof(fnums).name) // => [3]int

anums := fnums[0..fnums.len]
println(anums) // => [1, 10, 100]
println(typeof(anums).name) // => []int
```

Observe que o fatiamento fará com que os dados do array de tamanho fixo sejam copiados para o array comum recém-criado.

#### Mapas

```vlang
mut m := map[string]int{} // a map with `string` keys and `int` values
m['one'] = 1
m['two'] = 2
println(m['one']) // "1"
println(m['bad_key']) // "0"
println('bad_key' in m) // Use `in` to detect whether such key exists
m.delete('two')
```

Os mapas podem ter chaves do tipo **string**, **rune**, **integer**, **float** ou **voidptr**.

Todo o mapa pode ser inicializado usando esta sintaxe curta:

```vlang
numbers := map{
	1: 'one'
	2: 'two'
}
println(numbers)
```

Se uma chave não for encontrada, um valor zero é retornado por padrão:

```vlang
sm := map{
	'abc': 'xyz'
}
val := sm['bad_key']
println(val) // ''
intm := map{
	1: 1234
	2: 5678
}
s := intm[3]
println(s) // 0
```

Também é possível usar um bloco ou {} para lidar com as chaves ausentes:

```vlang
mm := map[string]int{}
val := mm['bad_key'] or { panic('chave não encontrada') }
```

A mesma verificação opcional se aplica a arrays:

```vlang
arr := [1, 2, 3]
large_index := 999
val := arr[large_index] or { panic('fora dos limites') }
```

### Importações de módulos

Para obter informações sobre como criar um módulo, consulte Módulos.

Os módulos podem ser importados usando **import keyword**:

```vlang
import os

fn main() {
	// read text from stdin
	name := os.input('Enter your name: ')
	println('Hello, $name!')
}
```

Este programa pode usar qualquer definição pública do módulo **os**, como a função **input**. Consulte a documentação da biblioteca padrão para obter uma lista de módulos comuns e seus símbolos públicos.

Por padrão, você deve especificar o prefixo do módulo sempre que chamar uma função externa. Isso pode parecer prolixo no início, mas torna o código muito mais legível e fácil de entender - é sempre claro qual função de qual módulo está sendo chamado. Isso é especialmente útil em grandes bases de código.

Importações de módulos cíclicos não são permitidas, como em Go.

#### Importações seletivas

Você também pode importar funções e tipos específicos de módulos diretamente:

```vlang
import os { input }

fn main() {
	// ler texto de stdin
	name := input('Digite seu nome: ')
	println('Olá, $name!')
}
```

Nota: Isso não é permitido para constantes - elas devem ser sempre prefixadas.

Você pode importar vários símbolos específicos de uma vez:

```vlang
import os { input, user_os }

name := input('Digite seu nome: ')
println('Nome: $name')
os := user_os()
println('Seu sistema operacional é ${os}.')
```

#### Aliasing de importação de módulo

Qualquer nome de módulo importado pode ter um **alias** usando a palavra-chave **as**:

NOTA: este exemplo não será compilado a menos que você tenha criado **mymod/sha256.v**

```vlang
import crypto.sha256
import mymod.sha256 as mysha256

fn main() {
	v_hash := sha256.sum('hi'.bytes()).hex()
	my_hash := mysha256.sum('hi'.bytes()).hex()
	assert my_hash == v_hash
}
```

Você não pode criar um alias para uma função ou tipo importado. No entanto, você pode declarar novamente um tipo.

```vlang
import time
import math

type MyTime = time.Time

fn (mut t MyTime) century() int {
	return int(1.0 + math.trunc(f64(t.year) * 0.009999794661191))
}

fn main() {
	mut my_time := MyTime{
		year: 2020
		month: 12
		day: 25
	}
	println(time.new_time(my_time).utc_string())
	println('Century: $my_time.century()')
}
```

### Declarações e expressões

#### if

```vlang
a := 10
b := 20
if a < b {
	println('$a < $b')
} else if a > b {
	println('$a > $b')
} else {
	println('$a == $b')
}
```

As instruções **if** são bastante diretas e semelhantes à maioria das outras linguagens. Ao contrário de outras linguagens semelhantes a C, não há parênteses ao redor da condição e as chaves são sempre necessárias.

**if** pode ser usado como uma expressão:

```vlang
num := 777
s := if num % 2 == 0 { 'even' } else { 'odd' }
println(s)
// "odd"
```

**Verificações de tipo e conversão**

Você pode verificar o tipo atual de um tipo de soma usando **is** e sua forma negada **!is**.

Você pode fazer isso em um **if**:

```vlang
struct Abc {
	val string
}

struct Xyz {
	foo string
}

type Alphabet = Abc | Xyz

x := Alphabet(Abc{'test'}) // sum type
if x is Abc {
	// x é convertido automaticamente para Abc e pode ser usado aqui println(x)
}
if x !is Abc {
	println('Not Abc')
}
```

ou usando match:

```vlang
match x {
	Abc {
		// x é convertido automaticamente para Abc e pode ser usado aqui println(x)
	}
	Xyz {
		// x é convertido automaticamente para Xyz e pode ser usado aqui println(x)
	}
}
```

Isso também funciona com campos de estrutura:

```vlang
struct MyStruct {
	x int
}

struct MyStruct2 {
	y string
}

type MySumType = MyStruct | MyStruct2

struct Abc {
	bar MySumType
}

x := Abc{
	bar: MyStruct{123} // MyStruct será convertido para o tipo MySumType automaticamente
}
if x.bar is MyStruct {
	// x.bar é lançado automaticamente
	println(x.bar)
}
match x.bar {
	MyStruct {
		// x.bar é lançado automaticamente
		println(x.bar)
	}
	else {}
}
```

Variáveis mutáveis podem mudar e fazer uma conversão não seria seguro. No entanto, às vezes é necessário ter um tipo de elenco, apesar da mutabilidade. Nesse caso, o desenvolvedor deve marcar a expressão com uma palavra-chave **mut** para informar ao compilador que você está ciente do que está fazendo.

Funciona assim:

```vlang
mut x := MySumType(MyStruct{123})
if mut x is MyStruct {
	// x é lançado em MyStruct mesmo que seja mutável
	// sem a palavra-chave mut que não funcionaria println(x)
}
// mesmo com match
match mut x {
	MyStruct {
		// x é lançado em MyStruct mesmo que seja mutável
		// sem a palavra-chave mut que não funcionaria println(x)
	}
}
```

#### Operador in

**in** permite verificar se um **array** ou mapa contém um elemento. Para fazer o oposto, use **!in**.

```vlang
nums := [1, 2, 3]
println(1 in nums) // true
println(4 !in nums) // true
m := map{
	'one': 1
	'two': 2
}
println('one' in m) // true
println('three' !in m) // true
```

Também é útil para escrever expressões booleanas mais claras e compactas:

```vlang
enum Token {
	plus
	minus
	div
	mult
}

struct Parser {
	token Token
}

parser := Parser{}
if parser.token == .plus || parser.token == .minus || parser.token == .div || parser.token == .mult {
	// ...
}
if parser.token in [.plus, .minus, .div, .mult] {
	// ...
}
```

V otimiza tais expressões, portanto, ambas as instruções **if** acima produzem o mesmo código de máquina e nenhum **array** é criado.

#### Loop for

V tem apenas uma palavra-chave de loop: **for**, com várias formas.

**for/in**

Esta é a forma mais comum. Você pode usá-lo com um **array**, **map** ou um **range** de números.

**Array for**

```vlang
numbers := [1, 2, 3, 4, 5]
for num in numbers {
	println(num)
}
names := ['Sam', 'Peter']
for i, name in names {
	println('$i) $name')
	// Output: 0) Sam
	//         1) Peter
}
```

O formato **for value in arr** é usado para percorrer os elementos de um array. Se um índice for necessário, uma forma **for index**, **value in arr** pode ser usada.

Observe que o valor é somente leitura. Se precisar modificar a array durante o loop, você precisará declarar o elemento como mutável:

```vlang
mut numbers := [0, 1, 2]
for mut num in numbers {
	num++
}
println(numbers) // [1, 2, 3]
```

Quando um identificador é apenas um único sublinhado, ele é ignorado.

**Map for**

```vlang
m := map{
	'one': 1
	'two': 2
}
for key, value in m {
	println('$key -> $value')
	// Output: one -> 1
	//         two -> 2
}
```

A chave ou o valor podem ser ignorados usando um único sublinhado como identificador.

```vlang
m := map{
	'one': 1
	'two': 2
}
// iterate over keys
for key, _ in m {
	println(key)
	// Output: one
	//         two
}
// iterate over values
for _, value in m {
	println(value)
	// Output: 1
	//         2
}
```

**Range for**

```vlang
// Prints '01234'
for i in 0 .. 5 {
	print(i)
}
```

**low..high** significa um **range** exclusivo, que representa todos os valores de **low** até **up** mas não incluindo **high**.

**Condição for**

```vlang
mut sum := 0
mut i := 0
for i <= 100 {
	sum += i
	i++
}
println(sum) // "5050"
```

Esta forma de loop é semelhante aos loops while em outras linguagens. O loop irá parar de iterar assim que a condição booleana for avaliada como falsa. Novamente, não há parênteses em torno da condição e as chaves são sempre necessárias.

**Apenas for**

```vlang
mut num := 0
for {
	num += 2
	if num >= 10 {
		break
	}
}
println(num) // "10"
```

A condição pode ser omitida, resultando em um loop infinito.

**for estilo C**

```vlang
for i := 0; i < 10; i += 2 {
	// Don't print 6
	if i == 6 {
		continue
	}
	println(i)
}
```

Finalmente, existe o estilo **C** tradicional para **loop**. É mais seguro do que a forma **while** porque com o último é fácil esquecer de atualizar o contador e ficar preso em um **loop** infinito.

Aqui, **i** não precisa ser declarado com **mut**, pois sempre será mutável por definição.

**Instruções break e continue**

**break** e **continue** controlando o **loop for** mais interno por padrão. Você também pode usar **break** e **continue** seguido por um nome de rótulo para se referir a um **loop for** externo:

```vlang
outer: for i := 4; true; i++ {
	println(i)
	for {
		if i < 7 {
			continue outer
		} else {
			break outer
		}
	}
}
```

O rótulo deve preceder imediatamente o loop externo. O código acima é impresso:

```shell
4
5
6
7
```

#### Match

```vlang
os := 'windows'
print('V is running on ')
match os {
	'darwin' { println('macOS.') }
	'linux' { println('Linux.') }
	else { println(os) }
}
```

Uma declaração de correspondência é uma maneira mais curta de escrever uma sequência de declarações **if - else**. Quando uma ramificação correspondente for encontrada, o seguinte bloco de instrução será executado. A outra ramificação ligada a instrução **else** será executado quando nenhum outro ramificação corresponder.

```vlang
number := 2
s := match number {
	1 { 'one' }
	2 { 'two' }
	else { 'many' }
}
```

Uma expressão de correspondência retorna o valor da expressão final da ramificação correspondente.

```vlang
enum Color {
	red
	blue
	green
}

fn is_red_or_blue(c Color) bool {
	return match c {
		.red, .blue { true } // a vírgula pode ser usada para testar vários valores values
		.green { false }
	}
}
```

Uma instrução de correspondência também pode ser usada para ramificar as variantes de um enum usando a sintaxe abreviada **.variant_here**. Uma ramificação **else** não é permitida quando todas as ramificações forem completas.

```vlang
c := `v`
typ := match c {
	`0`...`9` { 'digit' }
	`A`...`Z` { 'uppercase' }
	`a`...`z` { 'lowercase' }
	else { 'other' }
}
println(typ)
// 'lowercase'
```

Você também pode usar intervalos como padrões de correspondência. Se o valor estiver dentro do intervalo de uma ramificação, essa ramificação será executada.

Observe que os intervalos usam `...` (três pontos) em vez de `..` (dois pontos). Isso ocorre porque o intervalo inclui o último elemento, em vez de exclusivo (`as .. ranges`). Usar `..` em um branch de correspondência gerará um erro.

Nota: **match** como uma expressão não pode ser usado em instruções loop **for** e **if**.

#### Defer

Uma instrução **defer** adia a execução de um bloco de instruções até que a função envolvente retorne.

```vlang
import os

fn read_log() {
	mut ok := false
	mut f := os.open('log.txt') or { panic(err.msg) }
	defer {
		f.close()
	}
	// ...
	if !ok {
		// instrução defer será chamada aqui, o arquivo será fechado
		return
	}
	// ...
	// instrução defer será chamada aqui, o arquivo será fechado
}
```

#### Structs

```vlang
struct Point {
	x int
	y int
}

mut p := Point{
	x: 10
	y: 20
}
println(p.x) // Os campos de estrutura são acessados usando um ponto
// Sintaxe literal alternativa para structs com 3 campos ou menos
p = Point{10, 20}
assert p.x == 10
```

#### Heap structs

As estruturas são alocadas na pilha. Para alocar uma estrutura no **heap** e obter uma referência a ela, use o prefixo **&**:

```vlang
struct Point {
	x int
	y int
}

p := &Point{10, 10}
// As referências têm a mesma sintaxe para acessar os campos
println(p.x)
```

O tipo de **p** é **& Point**. É uma referência ao **Point**. As referências são semelhantes aos ponteiros **Go** e referências **C++**.

#### Structs embutidas

V não permite subclasses, mas suporta estruturas incorporadas:

```vlang
struct Widget {
mut:
	x int
	y int
}

struct Button {
	Widget
	title string
}

mut button := Button{
	title: 'Click me'
}
button.x = 3
```

Sem a incorporação, teríamos que nomear o campo Widget e fazer:

```vlang
button.widget.x = 3
```

#### Valores de campo padrão

```vlang
struct Foo {
	n   int    // n is 0 by default
	s   string // s is '' by default
	a   []int  // a is `[]int{}` by default
	pos int = -1 // custom default value
}
```

Todos os campos de estrutura são zerados por padrão durante a criação da estrutura. Campos de matriz e mapa são alocados.

Também é possível definir valores padrão personalizados.

#### Os campos obrigatórios

```vlang
struct Foo {
	n int [required]
}
```

Você pode marcar um campo de estrutura com o atributo [required], para dizer a V que esse campo deve ser inicializado ao criar uma instância dessa estrutura.

Este exemplo não será compilado, pois o campo n não foi inicializado explicitamente:

```vlang
_ = Foo{}
```

#### Sintaxe literal curta de estrutura

```vlang
struct Point {
	x int
	y int
}

mut p := Point{
	x: 10
	y: 20
}
// you can omit the struct name when it's already known
p = {
	x: 30
	y: 4
}
assert p.y == 4
```

Omitir o nome da estrutura também funciona para retornar um literal de estrutura ou passar um como um argumento de função.

**Argumentos literais de estrutura final**

V não tem argumentos de função padrão ou argumentos nomeados, pois essa sintaxe literal de estrutura final pode ser usada em seu lugar:

```vlang
struct ButtonConfig {
	text        string
	is_disabled bool
	width       int = 70
	height      int = 20
}

struct Button {
	text   string
	width  int
	height int
}

fn new_button(c ButtonConfig) &Button {
	return &Button{
		width: c.width
		height: c.height
		text: c.text
	}
}

button := new_button(text: 'Click me', width: 100)
// a altura não está definida, então é o valor padrão
assert button.height == 20
```

Como você pode ver, o nome da estrutura e as chaves podem ser omitidos, em vez de:

```vlang
new_button(ButtonConfig{text:'Click me', width:100})
```

Isso só funciona para funções que usam uma estrutura para o último argumento.

#### Modificadores de acesso

Os campos **Struct** são privados e imutáveis por padrão (tornando os **structs** imutáveis também). Seus modificadores de acesso podem ser alterados com **pub** e **mut**. No total, existem 5 opções possíveis:

```vlang
struct Foo {
	a int // privado imutável (padrão)
mut:
	b int // private mutable
	c int // (você pode listar vários campos com o mesmo modificador de acesso)
pub:
	d int // public imutável (somente leitura)
pub mut:
	e int // público, mas mutável apenas no módulo pai
__global:
	// (não recomendado para uso, é por isso que a palavra-chave 'global' começa com __)
	f int // público e mutável dentro e fora do módulo pai
}
```

Por exemplo, aqui está o tipo de string definido no módulo integrado:

```vlang
struct string {
	str byteptr
pub:
	len int
}
```

É fácil ver a partir dessa definição que string é um tipo imutável. O ponteiro de **byte** com os dados da **string** não pode ser acessado externamente. O campo **len** é público, mas imutável:

```vlang
fn main() {
	str := 'hello'
	len := str.len // OK
	str.len++      // Erro de compilação
}
```

Isso significa que definir campos públicos somente leitura é muito fácil em V, sem necessidade de **getters/setters** ou propriedades.

#### Métodos

```vlang
struct User {
	age int
}

fn (u User) can_register() bool {
	return u.age > 16
}

user := User{
	age: 10
}
println(user.can_register()) // "false"
user2 := User{
	age: 20
}
println(user2.can_register()) // "true"
```

V não tem classes, mas você pode definir métodos em tipos. Um método é uma função com um argumento receptor especial. O receptor aparece em sua própria lista de argumentos entre a palavra-chave fn e o nome do método. Os métodos devem estar no mesmo módulo que o tipo de receptor.

Neste exemplo, o método **can_register** possui um receptor do tipo **user** denominado **u**. A convenção não é usar nomes de destinatários como **self** ou **this**, mas um nome curto, de preferência com uma letra.

#### Unions

Assim como **structs**, os **unions** suportam a incorporação.

```vlang
struct Rgba32_Component {
	r byte
	g byte
	b byte
	a byte
}

union Rgba32 {
	Rgba32_Component
	value u32
}

clr1 := Rgba32{
	value: 0x008811FF
}

clr2 := Rgba32{
	Rgba32_Component: {
		a: 128
	}
}

sz := sizeof(Rgba32)
unsafe {
	println('Size: ${sz}B,clr1.b: $clr1.b,clr2.b: $clr2.b')
}
```

Saída: Tamanho: 4B, clr1.b: 136, clr2.b: 0

O acesso de membro da union deve ser executado em um bloco não seguro.

Observe que os argumentos de estrutura incorporados não são necessariamente armazenados na ordem listada.

### Funções 2

#### Funções puras por padrão

As funções V são puras por padrão, o que significa que seus valores de retorno são uma função de seus argumentos apenas e sua avaliação não tem efeitos colaterais (além de **E/S**).

Isso é obtido por falta de variáveis globais e todos os argumentos de função sendo imutáveis por padrão, mesmo quando as referências são passadas.

V não é uma linguagem puramente funcional.

Existe um sinalizador do compilador para habilitar variáveis ​​globais (--enable-globals), mas isso se destina a aplicativos de baixo nível como kernels e drivers.

**Argumentos mutáveis**

É possível modificar os argumentos da função usando a palavra-chave **mut**:

```vlang
struct User {
	name string
mut:
	is_registered bool
}

fn (mut u User) register() {
	u.is_registered = true
}

mut user := User{}
println(user.is_registered) // "false"
user.register()
println(user.is_registered) // "true"
```

Neste exemplo, o receptor (que é simplesmente o primeiro argumento) é marcado como mutável, portanto, **register()** pode alterar o objeto do usuário. O mesmo funciona com argumentos de não receptores:

```vlang
fn multiply_by_2(mut arr []int) {
	for i in 0 .. arr.len {
		arr[i] *= 2
	}
}

mut nums := [1, 2, 3]
multiply_by_2(mut nums)
println(nums)
// "[2, 4, 6]"
```

Observe que você deve adicionar **mut** antes de **nums** ao chamar esta função. Isso deixa claro que a função que está sendo chamada modificará o valor.

É preferível retornar valores em vez de modificar argumentos. A modificação de argumentos deve ser feita apenas em partes críticas de desempenho de seu aplicativo para reduzir as alocações e cópias.

Por esta razão, V não permite a modificação de argumentos com tipos primitivos (por exemplo, inteiros). Apenas tipos mais complexos, como matrizes e mapas, podem ser modificados.

Use **user.register()** ou **user = register(user)** em vez de register (**mut user**).

**Sintaxe de atualização de Struct**

V torna mais fácil retornar uma versão modificada de um objeto:

```vlang
struct User {
	name          string
	age           int
	is_registered bool
}

fn register(u User) User {
	return {
		...u
		is_registered: true
	}
}

mut user := User{
	name: 'abc'
	age: 23
}
user = register(user)
println(user)
```
#### Número variável de argumentos

```vlang
fn sum(a ...int) int {
	mut total := 0
	for x in a {
		total += x
	}
	return total
}

println(sum()) // 0
println(sum(1)) // 1
println(sum(2, 3)) // 5
// using array decomposition
a := [2, 3, 4]
println(sum(...a)) // <-- using prefix ... here. output: 9
b := [5, 6, 7]
println(sum(...b)) // output: 18
```

#### Funções anônimas e de alta ordem

```vlang
fn sqr(n int) int {
	return n * n
}

fn cube(n int) int {
	return n * n * n
}

fn run(value int, op fn (int) int) int {
	return op(value)
}

fn main() {
	// Functions can be passed to other functions
	println(run(5, sqr)) // "25"
	// Anonymous functions can be declared inside other functions:
	double_fn := fn (n int) int {
		return n + n
	}
	println(run(5, double_fn)) // "10"
	// Functions can be passed around without assigning them to variables:
	res := run(5, fn (n int) int {
		return n + n
	})
	println(res) // "10"
	// You can even have an array/map of functions:
	fns := [sqr, cube]
	println(fns[0](10)) // "100"
	fns_map := map{
		'sqr':  sqr
		'cube': cube
	}
	println(fns_map['cube'](2)) // "8"
}
```

#### Referências

```vlang
struct Foo {}

fn (foo Foo) bar_method() {
	// ...
}

fn bar_function(foo Foo) {
	// ...
}
```

Se um argumento de função for imutável (como **foo** nos exemplos acima), V pode passá-lo por valor ou por referência. O compilador decidirá e o desenvolvedor não precisa pensar sobre isso.

Você não precisa mais se lembrar se deve passar a estrutura por valor ou por referência.

Você pode garantir que a estrutura seja sempre passada por referência adicionando **&**:

```vlang
struct Foo {
	abc int
}

fn (foo &Foo) bar() {
	println(foo.abc)
}
```

**foo** ainda é imutável e não pode ser alterado. Para isso, (**mut foo Foo**) deve ser usado.

Em geral, as referências de V são semelhantes aos ponteiros **Go** e referências **C++**. Por exemplo, uma definição de estrutura de árvore genérica seria assim:

```vlang
struct Node<T> {
	val   T
	left  &Node
	right &Node
}
```

#### Constantes

```vlang
const (
	pi    = 3.14
	world = '世界'
)

println(pi)
println(world)
```

As constantes são declaradas com **const**. Eles só podem ser definidos no nível do módulo (fora das funções). Os valores constantes nunca podem ser alterados. Você também pode declarar uma única constante separadamente:

```vlang
const e = 2.71828
```

As constantes V são mais flexíveis do que na maioria dos idiomas. Você pode atribuir valores mais complexos:

```vlang
struct Color {
	r int
	g int
	b int
}

fn rgb(r int, g int, b int) Color {
	return Color{
		r: r
		g: g
		b: b
	}
}

const (
	numbers = [1, 2, 3]
	red     = Color{
		r: 255
		g: 0
		b: 0
	}
	// avaliar chamada de função em tempo de compilação*
	blue    = rgb(0, 0, 255)
)

println(numbers)
println(red)
println(blue)
```

**\*WIP** - por enquanto, as chamadas de função são avaliadas na inicialização do programa

Variáveis globais normalmente não são permitidas, então isso pode ser muito útil.

**Prefixo de módulo necessário**

Ao nomear constantes, **snake_case** deve ser usado. Para distinguir consts de variáveis locais, o caminho completo para consts deve ser especificado. Por exemplo, para acessar o **PI const**, o nome **math.pi** completo deve ser usado tanto fora do módulo **math** quanto dentro dele. Essa restrição é relaxada apenas para o módulo **main** (aquele que contém seu **fn main()**), onde você pode usar o nome não qualificado de constantes definidas lá, ou seja, números, em vez de **main.numbers**.

**vfmt** cuida desta regra, então você pode digitar **println(pi**) dentro do módulo math, e **vfmt** irá atualizá-lo automaticamente para println (**math.pi**).

#### Funções integradas

Algumas funções são integradas como **println**. Aqui está a lista completa:

```vlang
fn print(s string) // imprima qualquer coisa no sdtout
fn println(s string) // imprime qualquer coisa e uma nova linha no sdtout

fn eprint(s string) // mesmo que print(), mas use stderr
fn eprintln(s string) // mesmo que println(), mas use stderr

fn exit(code int) // encerre o programa com um código de erro personalizado
fn panic(s string) // imprime uma mensagem e retrocede em stderr, e termina o programa com o código de erro 1
fn print_backtrace() // imprimir backtraces em stderr
```

**println** é uma função embutida simples, mas poderosa, que pode imprimir qualquer coisa: **strings**, **números**, **arrays**, **maps**, **structs**.

```vlang
struct User {
	name string
	age  int
}

println(1) // "1"
println('hi') // "hi"
println([1, 2, 3]) // "[1, 2, 3]"
println(User{ name: 'Bob', age: 20 }) // "User{name:'Bob', age:20}"
```

#### Impressão de tipos personalizados

Se você deseja definir um valor de impressão personalizado para o seu tipo, basta definir um método de **string .str()**:

```vlang
struct Color {
	r int
	g int
	b int
}

pub fn (c Color) str() string {
	return '{$c.r, $c.g, $c.b}'
}

red := Color{
	r: 255
	g: 0
	b: 0
}
println(red)
```

#### Módulos

Cada arquivo na raiz de uma pasta faz parte do mesmo módulo. Programas simples não precisam especificar o nome do módulo; nesse caso, o padrão é **'main'**.

V é uma linguagem muito modular. A criação de módulos reutilizáveis é incentivada e muito fácil de fazer. Para criar um novo módulo, crie um diretório com o nome do seu módulo contendo arquivos .v com o código:

```shell
cd ~/code/modules
mkdir mymodule
vim mymodule/myfile.v
```

```vlang
// myfile.v
module mymodule

// Para exportar uma função, temos que usar `pub`
pub fn say_hi() {
	println('olá do meu módulo!')
}
```

Agora você pode usar **mymodule** em seu código:

```vlang
import mymodule

fn main() {
	mymodule.say_hi()
}
```

- Os nomes dos módulos devem ser curtos, com menos de 10 caracteres.
- Os nomes dos módulos devem usar snake_case.
- Importações circulares não são permitidas.
- Você pode ter quantos arquivos .v desejar em um módulo.
- Você pode criar módulos em qualquer lugar.
- Todos os módulos são compilados estaticamente em um único executável.

**funções init**

Se você quiser que um módulo chame automaticamente algum código de **configuração/inicialização** quando for importado, você pode usar uma função **init** do módulo:

```vlang
fn init() {
	// your setup code here ...
}
```

A função **init** não pode ser pública - ela será chamada automaticamente. Este recurso é particularmente útil para inicializar uma biblioteca C.

#### Tipos 2

**Interfaces**

```vlang
struct Dog {
	breed string
}

struct Cat {
	breed string
}

fn (d Dog) speak() string {
	return 'woof'
}

fn (c Cat) speak() string {
	return 'meow'
}

// ao contrário de Go e TypeScript, as interfaces de V podem definir campos, não apenas métodos.
interface Speaker {
	breed string
	speak() string
}

dog := Dog{'Leonberger'}
cat := Cat{'Siamese'}

mut arr := []Speaker{}
arr << dog
arr << cat
for item in arr {
	println('a $item.breed says: $item.speak()')
}
```

Um tipo implementa uma interface implementando seus métodos e campos. Não há declaração explícita de intenção, nem palavra-chave "**implements**".

**Casting de uma interface**

Podemos testar o tipo subjacente de uma interface usando operadores de conversão dinâmica:

```vlang
interface Something {}

fn announce(s Something) {
	if s is Dog {
		println('a $s.breed dog') // `s` is automatically cast to `Dog` (smart cast)
	} else if s is Cat {
		println('a $s.breed cat')
	} else {
		println('something else')
	}
}
```

Para obter mais informações, consulte [Casts dinâmicos](https://github.com/vlang/v/blob/master/doc/docs.md#dynamic-casts).

**Definições de métodos de interface**

Também ao contrário do **Go**, uma interface pode implementar um método. Esses métodos não são implementados por **structs** que implementam essa interface.

Quando uma estrutura é encapsulada em uma interface que implementou um método com o mesmo nome daquele implementado por esta estrutura, apenas o método implementado na interface é chamado.

```vlang
struct Cat {}

fn (c Cat) speak() string {
	return 'meow!'
}

interface Adoptable {}

fn (a Adoptable) speak() string {
	return 'adote-me!'
}

fn new_adoptable() Adoptable {
	return Cat{}
}

fn main() {
	cat := Cat{}
	assert cat.speak() == 'Miau!'
	a := new_adoptable()
	assert a.speak() == 'adote-me!'
	if a is Cat {
		println(a.speak()) // Miau!
	}
}
```

#### Enums

```vlang
enum Color {
	red
	green
	blue
}

mut color := Color.red
// V sabe que `color` é uma` Color`. Não há necessidade de usar `color = Color.green` aqui.
color = .green
println(color) // "verde"
match color {
	.red { println('a cor era vermelha') }
	.green { println('a cor era verde') }
	.blue { println('a cor era azul') }
}
```

A correspondência de **enum** deve ser exaustiva ou ter uma ramificação **else**. Isso garante que, se um novo campo enum for adicionado, ele será tratado em todos os lugares do código.

Os campos **enum** não podem reutilizar palavras-chave reservadas. No entanto, palavras-chave reservadas podem ser escapadas com um **@**.

```vlang
enum Color {
	@none
	red
	green
	blue
}

color := Color.@none
println(color)
```

Inteiros podem ser atribuídos a campos **enum**.

```vlang
enum Mercado {
	maçã
	laranja = 5
	pera
}

g1 := int(Mercado.maçã)
g2 := int(Mercado.laranja)
g3 := int(Mercado.pera)
println('IDs de mercado: $g1, $g2, $g3')
```

Resultado: IDs de mercado: 0, 5, 6.

Operações não são permitidas em variáveis **enum**; eles devem ser explicitamente convertidos em **int**.

**Tipos de soma**

Uma instância de tipo de soma pode conter um valor de vários tipos diferentes. Use a palavra-chave type para declarar um tipo de soma:

```vlang
struct Moon {}

struct Mars {}

struct Venus {}

type World = Mars | Moon | Venus

sum := World(Moon{})
assert sum.type_name() == 'Moon'
println(sum)
```

O método interno **type_name** retorna o nome do tipo mantido atualmente.

Com os tipos de soma, você pode construir estruturas recursivas e escrever um código conciso, mas poderoso, nelas.

```vlang
// V's binary tree
struct Empty {}

struct Node {
	value f64
	left  Tree
	right Tree
}

type Tree = Empty | Node

// sum up all node values
fn sum(tree Tree) f64 {
	return match tree {
		Empty { f64(0) } // TODO: as match gets smarter just remove f64()
		Node { tree.value + sum(tree.left) + sum(tree.right) }
	}
}

fn main() {
	left := Node{0.2, Empty{}, Empty{}}
	right := Node{0.3, Empty{}, Node{0.4, Empty{}, Empty{}}}
	tree := Node{0.5, left, right}
	println(sum(tree)) // 0.2 + 0.3 + 0.4 + 0.5 = 1.4
}
```

**Casts dinâmicos**

Para verificar se uma instância de tipo de soma contém um determinado tipo, use soma é Tipo. Para lançar um tipo de soma para uma de suas variantes, você pode usar soma como Tipo:

```vlang
struct Moon {}

struct Mars {}

struct Venus {}

type World = Mars | Moon | Venus

fn (m Mars) dust_storm() bool {
	return true
}

fn main() {
	mut w := World(Moon{})
	assert w is Moon
	w = Mars{}
	// use `as` to access the Mars instance
	mars := w as Mars
	if mars.dust_storm() {
		println('bad weather!')
	}
}
```

assim como entrará em pânico se **w** não contiver uma instância de **Mars**. Uma maneira mais segura é usar uma conversão inteligente.

**Conversão inteligente**

```vlang
if w is Mars {
	assert typeof(w).name == 'Mars'
	if w.dust_storm() {
		println('bad weather!')
	}
}
```

**w** tem o tipo **Mars** dentro do corpo da instrução **if**. Isso é conhecido como tipagem sensível ao fluxo. Se **w** for um identificador mutável, não seria seguro se o compilador o convertesse de maneira inteligente sem um aviso. É por isso que você deve declarar um **mut** antes da expressão **is**:

```vlang
if mut w is Mars {
	assert typeof(w).name == 'Mars'
	if w.dust_storm() {
		println('mau tempo!')
	}
}
```

Caso contrário, **w** manteria seu tipo original.

>Isso funciona para variáveis simples e expressões complexas como **user.name**

**Tipos de soma correspondentes**

Você também pode usar a correspondência para determinar a variante:

```vlang
struct Moon {}

struct Mars {}

struct Venus {}

type World = Mars | Moon | Venus

fn open_parachutes(n int) {
	println(n)
}

fn land(w World) {
	match w {
		Moon {} // sem atmosfera
		Mars {
			// atmosfera leve
			open_parachutes(3)
		}
		Venus {
			// atmosfera pesada
			open_parachutes(1)
		}
	}
}
```

**match** deve ter um padrão para cada variante ou ter um outro **branch**.

```vlang
struct Moon {}
struct Mars {}
struct Venus {}

type World = Moon | Mars | Venus

fn (m Moon) moon_walk() {}
fn (m Mars) shiver() {}
fn (v Venus) sweat() {}

fn pass_time(w World) {
	match w {
		// usando a variável de correspondência secundária, neste caso `w` (conversão inteligente)
		Moon { w.moon_walk() }
		Mars { w.shiver() }
		else {}
	}
}
```

**Tipos option/result e tratamento de erros**

Os tipos de **option** são declarados com **?type**:

```vlang
struct User {
	id   int
	name string
}

struct Repo {
	users []User
}

fn (r Repo) find_user_by_id(id int) ?User {
	for user in r.users {
		if user.id == id {
			// V envolve isso automaticamente em um tipo option
			return user
		}
	}
	return error('User $id not found')
}

fn main() {
	repo := Repo{
		users: [User{1, 'Andrew'}, User{2, 'Bob'}, User{10, 'Charles'}]
	}
	user := repo.find_user_by_id(10) or { // Os tipos de option devem ser tratados como blocos `or`
		return
	}
	println(user.id) // "10"
	println(user.name) // "Charles"
}
```

V combina option e result em um tipo, então você não precisa decidir qual usar.

A quantidade de trabalho necessária para "atualizar" uma função para uma função opcional é mínima; você tem que adicionar um **?** para o tipo de retorno e retorna um erro quando algo dá errado.

Se você não precisa retornar uma mensagem de erro, você pode simplesmente **return none** (este é um equivalente mais eficiente do **return error("")**).

Este é o mecanismo primário para tratamento de erros em V. Eles ainda são valores, como em **Go**, mas a vantagem é que os erros não podem ser solucionados e tratá-los é muito menos prolixo. Ao contrário de outras linguagens, V não lida com exceções com blocos **throw/try/catch**.

**err** é definido dentro de um bloco **or** e é definido como a mensagem de **string** passada para a função **error()**. **err** estará vazio se **none** for retornado.

```vlang
user := repo.find_user_by_id(7) or {
	println(err) // "User 7 not found"
	return
}
```

#### Manipulação de opcionais

Existem quatro maneiras de lidar com um opcional. O primeiro método é propagar o erro:

```vlang
import net.http

fn f(url string) ?string {
	resp := http.get(url) ?
	return resp.text
}
```

**http.get** retorna **?http.Response**. Porque **?** segue a chamada, o erro será propagado para o chamador de **f**. Ao usar **?** após uma chamada de função que produz um opcional, a função envolvente também deve retornar um opcional. Se a propagação do erro for usada na função **main()**, ela entrará em **panic**, uma vez que o erro não pode ser propagado mais.

O corpo de **f** é essencialmente uma versão condensada de:

```vlang
	resp := http.get(url) or { return err }
	return resp.text
```

O segundo método é interromper a execução mais cedo:

```vlang
user := repo.find_user_by_id(7) or { return }
```

Aqui, você pode chamar **panic()** ou **exit()**, que interromperá a execução de todo o programa, ou usar uma instrução de fluxo de controle (retornar, interromper, continuar, etc) para interromper o bloco atual. Observe que **break** e **continue** só podem ser usados dentro de um **loop for**.

O V não tem uma maneira de "**unwrap**" à força um opcional (como outras linguagens fazem, por exemplo, Rust's **unwrap()** ou Swift's!). Para fazer isso, use ou {panic (err.msg)}.

O terceiro método é fornecer um valor padrão no final do bloco **or**. Em caso de erro, esse valor seria atribuído em seu lugar, portanto, deve ser do mesmo tipo que o conteúdo do tratamento de **option**.

```vlang
fn do_something(s string) ?string {
	if s == 'foo' {
		return 'foo'
	}
	return error('invalid string') // Could be `return none` as well
}

a := do_something('foo') or { 'default' } // a will be 'foo'
b := do_something('bar') or { 'default' } // b will be 'default'
println(a)
println(b)
```

O quarto método deve ser usado para desembrulhar:

```vlang
import net.http

if resp := http.get('https://google.com') {
	println(resp.text) // resp is a http.Response, not an optional
} else {
	println(err)
}
```

Acima, **http.get** retorna um **?http.Response**. **resp** está apenas no escopo para o primeiro ramo **if**. **err** está apenas no escopo do ramo **else**.

#### Genéricos

```vlang
struct Repo<T> {
	db DB
}

struct User {
	id   int
	name string
}

struct Post {
	id   int
	user_id int
	title string
	body string
}

fn new_repo<T>(db DB) Repo<T> {
	return Repo<T>{db: db}
}

// Esta é uma função genérica. V irá gerá-lo para cada tipo com o qual é usado.
fn (r Repo<T>) find_by_id(id int) ?T {
	table_name := T.name // neste exemplo, obter o nome do tipo nos dá o nome da tabela
	return r.db.query_one<T>('select * from $table_name where id = ?', id)
}

db := new_db()
users_repo := new_repo<User>(db) // retorna Repo<User>
posts_repo := new_repo<Post>(db) // retorna Repo<Post>
user := users_repo.find_by_id(1)? // find_by_id<User>
post := posts_repo.find_by_id(1)? // find_by_id<Post>
```

Atualmente, as definições de função genérica devem declarar seus parâmetros de tipo, mas no futuro V inferirá parâmetros de tipo genérico a partir de nomes de tipo de letra única em tipos de parâmetro de tempo de execução. É por isso que **find_by_id** pode omitir `<T>`, porque o argumento do receptor **r** usa um tipo genérico **T**.

Outro exemplo:

```vlang
fn compare<T>(a T, b T) int {
	if a < b {
		return -1
	}
	if a > b {
		return 1
	}
	return 0
}

// compare<int>
println(compare(1, 0)) // Outputs: 1
println(compare(1, 1)) //          0
println(compare(1, 2)) //         -1
// compare<string>
println(compare('1', '0')) // Outputs: 1
println(compare('1', '1')) //          0
println(compare('1', '2')) //         -1
// compare<f64>
println(compare(1.1, 1.0)) // Outputs: 1
println(compare(1.1, 1.1)) //          0
println(compare(1.1, 1.2)) //         -1
```

#### Simultaneidade

#### Gerando Tarefas Simultâneas

O modelo de simultaneidade de **V** é muito semelhante ao de **Go**. Para executar **foo()** simultaneamente em uma **thread** diferente, basta chamá-lo com go **foo()**:

```vlang
import math

fn p(a f64, b f64) { // função comum sem valor de retorno
	c := math.sqrt(a * a + b * b)
	println(c)
}

fn main() {
	go p(3, 4)
	// p será executado em thread paralela
}
```

Às vezes, é necessário esperar até que uma **thread** paralela termine. Isso pode ser feito atribuindo um identificador ao **thread** iniciado e chamando o método **wait()** para este identificador mais tarde:

```vlang
import math

fn p(a f64, b f64) { // ordinary function without return value
	c := math.sqrt(a * a + b * b)
	println(c) // prints `5`
}

fn main() {
	h := go p(3, 4)
	// p() runs in parallel thread
	h.wait()
	// p() has definitely finished
}
```

Essa abordagem também pode ser usada para obter um valor de retorno de uma função que é executada em um **thread** paralelo. Não há necessidade de modificar a própria função para poder chamá-la simultaneamente.

```vlang
import math { sqrt }

fn get_hypot(a f64, b f64) f64 { //       ordinary function returning a value
	c := sqrt(a * a + b * b)
	return c
}

fn main() {
	g := go get_hypot(54.06, 2.08) // gerar thread e lidar com ela
	h1 := get_hypot(2.32, 16.74) //   faça algum outro cálculo aqui
	h2 := g.wait() //                 obter o resultado da thread gerada
	println('Results: $h1, $h2') //   imprime `Results: 16,9, 54,1`
}
```

Se houver um grande número de tarefas, pode ser mais fácil gerenciá-las usando uma variedade de **threads**.

```vlang
import time

fn task(id int, duration int) {
	println('task $id begin')
	time.sleep(duration * time.millisecond)
	println('task $id end')
}

fn main() {
	mut threads := []thread{}
	threads << go task(1, 500)
	threads << go task(2, 900)
	threads << go task(3, 100)
	threads.wait()
	println('done')
}

// Output:
// task 1 begin
// task 2 begin
// task 3 begin
// task 3 end
// task 1 end
// task 2 end
// done
```

Além disso, para **threads** que retornam o mesmo tipo, chamar **wait()** no **array** de **threads** retornará todos os valores calculados.

```vlang
fn expensive_computing(i int) int {
	return i * i
}

fn main() {
	mut threads := []thread int{}
	for i in 1 .. 10 {
		threads << go expensive_computing(i)
	}
	// Join all tasks
	r := threads.wait()
	println('Todos os trabalhos concluídos: $r')
}

// Resultado: Todos os trabalhos concluídos: [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

#### Canais

Os canais são a forma preferida de comunicação entre as **co-rotinas**. Os canais da V funcionam basicamente como os do **Go**. Você pode empurrar objetos para um canal em uma extremidade e eliminar objetos na outra extremidade. Os canais podem ser armazenados em **buffer** ou sem **buffer** e é possível selecionar (**select**) vários canais.

**Sintaxe e uso**

Os canais têm o tipo **chan objtype**. Um comprimento de **buffer** opcional pode ser especificado como a propriedade **cap** na declaração:

```vlagn
ch := chan int{} // unbuffered - "synchronous"
ch2 := chan f64{cap: 100} // buffer length 100
```

Os canais não precisam ser declarados como **mut**. O comprimento do **buffer** não é parte do tipo, mas uma propriedade do objeto de canal individual. Os canais podem ser passados para co-rotinas como variáveis normais:

```vlang
fn f(ch chan int) {
	// ...
}

fn main() {
	ch := chan int{}
	go f(ch)
	// ...
}
```

Os objetos podem ser enviados para os canais usando o operador de seta(arrow). O mesmo operador pode ser usado para eliminar objetos da outra extremidade:

```vlang
ch := chan int{}
ch2 := chan f64{}
n := 5
x := 7.3
ch <- n
// push
ch2 <- x
mut y := f64(0.0)
m := <-ch // pop creating new variable
y = <-ch2 // pop into existing variable
```

Um canal pode ser fechado para indicar que nenhum outro objeto pode ser empurrado. Qualquer tentativa de fazer isso resultará em um **panic** de tempo de execução (com exceção de **select** e **try_push()** - veja abaixo). As tentativas de pop retornarão imediatamente se o canal associado tiver sido fechado e o buffer estiver vazio. Esta situação pode ser tratada usando uma ramificação ou (consulte [Tratamento de opcionais](https://github.com/vlang/v/blob/master/doc/docs.md#handling-optionals)).

```vlang
ch := chan int{}
ch2 := chan f64{}
// ...
ch.close()
// ...
m := <-ch or {
    println('channel has been closed')
}

// propagate error
y := <-ch2 ?
```

**Seleção de canal**

O comando **select** permite monitorar vários canais ao mesmo tempo sem carga da **CPU** perceptível. Consiste em uma lista de possíveis transferências e ramos associados de instruções - semelhante ao comando **match**:

```vlang
import time
fn main () {
	c := chan f64{}
	ch := chan f64{}
	ch2 := chan f64{}
	ch3 := chan f64{}
	mut b := 0.0
	// ...
	select {
		a := <-ch {
			// faça algo com `a`
		}
		b = <-ch2 {
			// fazer algo com a variável pré-declarada `b`
		}
		ch3 <- c {
			// faça algo se `c` foi enviado
		}
		> 500 * time.millisecond {
			// faça algo se nenhum canal estiver pronto em 0,5s
		}
	}
}
```

O **branch** de tempo limite é opcional. Se estiver ausente, **select** espera por um período de tempo ilimitado. Também é possível prosseguir imediatamente se nenhum canal estiver pronto no momento em que o **select** é chamado adicionando um outro **else { ... } branch** . **else** e **> timeout** são mutuamente exclusivos.

O comando **select** pode ser usado como uma expressão do tipo **bool** que se torna **false** se todos os canais forem fechados:

```vlang
if select {
	ch <- a {
		// ...
	}
} {
// channel was open
} else {
	// channel is closed
}
```

**Recursos especiais do canal**

Para fins especiais, existem algumas propriedades e métodos integrados:

```vlang
struct Abc {
	x int
}

a := 2.13
ch := chan f64{}
res := ch.try_push(a) // tente executar `ch <- a`
println(res)
l := ch.len // número de elementos na fila
c := ch.cap // comprimento máximo da fila
is_closed := ch.closed // sinalizador bool - `ch` foi fechado
println(l)
println(c)
mut b := Abc{}
ch2 := chan Abc{}
res2 := ch2.try_pop(b) // tente executar `b = <-ch2`
```

Os métodos **try_push/pop()** retornarão imediatamente com um dos resultados **.success**, **.not_ready** ou **.closed** - dependendo se o objeto foi transferido ou o motivo pelo qual não. O uso desses métodos e propriedades na produção não é recomendado - algoritmos baseados neles geralmente estão sujeitos a condições de corrida. Principalmente **.len** e **.closed** não devem ser usados para tomar decisões. Use ou ramificações, propagação de erro ou selecione ao invés (veja [Sintaxe e Uso](https://github.com/vlang/v/blob/master/doc/docs.md#syntax-and-usage) e [Seleção de Canal](https://github.com/vlang/v/blob/master/doc/docs.md#channel-select) acima).

#### Objetos Compartilhados

Os dados podem ser trocados entre uma co-rotina e o **thread** de chamada por meio de uma variável compartilhada. Essas variáveis devem ser criadas como **shared** e passadas para a co-rotina como tal também. A **struct** subjacente contém um **mutex** oculto que permite bloquear o acesso simultâneo usando **rlock** para somente leitura e bloqueio para acesso de leitura/gravação.

```vlang
struct St {
mut:
	x int // data to shared
}

fn (shared b St) g() {
	lock b {
		// read/modify/write b.x
	}
}

fn main() {
	shared a := St{
		x: 10
	}
	go a.g()
	// ...
	rlock a {
		// read a.x
	}
}
```

Variáveis compartilhadas devem ser **structs**, **arrays** ou **maps**.

#### Decodificando JSON

```vlang
import json

struct Foo {
	x int
}

struct User {
	name string
	age  int
	// Use o atributo `skip` para pular certos campos
	foo Foo [skip]
	// Se o nome do campo for diferente em JSON, ele pode ser especificado
	last_name string [json: lastName]
}

data := '{ "name": "Frodo", "lastName": "Baggins", "age": 25 }'
user := json.decode(User, data) or {
	eprintln('Falha ao decodificar json')
	return
}
println(user.name)
println(user.last_name)
println(user.age)
// Você também pode decodificar array JSON:
sfoos := '[{"x":123},{"x":456}]'
foos := json.decode([]Foo, sfoos) ?
println(foos[0].x)
println(foos[1].x)
```

Devido à natureza onipresente do **JSON**, o suporte para ele é integrado diretamente no V.

A função **json.decode** leva dois argumentos: o primeiro é o tipo no qual o valor **JSON** deve ser decodificado e o segundo é uma **string** contendo os dados **JSON**.

**V** gera código para codificação e decodificação **JSON**. Nenhuma reflexão de tempo de execução é usada. Isso resulta em um desempenho muito melhor.

#### Testando

**Asserts**

```vlang
fn foo(mut v []int) {
	v[0] = 1
}

mut v := [20]
foo(mut v)
assert v[0] < 4
```

Uma declaração **assert** verifica se sua expressão é avaliada como verdadeira. Se uma declaração falhar, o programa será abortado. **Asserts** devem ser usados apenas para detectar erros de programação. Quando uma declaração falha, ela é informada a **stderr** e os valores em cada lado de um operador de comparação (como <, ==) serão impressos quando possível. Isso é útil para encontrar facilmente um valor inesperado. As declarações assert podem ser usadas em qualquer função.

**Arquivos de teste**

```vlang
// hello.v
module main

fn hello() string {
	return 'Hello world'
}

fn main() {
	println(hello())
}
module main
// hello_test.v
fn test_hello() {
	assert hello() == 'Hello world'
}
```

Para executar o teste acima, use `v hello_test.v`. Isso verificará se a função **hello** está produzindo a saída correta. V executa todas as funções de teste no arquivo.

- Todas as funções de teste devem estar dentro de um arquivo de teste cujo nome termina em _test.v.
- Os nomes das funções de teste devem começar com test_ para marcá-los para execução.
- As funções normais também podem ser definidas em arquivos de teste e devem ser chamadas manualmente. Outros símbolos também podem ser definidos em arquivos de teste, por exemplo, tipos.
- Existem dois tipos de testes: externos e internos.
- Os testes internos devem declarar seu módulo, assim como todos os outros arquivos .v do mesmo módulo. Os testes internos podem até chamar funções privadas no mesmo módulo.
- Os testes externos devem importar os módulos que eles testam. Eles não têm acesso às funções / tipos privados dos módulos. Eles podem testar apenas a API externa / pública fornecida por um módulo.

No exemplo acima, test_hello é um teste interno, que pode chamar a função privada hello() porque hello_test.v tem o módulo principal, assim como hello.v, ou seja, ambos fazem parte do mesmo módulo. Observe também que, como o módulo principal é um módulo regular como os outros, os testes internos também podem ser usados para testar funções privadas nos arquivos .v do seu programa principal.

Você também pode definir funções de teste especiais em um arquivo de teste:

- testsuite_begin que será executado antes de todas as outras funções de teste.
- testsuite_end que será executado após todas as outras funções de teste.

**Executando testes**

Para executar funções de teste em um arquivo de teste individual, use `v foo_test.v`.

Para testar um módulo inteiro, use `v test mymodule`. Você também pode usar o teste v. para testar tudo dentro de sua pasta atual (e subpastas). Você pode passar a opção `-stats` para ver mais detalhes sobre os testes individuais executados.

#### Gerenciamento de memória

V evita fazer alocações desnecessárias em primeiro lugar usando tipos de valor, buffers de string, promovendo um estilo de código simples livre de abstração.

A maioria dos objetos (~ 90-100%) são liberados pelo mecanismo autofree do V: o compilador insere chamadas gratuitas necessárias automaticamente durante a compilação. A pequena porcentagem restante de objetos é liberada por meio da contagem de referência.

O desenvolvedor não precisa alterar nada em seu código. "Ele simplesmente funciona", como em Python, Go ou Java, exceto que não há GC pesado rastreando tudo ou RC caro para cada objeto.

**Control**

Você pode aproveitar as vantagens do mecanismo autofree do V e definir um método **free()** em tipos de dados personalizados:

```vlang
struct MyType {}

[unsafe]
fn (data &MyType) free() {
	// ...
}
```

Assim como o compilador libera tipos de dados **C** com **free()** do **C**, ele irá inserir estaticamente chamadas **free()** para o seu tipo de dados no final do tempo de vida de cada variável.

Para desenvolvedores que desejam ter mais controle de baixo nível, o autofree pode ser desabilitado com **-manualfree**, ou adicionando um [manualfree] em cada função que deseja gerenciar sua memória manualmente. (Veja atributos).

Nota: neste momento, o **autofree** está escondido atrás da bandeira **-autofree**. Ele será habilitado por padrão em V 0.3. Se o **autofree** não for usado, os programas em V vazarão memória.

**Exemplos**

```vlang
import strings

fn draw_text(s string, x int, y int) {
	// ...
}

fn draw_scene() {
	// ...
	name1 := 'abc'
	name2 := 'def ghi'
	draw_text('hello $name1', 10, 10)
	draw_text('hello $name2', 100, 10)
	draw_text(strings.repeat(`X`, 10000), 10, 50)
	// ...
}
```

As **strings** não escapam de **draw_text**, então elas são limpas quando a função termina.

Na verdade, com o sinalizador **-prealloc**, as duas primeiras chamadas não resultarão em nenhuma alocação. Essas duas strings são pequenas, então V usará um buffer pré-alocado para elas.

```vlang
struct User {
	name string
}

fn test() []int {
	number := 7 // stack variable
	user := User{} // struct allocated on stack
	numbers := [1, 2, 3] // array allocated on heap, will be freed as the function exits
	println(number)
	println(user)
	println(numbers)
	numbers2 := [4, 5, 6] // array that's being returned, won't be freed here
	return numbers2
}
```

#### ORM

(Ainda está em estado alfa)

V tem um ORM (mapeamento objeto-relacional) embutido que oferece suporte a SQLite e em breve oferecerá suporte a MySQL, Postgres, MS SQL e Oracle.

ORM da V oferece uma série de benefícios:

- Uma sintaxe para todos os dialetos SQL. (Migrar entre bancos de dados se torna muito mais fácil.)
- As consultas são construídas usando a sintaxe de V. (Não há necessidade de aprender outra sintaxe.)
- Segurança. (Todas as consultas são automaticamente higienizadas para evitar injeção de SQL.)
- Compile verificações de tempo. (Isso evita erros de digitação que só podem ser detectados durante o tempo de execução.)
- Legibilidade e simplicidade. (Você não precisa analisar manualmente os resultados de uma consulta e, em seguida, construir manualmente os objetos a partir dos resultados analisados.)

```vlang
import sqlite

struct Customer {
	// nome da estrutura tem que ser igual ao nome da tabela (por enquanto)
	id        int // um campo chamado `id` do tipo inteiro deve ser o primeiro campo
	name      string
	nr_orders int
	country   string
}

db := sqlite.connect('customers.db') ?
// select count(*) from Customer
nr_customers := sql db {
	select count from Customer
}
println('number of all customers: $nr_customers')
// A sintaxe V pode ser usada para construir consultas
// db.select retorna uma array
uk_customers := sql db {
	select from Customer where country == 'uk' && nr_orders > 0
}
println(uk_customers.len)
for customer in uk_customers {
	println('$customer.id - $customer.name')
}
// adicionando `limit 1`, dizemos a V que haverá apenas um objeto
customer := sql db {
	select from Customer where id == 1 limit 1
}
println('$customer.id - $customer.name')
// insert a new customer
new_customer := Customer{
	name: 'Bob'
	nr_orders: 10
}
sql db {
	insert new_customer into Customer
}
```

Para obter mais exemplos, consulte [vlib/orm/orm_test.v](https://github.com/vlang/v/blob/master/vlib/orm/orm_test.v).

#### Escrevendo Documentação

A forma como funciona é muito semelhante ao Go. É muito simples: não há necessidade de escrever documentação separadamente para seu código, o vdoc irá gerá-lo a partir de docstrings no código-fonte.

A documentação para cada função/tipo/const deve ser colocada logo antes da declaração:

```vlang
// clearall limpa todos os bits no array
fn clearall() {
}
```

O comentário deve começar com o nome da definição.

Às vezes, uma linha não é suficiente para explicar o que uma função faz; nesse caso, os comentários devem se estender para a função documentada usando comentários de uma única linha:

```vlang
// copy_all copia recursivamente todos os elementos do array por seus valores,
// se `dupes` é falso, todos os valores duplicados são eliminados no processo.
fn copy_all(dupes bool) {
	// ...
}
```

Por convenção, é preferível que os comentários sejam escritos no tempo presente.

Uma visão geral do módulo deve ser colocada no primeiro comentário logo após o nome do módulo.

Para gerar a documentação, use o vdoc, por exemplo `v doc net.http`.

#### Ferramentas

**v fmt**

Você não precisa se preocupar em formatar seu código ou definir diretrizes de estilo. v fmt cuida disso:

```vlang
v fmt file.v
```

É recomendável configurar seu editor, de modo que v fmt -w seja executado em cada salvamento. Uma execução vfmt geralmente é muito barata (leva <30ms).

Sempre execute v fmt -w file.v antes de enviar seu código.

**Profiling**

V tem um bom suporte para a criação de perfil de seus programas: `v -profile profile.txt run file.v` Isso produzirá um arquivo profile.txt, que você pode analisar.

O arquivo profile.txt gerado terá linhas com 4 colunas: a) quantas vezes uma função foi chamada b) quanto tempo no total uma função levou (em ms) c) quanto tempo, em média, uma chamada para uma função levou (em ns) d) o nome da função v

Você pode classificar na coluna 3 (tempo médio por função) usando: `sort -n -k3 profile.txt|tail`

Você também pode usar cronômetros para medir apenas partes do seu código explicitamente:

```vlang
import time

fn main() {
	sw := time.new_stopwatch({})
	println('Hello world')
	println('Greeting the world took: ${sw.elapsed().nanoseconds()}ns')
}
```

### Tópicos Avançados

#### Código inseguro de memória

Às vezes, para eficiência, você pode querer escrever código de baixo nível que pode corromper a memória ou ser vulnerável a explorações de segurança. V suporta escrever esse código, mas não por padrão.

V requer que quaisquer operações potencialmente inseguras para a memória sejam marcadas intencionalmente. Marcá-los também indica a qualquer pessoa que esteja lendo o código que pode haver violações de segurança de memória se houver um erro.

Exemplos de operações potencialmente inseguras para a memória são:

- Aritmética de ponteiro
- Indexação de ponteiro
- Conversão para ponteiro de um tipo incompatível
- Chamar certas funções C, por exemplo free, strlen e strncmp.

Para marcar operações potencialmente inseguras para a memória, coloque-as em um bloco inseguro (unsafe):

```vlang
// aloca 2 bytes não inicializados e retorna uma referência a eles
mut p := unsafe { malloc(2) }
p[0] = `h` // Erro: a indexação do ponteiro só é permitida em blocos `inseguros` (`unsafe`)
unsafe {
    p[0] = `h` // OK
    p[1] = `i`
}
p++ // Erro: aritmética de ponteiro só é permitida em blocos `inseguros`
unsafe {
    p++ // OK
}
assert *p == `i`
```

A prática recomendada é evitar colocar expressões seguras para a memória dentro de um bloco inseguro, para que o motivo do uso de inseguro seja o mais claro possível. Geralmente, qualquer código que você acha que é seguro para a memória não deve estar dentro de um bloco inseguro, para que o compilador possa verificá-lo.

Se você suspeita que seu programa viola a segurança da memória, você tem uma vantagem para encontrar a causa: observe os blocos inseguros (e como eles interagem com o código ao redor).

- Nota: Este é um trabalho em andamento.

#### Structs com campos de referência

As estruturas com referências requerem a configuração explícita do valor inicial para um valor de referência, a menos que a estrutura já defina seu próprio valor inicial.

Referências de valor zero, ou ponteiros nulos, NÃO serão suportados no futuro, por enquanto estruturas de dados como Listas Vinculadas ou Árvores Binárias que dependem de campos de referência que podem usar o valor 0, entendendo que não é seguro e pode causar pânico.

```vlang
struct Node {
	a &Node
	b &Node = 0 // Auto-inicializado como nulo, use com cuidado!
}

// Os campos de referência devem ser inicializados, a menos que um valor inicial seja declarado.
// Zero (0) está OK, mas use com cuidado, é um ponteiro nulo.
foo := Node{
	a: 0
}
bar := Node{
	a: &foo
}
baz := Node{
	a: 0
	b: 0
}
qux := Node{
	a: &foo
	b: &bar
}
println(baz)
println(qux)
```

#### sizeof e __offsetof

- sizeof (Tipo) fornece o tamanho de um tipo em bytes.
- __offsetof (Struct, field_name) fornece o deslocamento em bytes de um campo de struct.

```vlang
struct Foo {
	a int
	b int
}

assert sizeof(Foo) == 8
assert __offsetof(Foo, a) == 0
assert __offsetof(Foo, b) == 4
```

#### Chamando C de V

**Exemplo**

```vlang
#flag -lsqlite3
#include "sqlite3.h"
// Veja também o exemplo de https://www.sqlite.org/quickstart.html
struct C.sqlite3 {
}

struct C.sqlite3_stmt {
}

type FnSqlite3Callback = fn (voidptr, int, &charptr, &charptr) int

fn C.sqlite3_open(charptr, &&C.sqlite3) int

fn C.sqlite3_close(&C.sqlite3) int

fn C.sqlite3_column_int(stmt &C.sqlite3_stmt, n int) int

// ... você também pode apenas definir o tipo de parâmetro e omitir o prefixo C.
fn C.sqlite3_prepare_v2(&C.sqlite3, charptr, int, &&C.sqlite3_stmt, &charptr) int

fn C.sqlite3_step(&C.sqlite3_stmt)

fn C.sqlite3_finalize(&C.sqlite3_stmt)

fn C.sqlite3_exec(db &C.sqlite3, sql charptr, cb FnSqlite3Callback, cb_arg voidptr, emsg &charptr) int

fn C.sqlite3_free(voidptr)

fn my_callback(arg voidptr, howmany int, cvalues &charptr, cnames &charptr) int {
	unsafe {
		for i in 0 .. howmany {
			print('| ${cstring_to_vstring(cnames[i])}: ${cstring_to_vstring(cvalues[i]):20} ')
		}
	}
	println('|')
	return 0
}

fn main() {
	db := &C.sqlite3(0) // isso significa `sqlite3 * db = 0`
	// passar uma string literal para uma chamada de função C resulta em uma string C, não uma string V
	C.sqlite3_open('users.db', &db)
	// C.sqlite3_open(db_path.str, &db)
	query := 'select count(*) from users'
	stmt := &C.sqlite3_stmt(0)
	// NB: você também pode usar o campo `.str` de uma string V,
	// para obter sua representação terminada em zero no estilo C
	C.sqlite3_prepare_v2(db, query.str, -1, &stmt, 0)
	C.sqlite3_step(stmt)
	nr_users := C.sqlite3_column_int(stmt, 0)
	C.sqlite3_finalize(stmt)
	println('Existem usuários $nr_users no banco de dados.')
	//
	error_msg := charptr(0)
	query_all_users := 'select * from users'
	rc := C.sqlite3_exec(db, query_all_users.str, my_callback, 7, &error_msg)
	if rc != C.SQLITE_OK {
		eprintln(cstring_to_vstring(error_msg))
		C.sqlite3_free(error_msg)
	}
	C.sqlite3_close(db)
}
```

#### Passando sinalizadores de compilação C

Adicione diretivas #flag ao topo de seus arquivos V para fornecer sinalizadores de compilação C como:

- -I para adicionar caminhos de pesquisa de arquivos de inclusão C
- -l para adicionar nomes de biblioteca C que você deseja vincular
- -L para adicionar caminhos de pesquisa de arquivos de biblioteca C
- -D para definir variáveis de tempo de compilação

Você pode (opcionalmente) usar sinalizadores diferentes para alvos diferentes. Atualmente, os sinalizadores linux, darwin, freebsd e windows são suportados.

NB: Cada bandeira deve ir em sua própria linha (por enquanto)

```vlang
#flag linux -lsdl2
#flag linux -Ivig
#flag linux -DCIMGUI_DEFINE_ENUMS_AND_STRUCTS=1
#flag linux -DIMGUI_DISABLE_OBSOLETE_FUNCTIONS=1
#flag linux -DIMGUI_IMPL_API=
```

No comando de construção do console, você pode usar:

- -cflags para passar sinalizadores personalizados para o compilador C de backend.
- -cc para alterar o compilador backend C padrão.
- Por exemplo: -cc gcc-9 -cflags -fsanitize=thread.

Você pode definir uma variável de ambiente VFLAGS em seu terminal para armazenar suas configurações -cc e -cflags, em vez de incluí-las no comando build a cada vez.

**#pkgconfig**

Adicionar a diretiva #pkgconfig é usada para informar ao compilador quais módulos devem ser usados para compilar e vincular usando os arquivos pkg-config fornecidos pelas respectivas dependências.

Desde que backticks não possam ser usados em #flag e processos de spawning não sejam desejáveis por razões de segurança e portabilidade, V usa sua própria biblioteca pkgconfig que é compatível com o freedesktop padrão.

Se nenhuma sinalização for passada, ele adicionará --cflags e --libs, ambas as linhas abaixo fazem o mesmo:

```shell
#pkgconfig r_core
#pkgconfig --cflags --libs r_core
```

Os arquivos .pc são pesquisados em uma lista codificada de caminhos padrão do pkg-config, o usuário pode adicionar caminhos extras usando a variável de ambiente PKG_CONFIG_PATH. Vários módulos podem ser passados.

**Incluindo código C**

Você também pode incluir o código C diretamente em seu módulo V. Por exemplo, digamos que seu código C esteja localizado em uma pasta chamada 'c' dentro da pasta do módulo. Então:

- Coloque um arquivo v.mod dentro da pasta de nível superior do seu módulo (se você criou seu módulo com v new, você já tem o arquivo v.mod). Por exemplo:

```vlang
Module {
	name: 'mymodule',
	description: 'Meu melhor módulo envolve uma biblioteca C simples.',
	version: '0.0.1'
	dependencies: []
}
```

- Adicione estas linhas ao topo do seu módulo:

```shell
#flag -I @VROOT/c
#flag @VROOT/c/implementation.o
#include "header.h"
```

NB: @VROOT será substituído por V com a pasta pai mais próxima, onde existe um arquivo v.mod. Qualquer arquivo .v ao lado ou abaixo da pasta onde está o arquivo v.mod pode usar #flag @ VROOT / abc para se referir a essa pasta. A pasta @VROOT também é anexada ao caminho de pesquisa do módulo, para que você possa importar outros módulos em seu @VROOT, apenas nomeando-os.

As instruções acima farão com que V procure um arquivo .o compilado em sua pasta de módulo folder/c/implementation.o. Se V o encontrar, o arquivo .o será vinculado ao executável principal, que usou o módulo. Se não o encontrar, V assume que existe um arquivo @VROOT/c/implementation.c, e tenta compilá-lo em um arquivo .o, então o usará.

Isso permite que você tenha um código C, que está contido em um módulo V, para que sua distribuição seja mais fácil. Você pode ver um exemplo mínimo completo para usar o código C em um módulo V wrapper aqui: project_with_c_code. Outro exemplo, demonstrando a passagem de estruturas de C para V e vice-versa: interoperar entre C para V para C.

**Tipos C**

Strings C normais terminadas em zero podem ser convertidas em strings V com unsafe {charptr (cstring) .vstring ()} ou se você já sabe seu comprimento com unsafe {charptr (cstring) .vstring_with_len (len)}.

NB: Os métodos .vstring () e .vstring_with_len () NÃO criam uma cópia do cstring, então você NÃO deve liberá-lo após chamar o método .vstring (). Se você precisar fazer uma cópia da string C (algumas APIs libc como getenv praticamente exigem isso, já que retornam ponteiros para a memória libc interna), você pode usar cstring_to_vstring (cstring).

No Windows, as APIs C geralmente retornam as chamadas strings largas (codificação utf16). Eles podem ser convertidos em strings V com string_from_wide (& u16 (cwidestring)).

V tem esses tipos para facilitar a interoperabilidade com C:

- voidptr para C's void*,
- byteptr para o byte* de C e
- charptr para char* de C.
- & charptr para C's char**

Para converter um voidptr para uma referência V, use user := &User (user_void_ptr).

voidptr também pode ser referenciado em uma estrutura V por meio de casting: user := User (user_void_ptr).

[um exemplo de um módulo que chama o código C de V](https://github.com/vlang/v/blob/master/vlib/v/tests/project_with_c_code/mod1/wrapper.v)

#### Declarações C

Os identificadores C são acessados com o prefixo C da mesma forma como os identificadores específicos do módulo são acessados. As funções devem ser declaradas novamente em V antes que possam ser usadas. Qualquer tipo C pode ser usado por trás do prefixo C, mas os tipos devem ser declarados novamente em V para acessar os membros do tipo.

Para redeclarar tipos complexos, como no seguinte código C:

```vlang
struct SomeCStruct {
	uint8_t implTraits;
	uint16_t memPoolData;
	union {
		struct {
			void* data;
			size_t size;
		};

		DataView view;
	};
};
```

membros de subestruturas de dados podem ser declarados diretamente na estrutura de contenção conforme abaixo:

```vlang
struct C.SomeCStruct {
	implTraits  byte
	memPoolData u16
	// Esses membros fazem parte de subestruturas de dados que atualmente não podem ser representadas em V.
	// Declará-los diretamente dessa forma é suficiente para o acesso.
	// union {
	// struct {
	data voidptr
	size size_t
	// }
	view C.DataView
	// }
}
```

A existência dos membros de dados é informada a V, e eles podem ser usados sem recriar exatamente a estrutura original.

Como alternativa, você pode [incorporar](https://github.com/vlang/v/blob/master/doc/docs.md#embedded-structs) as subestruturas de dados para manter uma estrutura de código paralela.

#### Depuração de código C gerado

Para depurar problemas no código C gerado, você pode passar estes sinalizadores:

- -g - produz um executável menos otimizado com mais informações de depuração nele. V irá forçar os números de linha dos arquivos .v nos stacktraces, que o executável irá produzir em pânico. Normalmente é melhor passar -g, a menos que você esteja escrevendo um código de baixo nível; nesse caso, use a próxima opção -cg.
- -cg - produz um executável menos otimizado com mais informações de depuração nele. O executável usará números de linha de origem C neste caso. É freqüentemente usado em combinação com -keepc, para que você possa inspecionar o programa C gerado em caso de pânico ou para que seu depurador (gdb, lldb etc.) possa mostrar o código-fonte C gerado.
- -showcc - imprime o comando C que é usado para construir o programa.
- -show-c-output - imprime a saída que seu compilador C produziu enquanto compilava seu programa.
- -keepc - não exclui o arquivo de código-fonte C gerado após uma compilação bem-sucedida. Além disso, continue usando o mesmo caminho de arquivo, para que seja mais estável e mais fácil de manter aberto em um editor / IDE.

Para obter a melhor experiência de depuração, se você estiver escrevendo um wrapper de baixo nível para uma biblioteca C existente, você pode passar vários desses sinalizadores ao mesmo tempo: v -keepc -cg -showcc yourprogram.v, em seguida, execute o depurador (gdb / lldb ) ou IDE no seu programa executável produzido.

Se você deseja apenas inspecionar o código C gerado, sem compilação adicional, você também pode usar o sinalizador -o (por exemplo, -o file.c). Isso fará com que V produza o file.c e pare.

Se quiser ver o código-fonte C gerado para apenas uma única função C, por exemplo main, você pode usar: -printfn main -o file.c.

Para ver uma lista detalhada de todos os sinalizadores que V suporta, use v help, v help build ev help build-c.

### Compilação condicional

#### Compilar o código de tempo

$ é usado como um prefixo para operações em tempo de compilação.

**$if**

```vlang
// Suporte para várias condições em um ramo
$if ios || android {
	println('Running on a mobile device!')
}
$if linux && x64 {
	println('64-bit Linux.')
}
// Uso como expressão
os := $if windows { 'Windows' } $else { 'UNIX' }
println('Using $os')
// ramos $if-$else
$if tinyc {
	println('tinyc')
} $else $if clang {
	println('clang')
} $else $if gcc {
	println('gcc')
} $else {
	println('different compiler')
}
$if test {
	println('testing')
}
// v -cg ...
$if debug {
	println('debugging')
}
// v -prod ...
$if prod {
	println('production build')
}
// v -d option ...
$if option ? {
	println('custom option')
}
```

Se você deseja que um if seja avaliado em tempo de compilação, ele deve ser prefixado com um sinal $. No momento, ele pode ser usado para detectar um sistema operacional, compilador, plataforma ou opções de compilação. $ if debug é uma opção especial como $ if windows ou $ if x32. Se estiver usando um ifdef personalizado, você precisa da opção $if ? {} e compilar com a opção v -d. Lista completa de opções integradas:

OS	                    | Compilers    | Platforms      | Other
----------------------- | ------------ | -------------- | -----
windows, linux, macos   | gcc, tinyc   | amd64, aarch64 | debug, prod, test
mac, darwin, ios,       | clang, mingw | x64, x32       | js, glibc, prealloc
android,mach, dragonfly |  msvc        | little_endian  | no_bounds_checking
gnu, hpux, haiku, qnx   | cplusplus    | big_endian     |
solaris, linux_or_macos |              |                |

**$embed_file**

```vlang
import os
fn main() {
	embedded_file := $embed_file('v.png')
	os.write_file('exported.png', embedded_file.to_string()) ?
}
```

V pode embutir arquivos arbitrários no executável com a chamada de tempo de compilação $embed_file(`<path>`). Os caminhos podem ser absolutos ou relativos ao arquivo de origem.

Quando você não usa -prod, o arquivo não é incorporado. Em vez disso, ele será carregado na primeira vez que seu programa chamar f.data() em tempo de execução, tornando mais fácil alterar em programas de editor externo, sem a necessidade de recompilar seu executável.

Ao compilar com -prod, o arquivo será embutido em seu executável, aumentando o tamanho do binário, mas tornando-o mais autocontido e, portanto, mais fácil de distribuir. Nesse caso, f.data() não causará nenhum IO e sempre retornará os mesmos dados.

**$tmpl para incorporar e analisar arquivos de modelo V**

V tem uma linguagem de modelo simples para modelos de texto e html, e eles podem ser facilmente incorporados via $tmpl ('path/to/template.txt'):

```vlang
fn build() string {
	name := 'Peter'
	age := 25
	numbers := [1, 2, 3]
	return $tmpl('1.txt')
}

fn main() {
	println(build())
}
```

**1.txt:**

```shell
name: @name

age: @age

numbers: @numbers

@for number in numbers
  @number
@end
```

**saída:**

```shell
name: Peter

age: 25

numbers: [1, 2, 3]

1
2
3
```

**$env**

```vlang
module main

fn main() {
	compile_time_env := $env('ENV_VAR')
	println(compile_time_env)
}
```

V pode trazer valores em tempo de compilação a partir de variáveis de ambiente. $env('ENV_VAR') também pode ser usado nas instruções #flag e #include de nível superior: #flag linux -I $ env('JAVA_HOME')/include.

#### Arquivos específicos do ambiente

Se um arquivo tiver um sufixo específico do ambiente, ele será compilado apenas para esse ambiente.

- .js.v => será usado apenas pelo backend JS. Esses arquivos podem conter JS. código.
- .c.v => será usado apenas pelo backend C. Esses arquivos podem conter código C..
- .x64.v => será usado apenas pelo backend x64 de V.
- _nix.c.v => será usado apenas em sistemas Unix (não Windows).
- _ $ {os} .c.v => será usado apenas no sistema operacional específico. Por exemplo, _windows.c.v será usado apenas ao compilar no Windows ou com -os windows.
- _default.c.v => será usado apenas se NÃO houver um arquivo de plataforma mais específico. Por exemplo, se você tem file_linux.c.v e file_default.c.v, e está compilando para linux, então apenas file_linux.c.v será usado e file_default.c.v será ignorado.

Aqui está um exemplo mais completo: main.v:

```vlang
module main
fn main() { println(message) }
```

main_default.c.v:

```vlang
module main
const ( message = 'Hello world' )
```

main_linux.c.v:

```vlang
module main
const ( message = 'Hello linux' )
```

main_windows.c.v:

```vlang
module main
const ( message = 'Hello windows' )
```

Com o exemplo acima:

- ao compilar para o Windows, você obterá 'Hello windows'
- quando você compilar para o Linux, você obterá 'Hello linux'
- ao compilar para qualquer outra plataforma, você receberá a mensagem não específica 'Olá, mundo'.
- _d_customflag.v => será usado apenas se você passar -d customflag para V. Isso corresponde a $ if customflag ? {}, mas para um arquivo inteiro, não apenas um único bloco. customflag deve ser um identificador snake_case, não pode conter caracteres arbitrários (apenas letras latinas minúsculas + números + _). NB: um postfix combinatório _d_customflag_linux.c.v não funcionará. Se você precisar de um arquivo de sinalizador personalizado, que tenha código dependente da plataforma, use o postfix _d_customflag.v e, em seguida, use blocos condicionais de tempo de compilação dependentes de plaftorm dentro dele, ou seja, $ if linux {} etc.
- _notd_customflag.v => semelhante a _d_customflag.v, mas será usado apenas se você NÃO passar -d customflag para V.

### Pseudo variáveis de tempo de compilação

V também dá ao seu código acesso a um conjunto de variáveis de pseudo string, que são substituídas no tempo de compilação:

- @FN => substituído pelo nome da função V atual
- @METHOD => substituído por ReceiverType.MethodName
- @MOD => substituído pelo nome do módulo V atual
- @STRUCT => substituído pelo nome da estrutura V atual
- @FILE => substituído pelo caminho do arquivo de origem V
- @LINE => substituído pelo número da linha V onde aparece (como uma string).
- @COLUMN => substituído pela coluna onde aparece (como uma string).
- @VEXE => substituído pelo caminho para o compilador V
- @VHASH => substituído pelo hash de commit encurtado do compilador V (como uma string).
- @VMOD_FILE => substituído pelo conteúdo do arquivo v.mod mais próximo (como uma string).

Isso permite que você faça o seguinte exemplo, útil ao depurar/registrar/rastrear seu código:

```vlang
eprintln('file: ' + @FILE + ' | line: ' + @LINE + ' | fn: ' + @MOD + '.' + @FN)
```

Outro exemplo, é se você deseja incorporar a versão / nome de v.mod dentro de seu executável:

```vlang
import v.vmod
vm := vmod.decode( @VMOD_FILE ) or { panic(err.msg) }
eprintln('$vm.name $vm.version\n $vm.description')
```

### Ajuste de desempenho

O código C gerado geralmente é rápido o suficiente, quando você compila seu código com -prod. No entanto, existem algumas situações em que você pode querer dar dicas adicionais ao compilador, para que ele possa otimizar ainda mais alguns blocos de código.

NB: Eles raramente são necessários e não devem ser usados, a menos que você crie um perfil de seu código e veja que há benefícios significativos para eles. Para citar a documentação do gcc: "os programadores são notoriamente ruins em prever como seus programas realmente funcionam".

[inline] - você pode marcar funções com [inline], de modo que o compilador C tentará embuti-las, o que em alguns casos pode ser benéfico para o desempenho, mas pode impactar o tamanho do seu executável.

[direct_array_access] - nas funções marcadas com [direct_array_access], o compilador irá traduzir as operações de array diretamente em operações de array C - omitindo a verificação de limites. Isso pode economizar muito tempo em uma função que itera em um array, mas ao custo de tornar a função insegura - a menos que os limites sejam verificados pelo usuário.

if _likely_(bool expression) {isso sugere ao compilador C, que a expressão booleana passada é muito provável que seja verdadeira, então ela pode gerar código assembly, com menos chance de erro de predição de ramificação. No back-end JS, isso não faz nada.

if _unlikely_(bool expression) {semelhante a _provável_ (x), mas sugere que a expressão booleana é altamente improvável. No back-end JS, isso não faz nada.

#### Reflexão em tempo de compilação

Ter suporte JSON integrado é bom, mas V também permite que você crie serializadores eficientes para qualquer formato de dados. V tem tempo de compilação se e para construções:

```vlang
// TODO: não totalmente implementado

struct User {
    name string
    age  int
}

// Nota: T deve receber apenas um nome de estrutura
fn decode<T>(data string) T {
	mut result := T{}
	// loop `for` em tempo de compilação
	// T.fields fornece um array de um tipo de metadados de campo
	$for field in T.fields {
		$if field.typ is string {
			// $(string_expr) produz um identificador
			result.$(field.name) = get_string(data, field.name)
		} $else $if field.typ is int {
			result.$(field.name) = get_int(data, field.name)
		}
	}
	return result
}

// `decodificar <User>` gera:
fn decode_User(data string) User {
	mut result := User{}
	result.name = get_string(data, 'name')
	result.age = get_int(data, 'age')
	return result
}
```

#### Sobrecarga limitada do operador

```vlang
struct Vec {
	x int
	y int
}

fn (a Vec) str() string {
	return '{$a.x, $a.y}'
}

fn (a Vec) + (b Vec) Vec {
	return Vec{a.x + b.x, a.y + b.y}
}

fn (a Vec) - (b Vec) Vec {
	return Vec{a.x - b.x, a.y - b.y}
}

fn main() {
	a := Vec{2, 3}
	b := Vec{4, 5}
	mut c := Vec{1, 2}
	println(a + b) // "{6, 8}"
	println(a - b) // "{-2, -2}"
	c += a
	println(c) // "{3, 5}"
}
```

A sobrecarga do operador vai contra a filosofia de simplicidade e previsibilidade da V. Mas, uma vez que as aplicações científicas e gráficas estão entre os domínios do V, a sobrecarga do operador é um recurso importante para melhorar a legibilidade:

a.add (b).add(c.mul(d)) é muito menos legível do que a + b + c * d.

Para melhorar a segurança e facilidade de manutenção, a sobrecarga do operador é limitada:

- Só é possível sobrecarregar +, -, *, /,%, <,>, ==,! =, <=,> = Operadores.
- == e! = são gerados automaticamente pelo compilador, mas podem ser substituídos.
- Chamar outras funções dentro das funções do operador não é permitido.
- As funções do operador não podem modificar seus argumentos.
- Ao usar os operadores < e ==, o tipo de retorno deve ser bool.
- ! =,>, <= e > = são gerados automaticamente quando == e < são definidos.
- Ambos os argumentos devem ter o mesmo tipo (assim como todos os operadores em V).
- Os operadores de atribuição (* =, + =, / =, etc) são gerados automaticamente quando os operadores são definidos, embora devam retornar o mesmo tipo.

#### Montagem embutida

```vlang
a := 100
b := 20
mut c := 0
asm amd64 {
	mov eax, a
	add eax, b
	mov c, eax
	; =r (c) as c // output 
	; r (a) as a // input 
		r (b) as b
}
println('a: $a') // 100 
println('b: $b') // 20 
println('c: $c') // 120
```

Para obter mais exemplos, consulte [github.com/vlang/v/tree/master/vlib/v/tests/assembly/asm_test.amd64.v](https://github.com/vlang/v/tree/master/vlib/v/tests/assembly/asm_test.amd64.v)

#### Traduzindo C para V

TODO: a tradução de C para V estará disponível em V 0.3.

V pode traduzir seu código C em código V legível por humanos e gerar invólucros V em cima de bibliotecas C.

Vamos criar um programa simples test.c primeiro:

```c
#include "stdio.h"

int main() {
	for (int i = 0; i < 10; i++) {
		printf("hello world\n");
	}
        return 0;
}
```

Execute v translate test.c, e V irá gerar test.v:

```vlang
fn main() {
	for i := 0; i < 10; i++ {
		println('hello world')
	}
}
```

Para gerar um wrapper em cima de uma biblioteca C, use este comando:

```vlang
v wrapper c_code/libsodium/src/libsodium
```

Isso irá gerar um diretório libsodium com um módulo V.

Exemplo de um invólucro libsodium gerado por C2V:

[https://github.com/medvednikov/libsodium](https://github.com/medvednikov/libsodium)

Quando você deve traduzir o código C e quando deve simplesmente chamar o código C de V?

Se você tiver um código C bem escrito e testado, é claro que sempre pode simplesmente chamar esse código C a partir de V.

Traduzir para V oferece várias vantagens:

- Se você planeja desenvolver essa base de código, agora você tem tudo em uma linguagem, que é muito mais seguro e fácil de desenvolver do que C.
- A compilação cruzada se torna muito mais fácil. Você não precisa se preocupar com isso.
- Não há mais sinalizadores de construção e arquivos de inclusão também.

#### Recarregando código quente

```vlang
module main

import time
import os

[live]
fn print_message() {
	println('Hello! Modify this message while the program is running.')
}

fn main() {
	for {
		print_message()
		time.sleep(500 * time.millisecond)
	}
}
```

Construa este exemplo com v -live message.v.

As funções que você deseja recarregar devem ter o atributo [live] antes de sua definição.

No momento, não é possível modificar os tipos durante a execução do programa.

Mais exemplos, incluindo um aplicativo gráfico: [github.com/vlang/v/tree/master/examples/hot_code_reload](https://github.com/vlang/v/tree/master/examples/hot_reload).

#### Compilação cruzada

Para compilar o seu projeto, basta executar

```vlang
v -os windows .
```

ou

```vlang
v -os linux .
```

(A compilação cruzada para macOS não é possível temporariamente.)

Se você não tiver nenhuma dependência C, isso é tudo que você precisa fazer. Isso funciona mesmo ao compilar aplicativos GUI usando o módulo ui ou aplicativos gráficos usando gg.

Você precisará instalar o Clang, LLD linker e baixar um arquivo zip com bibliotecas e arquivos de inclusão para Windows e Linux. V irá fornecer-lhe um link.

#### Scripts de shell de plataforma cruzada em V

V pode ser usado como uma alternativa ao Bash para escrever scripts de implantação, construir scripts, etc.

A vantagem de usar V para isso é a simplicidade e previsibilidade da linguagem e o suporte para várias plataformas. Os "scripts V" são executados em sistemas do tipo Unix e também no Windows.

Use a extensão de arquivo .vsh. Isso tornará todas as funções no módulo os globais (para que você possa usar mkdir() em vez de os.mkdir(), por exemplo).

Um exemplo de deploy.vsh:

```shellscript
#!/usr/bin/env -S v run
// O texto acima associa o arquivo a V em sistemas semelhantes ao Unix,
// para que possa ser executado apenas especificando o caminho para o arquivo
// uma vez que é tornado executável usando `chmod + x`.

// Remova se compilar / sair, ignore quaisquer erros se não
rmdir_all('build') or { }

// Criar build /, nunca falha porque build / não existe
mkdir('build') ?

// Mova arquivos *.v para construir /
result := exec('mv *.v build/') ?
if result.exit_code != 0 {
	println(result.output)
}
// Similar to:
// files := ls('.') ?
// mut count := 0
// if files.len > 0 {
//     for file in files {
//         if file.ends_with('.v') {
//              mv(file, 'build/') or {
//                  println('err: $err')
//                  return
//              }
//         }
//         count++
//     }
// }
// if count == 0 {
//     println('No files')
// }
```

Agora você pode compilar isso como um programa V normal e obter um executável que pode ser implementado e executado em qualquer lugar: v deploy.vsh && ./deploy

Ou apenas execute-o mais como um script Bash tradicional: v execute deploy.vsh

Em plataformas do tipo Unix, o arquivo pode ser executado diretamente após torná-lo executável usando chmod + x: ./deploy.vsh

#### Atributos

V possui vários atributos que modificam o comportamento de funções e estruturas.

Um atributo é uma instrução do compilador especificada dentro de [] logo antes de uma declaração de função/struct/enum e se aplica apenas à declaração a seguir.

```vlang
// Chamar esta função resultará em um aviso de suspensão de uso
[deprecated]
fn old_function() {
}

// Também pode exibir uma mensagem de suspensão de uso personalizada
[deprecated: 'use new_function() instead']
fn legacy_function() {}

// As chamadas desta função serão embutidas.
[inline]
fn inlined_function() {
}

// A estrutura a seguir deve ser alocada no heap. Portanto, ele só pode ser usado como um
// referência (`& Window`) ou dentro de outra referência (` &OuterStruct {Window {...}} `).
[heap]
struct Window {
}

// V não irá gerar esta função e todas as suas chamadas se o sinalizador fornecido for falso.
// Para usar um sinalizador, use `v -d sinalizador`
[if debug]
fn foo() {
}

fn bar() {
	foo() // não será chamado se `-d debug` não for passado
}

// As chamadas para a função a seguir devem ser em blocos não unsafe{}.
// verificado, a menos que você também o envolva em blocos `unsafe{}`.
// checked, unless you also wrap it in `unsafe {}` blocks.
// Isso é útil, quando você quer ter uma função `[unsafe]` que
// tem verificações before/after de uma determinada operação unsafe, que ainda
// beneficie-se dos recursos de segurança do V.
[unsafe]
fn risky_business() {
	// código que será verificado, talvez verificando as pré-condições
	unsafe {
		// código que * não será * verificado, como aritmética de ponteiro,
		// acessando campos de união, chamando outros fns `[unsafe], etc ...
		// Normalmente, é uma boa ideia tentar minimizar o código empacotado
		// unsafe{} tanto quanto possível.
		// Veja também [código memory-unsafe] (#memory-unsafe-code)
	}
	// código que será verificado, talvez verificando as condições de postagem and/or
	// mantendo invariantes
}

// O mecanismo autofree de V não cuidará do gerenciamento de memória nesta função.
// Você terá a responsabilidade de liberar memória manualmente nele.
[manualfree]
fn custom_allocations() {
}

// Somente para interoperabilidade C, diz a V que a estrutura a seguir é definida com `estrutura typedef` em C
[typedef]
struct C.Foo {
}

// Usado no código da API Win32 quando você precisa passar a função de retorno de chamada
[windows_stdcall]
fn C.DefWindowProc(hwnd int, msg int, lparam int, wparam int)

// Apenas Windows:
// Se uma biblioteca de gráficos padrão é importada (ex. Gg, ui), a janela gráfica leva
// prioridade e nenhuma janela de console é criada, efetivamente desabilitando as instruções println().
// Use para criar explicitamente a janela do console. Válido antes de main() apenas.
[console]
fn main() {
}
```

#### Goto

V permite pular incondicionalmente para um rótulo com goto. O nome do rótulo deve estar contido na mesma função da instrução goto. Um programa pode ir para um rótulo fora ou mais profundo do que o escopo atual. goto permite pular a inicialização da variável ou voltar para o código que acessa a memória que já foi liberada, portanto, requer unsafe.

```vlang
if x {
	// ...
	if y {
		unsafe {
			goto my_label
		}
	}
	// ...
}
my_label:
```

`goto` deve ser evitado, principalmente quando `for` pode ser usado em seu lugar. Instruções `break/continue` pode ser usado para interromper um loop aninhado e não corre o risco de violar a segurança da memória.

### Apêndices

#### Apêndice I: Palavras-chave

V tem 41 palavras-chave reservadas (3 são literais):

```vlang
as
asm
assert
atomic
break
const
continue
defer
else
embed
enum
false
fn
for
go
goto
if
import
in
interface
is
lock
match
module
mut
none
or
pub
return
rlock
select
shared
sizeof
static
struct
true
type
typeof
union
unsafe
__offsetof
```

Veja também [Types](https://github.com/vlang/v/blob/master/doc/docs.md#types).

#### Apêndice II: Operadores

Isso lista os operadores apenas para [tipos primitivos](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types).

```vlang
+    sum                    integers, floats, strings
-    difference             integers, floats
*    product                integers, floats
/    quotient               integers, floats
%    remainder              integers

~    bitwise NOT            integers
&    bitwise AND            integers
|    bitwise OR             integers
^    bitwise XOR            integers

!    logical NOT            bools
&&   logical AND            bools
||   logical OR             bools
!=   logical XOR            bools

<<   left shift             integer << unsigned integer
>>   right shift            integer >> unsigned integer


Precedence    Operator
    5             *  /  %  <<  >>  &
    4             +  -  |  ^
    3             ==  !=  <  <=  >  >=
    2             &&
    1             ||


Assignment Operators
+=   -=   *=   /=   %=
&=   |=   ^=
>>=  <<=
```

