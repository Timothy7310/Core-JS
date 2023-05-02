
# JSX

## What is it?
`JSX` - это синтаксический сахар, чтобы было удобнее писать разметку `React` компонентов.

Пример компонента `React` с помощью `JSX`

```js
const HiWorld = () => (
	<div>
    	<h1 className="title"> Hello </h1>
    	<span>Awesome text</span>
    </div>
)
```

Пример компонента `React` без `JSX`

```js
const HiWorld = () =>
  React.createElement(
    'div',
    null,
    React.createElement(
      'h1',
      {
        className: 'title',
      },
      ' Hello '
    ),
    React.createElement('span', null, 'Awesome text')
);
```

## Is it possible to use React without JSX?

Да, но не удобно

## Особенности

### Интерполяция
Происходит с помощью `{}`

```js
const vdom1 = <div>Hello, {name}</div>;
const vdom2 = <div>Hello, {name.repeat(3)}</div>;
const vdom3 = <div className={cname}>Hello!</div>;
```

### Услованя отрисовка

В реальной практике возникают ситуации, когда наличие того или иного компонента в DOM зависит от `некоторых условий`.   
Например, если в компонент `не передали текст`, то и не надо выводить соответствующий кусок. Пример:

```js
const vdom = (
  <div>
    {text ? <h1>{text}</h1> : null}
    <Hello />
  </div>
);
```

То есть `null` – это допустимое значение, которое рассматривается `React` как пустой компонент. Точно также интерпретируются `false`, `true` и `undefined`. Поэтому пример выше можно переписать ещё короче:

```js
const vdom = (
  <div>
    {text && <h1>{text}</h1>}
    <Hello />
  </div>
);
```

### Стили

Совсем по другому работает атрибут style. Если в HTML это обычная строка, то в JSX только объект.

```js
const TestComp = () => {
  const divStyle = {
    color: 'blue',
    fontSize: '50px',
  };

  return <div style={divStyle}>Hello World!</div>;
};
```
Для консистентности с именами атрибутов, имена CSS-свойств также должны быть записаны в стиле *camelCase*.

### Значение свойств по умолчанию
Если свойство передаётся в компонент без значения, то оно автоматически становится равным `true`.  
Примеры ниже эквивалентны:

```js
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```
