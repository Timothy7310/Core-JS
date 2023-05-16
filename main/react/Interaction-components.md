# Interaction between components

## How do you pass a value from parent to child?
Наиболее простой и часто встречающийся случай - это случай, когда **дочерний компонент принимает данные от родителя** через пропсы.

```js
const Parent = () => {
  const [value, setValue] = useState('')

  const handleChange = (event) => {
    setValue(event.target.value)
  }

  return (
    <div>
       <input
         type="text"
         onChange={handleChange }
       />
      {/* передаем проп в дочерний компонент */}
      <Child value={value} />
    </div>
  )
}

const Child = ({ value }) => {
  return (
    <span>Value is: {value || '<Not set>'}</span>
  )
}
```

## How do you pass a value from child to parent?
Если необходимо передать данные от дочернего реакт компонента к родительскому,  
используются функции обратного вызова `(callback-функции)`.

```js
const Child = ({ onChange }) => {
  const handleChange = (event) => {
    onChange(event.target.value) // callback-функция
  }

  return (
    <input
      type="text"
      onChange={handleChange}
    />
  )
}

const Parent = () => {
  const [value, setValue] = useState('')
  const handleChange = (value) => {
    setValue(value)
  }

  return (
    <div>
      <span>Value is: {value || '<Not set>'}</span>
      <Child onChange={handleChange} />
    </div>
  )
}
```

## How do you pass a value from sibling to sibling?
Данные между соседними компонентами, т.е. между компонентами на одном уровне, можно передать через общий предок.  
Обычно данные от одного Реакт компонента передаются вверх, в компонент-предок, через `callback-функцию`,  
а компонент-предок передает их в другой компонент через `проп`.

```js
const Parent = () => {
  const [value, setValue] = useState('')

  const handleChange = (value) => {
    setValue(value)
  }

  return (
    <div>
      <Sibling1 onChange={handleChange} />
      <Sibling2 value={value} />
    </div>
  )
}

const Sibling1 = ({ onChange }) => {
  const handleChange = (event) => {
    onChange(event.target.value)
  }

  return (
    <input
      type="text"
      onChange={handleChange}
    />
  )
}

const Sibling2 = ({ value}) => {
  return (
    <span>Value is: {value || '<Not set>'}</span>
  )
}
```

## What is prop drilling?
`Prop drilling` это когда создаем **цепочку** `props` например от компонента `<App />` до условного компонента `<AwesomeButton />`.   
В следвствие чего мы прокидываем `props` через компоненты которые не изпользуются данные `props`, а нужны только для того чтобы прокинуть `props` от `<App />` до `<AwesomeButton />`.   
Из-за этого происходит ненужный `re-render` компонентов, которые не используют данные `props`, а просто их прокидывают, также это введет к разрастания кода и трудной читаемости `props`.

## Can a child component modify its own props?
**НЕТ**. Компонент может обновлять свой `state`, но не может изменять свои `props`.  
`Props` указываются сверху вниз и не могут быть **изменены**.  
**Компонент** не может **изменять свои собственные** `props`, но отвечает за установку `props` дочерних компонентов. 

## Links
+ [Как передавать данные между компонентами в ReactJS](https://it-dev-journal.ru/articles/kak-peredavat-dannye-mezhdu-komponentami-v-react-js)
+ [Как ПРАВИЛЬНО передать данные между компонентами React?](https://www.youtube.com/watch?v=Cc2n4EOUzW4)
+ [React (продвинутый)`Props drilling`](https://www.youtube.com/live/HDajDASxn-w?feature=share&t=4980)
+ [Prop Drilling](https://kentcdodds.com/blog/prop-drilling)
