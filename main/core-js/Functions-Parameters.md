# Functions Parameters / Arguments

## Know how to define Function parameters
При вызове функции можно передать данные, они будут использованы кодом внутри.

Например, функция `showMessage` принимает два параметра под названиями `user` и `message`, а потом соединяет их для целого сообщения.

```js
function showMessage(user, message) {
  console.log(user + ': ' + message)
}
```

При вызове функции ей нужно передать аргументы. Функцию можно вызывать сколько угодно раз с любыми аргументами:

```js
showMessage('Маша', 'Привет!')
// Маша: Привет!

showMessage('Иван', 'Как делишки?')
// Иван: Как делишки?
```

📌Разница между параметрами и аргументами:
- **Параметр** – это переменная, указанная в круглых скобках в объявлении функции.
- **Аргумент** – это значение, которое передаётся функции при её вызове.

Если при вызове функции аргумент не был указан, то его значением становится `undefined`.
Это не приведёт к ошибке. Такой вызов выведет `"Маша: undefined"`. В вызове не указан параметр `text`, поэтому предполагается, что `text === undefined`.

```js
showMessage('Маша')
// Маша: undefined
```

Если мы хотим задать параметру `text` значение по умолчанию, мы должны указать его после `=`:

```js
function showMessage(user, message = 'текст не добавлен') {
  console.log(user + ': ' + message)
}

showMessage('Маша')
// Маша: текст не добавлен

showMessage('Маша', 'Привет!')
// Маша: Привет!
```

☝ Параметры по умолчанию всегда указываются в конце, иначе первый аргумент в вызове функции просто перезапишит значение по умолчанию

```js
function showMessage(message = 'текст не добавлен', user) {
  console.log(user + ': ' + message)
}

showMessage('Маша') // message === 'Маша', user === undefined
// undefined: Маша
```

## Know difference between parameters passing by value and by reference
_Строки_, _числа_, _логические значения_ передаются в функцию по **значению**. 
Иными словами при передаче значения в функцию, эта функция получает копию данного значения. 
Рассмотрим, что это значит в практическом плане:

```js
function change(x){
    x = 2 * x;
    console.log("x in change:", x);
}
 
const n = 10;
console.log("n before change:", n); // n before change: 10
change(n);                          // x in change: 20
console.log("n after change:", n);  // n after change: 10
```

Функция `change` получает некоторое число и увеличивает его в два раза. При вызове функции `change` ей передается число `n`. 
Однако после вызова функции мы видим, что число `n` **не изменилось**, хотя в самой функции произошло увеличение значения параметра. 
Потому что при вызове функция `change` получает копию значения переменной `n`. И любые изменения с этой копией никак не затрагивают саму переменную `n`.

_Объекты_ и _массивы_ передаются **по ссылке**. При использовании ссылочного типа данных копируется ссылка.
Все изменения в объекте, который был передан в качестве аргумента, будут видны всем, кто владеет ссылкой: 
То есть функция получает сам объект или массив, а не их копию.

```js
function change(user){
    user.name = "Tom";
}
 
const bob ={ 
    name: "Bob"
};
console.log("before change:", bob.name);    // Bob
change(bob);
console.log("after change:", bob.name);     // Tom
```

В данном случае функция `change` получает объект и меняет его свойство `name`. 
В итоге мы увидим, что после вызова функции изменился оригинальный объект `bob`, который передавался в функцию.

Однако если мы попробуем переустановить объект или массив полностью, оригинальное значение **не изменится**.

```js
function change(user){
    // полная переустановка объекта
    user= {
        name:"Tom"
    };
}
 
const bob ={ 
    name: "Bob"
};
console.log("before change:", bob.name);    // Bob
change(bob);
console.log("after change:", bob.name);     // Bob
```

То же самое касается массивов:

```js
function change(array){
    array[0] = 8;
}
function changeFull(array){
    array = [9, 8, 7];
}

const numbers = [1, 2, 3];

console.log("before change:", numbers);     // [1, 2, 3]
change(numbers);
console.log("after change:", numbers);      // [8, 2, 3]
changeFull(numbers);
console.log("after changeFull:", numbers);  // [8, 2, 3]
```

📌 Чтобы безопасно менять ссылочный тип данных его необходимо предварительно скопировать.
Используя `Object.assign()` или `spread-оператор ...`

## Know how to handle dynamic amount of Function parameters
Объект `arguments` это устаревший способ получить все значения, переданные в функцию при вызове в виде массивоподобного объекта.
Используется в функциях, которые принимают произвольное количество аргументов.

📌Не работает в стрелочных функциях.

```js
function joinStrings() {
  const strings = []
  for (let i = 0; i < arguments.length; i++) {
    if (typeof arguments[i] === 'string') {
      strings.push(arguments[i])
    }
  }
  return strings.join(' ')
}

const result = joinStrings('hello', 12, 'world', false, null)
console.log(result)
// hello world
```

`arguments` — _массивоподобный объект_, к его элементам можно обращаться по индексу, у него есть свойство `length`, 
но `arguments` не имеет остальных методов массива, таких как `push()` или `filter()`.

☝ Лучше использовать `rest оператор ...`, ведь его можно использовать со стрелочными функциями и наслаждаться плюшками массивов

```js
const joinStrings = (...args) => args.filter((x) => typeof x === 'string').join(' ')

const result = joinStrings('hello', 12, 'world', false, null)
console.log(result)
// hello world
```
`args` это просто устоявшееся название, на деле массиву можно дать какое угодно название


## Links
- [Функции Doka](https://doka.guide/js/function/)
- [Функции Learn JS](https://learn.javascript.ru/function-basics)
- [Хранение по ссылке и по значению](https://doka.guide/js/ref-type-vs-value-type/)
- [Передача параметров по значению и по ссылке](https://metanit.com/web/javascript/3.7.php)
- [Объект arguments](https://doka.guide/js/function-arguments-object/)
