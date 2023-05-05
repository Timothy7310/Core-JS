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
 
 ## What is the component lifecycle?
 
 ## How do you update lifecycle in function components?