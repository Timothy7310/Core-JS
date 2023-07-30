# Advanced Functions

## `this` in functions
`this` — это ссылка на некий объект, к свойствам которого можно получить доступ внутри вызова функции. Этот `this` — и есть _контекст выполнения_.

Когда мы вызываем _функцию_, значением `this` может быть лишь _глобальный объект_ или `undefined` при использовании `'use strict'`.

Можно сказать, что _строгий режим_ — неказистый способ борьбы с легаси.

Включается строгий режим с помощью директивы `'use strict'` в начале блока, который должен выполняться в строгом режиме.

```js
function nonStrict() {
  // Будет выполняться в нестрогом режиме.
}

function strict() {
  'use strict'
  // Будет выполняться в строгом режиме.
}
```

💡Также `use strict` неявно включается в файлах `.js` с атрибутом `module`: `<script src="main.js" type="module"></script>`  
и также внутри классов.

Вернёмся к `this`. В нестрогом режиме при выполнении в браузере `this` при вызове функции будет равен `window`:

```js
function whatsThis() {
  console.log(this === window)
}

whatsThis()
// true
```

То же — если функция объявлена внутри функции:

```js
function whatsThis() {
  function whatInside() {
    console.log(this === window)
  }

  whatInside()
}

whatsThis()
// true
```

И то же — если функция будет анонимной и, например, вызвана немедленно:

```js
;(function () {
  console.log(this === window)
})()
// true
```

В строгом режиме — значение будет равно `undefined`:

```js
'use strict'

function whatsThis() {
  console.log(this === undefined)
}

whatsThis()
// true
```

## Reference Type & losing `this`

## Understand difference between function and method
Если функция хранится в объекте — это метод этого объекта.

```js
const user = {
  name: 'Alex',
  greet() {
    console.log(`Hello, my name is ${this.name}`)
  },
}

user.greet()
// Hello, my name is Alex
```

При вызове через точку `user.greet()` значение `this` равняется объекту до точки (`user`). Без этого объекта `this` равняется глобальному объекту (в обычном режиме). В _строгом режиме_ мы бы получили ошибку `«Cannot read properties of undefined»`.

Обратите внимание, что `this` определяется в момент вызова функции. Если записать метод объекта в переменную и вызвать её, значение `this` изменится.

```js
const user = {
  name: 'Alex',
  greet() {
    console.log(`Hello, my name is ${this.name}`)
  },
}

const greet = user.greet
greet()
// Hello, my name is
```

## Understand how `this` works, realize `this` possible issues

## Manage `this`

## Be able to replace `this` value

## Be able to use `call` and `apply` Function built-in methods

## Know how to bind `this` scope to function

## Binding, binding one function twice

## Links
- [this: контекст выполнения функций](https://doka.guide/js/function-context/)
