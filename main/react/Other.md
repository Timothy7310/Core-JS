# Other

## Is it a good idea to use Math.random for keys?
Ключи помогают React идентифицировать, *какие элементы были изменены, добавлены или удалены*. Ключи должны быть заданы элементам внутри массива, чтобы предоставить элементам постоянный идентификатор:
```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```
Если у вас нет постоянных идентификаторов для отрисовываемых элементов, в крайнем случае вы можете использовать индекс элемента в качестве ключа:
```js
const todoItems = todos.map((todo, index) =>
  // Делайте подобное только в случае, если у элементов нет постоянных идентификаторов
  <li key={index}>
    {todo.text}
  </li>
);
```
📌 Не рекомендуется использовать индексы для ключей, если порядок элементов может измениться.   
Это может негативно сказаться на производительности и вызвать проблемы с состоянием компонента.
`Math.random` тем более нельзя использовать, так как он может повторяться


## What are the limitations of React?
Главный минус и в тоже время плюс React, это то что это **библиотека**.    
Из за этого нет четкой архитиктуры написания React-приложений.   
Это может привести к накладным расходам на общение и ускорить процесс обучения новичков.

## What is a higher order component?
`Хук` это функция, которая принимает компонент и возвращает новый, измененный компонент.

Хотя `Хук` связаны с React, они являются не столько функцией React, сколько шаблоном,   
вдохновленным шаблоном функционального программирования,   
называемым функциями высшего порядка, т.е. функций которая либо принимает в качестве параметров другие функция,   
либо возвращает другую функцию.

## What are uncontrolled and controlled components?
**Контролируемый компонент** - это компонент, такой как `input`, `textarea` или `select`, значение которого контролируется React.

И наоборот, **неконтролируемый компонент** управляет своим собственным состоянием - компонент не контролируется React и, следовательно, "неконтролируемый".

Представьте, что вы ставите `textarea` на страницу и начинаете печатать.
Все, что вы введете, будет сохранено в `textarea` автоматически и доступно через его `value` свойство.   
Хотя React может получить доступ к значению с помощью `ref`, React здесь не контролирует значение. Это было бы примером неконтролируемого компонента.

Чтобы получить контроль над этим компонентом в React, вам нужно будет подписаться на событие `onChange` `textareas` и обновить `state` (например, вызванную input) в ответ.

Управляемый компонент:
```js
import React, { useState } from "react";

const Controlled = () => {
  const [inputText, setInputText] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(inputText);
  };

  return (
    <form>
      <input
        type="text"
        value={inputText}
        onChange={(e) => setInputText(e.target.value)}
      />
      <button onClick={handleSubmit}>Submit</button>
    </form>
  );
};

export default Controlled;
```

Неуправляемый компонент:
```js
import React, { useRef } from "react";

const Uncontrolled = () => {
  const inputRef = useRef(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(inputRef.current.value);
  };

  return (
    <form>
      <input type="text" ref={inputRef} />
      <button onClick={handleSubmit}>Submit</button>
    </form>
  );
};

export default Uncontrolled;
```

## React optimizations

## Links
+ [keys](https://ru.react.js.org/docs/lists-and-keys.html#Ключи)
+ [keys2](https://ru.react.js.org/docs/reconciliation.html#recursing-on-children)
+ [Контролируемые и неконтролируемые компоненты в React не должны быть сложными](https://habr.com/ru/articles/502034/)
+ [Разница между контролируемыми и неконтролируемыми компонентами в React](https://digitrain.ru/articles/3041/)
