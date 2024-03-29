# Redux

## Enumerate base principles
1. Единственный источник истины (одно хранилище, которое управляет состоянием всего приложения); 
2. Состояние только для чтения (единственный способ обновить состояние – отправить action); 
3. Изменения вносятся с помощью чистых функций (reducers). 

### Единственный источник правды
**`Состояние` всего вашего приложения сохранено в дереве объектов внутри одного `стора`.**

Это облегчает создание универсальных приложений. Состояние на сервере может быть сериализировано и отправлено на клиент без дополнительных усилий. Это упрощает отладку приложения, когда мы имеем дело с единственным деревом состояния. Вы также можете сохранять состояние вашего приложения для ускорения процесса разработки. И с единственным деревом состояния вы получаете функциональность типа Undo/Redo из коробки.

```js
console.log(store.getState())

{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
```

### Состояние только для чтения
**Единственный способ изменить состояние — это применить `экшен` — объект, который описывает, что случится.**

Это гарантирует, что представления или функции, реагирующие на события сети (network callbacks), никогда не изменят состояние напрямую. Поскольку все изменения централизованы и применяются последовательно в строгом порядке, поэтому нет необходимости следить за "гонкой состояний". Экшены — это всего лишь простые объекты, поэтому они могут быть залогированы, сериализированы, сохранены и затем воспроизведены для отладки или тестирования.

```js
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
})

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

### Мутации написаны, как чистые функции
**Для определения того, как дерево состояния будет трансформировано экшенами, вы пишете чистые `редюсеры`.**

Редюсеры — это просто чистые функции, которые берут предыдущее состояние и экшен и возвращают новое состояние. Не забывайте возвращать новый объект состояния вместо того, чтобы изменять предыдущее. Вы можете начать с одного редюсера, но в дальнейшем, когда ваше приложение разрастется, вы можете разделить его на более маленькие редюсеры, которые управляют отдельными частями дерева состояния. Поскольку редюсеры — это просто функции, вы можете контролировать порядок, в котором они вызываются, отправлять дополнительные данные или даже писать переиспользуемые редюсеры для общих задач, например, для пагинации.

```js
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}

import { combineReducers, createStore } from 'redux'
let reducer = combineReducers({ visibilityFilter, todos })
let store = createStore(reducer)
```

## What is the typical flow of data in a React + Redux app?
Архитектура Redux вращается вокруг строго **однонаправленного потока данных**.

Это значит, что все данные в приложении следуют одному паттерну жизненного цикла, делая логику вашего приложения более предсказуемой и легкой для понимания. Также это способствует большей упорядоченности данных (data normalization), так что в конечном итоге у вас не будет нескольких изолированных копий одних и тех же данных, которые ничего не знают друг о друге.

Жизненный цикл данных в любом Redux-приложении включает в себя **4 шага**:
### 1. Вы вызываете `store.dispatch(action)`.
Экшен — это простой javascript-объект, который описывает что случилось. Например:
```js
{ type: 'LIKE_ARTICLE', articleId: 42 }
{ type: 'FETCH_USER_SUCCESS', response: { id: 3, name: 'Mary' } }
{ type: 'ADD_TODO', text: 'Read the Redux docs.' }
```

Думайте о экшене, как об очень коротком фрагменте новостей. "Мэри залайкала статью 42" или "'Прочитать документацию Redux' было добавлено в todo-список".

Вы можете вызвать `store.dispatch(action)` из любого места Вашего приложения, включая компоненты и XHR-колбеки или даже с запланированными интервалами.

### 2. Redux-стор вызывает функцию-редюсер, который вы ему передали.
Стор передаст два аргумента при вызове редюсера: текущее дерево состояния (current state tree) и экшен (action). Например, в todo-приложении главный редюсер может принимать что-то такое:

```js
// Текущее состояние приложения (список дел и выбранный фильтр)
let previousState = {
 visibleTodoFilter: 'SHOW_ALL',
 todos: [ 
   {
     text: 'Read the docs.',
     complete: false
   }
 ]
}

// Выполнение экшена (добавление дела)
let action = {
 type: 'ADD_TODO',
 text: 'Understand the flow.'
}

// Ваш редюсер возвращает следующее состояние приложения
let nextState = todoApp(previousState, action)
```

Обратите внимание на то, что редюсер — это чистая функция. Он только вычисляет следующее состояние. Он должен быть совершенно предсказуемым: тип возвращаемых данных не должен меняться, если на вход подаются данные одного типа. Он не должен совершать никаких сайд-эффектов, таких как обращение к API или маршрутизация по приложению. Все это должно происходить только после того, как экшен будет совершен.

### 3. Главный редюсер может комбинировать результат работы нескольких редюсеров в единственное дерево состояния приложения.
Каким образом вы будете структурировать главный редюсер, зависит только от Вас. Redux поставляется с хелпером `combineReducers()`, полезным для "разделения" главного редюсера на отдельные функции, которые управляют отдельными ветвями дерева состояния.

`combineReducers()` работает следующим образом. Допустим, у вас есть два редюсера: один для списка todo-дел, второй — для выбранного сейчас режима отображения этого списка:

```js
function todos(state = [], action) {
 // как-то вычисляет nextState...
 return nextState
}

function visibleTodoFilter(state = 'SHOW_ALL', action) {
 // как-то вычисляет nextState...
 return nextState
}

let todoApp = combineReducers({
 todos,
 visibleTodoFilter
})
```

Когда вы инициируете экшен, `todoApp`, которое вернул `combineReducers`, вызовет оба редюсера:

```js
let nextTodos = todos(state.todos, action)
let nextVisibleTodoFilter = visibleTodoFilter(state.visibleTodoFilter, action)
```

Затем оба набора состояний будут снова собраны в единое состояние:

```js
return {
 todos: nextTodos,
 visibleTodoFilter: nextVisibleTodoFilter
}
```

Так как `combineReducers()` — это просто удобная утилита, вы совершено не обязаны ее использовать. Вы можете написать главный редюсер самостоятельно!

### 4. Redux-стор сохраняет полное дерево состояния, которое возвращает главный редюсер.

Это новое дерево является следующим состоянием Вашего приложения! Каждый слушатель, зарегистрированный с помощью `store.subscribe(listener)`, будет вызван. Слушатели могут вызывать `store.getState()` для получения текущего состояния приложения.

Теперь UI может быть обновлен для отображения нового состояния приложения. Если вы используете такие биндинги (bindings), как React Redux, то это та точка, в которой стоит вызвать `component.setState(newState)`


Вот как этот поток данных выглядит визуально:
![схема](https://d33wubrfki0l68.cloudfront.net/01cc198232551a7e180f4e9e327b5ab22d9d14e7/b33f4/assets/images/reduxdataflowdiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

## Benefits of Redux?
Легко передовать состояния между компонентами

## Links
+ [Redux и все-все-все](https://hawkingbros.com/article/redux-i-vse-vse-vse)
+ [Три принципа](https://rajdee.gitbooks.io/redux-in-russian/content/docs/introduction/ThreePrinciples.html)
+ [Three Principles](https://redux.js.org/understanding/thinking-in-redux/three-principles)
+ [Поток данных (Data Flow)](https://rajdee.gitbooks.io/redux-in-russian/content/docs/basics/DataFlow.html)
+ [Redux Fundamentals, Part 2: Concepts and Data Flow](https://redux.js.org/tutorials/fundamentals/part-2-concepts-data-flow)
+ [Technical benefits of using Redux.](https://fintelics.medium.com/technical-benefits-of-using-redux-f7d345e7cc9c)
