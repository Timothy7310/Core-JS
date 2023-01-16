# String methods

Все в JavaScript это объект. Строка - это примитивный тип данных, который означает, что мы не можем изменить его после создания. 
У строкового объекта есть много строковых методов. Существуют разные строковые методы, которые могут помочь нам работать со строками.

## length
Метод строки _length_ возвращает количество символов в строке, включая пустое пространство.

### Пример
 ```js
 let js = "JavaScript";
 console.log(js.length); // 10
 let firstName = "Asabeneh";
 console.log(firstName.length); // 8
 ```

## Доступ к символам в строке
Мы можем получить доступ к каждому символу в строке, используя его индекс. В программировании отсчёт начинается с 0. 
Первый индекс строки равен нулю, а последний индекс равен одному минус длина строки.

![Accessing sting by index](https://github.com/Asabeneh/30-Days-Of-JavaScript/raw/master/RU/images/string_indexes.png)

### Пример
 ```js
 let string = "JavaScript";
 let firstLetter = string[0];

 console.log(firstLetter); // J

 let secondLetter = string[1]; // a
 let thirdLetter = string[2];
 let lastLetter = string[9];

 console.log(lastLetter); // t

 // Получить последний элемент. Вариант 1
 let lastIndex = string.length - 1;

 console.log(lastIndex); // 9
 console.log(string[lastIndex]); // t
 
// Получить последний элемент. Вариант 2
console.log(string.at(-1))
 ```
 
## toUpperCase()
Этот метод изменяет строку на заглавные буквы.

### Пример
   ```js
   let string = "JavaScript";

   console.log(string.toUpperCase()); // JAVASCRIPT

   let firstName = "Asabeneh";

   console.log(firstName.toUpperCase()); // ASABENEH

   let country = "Finland";

   console.log(country.toUpperCase()); // FINLAND
   ```

## toLowerCase()
Этот метод изменяет строку на заглавные буквы

### Пример
   ```js
   let string = "JavasCript";

   console.log(string.toLowerCase()); // javascript

   let firstName = "Asabeneh";

   console.log(firstName.toLowerCase()); // asabeneh

   let country = "Finland";

   console.log(country.toLowerCase()); // finland
   ```

## substr()
Требуется два аргумента: начальный индекс и количество символов для нарезки.

### Пример
   ```js
   let string = "JavaScript";
   console.log(string.substr(4, 6)); // Script

   let country = "Finland";
   console.log(country.substr(3, 4)); // land
   ```

## substring()
Он принимает два аргумента: начальный индекс и индекс остановки, но он не включает индекс остановки.

### Пример
   ```js
   let string = "JavaScript";

   console.log(string.substring(0, 4)); // Java
   console.log(string.substring(4, 10)); // Script
   console.log(string.substring(4)); // Script

   let country = "Finland";

   console.log(country.substring(0, 3)); // Fin
   console.log(country.substring(3, 7)); // land
   console.log(country.substring(3)); // land
   ```

## split()
Метод `split()` разделяет строку в указанном месте.

### Пример
   ```js
   let string = "30 Days Of JavaScript";

   console.log(string.split()); // ["30 Days Of JavaScript"]
   console.log(string.split(" ")); // ["30", "Days", "Of", "JavaScript"]

   let firstName = "Asabeneh";

   console.log(firstName.split()); // ["Asabeneh"]
   console.log(firstName.split("")); // ["A", "s", "a", "b", "e", "n", "e", "h"]

   let countries = "Finland, Sweden, Norway, Denmark, and Iceland";

   console.log(countries.split(",")); // ["Finland", " Sweden", " Norway", " Denmark", " and Iceland"]
   console.log(countries.split(", ")); //  ["Finland", "Sweden", "Norway", "Denmark", "and Iceland"]
   ```

## trim()
Удаляет пробелы в начале или конце строки.

### Пример
   ```js
   let string = "   30 Days Of JavaScript   ";

   console.log(string);
   console.log(string.trim(" "));

   let firstName = " Asabeneh ";

   console.log(firstName);
   console.log(firstName.trim());
   ```

   ```sh
     30 Days Of JavasCript
   30 Days Of JavasCript
     Asabeneh
   Asabeneh
   ```
 *Если нужно удалять пробелы только в начале или только в конце строки, то есть похожие методы **trimStart()** и **trimEnd()**.*

## includes()
Принимает аргумент подстроки и проверяет, существует ли аргумент подстроки в строке. `includes()` возвращает логическое значение. 
Он проверяет, существует ли подстрока в строке, и возвращает true, если она существует, и false, если она не существует.

### Пример
   ```js
   let string = "30 Days Of JavaScript";

   console.log(string.includes("Days")); // true
   console.log(string.includes("days")); // false
   console.log(string.includes("Script")); // true
   console.log(string.includes("script")); // false
   console.log(string.includes("java")); // false
   console.log(string.includes("Java")); // true

   let country = "Finland";

   console.log(country.includes("fin")); // false
   console.log(country.includes("Fin")); // true
   console.log(country.includes("land")); // true
   console.log(country.includes("Land")); // false
   ```

## replace()
Принимает к параметру старую подстроку и новую подстроку.

### Пример
    ```js
    string.replace(oldsubstring, newsubstring);
    ```

    ```js
    let string = "30 Days Of JavaScript";
    console.log(string.replace("JavaScript", "Python")); // 30 Days Of Python

    let country = "Finland";
    console.log(country.replace("Fin", "Noman")); // Nomanland
    ```

## charAt()
Принимает индекс и возвращает значение по этому индексу

### Пример
    ```js
    string.charAt(index);
    ```

    ```js
    let string = "30 Days Of JavaScript";
    console.log(string.charAt(0)); // 3

    let lastIndex = string.length - 1;
    console.log(string.charAt(lastIndex)); // t
    ```

## charCodeAt()
Принимает индекс и возвращает код символа (номер ASCII) значения по этому индексу

### Пример
    ```js
    string.charCodeAt(index);
    ```

    ```js
    let string = "30 Days Of JavaScript";
    console.log(string.charCodeAt(3)); // D ASCII number is 51

    let lastIndex = string.length - 1;
    console.log(string.charCodeAt(lastIndex)); // t ASCII is 116
    ```

## indexOf()
Принимает подстроку, и если подстрока существует в строке, она возвращает первую позицию подстроки, если не существует, она возвращает -1

### Пример
    ```js
    string.indexOf(substring);
    ```

    ```js
    let string = "30 Days Of JavaScript";

    console.log(string.indexOf("D")); // 3
    console.log(string.indexOf("Days")); // 3
    console.log(string.indexOf("days")); // -1
    console.log(string.indexOf("a")); // 4
    console.log(string.indexOf("JavaScript")); // 11
    console.log(string.indexOf("Script")); //15
    console.log(string.indexOf("script")); // -1
    ```

## lastIndexOf()
Принимает подстроку, и если подстрока существует в строке, она возвращает последнюю позицию подстроки, если она не существует, она возвращает -1

### Пример
    ```js
    //syntax
    string.lastIndexOf(substring);
    ```

    ```js
    let string =
      "I love JavaScript. If you do not love JavaScript what else can you love.";

    console.log(string.lastIndexOf("love")); // 67
    console.log(string.lastIndexOf("you")); // 63
    console.log(string.lastIndexOf("JavaScript")); // 38
    ```

## concat()
Принимает множество подстрок и конкатенирует их.

### Пример
    ```js
    string.concat(substring, substring, substring);
    ```

    ```js
    let string = "30";
    console.log(string.concat("Days", "Of", "JavaScript")); // 30DaysOfJavaScript

    let country = "Fin";
    console.log(country.concat("land")); // Finland
    ```

## startsWith()
Принимает подстроку в качестве аргумента и проверяет, начинается ли строка с указанной подстроки. Возвращает логическое значение (true или false).

### Пример
    ```js
    //syntax
    string.startsWith(substring);
    ```

    ```js
    let string = "Love is the best to in this world";

    console.log(string.startsWith("Love")); // true
    console.log(string.startsWith("love")); // false
    console.log(string.startsWith("world")); // false

    let country = "Finland";

    console.log(country.startsWith("Fin")); // true
    console.log(country.startsWith("fin")); // false
    console.log(country.startsWith("land")); //  false
    ```

## endsWith()
Принимает подстроку в качестве аргумента и проверяет, заканчивается ли строка указанной подстрокой. Возвращает логическое значение (true или false).

### Пример
    ```js
    string.endsWith(substring);
    ```

    ```js
    let string = "Love is the best to in this world";

    console.log(string.endsWith("world")); // true
    console.log(string.endsWith("love")); // false
    console.log(string.endsWith("in this world")); // true

    let country = "Finland";

    console.log(country.endsWith("land")); // true
    console.log(country.endsWith("fin")); // false
    console.log(country.endsWith("Fin")); //  false
    ```

## search()
Принимает подстроку в качестве аргумента и возвращает индекс первого совпадения.

### Пример
    ```js
    string.search(substring);
    ```

    ```js
    let string =
      "I love JavaScript. If you do not love JavaScript what else can you love.";
    console.log(string.search("love")); // 2
    ```

## match()
Принимает подстроку или шаблон регулярного выражения в качестве аргумента и возвращает массив, если есть совпадение, если нет, то возвращает ноль.
Регулярное выражение начинается с / знака и заканчивается / знаком.

### Пример
    ```js
    let string = "love";
    let patternOne = /love/; //без какого-либо флага
    let patternTwo = /love/gi; // g-означает поиск по всему тексту, i - без учета регистра
    ```

    Match syntax

    ```js
    // syntax
    string.match(substring);
    ```

    ```js
    let string =
      "I love JavaScript. If you do not love JavaScript what else can you love.";
    console.log(string.match("love"));
    ```

    ```sh
    ["love", index: 2, input: "I love JavaScript. If you do not love JavaScript what else can you love.", groups: undefined]
    ```

    ```js
    let pattern = /love/gi;
    console.log(string.match(pattern)); // ["love", "love", "love"]
    ```

    Давайте извлечём числа из текста, используя регулярное выражение.

    ```js
    let txt =
      "In 2019, I run 30 Days of Python. Now, in 2020 I super exited to start this challenge";
    let regEx = /\d+/;

    // d с escape-символом означает, что d - не просто символ d, а обозначает цифру 
    // + означает одно или несколько цифр,
    // если после этого есть g, значит глобальный, ищите везде.

    console.log(txt.match(regEx)); // ["2", "0", "1", "9", "3", "0", "2", "0", "2", "0"]
    console.log(txt.match(/\d+/g)); // ["2019", "30", "2020"]
    ```

## repeat()
Принимает числовой аргумент и возвращает повторную версию строки.

### Пример
    ```js
    string.repeat(n);
    ```

    ```js
    let string = "love";
    console.log(string.repeat(10)); // lovelovelovelovelovelovelovelovelovelove
    ```
