# Objects Built-in methods.

## Know static Object methods

### Object.create()
Создаёт новый объект с указанным прототипом и свойствами.

```js
const person = {
  name: "Massoud",
  lasName:"Sharifi"
};
const me = Object.create(person);
me.name = 'Marouf'; //change name of me object
console.log(me.name)// expected output: "Marouf"
```

### Object.assign()
Копирует значения всех перечисляемых свойств из одного или нескольких исходных объектов в целевой объект.

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const returnedTarget = Object.assign(target, source);
console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

### Object.keys()
Возвращает массив ключей объекта

```js
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};
console.log(Object.keys(object1));
// expected output: Array ["a", "b", "c"]
```

### Object.values()
Возвращает массив значений объекта

```js
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};
console.log(Object.values(object1));
// expected output: Array ["somestring", 42, false]
```

### Object.entries()
Возвращает двумерный массив вида: `[ [key1, value1], [key2, value2], ... ]`

```js
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]
```

### Object.fromEntries()
Преобразует двумерный массив вида `[ [key1, value1], [key2, value2], ... ]` в объект с данными ключами и значениями

### Object.freeze()
Этот метод замораживает объект. Замороженный объект больше нельзя изменить; замораживание объекта предотвращает добавление к нему новых свойств,
удаление существующих свойств, а также предотвращает изменение значений существующих свойств.

```js
const obj = {
  prop: 42
};
Object.freeze(obj);
obj.prop = 33;
// Throws an error in strict mode
console.log(obj.prop);
// expected output: 42
```

### Object.isFrozen()
Определяет заморожен ли объект.

```js
const object1 = {
  property1: 42
};
console.log(Object.isFrozen(object1));
// expected output: false
Object.freeze(object1);
console.log(Object.isFrozen(object1));
// expected output: true
```

### Object.seal()
Запрещает добавление новыйх свойств, но можно изменять уже существующие свойства

```js
const object1 = {
  property1: 42
};
Object.seal(object1);
object1.property1 = 33;
console.log(object1.property1);
// expected output: 33
delete object1.property1; // cannot delete when sealed
console.log(object1.property1);
// expected output: 33
```

### Object.isSealed()
Определяет запечатан ли объект.

## Object.getPrototypeOf()
Возвращает прототип (т. е. значение внутреннего свойства [[Prototype]]) указанного объекта.

```js
const prototype1 = {};
const object1 = Object.create(prototype1);
console.log(Object.getPrototypeOf(object1) === prototype1);
// expected output: true
```

### Object.setPrototypeOf()
Устанавливает прототип (т. е. внутреннее свойство [[Prototype]]) указанного объекта в другой объект или `null`.

```js
const obj = {};
const parent = { foo: 'bar' };

console.log(obj.foo);
// Expected output: undefined

Object.setPrototypeOf(obj, parent);

console.log(obj.foo);
// Expected output: "bar"
```

### Object.prototype.hasOwnProperty()
Возвращает логическое значение, указывающее, содержит ли объект указанное свойство.

```js
const obj = { answer: 42 }

console.log(obj.hasOwnProperty('answer'))
// Expected output: true
```

### Object.is()
Определяет, являются ли два значения одним и тем же значением.

```js
console.log(Object.is('1', 1));
// Expected output: false

console.log(Object.is(NaN, NaN));
// Expected output: true

console.log(Object.is(-0, 0));
// Expected output: false

const obj = {};
console.log(Object.is(obj, {}));
// Expected output: false
```

Похоже на `===`, за исключение нескольких случаев:

```js
// Case 1: Signed zero
Object.is(0, -0); // false
Object.is(+0, -0); // false
Object.is(-0, -0); // true

// Case 2: NaN
Object.is(NaN, 0 / 0); // true
Object.is(NaN, Number.NaN); // true

// for ===

0 === -0 // true
+0 === -0 // true
-0 === -0 // true

NaN === NaN // false
NaN === Number.NaN // false
```

## Property flags & descriptors (student is able to set property via `Object.defineProperty`)

### Object.defineProperty()
Определяет новое свойство непосредственно в объекте или изменяет существующее свойство объекта и возвращает объект.

```js
Object.defineProperty(obj, prop, descriptor)
```
__Варианты доступа к свойствам объекта:__
- `writable` – если `true`, свойство можно изменить, иначе оно только для чтения.
- `enumerable` – если `true`, свойство перечисляется в циклах, в противном случае циклы его игнорируют.
- `configurable` – если `true`, свойство можно удалить, а эти атрибуты можно изменять, иначе этого делать нельзя.

__Пример:__  
```js
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});

object1.property1 = 77;
// Throws an error in strict mode

console.log(object1.property1);
// Expected output: 42
```

### Object.defineProperties()
Тоже самое что и Object.defineProperty(), только позволяет определять множество свойств сразу.

```js
const object1 = {};
Object.defineProperties(object1, {
  property1: {
    value: 42,
    writable: true
  },
  property2: {
   value:"Massoud",
    writable:false
  }
});
console.log(object1.property1);
// expected output: 42
console.log(object1.property2);
// expected output: Massoud
```

## Links
- [Object Static Methods — Javascript](https://medium.com/@massoud-sharifi/object-static-methods-javascript-4444af635a9f)
- [4 Static Object Methods I Wish I Knew About Sooner](https://dev.to/sandricoprovo/4-static-object-methods-i-wish-i-knew-about-sooner-3l32)
