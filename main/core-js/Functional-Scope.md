# Functional Scope

## Know global scope and functional scope
**Область видимости** — это часть программы, в которой мы можем обратиться к переменной,
функции или объекту. Этой частью может быть функция, блок или вся программа
в целом — то есть *мы всегда находимся как минимум в одной области видимости*.

![image](https://github.com/Timothy7310/Core-JS/assets/55184984/eb9efd33-05d3-4500-826d-099dfa3522ad)

Области видимости помогают скрывать переменные от нежелательного доступа, управлять побочными эффектами и разбивать код на смысловые блоки.

**Глобальная область видимости** — это самая внешняя коробка из всех. Когда мы «просто объявляем переменную», 
вне функций, вне модулей, то эта переменная попадает в глобальную область видимости.

Переменная в примере сейчас находится в глобальной области видимости. Это значит, что она будет доступна откуда угодно внутри модуля:

```js
const a = 42
console.log(a)
// 42

function wrap() {
  const b = a
  // Без проблем, переменная a доступна в этой функции.
}

const c = {
  d: a,
  // Хорошо, переменная a доступна и здесь.
}

function secondWrap() {
  const e = {
    f: a,
    // И тут ок, переменная a всё ещё доступна.
  }
}
```

JS в браузерах так устроен, что глобальные переменные попадают в объект `window`. 
Если очень грубо, то можно сказать, что `window` в случае браузера — это и есть глобальная область видимости.

```js
console.log(console)
// Console {debug: function, error: function,
// log: function, info: function, warn: function, …}

console.log(window.console)
// Console {debug: function, error: function,
// log: function, info: function, warn: function, …}
// То же самое, потому что это один и тот же объект.
```

**Блочная область видимости** ограничена программным блоком, обозначенным при помощи `{` и `}`.
Простейший пример такой области — это выражение внутри скобок:

```js
const a = 42
console.log(a)
// 42

if (true) {
  const b = 43
  console.log(a)
  // 42
  console.log(b)
  // 43
}

console.log(b)
// ReferenceError: Can't find variable: b
```

**Функциональная область видимости** — это область видимости в пределах тела функции. Можно сказать, что она ограничена `{` и `}` функции.

```js
const a = 42

function scoped() {
  const b = 43
}

console.log(a)
// 42
console.log(b)
// Reference error.
```

Функциям доступны лишь переменные в её собственной области видимости (всё, что внутри её тела) и в родительских областях:

```js
function outer() {
  const a = 42

  function inner() {
    console.log(a)
    // 42
  }
}
```

## Know variables visibility areas
К переменным, объявленным при помощи ключевого слова `var`, можно обращаться до момента объявления. 
В отличие от `let` и `const`, ошибки это не вызовет. Такое поведение называется _hoisting_ - «всплытие»:

```js
console.log(a)
// undefined

var a = 5

console.log(a)
// 5
```

Код выше неявно превращается в следующий вид:

```js
var a; // всплытие объявления переменной a; 🔥
console.log(a)
// undefined

var a = 5

console.log(a)
// 5
```

💡 Значение объявленое с помощью `const` или `let` также всплывает, но в отличии от `var` к нему нельзя обратиться из-за TDZ (Temporal Dead Zone)

```js
console.log(a)
// ReferenceError: Cannot access 'a' before initialization
console.log(b)
// ReferenceError: Cannot access 'b' before initialization

let a = 5
const a = 5
```

💡 Однако переменная может использоваться и выше объявления, при условии, что содержащая её часть кода будет выполнена после инициализации:

```js
function foo() {
  console.log('from foo', a)
}

Promise.resolve()
  .then(() => console.log('from promise', a))

setTimeout(() => console.log('from timer',a))

let a = 10

foo()

// 'from foo 10', 'from promise 10', 'from timer 10'
```

_TDZ_ есть также и у ES6-классов, несмотря на то, что они являются «синтаксическим сахаром» над обычными функциями.

```js
console.log(Foo)

class Foo {
  constructor(bar) {
    this.bar = bar
  }
}
// ReferenceError: Cannot access 'Foo' before initialization
```

А функции (объявленные как _Function Declaration_) _TDZ_ не имеют. То есть они также как и `var` **всплывают**

```js
console.log(Foo)

function Foo() {
  this.bar = bar
}
// ƒ Foo() { this.bar = bar}
```

## Understand nested scopes and able work with them
Как мы видели выше, у дочерней функции есть доступ к области видимости родительской функции:

```js
function outer() {
  let a = 42

  function inner() {
    console.log(a)
  }

  inner()
}

outer()
// 42
```

Всё так же у функции `inner` локальных переменных нет, она лишь использует локальные переменные родительской функции `outer`. 
И всё так же у кода снаружи `outer` нет никакого доступа к её внутренностям.

Но что, если мы вернём из функции `outer` функцию `inner`?

```js
function outer() {
  let a = 42

  function inner() {
    console.log(a)
  }

  return inner
}
```

Теперь мы можем не просто вызывать функцию `outer`, но и присвоить результат вызова какой-то переменной:

```js
const accessToInner = outer()

accessToInner()
// 42
```

Теперь в переменной `accessToInner` находится функция `inner`, у которой всё ещё есть доступ к локальной переменной `a` функции `outer`!

То есть мы смогли «обойти» область видимости? Не совсем.

Мы действительно получили доступ к переменной `a` через функцию `inner`, 
но только в том виде и с такими ограничениями, которые описаны при создании функции `inner`.

У нас всё ещё нет прямого доступа к переменной `a`. Мы, например, не можем её поменять — только вывести в консоль.

Грубо говоря, мы создали функцию, которая даёт нам читать переменные, но не изменять их. Это полезно, если мы хотим дать ограниченный доступ к внутренностям модуля.

Допустим, мы хотим сделать счётчик, который можно увеличивать и уменьшать только на единицу:

```js
function counter() {
  // Начальное значение счётчика будет 0.
  // Мы используем let, потому что будем менять значение,
  // const не подойдёт.
  let state = 0

  // Функция increase будет увеличивать счётчик на единицу.
  function increase() {
    state++
  }

  // Функция decrease будет уменьшать счётчик на единицу.
  function decrease() {
    state--
  }

  // Функция valueOf будет выводить значение.
  function valueOf() {
    console.log(state)
  }

  // А наружу мы дадим только лишь доступ к этим функциям.
  // Вернём объект, значениями полей которого будут функции
  // increase и decrease.
  //
  // Прямого доступа к переменной state всё ещё нет,
  // но внешний код может изменять её состояние опосредованно —
  // через функции increase и decrease.
  return {
    increase,
    decrease,
    valueOf,
  }
}

const ticktock = counter()
ticktock.increase()
ticktock.valueOf()
// 1
ticktock.increase()
ticktock.valueOf()
// 2
ticktock.decrease()
ticktock.valueOf()
// 1
```

Такое контролируемое сокрытие доступа с помощью области видимости называется _замыканием_.

_Замыкания_ удобны тем, что каждый новый вызов создаёт _отдельную область_, в которой значения абсолютно независимы друг от друга:

```js
const tick1 = counter()
const tick2 = counter()

tick1.valueOf()
// 0
tick2.valueOf()
// 0

tick1.increase()
tick1.valueOf()
// 1
tick2.valueOf()
// 0

tick1.increase()
tick1.valueOf()
// 2
tick2.valueOf()
// 0

tick2.increase()
tick1.valueOf()
// 2
tick2.valueOf()
// 1

tick2.decrease()
tick1.valueOf()
// 2
tick2.valueOf()
// 0
```

Состояния обоих счётчиков друг от друга не зависят, хотя они создаются одной и той же функцией.

## Links
- [Области видимости](https://doka.guide/js/closures/)
- [Переменные const, let и var](https://doka.guide/js/var-let/)
- [JS scope + JS functions. Part1](https://www.youtube.com/watch?v=c_rHAYNBotQ)
- [JS scope + JS functions. Part2](https://www.youtube.com/watch?v=h5o_tgEMKxY)
