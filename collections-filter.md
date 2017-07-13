# Operacje na kolekcjach - filtrowanie

Filtorwanie pozwala wyciągnąć z tablicy te wartości, które spełniają zadany warunek.

![](/assets/coll_filter.png)

Wyciągnijmy z listy studentów tych, którzy są aktywni.

Zaczniemy od wykorzystania pętli `for-of`.

##### [Przykład 2.2.1](https://codepen.io/mmotel/pen/NgbNpL)
```js
let activeStudents = [];

for (let student of students) {
  if (student.isActive) {
    activeStudents.push(student);
  }
}

console.log(activeStudents);
// -> (0) Christina Richmond
// -> (2) Viola Shelton
```

Ponownie uprościmy implementację wykorzystując metodę `Array.forEach()`.

##### [Przykład 2.2.2](https://codepen.io/mmotel/pen/MobyQN)
```js
let activeStudents = [];

students.forEach(student => {
  if (student.isActive) {
    activeStudents.push(student);
  }
});

log(activeStudents);
// -> (0) Christina Richmond
// -> (2) Viola Shelton
```

Uzyskaliśmy uproszczoną implementację metody `Array.filter()`. Przyjmuje ona jako parametr funckję, która sprawdza czy element spełnia zadany warunek.

##### [Przykład 2.2.3](https://codepen.io/mmotel/pen/YQpqLr)
```js
let activeStudents = students.filter(student => student.isActive);

log(activeStudents);
// -> (0) Christina Richmond
// -> (2) Viola Shelton
```

Możemy również zagnieżdżać filtrowanie aby uzyskać nieco bardziej skomplikowane zapytania.

Wyciągnijmy z listy studentów tych, którzy uczęszczają do klasy `1B`.

##### [Przykład 2.2.4](https://codepen.io/mmotel/pen/qjqZKR)
```js
let studentsFrom1B = students
  .filter(student => student.classes.filter(c => c === '1B').length > 0);

log(studentsFrom1B);
// -> (1) Austin Wooten
// -> (3) Marisol Sargent
// -> (4) Hawkins Everett
```

## Połączenie mapowania oraz filtrowania

Bardzo często przydaje się połączenie metod `Array.map()` oraz `Array.filter()`.

Wyciągnijmy z tablicy studentów imiona, które zawierają w sobie literę `n`.

Możemy to zrobić korzystając z metody `Array.forEach()`.

##### [Przykład 2.2.5](https://codepen.io/mmotel/pen/XgNKpM)
```js
let namesWithN = [];
    
students.forEach(student => {
  let name = student.firstName;
  if (name.toLowerCase().indexOf('n') > 0) {
    namesWithN.push(name);
  }
});

console.log(namesWithN);
// -> ["Christina", "Austin", "Hawkins"]
```

Jednak o wiele lepiej będzie skorzystać z metod `Array.map()` i `Array.filter()`.

##### [Przykład 2.2.6](https://codepen.io/mmotel/pen/GENqqw)
```js
let namesWithN = students
  .map(student => student.firstName)
  .filter(name => name.toLowerCase().indexOf('n') > 0);

console.log(namesWithN);
// -> ["Christina", "Austin", "Hawkins"]
```

task 

---

### Ćwiczenia

(2.2.1) Wyciągnij z listy studentów tych, którzy są dorośli. Przyjmij, że dorosłym jest student, który ma conajmniej 21 lat.

(2.2.2) Wykorzsytując kod z ćwiczenia 2.2.1 wyciągnij z listy studentów imiona i nazwiska dorosłych studentów.

---

##### Źródła

* https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/Filter