# ECMAScript Intermediate

## Function default parameters
[link](https://github.com/Timothy7310/Core-JS/blob/main/main/core-js/Functions-Parameters.md#know-how-to-define-function-parameters)

## Know how to use spread operator for Function arguments
[link](https://github.com/Timothy7310/Core-JS/blob/main/main/core-js/Functions-Parameters.md#know-how-to-handle-dynamic-amount-of-function-parameters)

## Be able to compare `arguments` and `rest parameters`
[link](https://github.com/Timothy7310/Core-JS/blob/main/main/core-js/Functions-Parameters.md#know-how-to-handle-dynamic-amount-of-function-parameters)

## Spread operator for Array
Спред-синтаксис (spread) `...` позволяет передавать итерируемые коллекции (например, **массивы** или **строки**) 
как список аргументов функции или добавлять содержащиеся в них элементы в новый массив.

```js
function multiplyThreeNumbers(a, b, c) {
  return a * b * c
}

const nums = [1, 2, 3]

console.log(multiplyThreeNumbers(...nums))
// 6
```

## Understand and able to use spread operator for Array concatenation
Cкопировать элементы из другого массива в новый:
```js
const donor = ['это', 'старые', 'значения']
const newArray = [...donor, 1, true, 'мама']

console.log(newArray)
// ['это', 'старые', 'значения', 1, true, 'мама']
```

💡Cкопировать свойства из другого объекта в новый:
```js
const persona = { name: 'Иван', lastName: 'Объектов'}
const userData = { ...persona, username: 'killer3000' }

console.log(userData)
// {
//    name: 'Иван',
//    lastName: 'Объектов',
//    username: 'killer3000'
// }
```

📌При использовании спред-синтаксиса элементы массива копируются только _на один уровень вложенности_. 
Если массив содержит объекты, то это будут те же самые объекты, что и в исходном массиве. Для **глубокого копирования** пользуйтесь библиотеками, например, `lodash`

Пример поведения с уровнем вложенности больше одного:

```js
const users = [{ name: 'Иван', lastName: 'Объектов' }]
const copyUsers = [...users]

copyUsers[0].name = 'Николай'
console.log(users[0])
// { name: 'Николай', lastName: 'Объектов' }
```

## Destructuring assignment
Вот пример деструктуризации массива на переменные:

```js
// у нас есть массив с именем и фамилией
let arr = ["Ilya", "Kantor"];

// деструктурирующее присваивание
// записывает firstName = arr[0]
// и surname = arr[1]
let [firstName, surname] = arr;

alert(firstName); // Ilya
alert(surname);  // Kantor
```

Отлично смотрится в сочетании со `split` или другими методами, возвращающими массив:

```js
let [firstName, surname] = "Ilya Kantor".split(' ');
alert(firstName); // Ilya
alert(surname);  // Kantor
```

💡**Нежелательные** элементы массива также могут быть отброшены с помощью дополнительной запятой:

```js
// второй элемент не нужен
let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

alert( title ); // Consul
```

В примере выше второй элемент массива пропускается, а третий присваивается переменной `title`, 
оставшиеся элементы массива также пропускаются (так как для них нет переменных).

💡Работает с **любым перебираемым объектом** с правой стороны

```js
let [a, b, c] = "abc";
let [one, two, three] = new Set([1, 2, 3]);
```

💡Мы можем использовать что угодно «присваивающее» с левой стороны.
Например, можно присвоить свойству объекта:

```js
let user = {};
[user.name, user.surname] = "Ilya Kantor".split(' ');

alert(user.name); // Ilya
alert(user.surname); // Kantor
```

💡Трюк обмена переменных

```js
let guest = "Jane";
let admin = "Pete";

// Давайте поменяем местами значения: сделаем guest = "Pete", а admin = "Jane"
[guest, admin] = [admin, guest];

alert(`${guest} ${admin}`); // Pete Jane (успешно заменено!)
```

💡 Использование с rest оператором

```js
let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

// rest это массив элементов, начиная с 3-го
alert(rest[0]); // Consul
alert(rest[1]); // of the Roman Republic
alert(rest.length); // 2
```

Если мы хотим, чтобы значение «по умолчанию» заменило отсутствующее, мы можем указать его с помощью `=`:

```js
// значения по умолчанию
let [name = "Guest", surname = "Anonymous"] = ["Julius"];

alert(name);    // Julius (из массива)
alert(surname); // Anonymous (значение по умолчанию)
```

Деструктуризация объектов

```js
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

let {title, width, height} = options;

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
```

## Be able to discover destructuring assignment concept
Это круто😎 чаще всего используется чтобы у нас не было магических переменых по типу `items[2]`

## Understand variables and Function arguments destructuring assignment
Мы можем передать параметры как объект, и функция немедленно деструктурирует его в переменные:

```js
// мы передаём объект в функцию
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

// ...и она немедленно извлекает свойства в переменные
function showMenu({title = "Untitled", width = 200, height = 100, items = []}) {
  // title, items – взято из options,
  // width, height – используются значения по умолчанию
  alert( `${title} ${width} ${height}` ); // My Menu 200 100
  alert( items ); // Item1, Item2
}

showMenu(options);
```

Мы также можем использовать более сложное деструктурирование с вложенными объектами и двоеточием:

```js
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

function showMenu({
  title = "Untitled",
  width: w = 100,  // width присваиваем в w
  height: h = 200, // height присваиваем в h
  items: [item1, item2] // первый элемент items присваивается в item1, второй в item2
}) {
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}

showMenu(options);
```

## Links
- [Спред-синтаксис ...](https://doka.guide/js/spread/)
- [Деструктурирующее присваивание](https://learn.javascript.ru/destructuring-assignment)
