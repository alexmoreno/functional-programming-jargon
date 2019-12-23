# Jargões da Programação Funcional

O objetivo desse documento é definir os jargões da programação funcional em inglês simples com exemplos.

*Isso aqui é um WIP; Sinta-se a vontade para enviar um PR ;)*

Esse documento usa termos definidos na [Fantasy Land spec](https://github.com/fantasyland/fantasy-land) quando aplicável.

<!-- RM(noparent,notop) -->

* [Aridade](#aridade)
* [Funções de Ordem Superior (HOF)](#fun%C3%A7%C3%B5es-de-ordem-superior-hof)
* [Aplicações Parciais](#aplica%C3%A7%C3%B5es-parciais)
* [Currying](#currying)
* [Composição de Função](#composi%C3%A7%C3%A3o-de-fun%C3%A7%C3%A3o)
* [Pureza](#pureza)
* [Efeitos Colaterais](#pureza)
* [Idempotente](#idempotente)
* [Estilo Livre de Apontamento](#estilo-livre-de-apontamento)
* [Predicado](#predicado)
* [Contratos](#contratos)
* [Funções Guardadas](#fun%C3%A7%C3%B5es-guardadas)
* [Categorias](#categorias)
* [Valor](#valor)
* [Constante](#constante)
* [Functor](#functor)
* [Functor Apontado](#functor-apontadoo)
* [Levantamento](#levantamento)
* [Transparência Referencial](#transpar%C3%AAncia-referencial)
* [Raciocínio Equacional](#racioc%C3%ADnio-equacional)
* [Avaliação Preguiçosa](#avalia%C3%A7%C3%A3o-pregui%C3%A7osa)
* [Monóide](#mon%C3%B3ide)
* [Monade](#monade)
* [Comonade](#comonade)
* [Functor Aplicativo](#functor-aplicativo)
* [Morfismo](#morfismo)
* [Isomorfismo](#isomorfismo)
* [Setóide](#set%C3%B3ide)
* [Semigrupo](#semigrupo)
* [Dobrável](#dobr%C3%A1vel)
* [Transversável](#travers%C3%A1vel)
* [Assinatura de Tipos](#assinaturas-de-tipo)
* [Tipo de União](#tipo-de-uni%C3%A3o)
* [Tipo de Produto](#tipo-de-produto)
* [Opção](#op%C3%A7%C3%A3o)


<!-- /RM -->

## Aridade

O número de argumentos que uma função recebe. Vem das palavras tipo unário, binário, ternário, etc. Essa palavra possui a característica de ser formada por dois sufixos, "-ário" e "-idade". Soma, por exemplo, recebe dois argumentos, portanto é definido como uma função binária ou uma função com aridade dois. Funções assim podem ser chamadas de "Diádica" pelos amantes das raízes gregas ao invés das latinas. Da mesma forma, uma função que recebe um número variável de argumentos é chamada de "variádica", enquanto uma função binária deve possuir dois e apenas dois argumentos, não obstante o currying e a aplicação parcial (veja abaixo).



```js
const soma = (a, b) => a + b;

const aridade = soma.length;
console.log(aridade); // 2

// A aridade de soma é 2
```

## Funções de Ordem Superior (HOF)

Uma função que recebe outra função como argumento e/ou retorna uma função.


```js
const filter = (pred, xs) => {
    const result = [];
    for (let idx = 0; idx < xs.length; idx++) {
        if (pred(xs[idx])) {
            result.push(xs[idx]);
        }
    }
    return result;
};
```

```js
const is = (type) => (x) => Object(x) instanceof type;
```

```js
filter(is(Number), [0, '1', 2, null]); // [0, 2]
```

## Aplicações Parciais

O processo de receber uma função com menor aridade comparada à função original, corrigindo o número de argumentos, é conhecido como aplicação parcial.

```js
let sum = (a, b) => a + b;

// aplicando parcialmente `a` para `40`
let partial = sum.bind(null, 40);

// invocando com b `b`
partial(2); // 42
```

## Currying

O processo de converter uma função que recebe múltiplos argumentos em uma função que os recebe um de cada vez.

Cada vez que a função é chamada, aceita somente um argumento e retorna uma função que recebe um argumento até que todos sejam passados.



```js
const sum = (a, b) => a + b;

const curriedSum = (a) => (b) => a + b;

curriedSum(40)(2) // 42.

const add2 = curriedSum(2); // (b) => 2 + b

add2(10) // 12

```

## Composição de Função

O ato de colocar duas funções juntas para forma uma terceira função onde a saída dessa função é a entrada de outra.


```js
const compose = (f, g) => (a) => f(g(a)) // Definition
const floorAndToString = compose((val) => val.toString(), Math.floor) // Uso
floorAndToString(121.212121) // "121"
```

## Pureza

Uma função é pura quando o valor de retorno é determinado apenas pelas suas entradas, e não produz efeitos colaterais.

```js
let greet = (name) => "Olá, " + name ;

greet("Brianne") // "Olá, Brianne"

```

Em oposição à:

```js

let greeting;

let greet = () => greeting = "Olá, " + window.name;

greet(); // "Olá, Brianne"

```

## Efeitos Colaterais

Dizemos que uma função ou expressão tem efeitos colaterais se além de retornar um valor, ela interaja com (lê ou escreve) estados mutáveis eternos.

```js
var differentEveryTime = new Date();
```

```js
console.log("IO é um efeito colateral!");
```

## Idempotente

Uma função é idempotente se ao reaplicá-la ao seu próprio resultado não produz um resultado diferente.

```js
f(f(x)) = f(x)
```

```js
Math.abs(Math.abs(10))
```

```js
sort(sort(sort([2,1])))
```

## Estilo Livre de Apontamento

Esse estilo se define ao escrever funções que não identificam explicitamente os argumentos usados. Geralmente requer [currying](#currying) ou outra [função de ordem superior](#fun%C3%A7%C3%B5es-de-ordem-superior-hoff). Mais conhecido como programação implícita.

```js
// Dado
let map = (fn) => (list) => list.map(fn);
let add = (a) => (b) => a + b;

// Então

// Não é livre de apontamento - `numbers` é um argumento explícito
let incrementAll = (numbers) => map(add(1))(numbers);

// Livre de apontamento - A lista é um argumento implícito
let incrementAll2 = map(add(1));
```

`incrementAll` identifica e usa o parâmetro `numbers`, logo, não é livre de apontamento.  `incrementAll2` é escrita combinando funções e valores, não fazendo menção aos seus argumentos. Ela __é__ livre de apontamento.

Definição de funções livre de apontamento são como funções normais, mas sem `function` ou `=>`.


## Predicado
Um predicado é uma função que retorna verdadeiro ou falso de acordo com um valor dado. Um uso comum de predicados é o callback de filtros de array.

```js
const predicate = (a) => a > 2;

[1, 2, 3, 4].filter(predicate); // [3, 4]
```

## Contratos

TODO

## Funções Guardadas

TODO

## Categorias

Objetos com funções associadas que aderem a certas regras. Exemplo:  [Monóide](#monoide)

## Valor

Qualquer coisa que possa ser atribuída a uma variável.

```js
5
Object.freeze({name: 'John', age: 30}) // A função `freeze` força a imutabilidade.
(a) => a
[1]
undefined
```

## Constante

Uma variável que não pode ser reatribuída após ser definida pela primeira vez.

```js
const five = 5
const john = {name: 'John', age: 30}
```

Constantes são  [transparentes referencialmente](#transpar%C3%AAncia-referencial). Ou seja, podem ser substituídas pelos valores que representam sem afetar o resultado.

Com as duas constantes acima, a expressão a seguir vai retornar `true` sempre.

```js
john.age + five === ({name: 'John', age: 30}).age + (5)
```

## Functor

Um objeto com uma função `map` que adere a certas regras. `Map` executa uma função sobre os valores de um objeto e retorna um objeto novo.


Um functor comum no js é `Array`

```js
[2, 3, 4].map((n) => n * 2); // [4, 6, 8]
```

Se `func` é um objeto implementando uma função de `map` e `f` e `g` são funções arbitrárias, então diz-se que `func` é o functor se a função map adere às seguintes regras.

```js
// identidade
func.map((x) => x) === func
```

e

```js
// composição
func.map((x) => f(g(x))) === func.map(g).map(f)
```

Podemos agora ver que  `Array` é um functor pois adere às regras do functor.

```js
[1, 2, 3].map((x) => x); // = [1, 2, 3]
```

e

```js
let f = (x) => x + 1;
let g = (x) => x * 2;

[1, 2, 3].map((x) => f(g(x))); // = [3, 5, 7]
[1, 2, 3].map(g).map(f);     // = [3, 5, 7]
```

## Functor Apontado
Um functor com uma função `of` que coloca _qualquer_ valor individual naquele functor.

Implementação em Array:

```js
Array.prototype.of = (v) => [v];

[].of(1) // [1]
```

## Levantamento

Levantamento é tipo um  `map`, exceto pelo fato que pode ser aplicado a múltiplos functores.

Map é o mesmo que um levantamento sobre uma função de um argumento.


```js
lift((n) => n * 2)([2, 3, 4]); // [4, 6, 8]
```

Diferente de map, levantamento pode ser usado para combinar valores de múltiplas arrays.

```js
lift((a, b) => a * b)([1, 2], [3]); // [3, 6]
```

## Transparência Referencial

Uma expressão que pode ser substituída pelo seu valor sem alterar o comportamento do programa, é dita como referencialmente transparente.

Vamos supor que temos a função `greet`:

```js
let greet = () => "Hello World!";
```

Qualquer invocação de `greet` pode ser substituida com `Hello World!`, logo `greet` é referencialmente transparente.


## Raciocínio Equacional

Quando uma aplicação é composta de expressão e desprovida de efeitos colaterais, as verdades sobre o sistema podem ser derivadas de suas partes.

## Avaliação Preguiçosa

Avaliação Preguiçosa é um mecanismo de avaliação chamado conforme necessidade que atrasa a avaliação de uma expressão até que seu valor seja requerido. Em linguagens funcionais, isso permite o uso de estruturas tipo listas infinitas, o que não seria possível normalmente em uma linguagem imperativa onde a sequência dos comandos é significante.

```js
let rand = function*() {
    while (1 < 2) {
        yield Math.random();
    }
}
```

```js
let randIter = rand();
randIter.next(); // Cada execução retorna um valor conforme necessário.
```

## Monóide

Monóide é um tipo de dado junto de uma função de dois parâmetros que "combina" dois valores do tipo, onde um valor de identidade que não afeta o resultado da função também existe.

Um monóide bem simples é números e adição.


```js
1 + 1; // 2
```

O tipo do dado é número e a função é `+`, a adição dos dois números.

```js
1 + 0; // 1
```

O valor de identidade é `0`, adicionar `0` a qualquer número não vai alterá-lo.

Para algo ser um monóide, também é necessário que o agrupamento das operações não afete o resultado.

```js
1 + (2 + 3) === (1 + 2) + 3; // true
```

Concatenação de arrays também pode ser chamado de monóide.

```js
[1, 2].concat([3, 4]); // [1, 2, 3, 4]
```

O valor identidade é um array vazio `[]`

```js
[1, 2].concat([]); // [1, 2]
```

Se identidade e funções de composições forem dados, as funções foram monóides por elas mesmas.

```js
var identity = (a) => a;
var compose = (f, g) => (x) => f(g(x));

compose(foo, identity) ≍ compose(identity, foo) ≍ foo
```

## Monade

Uma monade é um objeto com funções [`of`](#functor-apontado) e `chain`. `chain` é tipo [`map`](#functor), exceto pelo fato que "desninha" o objeto aninhado resultante.

```js
['cat,dog', 'fish,bird'].chain((a) => a.split(',')) // ['cat', 'dog', 'fish', 'bird']

// Em contraste ao map
['cat,dog', 'fish,bird'].map((a) => a.split(',')) // [['cat', 'dog'], ['fish', 'bird']]
```

`of` também é conhecido como  `return` em outras linguagens funcionais.
`chain` também é conhecido como  `flatmap` e `bind` em outras linguagens.

## Comonade

Um objeto que possui as funções  `extract` e `extend`.

```js
const CoIdentity = (v) => ({
    val: v,
    extract() { return this.val },
    extend(f) { return CoIdentity(f(this)) }
})
```

Extract tira um valor do functor.

```js
CoIdentity(1).extract() // 1
```

Extend executa a função na comonade. A função deve retornar o mesmo tipo de comonade.

```js
CoIdentity(1).extend((co) => co.extract() + 1) // CoIdentity(2)
```

## Functor Aplicativo

Um functor aplicativo é um objeto com uma função  `ap`. `ap` aplica uma função no objeto a outro valor em outro objeto do mesmo tipo.

```js
[(a) => a + 1].ap([1]) // [2]
```

## Morfismo

Uma função de transformação.

## Isomorfimos

Um par de transformações entre 2 tipos de objetos que é estrutura por natureza e não há perda de dados.

Exemplo, coordenadas 2D devem ser armazenadas em uma array `[2,3]` ou objeto `{x: 2, y: 3}`.

```js
// Prover funções para converter em ambas as direções as faz isomórficas.
const pairToCoords = (pair) => ({x: pair[0], y: pair[1]})

const coordsToPair = (coords) => [coords.x, coords.y]

coordsToPair(pairToCoords([1, 2])) // [1, 2]

pairToCoords(coordsToPair({x: 1, y: 2})) // {x: 1, y: 2}
```



## Setóide

Um objeto que tem uma função  `equals` que pode ser usada para comparar outros objetos do mesmo tipo.

Vamos transformar Array em setóide:

```js
Array.prototype.equals = (arr) => {
    var len = this.length
    if (len != arr.length) {
        return false
    }
    for (var i = 0; i < len; i++) {
        if (this[i] !=== arr[i]) {
            return false
        }
    }
    return true
}

[1, 2].equals([1, 2]) // true
[1, 2].equals([0]) // false
```

## Semigrupo

Um objeto que tem uma função `concat` que combina com outro objeto deo mesmo tipo.

```js
[1].concat([2]) // [1, 2]
```

## Dobrável

Um objeto que possui uma função `reduce` que pode transformar o objeto em outro tipo.

```js
let sum = (list) => list.reduce((acc, val) => acc + val, 0);
sum([1, 2, 3]) // 6
```

## Traversável

TODO

## Assinaturas de Tipo

Frequentemente, as funçẽos vão incluir comentário quy indicam os tipos dos seus argumentos e retornos.

Há uma pequena variância na comunidade, mas geralmente seguem os seguintes padrões:

```js
// nomeDaFunção :: tipoDoPrimeiroArgumento -> tipoDoSegundoArgumento -> tipoDoRetorno

// add :: Number -> Number -> Number
let add = (x) => (y) => x + y

// increment :: Number -> Number
let increment = (x) => x + 1
```

Se uma função aceita outra função como parâmetro, ela é envolva em parêntese.

```js
// call :: (a -> b) -> a -> b
let call = (f) => (x) => f(x)
```

As letras  `a`, `b`, `c`, `d` são usadas para mostrar que o argumento pode ser de qualquer tipo. Nesse `map`, recebe uma função que transforma um valor do tipo `a` em outro do tipo `b`, ou um array de valores do tipo `a` e retorna um array de valores do tipo `b`.

```js
// map :: (a -> b) -> [a] -> [b]
let map = (f) => (list) => list.map(f)
```

## Tipo de união
Um tipo de união é a combinação de dois tipos em um terceiro.

JS não tem tipos estáticos, mas vamos supor que queremos inventar um tipo `NumOrString`, que é a soma de um `String` e um `Number`.

O operador `+` no JS funciona em strings e números, então podemos usar esse novo tipo para descrever suas entradas e saídas.

```js
// add :: (NumOrString, NumOrString) -> NumOrString
const add = (a, b) => a + b;

add(1, 2); // Retorna número 3
add('Foo', 2); // Retorna string "Foo2"
add('Foo', 'Bar'); // Retorna string "FooBar"
```

Tipos de união também são conhecidos como tipos algebráicos, uniões marcadas, ou tipos de soma.

Há [algumas](https://github.com/paldepind/union-type) [bibliotecas](https://github.com/puffnfresh/daggy) em JS que ajudam a definir e usar tipos de união. 

## Tipo de produto

Um tipo de **produto** combina tipos de uma forma que você provavelmente já está familiarizado.

```js
// point :: (Number, Number) -> {x: Number, y: Number}
const point = (x, y) => ({x: x, y: y});
```
Isso é chamado de produto porque o total de valores possíveis da estrutura de dados ´e o produto dos valores diferentes.

Veja também [Teoria dos conjuntos](https://en.wikipedia.org/wiki/Set_theory).

## Opção
Opção é um [tipo de união](#tipo-de-uni%C3%A3o) com duas classes que geralmente são chamadas de  `Some` (_alguma_) e  `None` (_nenhuma_). 

Opção é útil para compor funções que podem não retornar um valor.

```js
// Definição inocente

const Some = (v) => ({
    val: v,
    map(f) {
        return Some(f(this.val));
    },
    chain(f) {
        return f(this.val);
    }
});

const None = () => ({
    map(f){
        return this;
    },
    chain(f){
        return this;
    }
});

// maybeProp :: (String, {a}) -> Option a
const maybeProp = (key, obj) => typeof obj[key] === 'undefined' ? None() : Some(obj[key]);
```
Use `chain` para colocar em sequência funções que retornam  `Option`s
```js

// getItem :: Cart -> Option CartItem
const getItem = (cart) => maybeProp('item', cart);

// getPrice :: Item -> Option Number
const getPrice = (item) => maybeProp('price', item);

// getNestedPrice :: cart -> Option a
const getNestedPrice = (cart) => getItem(obj).chain(getPrice);

getNestedPrice({}); // None()
getNestedPrice({item: {foo: 1}}); // None()
getNestedPrice({item: {price: 9.99}}); // Some(9.99)
```

`Option` também é conhecido como  `Maybe` (_talvez_). `Some` às vezes é chamada de  `Just` (_apenas_). `None` às vezes é chamada de `Nothing` (_nada_).

---

__P.S:__ Sem as [contribuções maravilhosas](https://github.com/hemanth/functional-programming-jargon/graphs/contributors) esse repositório não teria sentido algum!
