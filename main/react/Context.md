# Context

## What is the difference between the context API and prop drilling?
В React вы явно передаете данные из родительского компонента в дочерние компоненты через `props`.
Однако, если дочерний компонент, которому нужны данные, оказывается глубоко вложенным, нам приходится использовать `prop-drilling`.
`Context API` же предоставляет нам центральное хранилище данных, к которому мы можем получить доступ для использования данных из любого компонента без необходимости явно запрашивать его как `prop`.  
Пример:
```js
import React from 'react';
export const UserContext = React.createContext();
export default function App() {
  return (
    <UserContext.Provider value="Reed">
      <User />
    </UserContext.Provider>
  )
}
function User() {
  return (
    <UserContext.Consumer>
      {value => <h1>{value}</h1>} 
      {/* prints: Reed */}
    </UserContext.Consumer>
  )
}
```
Разобор кода:
1. В компоненте `<App/>` создаем **Context** с помощью `React.createContext()` и помещаем результат в переменную `UserContext`.
Его практически всегда можно экспортировать (как сделано здесь), потому что ваш компонент будет в другом файле. 
Обратим внимание, что при вызове `React.createContext()` можно передать начальное значение в **prop** `value`.
2. В компоненте `<App/>` используем `UserContext`. В данном случае `UserContext.Provider`. 
Созданный `Context` — это объект с двумя свойствами: `Provider` и `Consumer`. 
Они являются **компонентами**. Чтобы передать `value` вниз каждому компоненту `<App/>`, используем в качестве оболочки компонент поставщика (в данном случае `<User/>`).
3. В `<UserContext.Provider/>` помещаем значение, которое хотим передать вниз по иерархии компонентов. Для этого устанавливаем его равным **prop** `value`. В данном случае это имя `Reed`.
4. Используем потребительский компонент в `<User/>` `<UserContext.Consumer/>` и везде, где хотим использовать представленные в контексте данные. 
Для использования переданного вниз значения применяем `render props pattern`. 
Это просто функция, которую потребительский компонент `(consumer component)` предоставляет нам в качестве **prop**. 
Данная функция возвращает используемое в дальнейшем `value`.  

Пример с хуком:
```js
import React from 'react';
export const UserContext = React.createContext();
export default function App() {
  return (
    <UserContext.Provider value="Reed">
      <User />
    </UserContext.Provider>
  )
}
function User() {
  const value = React.useContext(UserContext);  
    
  return <h1>{value}</h1>;
}
```

Чтобы код выглядил более чистым, для того чтобы достать значения из `UserContext` можно использовать `React.useContext(UserContext)`, вместо `<UserContext.Consumer/>`

## When shouldn't you use the context API?
Так как `UserContext.Provider/>` является родительским компонентом, то при изменении его `prop` (контекста) вместе с ним `ре-рендерятся` все его дочерние компоненты.
Что также может привести к не нужным `re-render`. Это может иметь негативные последствия для производительности.

## Links
+ [Полное руководство по React Context](https://medium.com/nuances-of-programming/полное-руководство-по-react-context-2021-dc8fd08db68e)
+ [React (продвинутый)](https://www.youtube.com/live/HDajDASxn-w?feature=share&t=5160)
