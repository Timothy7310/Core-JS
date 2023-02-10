# Дескрипторы

Объекты, содержат свойства. У каждого из свойств объекта, кроме значения, есть ещё три флага конфигурации, 
которые могут принимать значения `true` или `false`. Эти флаги называются дескрипторами:

- `writable` — доступно ли свойство для записи;
- `enumerable` — является ли свойство видимым при перечислениях (например, в цикле `for..in`);
- `configurable` — доступно ли свойство для переконфигурирования. (удалить или изменить свойство)

Когда мы создаём свойство объекта «обычным способом», эти три флага устанавливаются в значение true.

## Только для чтения

Чтобы сделать свойство достпуным только для чтения нужно установить `writable` значение в `false`

```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false
});

user.name = "Pete"; // Ошибка: Невозможно изменить доступное только для чтения свойство 'name'
```

Вот тот же пример, но свойство создано __с нуля__:

```js
let user = { };

Object.defineProperty(user, "name", {
  value: "John",
  // для нового свойства необходимо явно указывать все флаги, для которых значение true
  enumerable: true,
  configurable: true
});

alert(user.name); // John
user.name = "Pete"; // Ошибка
```

## Неперечислимое свойство

```js
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

Object.defineProperty(user, "toString", {
  enumerable: false
});

// Теперь наше свойство toString пропало из цикла:
for (let key in user) alert(key); // name
```

Неперечислимые свойства также не возвращаются `Object.keys`:

```js
alert(Object.keys(user)); // name
```

## Неконфигурируемое свойство

Неконфигурируемое свойство не может быть удалено, его атрибуты не могут быть изменены.

__Обратите внимание: configurable: false не даст изменить флаги свойства, а также не даст его удалить. При этом можно изменить значение свойства.__

В коде ниже свойство `user.name` является неконфигурируемым, но мы все ещё можем изменить его значение (т.к. `writable: true`).

```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  configurable: false
});

user.name = "Pete"; // работает
delete user.name; // Ошибка
```

А здесь мы делаем `user.name` «навечно запечатанной» константой

```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false,
  configurable: false
});

// теперь невозможно изменить user.name или его флаги
// всё это не будет работать:
user.name = "Pete";
delete user.name;
Object.defineProperty(user, "name", { value: "Pete" });
```

## Links
[Learn JS](https://learn.javascript.ru/property-descriptors)
[Doka](https://doka.guide/js/descriptors/)
[MDN](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
