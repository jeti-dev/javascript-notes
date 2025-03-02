# RXJS

## Observable vs Subject

```js
import { Observable } from "rxjs";

const observable = new Observable((subscriber) => {
  console.log("New subscription started!");
  subscriber.next(Math.random()); // Each subscriber gets a different value = unicast
});

observable.subscribe((value) => console.log("Subscriber 1:", value));
observable.subscribe((value) => console.log("Subscriber 2:", value));

import { Subject } from "rxjs";

const subject = new Subject();

subject.subscribe((value) => console.log("Subscriber 1:", value));
subject.subscribe((value) => console.log("Subscriber 2:", value));

subject.next(Math.random()); // Emits the same value to all subscribers = multicast
```

## switchMap

- An order arrives every 1 sec.
- The chef gets this order and starts cooking. The cooking takes 2 seconds.
- `switchMap` works in a way that when a new order arrives, the chef will cancel the current order and start to cook the new one.
- Because the orders come every 1 sec and the chef cooks for 2 sec, the order will never be served!

```js
import { delay, of, Subject, switchMap, tap } from "rxjs";

const order$$ = new Subject<number>();
const order$ = order$$.asObservable();

let orderIndex = 0;
setInterval(() => {
  orderIndex += 1;

  console.log("- sending order: ", orderIndex);
  order$$.next(orderIndex);
}, 1000);

function chef(i: number) {
  console.log("-- cooking: ", i);

  return of(i).pipe(delay(2000));
}

order$
  .pipe(switchMap((order) => chef(order)))
  .subscribe((v) => console.log("--- serving order: ", v));

// - sending order:  1
// -- cooking:  1
// - sending order:  2
// -- cooking:  2
// - sending order:  3
// -- cooking:  3
```

## concatMap

- A new order arrives every 1 sec.
- The chef gets this order and starts cooking for 2 sec.
- When a new order arrives while the chef is cooking, the chef keeps cooking until finished.
- The cooked order is served.
- The chef now moves to cook the next order.
- As the orders arrive every 1 sec and the chef cooks for 2 sec, there will be many orders waiting to be cooked.

```js
import { concatMap, delay, of, Subject, switchMap, tap } from "rxjs";

const order$$ = new Subject<number>();
const order$ = order$$.asObservable();

let orderIndex = 0;
setInterval(() => {
  orderIndex += 1;

  console.log("- sending order: ", orderIndex);
  order$$.next(orderIndex);
}, 1000);

function cook(i: number) {
  console.log("-- cooking: ", i);

  return of(i).pipe(delay(2000));
}

order$
  .pipe(concatMap((order) => cook(order)))
  .subscribe((v) => console.log("--- serving order: ", v));


// - sending order:  1
// -- cooking:  1
// - sending order:  2
// --- serving order:  1
// -- cooking:  2
// - sending order:  3
// - sending order:  4
// --- serving order:  2
// -- cooking:  3
// - sending order:  5
// - sending order:  6
// --- serving order:  3
```
