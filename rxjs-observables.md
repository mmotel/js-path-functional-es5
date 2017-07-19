# Programowanie reaktywne

Programowanie reaktywne najłatwiej opisać posługując się przykładem. Załóżmy, że mamy dwa obiekty _A_ i _B_, które są ze sobą powiązane - obiekt _B_ potrzebuje do poprawnego działania stanu obiektu _A_. Gdy stan obiektu _A_ ulega zmianie należy zaktualizować informacje, które posiada obiekt _B_.

W programowaniu interaktywnym to obiekt _A_ musi wiedzieć jakie inne obiekty od niego należą i co należy zrobić aby je poinformować o zmianie swojego stanu. Wraz z rozrastaniem się aplikacji, ilość obiektów zależnych od _A_ będzie rosła i w końcu utrzymanie stanie się bardzo trudne.

Na pomoc przychodzi programowanie reaktywne. Teraz obiekt _A_ wystawia specjalny interfejs, przez który informuje o zmianie swojego stanu. To obiekt _B_ musi skorzystać z tego interfejsu i zaktualizować swój stan. Zwiększenie ilości obiektów zależnych od _A_ nie powoduje zmian jego implementacji.

![](/assets/interactive_vs_reactive.png)

Opisana powyżej komunikacja obiektów _A_ i _B_ przez specjalny interfejs jest zastosowaniem [wzorzeca obserwatora](https://bigismall.gitbooks.io/ecmascript-design-patterns/content/obserwator.html).

## _everything is a stream_

![](/assets/data_as_stream2.png)

W przypadku aplikacji wszystkie występujące zdarzenia mogą być traktowane jako strumień zdarzeń. Użytkownik klika przycisk - zdarzenie. Kursor myszy jest umieszczany w pewnym obszarze - zdarzenie. Zmienia się czas na zegarze - zdarzenie. Taki strumień zdarzeń jest dostawcą danych.

W programowaniu reaktywnym wszyscy dostawcy danych są ujednoliceni i przedstawiani jako obserwowalny strumień. Strumień to sekwencja uporządkowanych zdarzeń zachodzących w czasie.

Aby pobrać wartość ze strumienia, trzeba go zasubskrybować. Obiekt subskrybowalny to dowolny obiekt z danymi, który implementuje wzorzec obserwatora.

W przypadku obserwowalnego strumienia zdarzeń najprościej nie myśleć o nim jak o strumieniach lecz jak o tablicach. Można na nich wykonać operacje typowe dla tablic takie jak mapowanie czy filtrowanie.

## RxJS

[RxJS](http://reactivex.io/rxjs/) \(_Reactive Extensions for JavaScript_\) jest reaktywną biblioteką, która umożliwia zgrabne łączenie kodu asynchronicznego i opartego na zdarzeniach. Zapewnia ona wysoki poziom abstrakcji i wiele operacji dających dodatkowe możliwości.

> Think of RxJS as [Lodash](https://lodash.com) for events.

Jest ona rozwijana przez _Microsoft_ ze wsparciem społeczności.

### Observable

Typ `Rx.Observable` reprezentuje obserwowalny strumień. Dzięki niemu możemy pracować z dowolnym typem danych w ten sam sposób ponieważ są one przekształcane w strumień. Typ `Rx.Observable` łączy świat programowania funkcyjnego i raektywnego.

`Observables` wypełniają brakujący element tabeli `pull-push`.

Strategia `pull` zakłada, że producent jest pasywny i zwraca dane gdy konsument o to poprosi. Producent nie wie kiedy będzie działał, pełna kontrola leży po stronie konsumenta.

Strategia `push` zakłada, że to producent jest aktywny i zwraca dane kiedy _chce_. Konsument jest stroną pasywną i jedynie reaguje na nadejście nowych informacji.![](/assets/push-pull-single-multiple.png)

##### [Przykład 3.1](https://codepen.io/mmotel/pen/owRRzV)

```js
Rx.Observable.from([1, 2, 3, 4])
   .subscribe(function (item) { 
      console.log('item: ' + item); 
   });
// -> 'item: 1'
// -> 'item: 2'
// -> 'item: 3'
// -> 'item: 4'
```

Metoda `Rx.Observable.from()` pozwala stworzyć strumień z tablicy, który emituje jej elementy jako kolejne zdarzenia. Aby pobrać elementy ze strumienia korszystamy z metody `Observable.subscribe()` i przekazujemy do niej _callback_ nazywany też _Observer-em_.

Metoda `Observable.subscribe()` przyjmuje jako parametr trzy funkcje.

##### [Przykład 3.2](https://codepen.io/mmotel/pen/LLoobq)

```js
var observer = {
   next: function (item) { console.log('next: ' + item); },
   error: function (err) { console.log('error: ' + err); },
   complete: function () { console.log(`complete`); }
};

Rx.Observable.from([1, 2, 3, 4])
   .subscribe(observer);
// -> 'next: 1'
// -> 'next: 2'
// -> 'next: 3'
// -> 'next: 4'
// -> 'complete'
```

Pierwsza z funkcji - _next_ - odpowiada za obsługę kolejnych elementów strumienia.

Druga - _error_ - za zakończenie przetwarzania spowodowane błędem.

Trzecia - _complete_ -za zakończenie przetwarzania strumienia.

Możemy podać dowolną ilość funkcji, jednak zawsze trzeba pamiętać o ich kolejności.

### Operatory

Biblioteka `RxJS` zapewnia zbiór operatorów do przetwarzania strumieni, część z nich znamy już z przetwarzania kolekcji - _map_, _filter_.

##### Mapowanie strumienia

Operator `Rx.Observable.map()` pozwala na mapowanie wartości elementów strumienia podobnie jak metoda `Array.map()`.

![](/assets/stream_map.png)

##### [Przykład 3.3](https://codepen.io/mmotel/pen/webbdz)

```js
Rx.Observable.from([1, 2, 3, 4])
   .map(function (item) { return item * item; })
   .subscribe(observer);
// -> 'next: 1'
// -> 'next: 4'
// -> 'next: 9'
// -> 'next: 16'
// -> 'complete'
```

##### Filtrowanie strumienia

Metoda `Rx.Observable.filter()` pozwala na filtrowanie elementów strumienia, podobnie jak metoda `Array.filter()`.

![](/assets/stream_filter2.png)

##### [Przykład 3.4](https://codepen.io/mmotel/pen/BZeedL)

```js
Rx.Observable.from([1, 2, 3, 4])
   .filter(function (item) { return item % 2 === 0; })
   .subscribe(observer);
// -> 'next: 2'
// -> 'next: 4'
// -> 'complete'
```

##### Operator `Rx.Observable.do()`

Pozwala na wykonanie zadanej akcji bez wypływania na elementy strumienia. Możemy go wykorzystać do wypisania wartości elementu na przykład podczas debugowania.

##### [Przykład 3.5](https://codepen.io/mmotel/pen/RgmmjN)

```js
Rx.Observable.from([1, 2, 3, 4])
   .do(function (item) { console.log('do: ' + item); })
   .subscribe(observer);
// -> 'do: 1'
// -> 'next: 1'
// -> 'do: 2'
// -> 'next: 2'
// -> 'do: 3'
// -> 'next: 3'
// -> 'do: 4'
// -> 'next: 4'
// -> 'complete'
```

##### Scalanie strumieni

Operator `Rx.Observable.merge()` pozwala scalić dwa strumienie zdarzeń w jeden, podobnie jak metoda `Array.concat()`.

![](/assets/stream_merge.png)

##### [Przykład 3.6](https://codepen.io/mmotel/pen/BZeeOW)

```js
var stream1$ = Rx.Observable.from([1, 2, 3, 4]),
    stream2$ = Rx.Observable.from(['a', 'b', 'c']);

stream1$
   .merge(stream2$)
   .subscribe(observer); 
// -> 'next: 1'
// -> 'next: 2'
// -> 'next: 3'
// -> 'next: 4'
// -> 'next: a'
// -> 'next: b'
// -> 'next: c'
// -> 'complete'
```

##### Ograniczanie strumienia

Operator `Rx.Observable.take()` pozwala ograniczyć ilość elementów pobieranych ze strumienia i jego zakończenie.

##### [Przykład 3.7](https://codepen.io/mmotel/pen/Xgwwxy)

```js
Rx.Observable.from([1, 2, 3, 4])
   .take(2)
   .subscribe(observer);
// -> 'next: 1'
// -> 'next: 2'
// -> 'complete'
```

##### Obsługa zdarzeń

Obserwowalne strumienie idealnie nadają się do obsługi asynchronicznych akcji oraz zdarzeń.

Aby stworzyć strumienień ze zdarzeń skorzystamy z metody `Rx.Observable.fromEvent()`. Przyjmuje ona dwa parametry: element oraz typ zdarzenia.

Utworzymy strumień ze zdarzeń `mousemove`, które są wyzwalane na elemencie `window`.

##### [Przykład 2.8](https://codepen.io/mmotel/pen/rwggoQ)

```js
Rx.Observable.fromEvent(window, 'mousemove')
   .subscribe(observer);
```

Jednak interesują nas jedynie te ruchy myszy, które odbywają się w kwadracie 200 na 200 pikseli w lewym górnym rogu ekranu. Skorzystamy z mapowania i filtrowania strumieni aby ograniczyć zdarzenia tylko do tego obszaru.

##### [Przykład 2.9](https://codepen.io/mmotel/pen/mwYZVW)

```js
Rx.Observable.fromEvent(window, 'mousemove')
   .map(function (event) {
      return {x: event.clientX, y: event.clientY};
   })
   .filter(function (pointer) { 
      return pointer.x < 200 && pointer.y < 200; 
   })
   .subscribe(observer);
```

##### Obsługa błędów

Zawsze musimy się liczyć, że coś może pójść nie po naszej myśli. Aby zabezpieczyć się przed błędami, które mogą pojawić się w czasie przetwarzania stumienia wykorzystamy metodę `Rx.Observable.catch()`.

##### [Przykład 2.10](https://codepen.io/mmotel/pen/KqLjeE)

```js
Rx.Observable.fromEvent(window, 'mousemove')
   .map(function (event) { 
      return {
         x: event.clientX, 
         y: event.clientY, 
         z: event.client.zIndex
      };
   })
   .catch(function (error) { 
      console.log('catched error: ' + error.message); 
      return Rx.Observable.of({x: 0, y:0, z: 0}); 
   })
   .subscribe(observer);
```

Pozwala ona na obsłużenie błędu i przekazanie do subskrybenta innej wartości lub lepszego komuniaktu o błędzie.

### Tworzenie obiektów typu `Rx.Observable`

Poznaliśmy już kilka sposobów na utworzenie strumienia. `Rx.Observable.from()` pozwala utworzyć strumień z obiektu iterowalnego. `Rx.Observable.fromEvent()` ze zdarzeń a `Rx.Observable.of()` `pojedynczej wartości. Biblioteka`RxJS\` dostarcza jeszcze wiele przydatnych metod pozwalających na tworzenie strumieni.

Przyjrzyjmy się jak samemu utworzyć strumień. Pozwala na to metoda  `Rx.Observable.create()`. Przyjmuje ona jako parametr funkcję, która jest odpowiedzialna za generowanie elementów strumienia i jego ewentualne zakończenie - nazywaną _subscribe_.

##### [Przykład 3.11](https://codepen.io/mmotel/pen/owrazQ)

```js
Rx.Observable.create(function (observer) {
   observer.next(1);
   observer.next(2);
   observer.next(3);
   observer.next(4);
   observer.complete();
})
.subscribe(observer);
```

Funkcja _subscribe_ przyjmuje jako argument obiekt _observer_, który podobnie jak omawiany wcześniej _Observer_, posiada trzy metody:

* `observer.next()` - emituje kolejny element strumienia,
* `observer.complete()` - zamyka strumień,
* `observer.error()` - zamyka strumień zwracając błąd błędem.

##### Jednoczesne wykonywanie strumieni

Podczas obsługi asynchronicznych akcji, na przykład komunikacji z serwerem, zdarza się, że chcemy jednocześnie wykonać kilka strumieni. Pozwala na to metoda `Rx.Observable.forkJoin()`, która subskrybuje kilka strumieni, agreguje ostatnie zwrócone przez nie wartości i kończy działanie kiedy wszystkie strumienie zostaną zakończone.

##### [Przykład 3.12](https://codepen.io/mmotel/pen/dRBgOw)

```js
var stream1$, stream2$;

stream1$ = Rx.Observable.create(function (observer) {
   observer.next(1);
   observer.complete();
});

stream2$ = Rx.Observable.create(function (observer) {
   setTimeout(() => {
      observer.next(2);
      observer.complete();
   }, 1000);
});

Rx.Observable
   .forkJoin(stream1$, stream2$)
   .subscribe(observer);
```

##### _Chain-owanie_ strumieni

Podczas komunikacji z serwerem często zdarza się tak, że jedno zapytanie jest zależne od drugiego - wymaga zwróconej przez nie wartości. Możemy dokonać _chain-owania_ strumieni wykorzystując metodę `Rx.Observable.flatMap()`.

##### [Przykład 3.13](https://codepen.io/mmotel/pen/pwXxeP)

```js
var stream1$, stream2$;

stream1$ = Rx.Observable.of(3);

stream2$ = function (count) { 
   return Rx.Observable
      .create(function (observer) {
         for(let i = 0; i < count; i += 1) {
            observer.next(i);
         }
         observer.complete();
      });
}

stream1$
   .flatMap(function (count) { return stream2$(count); })
   .subscribe(observer);
```

### `Observables` w akcji

##### Pobieranie danych na podstawie wartości pola

Załóżmy, że mamy pole _input_, w którym znajdzie się zapytanie. Chcemy nasłuchiwać na jego zmiany i wykonać akcję tylko gdy długość zapytania jest równa lub większa niż 3.

##### [Przykład 3.14](https://codepen.io/mmotel/pen/NgZOYb)
```js
var queryInput = document.querySelector('#query');

Rx.Observable.fromEvent(queryInput, 'input')
   .map(function (event) { return event.target.value; })
   .filter(function (value) { return value && value.length >= 3; })
   .subscribe(observer);
```

Gdy zapytanie jest odpowiednie pobieramy dane z API wykorzystując operator `Rx.Observble.ajax()`.

##### [Przykład 3.15](https://codepen.io/mmotel/pen/Zydqjx)
```js
function search (query) {
   var id = query.split('')
      .map(function (c) { return c.charCodeAt(0); })
      .reduce(function (c, acc) { return acc + c; }, 0) % 100 + 1;
   
   return Rx.Observable
      .ajax(`https://jsonplaceholder.typicode.com/posts/${id}`);
}

var queryInput = document.querySelector('#query');

Rx.Observable.fromEvent(queryInput, 'input')
   .map(function (event) { return event.target.value; })
   .filter(function (value) { return value && value.length >= 3; })
   .flatMap(function (value) { return search(value); })
   .map(function (response) { return response.response; })
   .subscribe(observer);
```

Powstaje jednak pewny problem. Kiedy szybko wpisujemy zapytanie to wysyłanych jest wiele żądań i nie wiadomo w jakiej kolejności wrócą. Gdy wpisujemy zapytanie interesuje nas wynik ostatniego z żądań, poprzednie są zbędne i warto byłoby je anulować.

##### [Przykład 3.16](https://codepen.io/mmotel/pen/xroyym)
```js
Rx.Observable.fromEvent(queryInput, 'input')
   .map(function (event) { return event.target.value; })
   .filter(function (value) { return value && value.length >= 3; })
   .switchMap(function (value) { return search(value); })
   .map(function (response) { return response.response; })
   .subscribe(observer);
```

Operator `Rx.Observable.switchMap()` zwróci nam wynik ostatniego z zapytań. Dodatkowo anuluje poprzednie żądania dzięki czemu nie będziemy niepotrzebnie obciążać serwera.

##### Obsługa kilkania

Załóżmy, że mamy przycisk _Click me!_ i chcemy reagować na kliknięcia w niego.

##### [Przykład 3.17](https://codepen.io/mmotel/pen/JJQmez)

```js
var clickMeBtn, clicks$; 

clickMeBtn = document.querySelector('#click-me');

clicks$ = Rx.Observable.fromEvent(clickMeBtn, 'click');

clicks$.subscribe(observer);
```

Jednak interesują nas tylko potrójne kliknięcia.

##### [Przykład 3.18](https://codepen.io/mmotel/pen/QgXZoM)

```js
clicks$
   .bufferCount(3)
   .subscribe(observer);
```

Operator `Rx.Observable.bufferCount()` pozwala na buforowanie określonej liczby zdarzeń i zwraca je jako tablicę.

Jednak to jeszcze nie to o co nam chodziło. Potrójne kliknięcie musi odbyć się w krótkim przedziale czasu, na przykład w przeciągu 400 milisekund.

##### [Przykład 3.19](https://codepen.io/mmotel/pen/rwEqbZ)

```js
clicks$
   .bufferWhen(function () { return clicks$.delay(400); })
   .filter(function (events) { return events.length > 3; })
   .subscribe(observer);
```

Operator `Rx.Observable.bufferWhen()` zbie­ra wszyst­kie klik­nię­cia aż do mo­men­tu gdy prze­ka­za­na funk­cja coś wy­emi­tu­je. Ta ro­bi to dopie­ro po 400ms po klik­nię­ciu. Dzięki temu udało nam się wykryć potrójne kliknięcia, które miały miejsce w przeciągu maksymalnie 400 milisekund.

---

###### Źródła

* _JavaScript i wzorce projektowe_, wydanie II - Simon Timms,
* _Programowanie funkcyjne z JavaScriptem. Sposoby na lepszy kod_ - Luis Atencio,
* [http://reactivex.io/learnrx/](http://reactivex.io/learnrx/)
* [https://xgrommx.github.io/rx-book/why\_rx.html](https://xgrommx.github.io/rx-book/why_rx.html)
* [http://reactivex.io/documentation/observable.html](http://reactivex.io/documentation/observable.html)
* [https://medium.com/@benlesh/learning-observable-by-building-observable](https://medium.com/@benlesh/learning-observable-by-building-observable-d5da57405d87)
* [https://typeofweb.com/2017/05/20/observable-rxjs/](https://typeofweb.com/2017/05/20/observable-rxjs/)



