# Jargões da programação funcional <!-- omit in toc -->

A programação funcional (ou **FP** de _Functional programming_) oferece muitas vantagens e, como resultado disso, sua popularidade tem aumentado. No entanto, cada paradigma de programação vem com seus próprios jargões e FP não é uma exceção. Ao fornecer um glossário, esperamos tornar o aprendizado de FP mais fácil.

Os exemplos são apresentados em Javascript (ES2015).

_**Work in progress**; Sinta-se a vontade para enviar um PR ;)_

Esse documento usa termos definidos na [Fantasy Land spec](https://github.com/fantasyland/fantasy-land) quando aplicável.

**Sumário**

- [Aridade (_Arity_)](#aridade-arity)
- [Funções de ordem maior (_Higher-Order Functions_) (HOF)](#fun%C3%A7%C3%B5es-de-ordem-maior-higher-order-functions-hof)
- [_Closure_](#closure)
- [Aplicação parcial (_Partial Application_)](#aplica%C3%A7%C3%A3o-parcial-partial-application)
- [_Currying_](#currying)
- [_Auto Currying_](#auto-currying)
- [Composição de funções (_Function Composition_)](#composi%C3%A7%C3%A3o-de-fun%C3%A7%C3%B5es-function-composition)
- [Continuação (_Continuation_)](#continua%C3%A7%C3%A3o-continuation)
- [Pureza (_Purity_)](#pureza-purity)
- [Efeitos Colaterais (_Side effects_)](#efeitos-colaterais-side-effects)
- [Idempontente (_Idempotent_)](#idempontente-idempotent)
- [Estilo livre de apontamento (_Point-Free Style_)](#estilo-livre-de-apontamento-point-free-style)
- [Predicado (_Predicate_)](#predicado-predicate)
- [Contratos (_Contracts_)](#contratos-contracts)
- [Categoria (_Category_)](#categoria-category)
- [Valor (_Value_)](#valor-value)
- [Constante (_Constant_)](#constante-constant)
- [_Functor_](#functor)
- [_Functor_ Apontado (_Pointed Functor_)](#functor-apontado-pointed-functor)
- [Levantamento (_Lift_)](#levantamento-lift)
- [Transparência referencial (_Referential Transparency_)](#transpar%C3%AAncia-referencial-referential-transparency)
- [Racioncício equacional (_Equational Reasoning_)](#racionc%C3%ADcio-equacional-equational-reasoning)
- [_Lambda_](#lambda)
- [Cálculo lambda (_Lambda Calculus_)](#c%C3%A1lculo-lambda-lambda-calculus)
- [Avaliação preguiçosa (_Lazy evaluation_)](#avalia%C3%A7%C3%A3o-pregui%C3%A7osa-lazy-evaluation)
- [Monóide (_Monoid_)](#mon%C3%B3ide-monoid)
- [Monade (_Monad_)](#monade-monad)
- [Comonade (_Comonad_)](#comonade-comonad)
- [_Functor_ Aplicativo (_Applicative Functor_)](#functor-aplicativo-applicative-functor)
- [Morfismo (_Morphism_)](#morfismo-morphism)
  - [Endomorfismo (_Endomorphism_)](#endomorfismo-endomorphism)
  - [Isomorfismo (_Isomorphism_)](#isomorfismo-isomorphism)
  - [Homomorfismo (_Homomorphism_)](#homomorfismo-homomorphism)
  - [Catamorfismo (_Catamorphism_)](#catamorfismo-catamorphism)
  - [Anamorfismo (_Anamorphism_)](#anamorfismo-anamorphism)
  - [Hylomorfismo (_Hylomorphism_)](#hylomorfismo-hylomorphism)
  - [Paramorfismo (_Paramorphism_)](#paramorfismo-paramorphism)
  - [Apomorfismo (_Apomorphism_)](#apomorfismo-apomorphism)
- [Setóide (_Setoid_)](#set%C3%B3ide-setoid)
- [Semigrupo (_Semigroup_)](#semigrupo-semigroup)
- [Dobrável (_Foldable_)](#dobr%C3%A1vel-foldable)
- [_Lens_](#lens)
- [Tipos de assinatura (_Type Signatures_)](#tipos-de-assinatura-type-signatures)
- [Tipos algébricos (_Algebraic data type_)](#tipos-alg%C3%A9bricos-algebraic-data-type)
  - [_Sum type_](#sum-type)
  - [_Product type_](#product-type)
- [Opção (_Option_)](#op%C3%A7%C3%A3o-option)
- [Função (_Function_)](#fun%C3%A7%C3%A3o-function)
- [Funções parciais (_Partial function_)](#fun%C3%A7%C3%B5es-parciais-partial-function)
  - [Lidando com funções parciais](#lidando-com-fun%C3%A7%C3%B5es-parciais)
- [Bibliotecas para programação funcional em Javascript](#bibliotecas-para-programa%C3%A7%C3%A3o-funcional-em-javascript)

## Aridade (_Arity_)

É o número de argumentos que uma função recebe. Vem de palavras como unário, binário, ternário, etc. Essa palavra tem a distinção de ser composta de dois sufixos, "-ário" e "-idade". A função _sum_, por exemplo, recebe dois argumentos e, portanto, é definida como uma função binária ou uma função com aridade dois. Funções assim podem ser chamadas de "Diádica" por pessoas que preferem as raízes gregas ao invés das latinas. Da mesma forma, uma função que recebe um número variável de argumentos é chamada de "variádica", enquanto uma função binária deve receber dois e apenas dois argumentos, não obstante o currying e aplicação parcial (veja abaixo).

```js
const sum = (a, b) => a + b;

const arity = sum.length;
console.log(arity); // 2

// A aridade de sum é 2
```

## Funções de ordem maior (_Higher-Order Functions_) (HOF)

Uma função que recebe uma função como argumento e/ou retorna uma função.

```js
const filter = (predicate, xs) => xs.filter(predicate);
```

```js
const is = type => x => Object(x) instanceof type;
```

```js
filter(is(Number), [0, '1', 2, null]); // [0, 2]
```

## _Closure_

Um _closure_ é um escopo que mantém variáveis disponíveis para uma função quando ela é criada. Formalmente, _closure_ é uma técnica para implementar a vinculação nomeada com o escopo léxico. É importante para
aplicação parcial funcionar.

```js
const addTo = x => {
  return y => {
    return x + y;
  };
};
```

Podemos chamar `addTo` passando um número como parâmetro e obter uma função com um `x` "enclausurado".

```js
var addToFive = addTo(5);
```

Nesse caso, o `x` é mantido no _closure_ de `addToFive` com o valor `5`. Podemos então chamar `addToFive` com o parâmetro `y` e obter o número desejado.

```
addToFive(3) // => 8
```

Isso funciona porque variáveis que estão em escopos pai não são coletadas como lixo desde que a função em si seja retida.

_Closures_ são comumente usados em manipuladores de eventos para que eles ainda tenham acesso a variáveis definidas em seus pais quando são eventualmente chamados.

**Leitura adicional (em inglês)**

- [Lambda Vs Closure](http://stackoverflow.com/questions/220658/what-is-the-difference-between-a-closure-and-a-lambda)
- [How do JavaScript Closures Work?](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work)

## Aplicação parcial (_Partial Application_)

É o processo de recebe uma função com menor aridade comparada à função original.

```js
// Helper para criar funções com aplicação parcial
// Recebe uma função e alguns argumentos
const partial = (f, ...args) =>
  // Retorna uma função que recebe o restante dos argumentos
  (...moreArgs) =>
    // e chama a função original com todos os argumentos passados
    f(...args, ...moreArgs);

const add3 = (a, b, c) => a + b + c;

// A aplicação parcial de `2` e `3` para `add3` fornece uma função de único argumento.
const fivePlus = partial(add3, 2, 3); // (c) => 2 + 3 + c

fivePlus(4); // 9
```

Nós também podemos usar `Function.prototype.bind` para aplicar parcialmente uma função em Javascript:

```js
const add1More = add3.bind(null, 2, 3); // (c) => 2 + 3 + c
```

As aplicações parciais nos ajuda a criar funções com lógica mais simples ao associarmos os dados que já possuimos.

## _Currying_

É o processo de converter uma função que recebe múltiplos argumentos em uma função que os recebe um de cada vez.

Cada vez que a função é chamada, aceita somente um argumento e retorna uma função que recebe um outro argumento até que todos sejam passados.

```js
const sum = (a, b) => a + b;

const curriedSum = a => b => a + b;

curriedSum(40)(2); // 42.

const add2 = curriedSum(2); // (b) => 2 + b

add2(10); // 12
```

## _Auto Currying_

Transforma uma função que recebe vários argumentos em uma outra função que, se receber um número menor de argumentos, retorna uma função que recebe o restante. Quando a função recebe o número correto de argumentos ela é avaliada, como pode ser visto abaixo.

As bibliotecas lodash e Ramda possuem a função `curry` que funciona dessa forma.

```js
const add = (x, y) => x + y;

const curriedAdd = _.curry(add);
curriedAdd(1, 2); // 3
curriedAdd(1); // (y) => 1 + y
curriedAdd(1)(2); // 3
```

**Leitura adicional (em inglês)**

- [Favoring Curry](http://fr.umio.us/favoring-curry/)
- [Hey Underscore, You're Doing It Wrong!](https://www.youtube.com/watch?v=m3svKOdZijA)

## Composição de funções (_Function Composition_)

O ato de colocar duas funções juntas para forma uma terceira função onde a saída dessa função é a entrada de outra.

```js
const compose = (f, g) => a => f(g(a)); // Definição
const floorAndToString = compose(
  val => val.toString(),
  Math.floor
); // Uso
floorAndToString(121.212121); // '121'
```

## Continuação (_Continuation_)

Em qualquer parte de um programa, a parte do código que ainda está para ser executada é conhecida como continuação.

```js
const printAsString = num => console.log(`Given ${num}`);

const addOneAndContinue = (num, cc) => {
  const result = num + 1;
  cc(result);
};

addOneAndContinue(2, printAsString); // 'Given 3'
```

As continuações são mais frequentemente em programação assíncrona quando o programa precisa esperar para receber dados antes que possa continuar. Quando recebida, a resposta é passada para o resto do programa, que é a continuação.

```js
const continueProgramWith = data => {
  // Continua o programa com os dados
};

readFileAsync('path/to/file', (err, response) => {
  if (err) {
    // tratamento de erros
    return;
  }
  continueProgramWith(response);
});
```

## Pureza (_Purity_)

Uma função é pura quando o valor de retorno é determinado apenas pelas suas entradas e não produz efeitos colaterais.

```js
const greet = name => `Olá, ${name}`;

greet('Brianne'); // 'Olá, Brianne'
```

Em oposição, temos que as funções abaixo utilizam ou modificam dados fora do escopo da função:

```js
window.name = 'Brianne';

const greet = () => `Olá, ${window.name}`;

greet(); // "Olá, Brianne"
```

```js
let greeting;

const greet = name => {
  greeting = `Olá, ${name}`;
};

greet('Brianne');
greeting; // "Olá, Brianne"
```

## Efeitos Colaterais (_Side effects_)

Uma função ou expressão tem efeito colateral se, além de retornar um valor, intereage com algum estado externo mutável.

```js
const differentEveryTime = new Date();
```

```js
console.log('IO é um efeito colateral!');
```

## Idempontente (_Idempotent_)

Uma função é idempotente se ao reaplicá-la ao seu próprio resultado não produz um resultado diferente.

```
f(f(x)) ≍ f(x)
```

```js
Math.abs(Math.abs(10));
```

```js
sort(sort(sort([2, 1])));
```

## Estilo livre de apontamento (_Point-Free Style_)

Esse estilo se define ao escrever funções que não identificam explicitamente os argumentos usados. Esse estilo geralmente requerer currying ou outra função de ordem maior. Mais conhecido como programação implícita.

```js
// Dado
const map = fn => list => list.map(fn);
const add = a => b => a + b;

// Então

// Não é livre de apontamento - `numbers` é um argumento explícito
const incrementAll = numbers => map(add(1))(numbers);

// Livre de apontamento - A lista é um argumento implícito
const incrementAll2 = map(add(1));
```

`incrementAll` identifica e usa o parâmetro numbers, logo, não é livre de apontamento. `incrementAll2` é escrita combinando funções e valores, não fazendo menção aos seus argumentos. Ela é livre de apontamento.

Definição de funções livre de apontamento são como funções normais, mas sem `function` ou `=>`.

## Predicado (_Predicate_)

Um predicado é uma função que retorna verdadeiro ou falso de acordo com um valor dado. Um uso comum de predicados é o callback de filtros de array.

```js
const predicate = a => a > 2;

[1, 2, 3, 4].filter(predicate); // [3, 4]
```

## Contratos (_Contracts_)

Um contrato especifica as obrigações e garantias do comportamento que função/expressão deve ter em tempo de execução, agindo como um conjunto de regras que são esperadas para a entrada e a saída das funções/expressões. Geralmente os erros são relatados quando um contrato é violado.

```js
// Definindo nosso contrato: int -> boolean
const contract = input => {
  if (typeof input === 'number') return true;
  throw new Error('Contrato violado: era esperado int -> boolean');
};

const addOne = num => contract(num) && num + 1;

addOne(2); // 3
addOne('some string'); // Contrato violado: era esperado int -> boolean
```

## Categoria (_Category_)

Uma categoria, na **teoria das categorias**, é uma coleção de objetos e morfismos entre eles. Na programação, os tipos atuam como objetos e funções como morfismos.

Para ser uma categoria válida, 3 regras precisam ser atendidas:

1. Deve haver um morfismo de identidade que mapeie um objeto para si mesmo. Onde `a` é um objeto em alguma categoria, deve haver uma função de `a -> a`.
2. Morfismos poderão ser compostos. Onde `a`,`b` e `c` são objetos em alguma categoria, e`f` é um morfismo de `a -> b` e`g` é um morfismo de `b -> c`; `g (f (x))` deve ser equivalente a `(g • f) (x)`.
3. Composições deverão ser associativas. `f • (g • h)` é o mesmo que `(f • g) • h`

Como essas regras gerem a composição em um nível muito abstrato, a teoria das categorias é uma ótima ferramenta para descobrir novas maneiras de compor as coisas.

**Leitura adicional (em inglês)**

- [Category Theory for Programmers](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/)

## Valor (_Value_)

Qualquer coisa que possa ser atribuída a uma variável.

```js
5;
Object.freeze({ name: 'John', age: 30 }); // A função `freeze` força a imutabilidade.
a => a;
[1];
undefined;
```

## Constante (_Constant_)

Uma variável que não pode ser reatribuída após ser definida pela primeira vez.

```js
const five = 5;
const john = Object.freeze({ name: 'John', age: 30 });
```

Constantes são transparentes referencialmente. Ou seja, podem ser substituídas pelos valores que representam sem afetar o resultado.

Com as duas constantes acima, a expressão a seguir vai retornar `true` sempre.

```js
john.age + five === { name: 'John', age: 30 }.age + 5;
```

## _Functor_

Um objeto que implementa uma função `map` que, enquanto executa sobre cada valor no objeto para produzir um novo objeto, adere a duas regras:

**Preserva a identidade**

```
object.map(x => x) ≍ object
```

**É passível a composição**

```
object.map(compose(f, g)) ≍ object.map(g).map(f)
```

(`f`, `g` são funções arbitrárias)

Um _functor_ comum no Javascript é o `Array` uma vez que cumpre as duas regras citadas a cima:

```js
[1, 2, 3].map(x => x); // = [1, 2, 3]
```

e

```js
const f = x => x + 1;
const g = x => x * 2;

[1, 2, 3].map(x => f(g(x))); // = [3, 5, 7]
[1, 2, 3].map(g).map(f); // = [3, 5, 7]
```

## _Functor_ Apontado (_Pointed Functor_)

Um objeto com a função `of` que coloca _qualquer_ valor único nele.

ES2015 adicionou `Array.of` transformando arrays em um _functor_ apontado.

```js
Array.of(1); // [1]
```

## Levantamento (_Lift_)

Levantamento é quando você pega um valor e coloca em um objeto como um functor. Se você levantar uma função em um _Functor_ Aplicativo então você pode fazê-lo funcionar em valores que também estão nesse _functor_.

Algumas implementações tem uma função chamada `lift` ou`liftA2` para facilitar a execução de funções em _functors_.

```js
const liftA2 = f => (a, b) => a.map(f).ap(b); // note que é `ap` e não `map`.

const mult = a => b => a * b;

const liftedMult = liftA2(mult); // essa função agora funciona em functors como array

liftedMult([1, 2], [3]); // [3, 6]
liftA2(a => b => a + b)([1, 2], [3, 4]); // [4, 5, 5, 6]
```

Levantar uma função de um único argumento e aplicá-la faz o mesmo que `map`.

```js
const increment = x => x + 1;

lift(increment)([2]); // [3]
[2].map(increment); // [3]
```

## Transparência referencial (_Referential Transparency_)

Uma expressão que pode ser substituída pelo seu valor sem alterar o comportamento do programa, é dita como referencialmente transparente.

Vamos supor que temos a função `greet`:

```js
const greet = () => 'Hello World!';
```

Qualquer invocação de `greet` pode ser substituida com `Hello World!`, logo `greet` é referencialmente transparente.

## Racioncício equacional (_Equational Reasoning_)

Quando uma aplicação é composta de expressão e desprovida de efeitos colaterais, as verdades sobre o sistema podem ser derivadas de suas partes.

## _Lambda_

É uma função anônima que pode ser tratada como um valor.

```js
(function(a) {
  return a + 1;
});
a => a + 1;
```

_Lambdas_ são frequentemente passados como argumentos para funções de ordem superior.

```js
[1, 2].map(a => a + 1); // [2, 3]
```

Você pode atribuir um _lambda_ a uma variável.

```js
const add1 = a => a + 1;
```

## Cálculo lambda (_Lambda Calculus_)

Um ramo da matemática que usa funções para criar um [modelo universal de computação](https://en.wikipedia.org/wiki/Lambda_calculus).

## Avaliação preguiçosa (_Lazy evaluation_)

Avaliação Preguiçosa é um mecanismo de avaliação chamado conforme necessidade que atrasa a avaliação de uma expressão até que seu valor seja requerido. Em linguagens funcionais, isso permite o uso de estruturas tipo listas infinitas, o que não seria possível normalmente em uma linguagem imperativa onde a sequência dos comandos é significante.

```js
const rand = function*() {
  while (1 < 2) {
    yield Math.random();
  }
};
```

```js
const randIter = rand();
randIter.next(); // Cada execução retorna um valor conforme necessário.
```

## Monóide (_Monoid_)

Monóide é um tipo de dado junto de uma função de dois parâmetros que "combina" dois valores do tipo, onde um valor de identidade que não afeta o resultado da função também existe.

Um monóide bem simples é números e adição.

```js
1 + 1; // 2
```

Nesse caso o número é o objeto e o `+` é a função.

Um valor de "identidade" também deve existir quando combinado com um valor não o altera.

O valor "identidade" para adição é `0`.

```js
1 + 0; // 1
```

Também é necessário que o agrupamento de operações não afete o resultado (associatividade):

```js
1 + (2 + 3) === 1 + 2 + 3; // true
```

A concatenação de matrizes também forma um monóide:

```js
[1, 2].concat([3, 4]); // [1, 2, 3, 4]
```

O valor identidade nesse caso é o array vazio `[]`

```js
[1, 2].concat([]); // [1, 2]
```

Se as funções identidade e composição são fornecidas, as próprias funções formam um monóide:

```js
const identity = a => a;
const compose = (f, g) => x => f(g(x));
```

`foo` é qualquer função que aceita um argumento.

```
compose(foo, identity) ≍ compose(identity, foo) ≍ foo
```

## Monade (_Monad_)

Uma monade é um objeto com funções `of` e `chain`. `chain` é como o `map` exceto pelo fato que "desninha" o objeto aninhado resultante.

```js
// Implementação
Array.prototype.chain = function(f) {
  return this.reduce((acc, it) => acc.concat(f(it)), []);
};

// Uso
Array.of('cat,dog', 'fish,bird').chain(a => a.split(',')); // ['cat', 'dog', 'fish', 'bird']

// Diferença para o map
Array.of('cat,dog', 'fish,bird').map(a => a.split(',')); // [['cat', 'dog'], ['fish', 'bird']]
```

`of` também é conhecido como `return` em outras linguagens funcionais.
`chain` também é conhecido como `flatmap` e `bind` em outras linguagens.

## Comonade (_Comonad_)

Um objeto que possui as funções `extract` e `extend`.

```js
const CoIdentity = v => ({
  val: v,
  extract() {
    return this.val;
  },
  extend(f) {
    return CoIdentity(f(this));
  }
});
```

Extract tira um valor do _functor_.

```js
CoIdentity(1).extract(); // 1
```

Extend executa a função na comonade. A função deve retornar o mesmo tipo de comonade.

```js
CoIdentity(1).extend(co => co.extract() + 1); // CoIdentity(2)
```

## _Functor_ Aplicativo (_Applicative Functor_)

Um functor aplicativo é um objeto com uma função `ap`. `ap` aplica uma função no objeto a um valor em outro objeto do mesmo tipo.

```js
// Implementação
Array.prototype.ap = function(xs) {
  return this.reduce((acc, f) => acc.concat(xs.map(f)), []);
};

// Exemplo de uso
[a => a + 1].ap([1]); // [2]
```

Isso é útil se você tiver dois objetos e quiser aplicar uma função binária ao seu conteúdo.

```js
// Array que você pode combinar
const arg1 = [1, 3];
const arg2 = [4, 5];

const add = x => y => x + y;

const partiallyAppliedAdds = [add].ap(arg1); // [(y) => 1 + y, (y) => 3 + y]
```

Isto lhe dá uma série de funções que você pode chamar `ap` para obter o resultado:

```js
partiallyAppliedAdds.ap(arg2); // [5, 6, 7, 8]
```

## Morfismo (_Morphism_)

Uma função de transformação.

### Endomorfismo (_Endomorphism_)

Uma função onde o tipo do _input_ tem o mesmo tipo do _output_.

```js
// uppercase :: String -> String
const uppercase = str => str.toUpperCase();

// decrement :: Number -> Number
const decrement = x => x - 1;
```

### Isomorfismo (_Isomorphism_)

Um par de transformações entre 2 tipos de objetos que é estrutura por natureza e não há perda de dados.

Exemplo, coordenadas 2D devem ser armazenadas em uma array `[2,3]` ou objeto `{x: 2, y: 3}`.

```js
// Prover funções para converter em ambas as direções as faz isomórficas.
const pairToCoords = pair => ({ x: pair[0], y: pair[1] });

const coordsToPair = coords => [coords.x, coords.y];

coordsToPair(pairToCoords([1, 2])); // [1, 2]

pairToCoords(coordsToPair({ x: 1, y: 2 })); // {x: 1, y: 2}
```

### Homomorfismo (_Homomorphism_)

Um homomorfismo é apenas um mapa de preservação de estrutura. Um functor é apenas um homomorfismo entre categorias, pois preserva a estrutura da categoria original sob o mapeamento.

```js
A.of(f).ap(A.of(x)) == A.of(f(x));

Either.of(_.toUpper).ap(Either.of('oreos')) == Either.of(_.toUpper('oreos'));
```

### Catamorfismo (_Catamorphism_)

Uma função `reduceRight` que aplica uma função contra um acumulador e cada valor da matriz (right-to-left) para reduzi-lo a um único valor.

```js
const sum = xs => xs.reduceRight((acc, x) => acc + x, 0);

sum([1, 2, 3, 4, 5]); // 15
```

### Anamorfismo (_Anamorphism_)

Uma função `unfold`. Um `unfold` é o oposto de `fold` (`reduce`). Ele gera uma lista a partir de um único valor.

```js
const unfold = (f, seed) => {
  function go(f, seed, acc) {
    const res = f(seed);
    return res ? go(f, res[1], acc.concat([res[0]])) : acc;
  }
  return go(f, seed, []);
};
```

```js
const countDown = n =>
  unfold(n => {
    return n <= 0 ? undefined : [n, n - 1];
  }, n);

countDown(5); // [5, 4, 3, 2, 1]
```

### Hylomorfismo (_Hylomorphism_)

A combinação de anamorfismo e catamorfismo.

### Paramorfismo (_Paramorphism_)

Uma função como `reduceRight`. No entanto, há uma diferença:

No paramorfismo, os argumentos do seu redutor são o valor atual, a redução de todos os valores anteriores e a lista de valores que formaram essa redução.

```js
// Obviamente não é seguro para listas contendo `undefined`
const para = (reducer, accumulator, elements) => {
  if (elements.length === 0) return accumulator;

  const head = elements[0];
  const tail = elements.slice(1);

  return reducer(head, tail, para(reducer, accumulator, tail));
};

const suffixes = list => para((x, xs, suffxs) => [xs, ...suffxs], [], list);

suffixes([1, 2, 3, 4, 5]); // [[2, 3, 4, 5], [3, 4, 5], [4, 5], [5], []]
```

O terceiro parâmetro no redutor (no exemplo acima, `[x, ...xs]`) é como ter um histórico do que levou você ao seu valor atual.

### Apomorfismo (_Apomorphism_)

É o oposto do paramorfismo assim como o anamorfismo é o oposto do catamorfismo. Enquanto que com o paramorfismo você combina o acesso ao acumulador e com o que foi acumulado, o apomorfismo permite que você se "desdobre" com a possibilidade de retornar cedo.

## Setóide (_Setoid_)

Um objeto que tem uma função `equals` que pode ser usada para comparar outros objetos do mesmo tipo.

Vamos transformar Array em setóide:

```js
Array.prototype.equals = function(arr) {
  const len = this.length;
  if (len !== arr.length) {
    return false;
  }
  for (let i = 0; i < len; i++) {
    if (this[i] !== arr[i]) {
      return false;
    }
  }
  return true;
};
[1, 2].equals([1, 2]); // true
[1, 2].equals([0]); // false
```

## Semigrupo (_Semigroup_)

Um objeto que tem uma função `concat` que combina com outro objeto do mesmo tipo.

```js
[1].concat([2]); // [1, 2]
```

## Dobrável (_Foldable_)

Um objeto que possui uma função `reduce` que pode transformar o objeto em outro tipo.

```js
const sum = list => list.reduce((acc, val) => acc + val, 0);
sum([1, 2, 3]); // 6
```

## _Lens_

Um _lens_ é uma estrutura (geralmente um objeto ou função) que emparelha um getter e um setter não-mutante para alguma outra estrutura de dados.

```js
// Usando [lens da biblioteca Ramda](http://ramdajs.com/docs/#lens)
const nameLens = R.lens(
  // getter para a propriedade name de um objeto
  obj => obj.name,
  // setter para a propriedde name
  (val, obj) => Object.assign({}, obj, { name: val })
);
```

Ter o par `get e set` para uma estrutura de dados permite alguns recursos chave.

```js
const person = { name: 'Gertrude Blanch' };

// chamando o getter
R.view(nameLens, person); // 'Gertrude Blanch'

// chamando o setter
R.set(nameLens, 'Shafi Goldwasser', person); // {name: 'Shafi Goldwasser'}

// executar uma função no valor da estrutura
R.over(nameLens, uppercase, person); // {name: 'GERTRUDE BLANCH'}
```

Os _lens_ também são composíveis. Isso permite atualizações fáceis e imutáveis em dados profundamente aninhados.

```js
// Esse lens foca no primeiro item em uma matriz não vazia
const firstLens = R.lens(
  // pega o primeiro item
  xs => xs[0],
  // setter não mutável para o primeiro item no array
  (val, [__, ...xs]) => [val, ...xs]
);

const people = [{ name: 'Gertrude Blanch' }, { name: 'Shafi Goldwasser' }];

R.over(
  compose(
    firstLens,
    nameLens
  ),
  uppercase,
  people
); // [{'name': 'GERTRUDE BLANCH'}, {'name': 'Shafi Goldwasser'}]
```

Outras implementações:

- [partial.lenses](https://github.com/calmm-js/partial.lenses)
- [nanoscope](http://www.kovach.me/nanoscope/)

## Tipos de assinatura (_Type Signatures_)

Frequentemente, as funções no JavaScript incluirão comentários que indicam os tipos de seus argumentos e valores de retorno.

Há um pouco de variação na comunidade, mas eles geralmente seguem os seguintes padrões:

```js
// functionName :: firstArgType -> secondArgType -> returnType

// add :: Number -> Number -> Number
const add = x => y => x + y;

// increment :: Number -> Number
const increment = x => x + 1;
```

Se uma função aceita outra função como um argumento, ela é colocada entre parênteses.

```js
// call :: (a -> b) -> a -> b
const call = f => x => f(x);
```

As letras `a`,`b`, `c`,`d` são usadas para significar que o argumento pode ser de qualquer tipo. A seguinte versão do `map` usa uma função que transforma um valor de algum tipo `a` em outro tipo `b`, uma matriz de valores do tipo `a`, e retorna uma matriz de valores do tipo `b`.

```js
// map :: (a -> b) -> [a] -> [b]
const map = f => list => list.map(f);
```

**Leitura adicional (em inglês)**

- [Ramda's type signatures](https://github.com/ramda/ramda/wiki/Type-Signatures)
- [Mostly Adequate Guide](https://drboolean.gitbooks.io/mostly-adequate-guide/content/ch7.html#whats-your-type)
- [What is Hindley-Milner?](http://stackoverflow.com/a/399392/22425) no Stack Overflow

## Tipos algébricos (_Algebraic data type_)

Um tipo composto criado a partir de outros tipos. Dois tipos algébricos comuns são sum e product.

### _Sum type_

Um tipo Sum é a combinação de dois tipos juntos em outro. É chamado soma porque o número de valores possíveis no tipo de resultado é a soma dos tipos de entrada.

JavaScript não tem tipos como este, mas podemos usar `Set`s para fingir:

```js
const bools = new Set([true, false]);
const halfTrue = new Set(['half-true']);

const weakLogicValues = new Set([...bools, ...halfTrue]);
```

Às vezes, os tipos de soma são chamados de tipos de união, uniões discriminadas ou uniões marcadas.

### _Product type_

Um **_product type_** combina os tipos de uma maneira que você provavelmente está mais familiarizado:

```js
// point :: (Number, Number) -> {x: Number, y: Number}
const point = (x, y) => ({ x, y });
```

É chamado de produto porque os valores totais possíveis da estrutura de dados são o produto dos diferentes valores. Muitas linguagens tem um tipo de tupla, que é a formulação mais simples de um product type.

Veja também (em inglês): [Set theory](https://en.wikipedia.org/wiki/Set_theory).

## Opção (_Option_)

Opção é um _sum type_ com duas classes geralmente chamadas de `Some` (_alguma_) and `None` (_nenhuma_).

Opção é útil para compor funções que podem não retornar um valor.

```js
// Definição inocente

const Some = v => ({
  val: v,
  map(f) {
    return Some(f(this.val));
  },
  chain(f) {
    return f(this.val);
  }
});

const None = () => ({
  map(f) {
    return this;
  },
  chain(f) {
    return this;
  }
});

// maybeProp :: (String, {a}) -> Option a
const maybeProp = (key, obj) =>
  typeof obj[key] === 'undefined' ? None() : Some(obj[key]);
```

Use `chain` para colocar em sequência funções que retornam `Option`s

```js
// getItem :: Cart -> Option CartItem
const getItem = cart => maybeProp('item', cart);

// getPrice :: Item -> Option Number
const getPrice = item => maybeProp('price', item);

// getNestedPrice :: cart -> Option a
const getNestedPrice = cart => getItem(cart).chain(getPrice);

getNestedPrice({}); // None()
getNestedPrice({ item: { foo: 1 } }); // None()
getNestedPrice({ item: { price: 9.99 } }); // Some(9.99)
```

`Option` também é conhecido como `Maybe`. `Some` às vezes é chamado `Just`. `None` às vezes é chamado de `Nothing`.

## Função (_Function_)

Uma **função** `f :: A => B` é uma expressão - frequentemente chamade de _arrow_ ou _lambda_ - com **exatamente um (imutável)** parâmetro do tipo `A` e **exatamente um** valro do tipo `B` retornado. Esse valor depende inteiramente do argumento, tornando as funções independentes do contexto ou referencialmente transparentes. O que está implícito aqui é que uma função não deve produzir nenhum efeito colateral - uma função é sempre pura por definição. Essas propriedades tornam as funções agradáveis de se trabalhar: elas são inteiramente determinísticas e, portanto, previsíveis.

```js
// times2 :: Number -> Number
const times2 = n => n * (2)[(1, 2, 3)].map(times2); // [2, 4, 6]
```

## Funções parciais (_Partial function_)

Uma função parcial é uma função que não está definida para todos os argumentos - pode retornar um resultado inesperado ou nunca terminar. Funções parciais adicionam sobrecarga cognitiva, são mais difíceis de raciocinar e podem levar a erros de tempo de execução. Alguns exemplos:

```js
// exemplo 1: soma da lista
// sum :: [Number] -> Number
const sum = arr => arr.reduce((a, b) => a + b);
sum([1, 2, 3]); // 6
sum([]); // TypeError: Reduce of empty array with no initial value

// exemplo 2: pega o primeiro item da lista
// first :: [A] -> A
const first = a => a[0];
first([42]); // 42
first([]); // undefined
// ou pior ainda:
first([[42]])[0]; // 42
first([])[0]; // Uncaught TypeError: Cannot read property '0' of undefined

// exemplo 3: repetir a função N vezes
// times :: Number -> (Number -> Number) -> Number
const times = n => fn => n && (fn(n), times(n - 1)(fn));
times(3)(console.log);
// 3
// 2
// 1
times(-1)(console.log);
// RangeError: Maximum call stack size exceeded
```

### Lidando com funções parciais

As funções parciais são perigosas, pois precisam ser tratadas com grande cautela. Você pode obter um resultado inesperado (errado) ou executar erros de tempo de execução. Às vezes, uma função parcial pode não retornar. Estar ciente e tratar todos esses casos específicos pode se tornar muito tedioso.
Felizmente, uma função parcial pode ser convertida em uma função regular (ou total). Podemos fornecer valores padrão ou usar proteções para lidar com entradas para as quais a função parcial (anteriormente) é indefinida. Utilizando o tipo `Opção`, podemos fornecer `Some (value)` ou `None` onde, de outra forma, teríamos nos comportado de maneira inesperada:

```js
// exemplo 1: soma da lista
// podemos fornecer o valor padrão para que ele sempre retorne o resultado
// sum :: [Number] -> Number
const sum = arr => arr.reduce((a, b) => a + b, 0);
sum([1, 2, 3]); // 6
sum([]); // 0

// exemplo 2: pega o primeiro item da lista
// mude o resultado para Option
// first :: [A] -> Option A
const first = a => (a.length ? Some(a[0]) : None());
first([42]).map(a => console.log(a)); // 42
first([]).map(a => console.log(a));
// nosso pior caso anterior
first([[42]]).map(a => console.log(a[0])); // 42
first([]).map(a => console.log(a[0])); // não será executado, então não teremos erros aqui
// mais disso, você saberá pelo tipo de retorno da função (Opção)
// que você deve usar o método `.map` para acessar os dados e você nunca esquecerá
// para checar sua entrada porque tal verificação se torna embutida na função

// exemplo 3: repetir a função N vezes
// devemos fazer a função sempre terminar alterando as condições:
// times :: Number -> (Number -> Number) -> Number
const times = n => fn => n > 0 && (fn(n), times(n - 1)(fn));
times(3)(console.log);
// 3
// 2
// 1
times(-1)(console.log);
// não executará nada
```

Fazer suas funções parciais totais pode evitar erros de tempo de execução. Sempre retonar um valor também fará com que o código seja mais fácil de manter e raciocinar.

## Bibliotecas para programação funcional em Javascript

- [mori](https://github.com/swannodette/mori)
- [Immutable](https://github.com/facebook/immutable-js/)
- [Immer](https://github.com/mweststrate/immer)
- [Ramda](https://github.com/ramda/ramda)
- [ramda-adjunct](https://github.com/char0n/ramda-adjunct)
- [Folktale](http://folktale.origamitower.com/)
- [monet.js](https://cwmyers.github.io/monet.js/)
- [lodash](https://github.com/lodash/lodash)
- [Underscore.js](https://github.com/jashkenas/underscore)
- [Lazy.js](https://github.com/dtao/lazy.js)
- [maryamyriameliamurphies.js](https://github.com/sjsyrek/maryamyriameliamurphies.js)
- [Haskell in ES6](https://github.com/casualjavascript/haskell-in-es6)
- [Sanctuary](https://github.com/sanctuary-js/sanctuary)
- [Crocks](https://github.com/evilsoft/crocks)
- [Fluture](https://github.com/fluture-js/Fluture)
