# Operacje na kolekcjach - redukowania

Redukcja pozwala na akumulację wartości z tablicy do pojedynczego obiektu.

![](/assets/coll_reduce.png)

Obliczmy sumę wieku studentów.

Ponownie zaczniemy od wykorzystania metody `Array.forEach()`.

##### [Przykład 2.3.1](https://codepen.io/mmotel/pen/eRoRYq)

```js
var sumOfAges = 0;

students.forEach(function (student) {
  sumOfAges += student.age;
});

console.log(sumOfAges); // -> 112
```

Uzyskaliśmy uproszczoną implementację metody `Array.reduce()`. Jako parametr przyjmuje ona funckję, do której przekazuje `akumulator` oraz kolejne wartości tablicy. Drugim opcjonalnym parametrem metody `Array.reduce()` jest wartość początkowa `akumulatora`.

##### [Przykład 2.3.2](https://codepen.io/mmotel/pen/KqNNmJ)

```js
var sumOfAges = students
  .reduce(function (acc, student) { return acc += student.age; }, 0);

console.log(sumOfAges); // -> 112
```

Wykorzystując metodę `Array.reduce()` możemy łatwo odnaleźć najwyższą wartość wieku studentów.

##### [Przykład 2.3.3](https://codepen.io/mmotel/pen/OgGgVZ)

```js
var maxAge = students
  .reduce(
    function (acc, student) {
      return acc && acc >= student.age ? acc : student.age; 
    },
    null
  );

console.log(maxAge);// -> 26
```

Możemy również przekształcić tablicę w obiekt pełniący rolę słownika. 

##### [Przykład 2.3.4](https://codepen.io/mmotel/pen/OgbbvE)

```js
var studentsDict = students
  .reduce(
    function (acc, student) { 
      acc[student.id] = student; 
      return acc; 
    }, 
    {}
  );

console.log(studentsDict);
// {
//    0: Student {id: 0, firstName: "Christina", lastName: "Richmond", …},
//    1: Student {id: 1, firstName: "Austin", lastName: "Wooten", …},
//    2: Student {id: 2, firstName: "Viola", lastName: "Shelton", …},
//    3: Student {id: 3, firstName: "Marisol", lastName: "Sargent", …},
//    4: Student {id: 4, firstName: "Hawkins", lastName: "Everett", …}
// }
```


## Połączenie mapowania oraz redukowania

Operacja `map-reduce` jest popularna w świenie baz danych, np. `Mongo`. Dzięki metodom `Array.map()` oraz `Array.reduce()` możemy ją wykonać na tablicy studentów.

Wyciągnijmy z listy studentów listę klas do których uczęszczają.

Rozpoczniemy od wersji z wykorzystaniem `Array.forEach()`.

##### [Przykład 2.3.5](https://codepen.io/mmotel/pen/mwYJOe)

```js
var allClasses = [],
    uniqueClasses;

students.forEach(function (student) {
  student.classes.forEach(function (c) {
    allClasses.push(c);
  });
});

console.log(allClasses);
// -> ["1A", "Art", "1B", "Science", "1A", "Science", 
//     "1B", "Music", "1B", "Art", "Music"]

uniqueClasses = _.uniq(allClasses);

console.log(uniqueClasses);
// -> ["1A", "Art", "1B", "Science", "Music"]
```

O wiele prościej będzie wykorzystać `Array.map()` oraz `Array.reduce()`.

##### [Przykład 2.3.6](https://codepen.io/mmotel/pen/PjvqKb)

```js
var allClasses,
    uniqueClasses;

allClasses = students
  .map(function (student) { return student.classes; })
  .reduce(function (acc, classes) { return acc.concat(classes); }, []);

console.log(allClasses);
// -> ["1A", "Art", "1B", "Science", "1A", "Science", 
//     "1B", "Music", "1B", "Art", "Music"]

uniqueClasses = _.uniq(allClasses);

console.log(uniqueClasses);
// -> ["1A", "Art", "1B", "Science", "Music"]
```

## Połączenie mapowania, filtrowania oraz redukowania

Możemy również wykorzystać wszystkie trzy metody - `Array.map()`, `Array.filter()` oraz `Array.reduce()` - jednocześnie wykorzystując tym samym pełną moc jaką daje nam przetwarzanie potokowe.

Wyciągnijmy z tablicy studentów napis zawierający imiona i nazwiska dorosłych studentów.

Implementacja z wykorzystaniem `Array.forEach()` nie jest zbyt skomplikowana.

##### [Przykład 2.3.7](https://codepen.io/mmotel/pen/owRXpN)

```js
var adultsNames = '';

students.forEach(function (student) {
  if (student.age >= 21) {
    adultsNames += student.firstName + ' ' + student.lastName + ', ';
  }
});

console.log(adultsNames);
// -> 'Austin Wooten, Marisol Sargent, Hawkins Everett, '
```

Jednak wersja z `Array.map()`, `Array.filter()` oraz `Array.reduce()` jest znacznie łatwiejsza w utrzymaniu.

##### [Przykład 2.3.8](https://codepen.io/mmotel/pen/jwoPZZ)

```js
var adultsNames = students
  .filter(function (student) { 
    return student.age >= 21; 
  })
  .map(function (student) { 
    return student.firstName + ' ' + student.lastName; 
  })
  .reduce(function (acc, name) { 
    return acc ? acc + ', ' + name : name; 
  }, '');

console.log(adultsNames);
// -> 'Austin Wooten, Marisol Sargent, Hawkins Everett'
```

## Uniwersalność redukcji

`Array.reduce()` pozwala na implementację dowolnej innej metody operującej na tablicach, w tym również `Array.map()` czy `Array.filter()`.

Zaimplementujmy mapowanie poprzez redukcję.

##### [Przykład 2.3.9](https://codepen.io/mmotel/pen/ZyNGRq)

```js
var studentAges;

function map (array, mapper) {
  return array.reduce(function (acc, item) { 
    return acc.concat([ mapper(item) ]); 
  }, 
  []);
}

studentAges = map(students, function (student) { 
  return student.age; 
});

console.log(studentAges);
// -> [19, 21, 20, 26, 24]
```

Analogicznie możemy zaimplementować filtrowanie.

##### [Przykład 2.3.10](https://codepen.io/mmotel/pen/LLoVJE)

```js
var adultStudents;

function filter (array, condition) {
  return array.reduce(
    function (acc, item) { 
      return condition(item) ? acc.concat([item]) : acc 
    }, 
    []
  );
}

adultStudents = filter(students, function (student) { 
  return student.age >= 21; 
});

log(adultStudents);
// -> (1) Austin Wooten
// -> (3) Marisol Sargent
// -> (4) Hawkins Everett
```

---

### Ćwiczenia

(2.3.1) Wyciągnij z listy studentów napis zawierający ich imiona oraz nazwiska oddzielone przecinkami.

---

##### Źródła

* [https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/Reduce](https://developer.mozilla.org/pl/docs/Web/JavaScript/Referencje/Obiekty/Array/Reduce)



