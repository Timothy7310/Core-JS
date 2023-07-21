# ECMAScript Data Types & Expressions

## Object computed props
Мы можем использовать квадратные скобки в литеральной нотации для создания вычисляемого свойства.

```js
let fruit = prompt("Какой фрукт купить?", "apple");

let bag = {
  [fruit]: 5, // имя свойства будет взято из переменной fruit 🔥
};

alert( bag.apple ); // 5, если fruit="apple"
```

Смысл **вычисляемого свойства** прост: запись `[fruit]` означает, что имя свойства необходимо взять из переменной `fruit`.

И если посетитель введёт слово `"apple"`, то в объекте `bag` теперь будет лежать свойство `{apple: 5}`.

По сути, пример выше работает так же, как и следующий пример:

```js
let fruit = prompt("Какой фрукт купить?", "apple");
let bag = {};

// имя свойства будет взято из переменной fruit
bag[fruit] = 5;
```

…Но первый пример выглядит лаконичнее.

Мы можем использовать и более сложные выражения в квадратных скобках:

```js
let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5 // bag.appleComputers = 5
};
```

## Be able to loop through Object keys
Объект для примера

```js
const obj = {
    a: 4,
    c: 2,
    d: 'Number',
    f: 'Answer',
    g: 'All'
}
```

```js
function loopByObject1(obj) {
    console.log('loop by Object 1');
    const keys = Object.keys(obj); // Получаем массив ключей obj
    keys.forEach((key) => {
        console.log(obj[key]);
    });
}
```

```js
function loopByObject2(obj) {
    console.log('loop by Object 2');
    const keys = Object.keys(obj); // Получаем массив ключей obj
    for (const key of keys) {
        console.log(obj[key]);
    }
}
```

```js
function loopByObject3(obj) {
    console.log('loop by Object 3');
    const keys = Object.keys(obj); // Получаем массив ключей obj
    for (let i = 0; i < keys.length; i += 1) {
        const objKey = keys[i]
        console.log(obj[objKey]);
    }
}
```

```js
function loopByObject4(obj) {
    console.log('loop by Object 4');
    for (const key in obj) {
        console.log(obj[key]);
    }
}
```

```js
function loopByObject5(obj) {
    console.log('loop by Object 5');
    Object.entries(obj).forEach(([_key, value]) => console.log(value))

    // С помощью Object.entries получаем массив вида: [[key1, value1], [key2, value2], ...]
    // А затем проходим по нему методом forEach, где используем деструктуризацию
}
```

## Links
- [Вычисляемые свойства](https://learn.javascript.ru/object#vychislyaemye-svoystva)
