# Redux

## Enumerate base principles
📌 
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

## Benefits of Redux?

## Links
+ [Redux и все-все-все](https://hawkingbros.com/article/redux-i-vse-vse-vse)
+ [Три принципа](https://rajdee.gitbooks.io/redux-in-russian/content/docs/introduction/ThreePrinciples.html)
+ [Three Principles](https://redux.js.org/understanding/thinking-in-redux/three-principles)