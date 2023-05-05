# Lifecycle and State

## What is the difference between props and state?
`Props` - это, по сути, параметры, с помощью которых вы инициализируете дочерний компонент.  
Эти *параметры* принадлежат родительскому компоненту и не могут обновляться получающим их дочерним компонентом.

`State`, с другой стороны, принадлежит компоненту и управляется им.

`State` всегда инициируется значением по умолчанию, и это значение может меняться в течение срока службы компонента   
в ответ на такие события, как пользовательский ввод или сетевые ответы.  
При изменении `state` компонент реагирует повторным рендерингом.

📌 `props` и `state` похожи тем, что они оба содержат информацию, которая влияет на результат рендеринга,   
 однако `props` передаются компоненту (аналогично параметрам функции),   
 тогда как управление `state` осуществляется внутри компонента (аналогично переменным, объявленным в функции).
 
## How does state in a class component differ from state in a functional component?
`State` в *классовом компоненте* принадлежит экземпляру класса `(this)`, тогда как `state` в *функциональном компоненте*   
сохраняется с помощью `React` между визуализациями и вызывается каждый раз.

В *классовом компоненте* начальный `state` задается в функции конструктора компонента,   
к которой затем обращаются или задают с помощью `this.state` и `this.setState()` соответственно.

В *функциональном компоненте* управление `state` осуществляется с помощью хука `useState`.   
`useState` принимает аргумент для своего начального состояния, и возвращает массив вида:   
```js
[value, setValue]
```
Где `value` значение `state`, а `setValue` функция для обновления `value`

Функциональный компонент:

```js
const FunctionalComponent = () => {
 const [count, setCount] = React.useState(0);

 return (
   <div>
     <p>count: {count}</p>
     <button onClick={() => setCount(count + 1)}>Click</button>
   </div>
 );
};
```

Классовый компонент:

```js
class ClassComponent extends React.Component {
 constructor(props) {
   super(props);
   this.state = {
     count: 0
   };
 }

 render() {
   return (
     <div>
       <p>count: {this.state.count} times</p>
       <button onClick={() => this.setState({ count: this.state.count + 1 })}>
         Click
       </button>
     </div>
   );
 }
}
```

## What is the component lifecycle?
Компоненты React имеют **4** отдельные фазы «жизни»:
+ Монтирование
+ Обновление
+ Размонтирование
+ Обработка ошибок

### Монтирование
Эти методы вызывают в следующем порядке, когда экземпляр компонента создаётся и добавляется в DOM:
+ [constructor()](https://ru.react.js.org/docs/react-component.html#constructor)
+ [static getDerivedStateFromProps()](https://ru.react.js.org/docs/react-component.html#static-getderivedstatefromprops)
+ [render()](https://ru.react.js.org/docs/react-component.html#render)
+ [componentDidMount()](https://ru.react.js.org/docs/react-component.html#componentdidmount)

### Обновление
Обновление может быть вызвано изменениями в свойствах или состоянии. Эти методы вызываются в следующем порядке, когда компонент повторно отрисовывается:
+ [static getDerivedStateFromProps()](https://ru.react.js.org/docs/react-component.html#static-getderivedstatefromprops)
+ [shouldComponentUpdate()](https://ru.react.js.org/docs/react-component.html#shouldcomponentupdate)
+ [render()](https://ru.react.js.org/docs/react-component.html#render)
+ [getSnapshotBeforeUpdate()](https://ru.react.js.org/docs/react-component.html#getsnapshotbeforeupdate)
+ [componentDidUpdate()](https://ru.react.js.org/docs/react-component.html#componentdidupdate)

### Размонтирование
Этот метод вызывается, когда компонент удаляется из DOM:
+ [componentWillUnmount()](https://ru.react.js.org/docs/react-component.html#componentwillunmount)

### Обработка ошибок
Этот метод вызывается при возникновении ошибки во время отрисовки, в методе жизненного цикла или в конструкторе любого дочернего компонента.
+ [static getDerivedStateFromError()](https://ru.react.js.org/docs/react-component.html#static-getderivedstatefromerror)
+ [componentDidCatch()](https://ru.react.js.org/docs/react-component.html#componentdidcatch)

![image](https://user-images.githubusercontent.com/55184984/236428782-51d7b7ad-9638-45f4-9ebc-aea5da6bb449.png)

## How do you update lifecycle in function components?
С помощью хука [useEffect](https://reactdev.ru/handbook/hooks-effect/#explanation-why-effects-run-on-each-update)

Вы можете думать о `useEffect` как об объединении компонентов `componentDidMount`, `componentDidUpdate` и `componentWillUnmount`.

## Links
+ [Understanding Functional Components vs. Class Components in React](https://www.twilio.com/blog/react-choose-functional-components)
+ [Жизненный цикл компонента-React.org](https://ru.react.js.org/docs/react-component.html#Жизненный-цикл-компонента)
+ [Жизненный цикл компонента-Tproger](https://tproger.ru/translations/react-after-learning-basics/)
