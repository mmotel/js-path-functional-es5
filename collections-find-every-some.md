# Operacje na kolekcjach - nowe metody w ES6

Wraz z `ECMAScript 6` obiekt `Array` zyskał nowe metody:

* `Array.find()` - pozwala wyszukać pojedynczy element tablicy,

* `Array.every()` - sprawdza czy każdy element tablicy spełnia zadany warunek,

* `Array.some()` - sprawdza czy jakikolwiek element tablicy spełnia zadany warunek.


## `Array.find()`

Aby wyszukać konkretny element z tablicy w ES5 trzeba użyć metodę `Array.filter()` a następnie pobrać pierwszy element ze zwróconej tablicy.

##### [Przykład 2.4.1](https://codepen.io/mmotel/pen/BZpJXW)
```js
const id = 3;
const foundStudent = 
    students.filter(student => student.id === id)[0];

foundStudent.log(); 
// -> (3) Marisol Sargent
```

Dzięki ES6 możemy wykonać to w bardziej elegancji sposób. 

##### [Przykład 2.4.2](https://codepen.io/mmotel/pen/mwRXdP)
```js
const id = 3;
const foundStudent = students.find(student => student.id === id);

foundStudent.log(); 
// -> (3) Marisol Sargent
```

Podobnie jak `Array.filter()`, `Array.find()` przyjmuje funkcję, którą następnie wywołuje z trzema parametrami: elementem tablicy, jego indeksem oraz całą tablicą.

## `Array.every()`

Sprawdzenia czy wszystkie elementy tablicy spełniają zadany warunek można dokonać wykorzystując metodę `Array.reduce()`.

##### [Przykład 2.4.3](https://codepen.io/mmotel/pen/JJEpoQ)
```js
let areAllStudentsActive = 
    students.reduce((acc, student) => acc && student.isActive, true);

console.log(areAllStudentsActive); // -> false
```

Ponownie z pomocą przychodzi ES6 oraz metoda `Array.every()`.

##### [Przykład 2.4.4](https://codepen.io/mmotel/pen/KqaQpo)
```js
let areAllStudentsActive = 
    students.every(student => student.isActive);

console.log(areAllStudentsActive); // -> false
```

Ponownie, podobnie jak `Array.reduce()`, `Array.every()` przyjmuje funkcję, którą następnie wywołuje z trzema parametrami: elementem tablicy, jego indeksem oraz całą tablicą.

## `Array.some()`

Aby sprawdzić czy jakikolwiek element tablicy spełnia zadany warunek ponownie wykorzystamy metodę `Array.reduce()`.

##### [Przykład 2.4.5](https://codepen.io/mmotel/pen/awpqmg)
```js
let isAnyoneWithBlueEyes = students
    .filter(student => student.eyeColor === 'blue')
    .length > 0;

console.log(isAnyoneWithBlueEyes); // -> false
```

ES6 dostarcza nam metodę `Array.some()`.

##### [Przykład 2.4.6](https://codepen.io/mmotel/pen/mwRXRO)
```js
let isAnyoneWithBlueEyes = 
    students.some(student => student.eyeColor === 'blue');

console.log(isAnyoneWithBlueEyes); // -> false
```

Ponownie, podobnie jak `Array.reduce()`, `Array.every()` przyjmuje funkcję, którą następnie wywołuje z trzema parametrami: elementem tablicy, jego indeksem oraz całą tablicą.

---

###### Źródła

* https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/Find
* https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/Every
* https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/Some