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
