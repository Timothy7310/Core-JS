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

## Reference Type & losing `this`

Как только метод передается отдельно от объекта — `this` теряется.

Вот как это может произойти в случае с `setTimeout`:

```js
const user = {
  firstname: 'Вася',
  sayHi() {
    console.log(`Привет, ${this.firstname}!`)
  }
}

setTimeout(user.sayHi, 1000); // -> Привет, undefined!
```

Это произошло потому, что `setTimeout` получил функцию `sayHi` отдельно от объекта `user` (именно здесь функция потеряла контекст). То есть последняя строка может быть переписана как:

```js
let f = user.sayHi;
setTimeout(f, 1000); // контекст user потерян
```

Еще внутренняя функция имеет доступ ко всем переменным внешней, но не имеет доступа `this`. 
То есть: если внешняя функция привязана к какому-то DOM элементу, то `this` в ней будет указывать на этот элемент, но `this` внутренней функции - не будет!

```js
"use strict";

let elem = document.querySelector('#elem');
elem.addEventListener('blur', parent);

function parent() {
	console.log(this); // выведет ссылку на наш инпут
	
	function child() {
		console.log(this); // выведет undefined
	}
	child();
}
```

## Understand how `this` works, realize `this` possible issues
same question

## Manage `this`
_Непрямым вызовом_ называют вызов функций через `call()` или `apply()`.

Оба первым аргументом принимают `this`. То есть они позволяют настроить контекст снаружи, к тому же — явно.

```js
function greet() {
  console.log(`Hello, ${this.name}`)
}

const user1 = { name: 'Alex' }
const user2 = { name: 'Ivan' }

greet.call(user1)
// Hello, Alex
greet.call(user2)
// Hello, Ivan

greet.apply(user1)
// Hello, Alex
greet.apply(user2)
// Hello, Ivan
```

В обоих случаях в первом вызове `this` === `user1`, во втором — `user2`

Разница между `call()` и `apply()` — в том, как они принимают аргументы для самой функции после `this`.

`call()` принимает аргументы списком через запятую, `apply()` же — принимает массив аргументов. В остальном они идентичны:

```js
function greet(greetWord, emoticon) {
  console.log(`${greetWord} ${this.name} ${emoticon}`)
}

const user1 = { name: 'Alex' }
const user2 = { name: 'Ivan' }

greet.call(user1, 'Hello,', ':-)')
// Hello, Alex :-)
greet.call(user2, 'Good morning,', ':-D')
// Good morning, Ivan :-D
greet.apply(user1, ['Hello,', ':-)'])
// Hello, Alex :-)
greet.apply(user2, ['Good morning,', ':-D'])
// Good morning, Ivan :-D
```

💡 Существует мнемоническое правило: первая буква в методе `apply()` означается `Array`

## Be able to replace `this` value
`call()`, `apply()`, `bind()`

## Be able to use `call` and `apply` Function built-in methods
Описано здесь [тута](https://github.com/Timothy7310/Core-JS/edit/main/main/core-js/Advanced-Functions.md#manage-this)

## Know how to bind `this` scope to function
Особняком стоит `bind()`. Это метод, который позволяет связывать контекст выполнения с функцией, чтобы «заранее и точно» определить, какое именно значение будет у `this`.

```js
function greet() {
  console.log(`Hello, ${this.name}`)
}

const user1 = { name: 'Alex' }

const greetAlex = greet.bind(user1)
greetAlex()
// Hello, Alex
```

Обратите внимание, что `bind()`, в отличие от `call()` и `apply()`, не вызывает функцию сразу. 
Вместо этого он возвращает другую функцию — связанную с указанным контекстом навсегда. Контекст у этой функции изменить невозможно.

```js
function getAge() {
  console.log(this.age);
}

const howOldAmI = getAge.bind({age: 20}).bind({age: 30})

howOldAmI();
//20
```

## Binding, binding one function twice

## Links
- [this: контекст выполнения функций](https://doka.guide/js/function-context/)
- [Потеря контекста в JavaScript](https://www.code.mu/ru/javascript/book/prime/context/context-losing/)
- [Привязка контекста к функции](https://frontend-blog-tau.vercel.app/bind)
