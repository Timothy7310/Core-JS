# Coding

## Задача на палиндром (слово без учета регистра)

```js
const isPalindrome = (string) => string.toLowerCase().split("").reverse().join("") === string.toLowerCase()

// Пример
console.log(isPalindrome("teNet")); // true
console.log(isPalindrome("cat")); // false
```

## Задача на палиндром (предложение без учета регистра)

```js
const isPalindrome = (string) => {
  return (
    string.toLowerCase().replaceAll(" ", "").split("").reverse().join("") ===
    string.toLowerCase().replaceAll(" ", "")
  );
};

// Пример
console.log(isPalindrome("Аргентина манит негра")); // true
console.log(isPalindrome("стране нужны паровозы нам нужен металл")); // false
```

## Функция каррирования

```js
const curry = (fn) => {
  return (...args) =>
    args.length >= fn.length
      ? fn(...args)
      : (...args2) => curry(fn)(...args, ...args2);
};

// Пример
const sum = (a, b, c) => a + b + c;

const curriedSum = curry(sum);

console.log(curriedSum(1, 2, 3)); // 6
console.log(curriedSum(1, 2)(3)); // 6
console.log(curriedSum(1)(2)(3)); // 6
```
