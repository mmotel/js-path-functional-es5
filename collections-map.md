# Operacje na kolekcjach - mapowanie

Mapowanie pozwala przekształcić tablicę wartości na tablicę innych wartości.

![](/assets/coll_map.png)

Przekształćmy tablicę studentów na tablicę ich imion oraz nazwisk.

Zaczniemy od wykorzystania pętli `for-of`.

##### [Przykład 2.1.1](https://codepen.io/mmotel/pen/mwOeoN)
```js
let names = [];

for (let student of students) {
  names.push(`${student.firstName} ${student.lastName}`);
}

console.log(names);
// -> ["Christina Richmond", "Austin Wooten", 
//     "Viola Shelton", "Marisol Sargent", "Hawkins Everett"]
```

Możemy uprościć implementację wykorzystując metodę `Array.forEach()`.

##### [Przykład 2.1.2](https://codepen.io/mmotel/pen/VWmeEv)
```js
let names = [];

students.forEach(student => {
  names.push(`${student.firstName} ${student.lastName}`);
});

console.log(names);
// -> ["Christina Richmond", "Austin Wooten", 
//     "Viola Shelton", "Marisol Sargent", "Hawkins Everett"]
```

To co udało nam się osiągnąć jest uproszczoną iplementacją metody `Array.map()`. Przyjmuje ona jako argument funckję, która wykona mapowanie wartości.

##### [Przykład 2.1.3](https://codepen.io/mmotel/pen/rwWxQP)
```js
let names = students
  .map(student => `${student.firstName} ${student.lastName}`);

console.log(names);
// -> ["Christina Richmond", "Austin Wooten", 
//     "Viola Shelton", "Marisol Sargent", "Hawkins Everett"]
```

Korzystając z `Array.map()` oraz zbiorów (`Set`) możemy łatwo i szybko wyciągnąć z tablicy zbiór występujących wartości danego pola.

Wyciągnijmy zbiór kolorów oczy studentów.

##### [Przykład 2.1.4](https://codepen.io/mmotel/pen/pwNgBO)
```js
let eyeColors = students.map(student => student.eyeColor);

console.log(eyeColors);
// -> ["green", "blue", "blue", "brown", "blue"]

let uniqueEyeColors = new Set(eyeColors);

console.log(uniqueEyeColors);
// -> Set {"green", "blue", "brown"}
```

---

### Ćwiczenia

(2.1.1) Wyciągnij z tablicy studentów uniklane wartości ich wieku.

---

###### Źródła

* https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/map
