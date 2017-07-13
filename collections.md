# Operacje na kolekcjach

W ES5 mamy jeden typ reprezentujący kolekcje - `Array`. Razem z ES6 zyskaliśmy dwa kolejne `Set`oraz`Map`. Programowanie funkcyjne znacznie ułatwia operowanie na nich.

W przykładach będziemy korzystali z tablicy [`students`](https://codepen.io/mmotel/pen/EXrOKW), przechowującej instanceje klasy `Student`.

```js
Student = (function () {
  function Student (studentData) {
    this.id = studentData.id;
    this.firstName = studentData.firstName;
    this.lastName= studentData.lastName;
    this.age = studentData.age;
    this.eyeColor = studentData.eyeColor;
    this.isActive = studentData.isActive;
    this.classes = studentData.classes;
  }
  
  Student.prototype.toString = function () {
    return '(' + this.id + ') ' + this.firstName + ' ' + this.lastName;
  }
  
  Student.prototype.log = function () {
    console.log(this.toString());
  }
  
  return Student;
})();
```

Tak wyglądają dane naszych studentów.

```js
const students = [
  new Student({
    "id": 0,
    "firstName": "Christina",
    "lastName": "Richmond",
    "age": 19,
    "eyeColor": "green",
    "isActive": true,
    "classes": ["1A", "Art"]
  }),
  new Student({
    "id": 1,
    "firstName": "Austin",
    "lastName": "Wooten",
    "age": 21,
    "eyeColor": "blue",
    "isActive": false,
    "classes": ["1B", "Science"]
  }),
  new Student({
    "id": 2,
    "firstName": "Viola",
    "lastName": "Shelton",
    "age": 20,
    "eyeColor": "blue",
    "isActive": true,
    "classes": ["1A", "Science"]
  }),
  new Student({
    "id": 3,
    "firstName": "Marisol",
    "lastName": "Sargent",
    "age": 26,
    "eyeColor": "brown",
    "isActive": false,
    "classes": ["1B", "Music"]
  }),
  new Student({
    "id": 4,
    "firstName": "Hawkins",
    "lastName": "Everett",
    "age": 24,
    "eyeColor": "blue",
    "isActive": false,
    "classes": ["1B", "Art", "Music"]
  })
];
```

Pomocnicza funkcja do wyświetlania naszej tablicy studentów.

```js
function log (students) {
  students.forEach(function (student) { student.log() });
}
```

## Iteracja

Podstawową operacją na kolekcji jest iteracja. Najprostszym sposobem jest pęta `for-of`.

##### [Przykład 2.1](https://codepen.io/mmotel/pen/pwNJXo)
```js
for(let student of students) {
  student.log();
}
// -> (0) Christina Richmond
// -> (1) Austin Wooten
// -> (2) Viola Shelton
// -> (3) Marisol Sargent
// -> (4) Hawkins Everett
```

Korzystając z metody `Array.forEach()` możemy skupić się na tym co chcemy zrobić z elementami tablicy, a nie na tym jak po niej iterować.

##### [Przykład 2.2](https://codepen.io/mmotel/pen/ZyBbqy)
```js
students.forEach(student => {
  student.log();
});
// -> (0) Christina Richmond
// -> (1) Austin Wooten
// -> (2) Viola Shelton
// -> (3) Marisol Sargent
// -> (4) Hawkins Everett
```

Metoda `Array.forEach()` przekazuje do funkcji obsługującej trzy parametry: element tablicy, jego indeks oraz całą tablicę.

##### [Przykład 2.3](https://codepen.io/mmotel/pen/NgbGEx)
```js
students.forEach( (student, index, students) => {
  console.log(`Student ${index+1}/${students.length}: ${student}`);
});
// -> Student 1/5: (0) Christina Richmond
// -> Student 2/5: (1) Austin Wooten
// -> Student 3/5: (2) Viola Shelton
// -> Student 4/5: (3) Marisol Sargent
// -> Student 5/5: (4) Hawkins Everett

```

---

###### Źródła

* https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/forEach