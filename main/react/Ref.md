# Ref

## What is the difference between refs and state variables?
Как `refs`, так и `state` предоставляют способ сохранения значений между рендерами; 
однако только обновление `state` запускают повторный рендеринг.

Хотя `refs` традиционно использовались (и используются до сих пор) для прямого доступа к элементам DOM   
(например при использовании **Неконтролируемых компонентов** формы),  
становится все более распространенным использование ссылок   
в функциональных компонентах для сохранения значений между рендерингами,   
которые не должны вызывать повторный рендеринг при обновлении значения.

По этой причине нет особых причин использовать `refs` в компонентах класса,   
потому что более естественно хранить эти значения в полях,   
которые принадлежат экземпляру класса и будут сохраняться между рендерами независимо.

📌 Оба значения сохраняются между рендерингами, но только `state` вызывают повторный рендеринг компонента

## When is the best time to use refs?
Одним из способов использования ссылок является прямой доступ к элементу DOM для управления им. 

Второе применение ссылок - в функциональных компонентах, где они иногда являются хорошим выбором утилиты   
для сохранения значений между рендерингами без запуска компонента для повторного рендеринга в случае изменения значения.

Есть несколько хороших примеров использования ссылок:
+ Управление фокусом, выделение текста или воспроизведение медиаресурсами.
+ Выполнение анимаций в императивном подходе.
+ Интеграция со сторонними библиотеками, взаимодействующие с DOM.

📌 При работе с формами стоит выбирать вариант с **неконтролируемых компонентов** только в случае   
большого количества изменняемых полей (очень большого количества)


## What is the proper way to update a ref in a function component?
С помощью хука [useRef](https://ru.legacy.reactjs.org/docs/hooks-reference.html#useref)


## Links
+ [Refs и DOM](https://ru.react.js.org/docs/refs-and-the-dom.html)
+ [Неконтролируемые компоненты в React](https://habr.com/ru/articles/319520/)
