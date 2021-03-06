# Reactive Programming (反應式程式設計)

### Reference Resources (參考資源)

* https://github.com/ReactiveX/rxjs

### Actual Operation (實作執行)

* https://github.com/Shyam-Chen/Frontend-Starter-Kit
* https://github.com/Shyam-Chen/Backend-Starter-Kit

***

### Table of Contents (目錄)

* [Observable (可觀察)](#observable-可觀察)
  * [Subject (主體)](#subject-主體)
    * [AsyncSubject (非同步主體)](#asyncsubject-非同步主體)
    * [BehaviorSubject (行為主體)](#behaviorsubject-行為主體)
    * [ReplaySubject (重放主體)](#replaysubject-重放主體)
    * AnonymousSubject (匿名主體)
  * ConnectableObservable (可連接的 Observable)
  * GroupedObservable (已分組的 Observable)
* [Subscription (訂閱)](#Subscription-訂閱)
  * [unsubscribe](#unsubscribe)
  * add
  * remove
* [Scheduler (調度)](#scheduler-調度)
  * [animationFrame](#animationframe)
  * [asap](#asap)
  * [async](#async)
  * [queue](#queue)
* [Creation (建立)](#creation-建立)
  * [ajax](#ajax)
  * [bindCallback](#bindcallback)
  * [bindNodeCallback](#bindnodecallback)
  * [create](#create)
  * [defer](#defer)
  * [empty](#empty)
  * [from](#from)
  * [fromEvent](#fromevent)
  * [fromEventPattern](#fromeventpattern)
  * [fromPromise](#frompromise)
  * generate
  * [interval](#interval)
  * [never](#never)
  * [of](#of)
  * [range](#range)
  * [repeat](#repeat)
  * repeatWhen
  * [throw](#throw)
  * [timer](#timer)
* [Transformation (轉化)](#transformation-轉化)
  * [buffer](#buffer)
  * [bufferCount](#buffercount)
  * [bufferTime](#buffertime)
  * bufferToggle
  * bufferWhen
  * [concatMap](#concatmap)
  * [concatMapTo](#concatmapto)
  * exhaustMap
  * expand
  * [groupBy](#groupby)
  * [map](#map)
  * [mapTo](#mapto)
  * [mergeMap](#mergemap)
  * mergeMapTo
  * mergeScan
  * pairwise
  * partition
  * pluck
  * [scan](#scan)
  * [switchMap](#switchmap)
  * switchMapTo
  * [window](#window)
  * windowCount
  * windowTime
  * windowToggle
  * windowWhen
* [Filtering (過濾)](#filtering-過濾)
  * [audit](#audit)
  * auditTime
  * [debounce](#debounce)
  * [debounceTime](#debouncetime)
  * [distinct](#distinct)
  * distinctKey
  * distinctUntilChanged
  * distinctUntilKeyChanged
  * elementAt
  * [filter](#filter)
  * [first](#first)
  * [ignoreElements](#ignoreelements)
  * [last](#last)
  * [sample](#sample)
  * sampleTime
  * [single](#single)
  * [skip](#skip)
  * skipLast
  * [skipUntil](#skipuntil)
  * [skipWhile](#skipwhile)
  * [take](#take)
  * takeLast
  * [takeUntil](#takeuntil)
  * [takeWhile](#takewhile)
  * [throttle](#throttle)
  * [throttleTime](#throttletime)
* [Combination (組合)](#combination-組合)
  * [combineAll](#combineall)
  * [combineLatest](#combinelatest)
  * [concat](#concat)
  * [concatAll](#concatall)
  * exhaust
  * [forkJoin](#forkjoin)
  * [merge](#merge)
  * [mergeAll](#mergeall)
  * [race](#race)
  * [startWith](#startwith)
  * switch/switchAll
  * [withLatestFrom](#withlatestfrom)
  * [zip](#zip)
  * zipAll
* [Multicasting (組播)](#multicasting-組播)
  * multicast
  * [publish](#publish)
  * publishBehavior
  * publishLast
  * publishReplay
  * [share](#share)
* [Error Handling (錯誤處理)](#error-handling-錯誤處理)
  * [catch/catchError](#catch)
  * [retry](#retry)
  * [retryWhen](#retrywhen)
* [Utility (公用)](#utility-公用)
  * [do/tap](#do)
  * [delay](#delay)
  * [delayWhen](#delaywhen)
  * dematerialize
  * finally/finalize
  * let
  * materialize
  * observeOn
  * subscribeOn
  * timeInterval
  * timestamp
  * timeout
  * timeoutWith
  * toArray
  * toPromise
* [Conditional and Boolean (附條件和布林值)](#conditional-and-boolean-附條件和布林值)
  * [defaultIfEmpty](#defaultifempty)
  * [every](#every)
  * [find](#find)
  * [findIndex](#findindex)
  * [isEmpty](#isempty)
* [Mathematical and Aggregate (運算和合計)](#mathematical-and-aggregate-運算和合計)
  * [count](#count)
  * [max](#max)
  * [min](#min)
  * [reduce](#reduce)

***

## Observable (可觀察)

```js
import { Observable } from 'rxjs';

new Observable(
  // an observer (一個觀察者)
  (observer) => {
    // callback method (回呼方法): next(), error(), & complete()
    setTimeout(() => observer.next('foo'), 0);
    setTimeout(() => observer.next('bar'), 1000);
    setTimeout(() => observer.next('baz'), 2000);
    setTimeout(() => observer.complete(), 3000);
  })
  .subscribe(
    // 訂閱一個或多個 Observable (可觀察的物件)
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );
  // foo
  // bar
  // baz
  // done
```

### Subject (主體)

```js
import { Subject } from 'rxjs/Subject';

const subject = new Subject();

subject.subscribe(
  value => console.log(value),
  error => console.error(error),
  () => console.log('done')
);

subject.next('foo');
subject.next('bar');
subject.next('baz');
subject.complete();
// foo
// bar
// baz
// done
```

#### AsyncSubject (非同步主體)

```js
import { Observable } from 'rxjs/Observable';
import { AsyncSubject } from 'rxjs/AsyncSubject';

import { from } from 'rxjs/observable/from';

import { delay } from 'rxjs/operator/delay';

const source$ = Observable::from([1, 2, 3])::delay(1000);
const subject = new AsyncSubject();

source$.subscribe(subject);

subject.subscribe(
  value => console.log(value),
  error => console.error(error),
  () => console.log('done')
);
// 3
// done
```

#### BehaviorSubject (行為主體)

```js
import { BehaviorSubject } from 'rxjs/BehaviorSubject';

const subject = new BehaviorSubject('foo');

subject.subscribe(
  value => console.log(value),
  error => console.error(error),
  () => console.log('done')
);

subject.next('bar');
subject.complete();
// foo
// bar
// done
```

#### ReplaySubject (重放主體)

可以是可觀察的序列，也可以是觀察者的物件。

每個通知被推播給所有訂閱和未來的觀察者，並尊從緩衝區修整的策略。

```js
import { ReplaySubject } from 'rxjs/ReplaySubject';

const subject = new ReplaySubject(2);  // 緩衝區域大小為 2

subject.next(1);
subject.next(2);
subject.next(3);

subject.subscribe(
  value => console.log(value),
  error => console.error(error),
  () => console.log('done')
);

subject.next(2);
subject.next(1);
subject.next(0);
subject.complete();
// 2
// 3
// 2
// 1
// 0
// done
```

## Subscription (訂閱)

### unsubscribe

```js
import { from } from 'rxjs/observable';

const source = from(['foo', 'bar']);
const subscription = source.subscribe(value => console.log(value));

subscription.unsubscribe();
```

## Scheduler (調度)

```js
import { Scheduler } from 'rxjs';
import { Observable } from 'rxjs/Observable';

import { observeOn } from 'rxjs/operator/observeOn';

const source$ = new Observable(observer => /* ... */);

console.log('before subscribe');

source$::observeOn(Scheduler.<animationFrame|asap|async|queue>)
  .subscribe(
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );

console.log('after subscribe');
```

### animationFrame

Advanced version of `requestAnimationFrame`
`requestAnimationFrame` 的進階版

```js
import { Observable, Scheduler } from 'rxjs';

import { observeOn } from 'rxjs/operator';

const source$ = new Observable(observer => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
});

console.log('before subscribe');

source$::observeOn(Scheduler.animationFrame)
  .subscribe(
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );

console.log('after subscribe');
// before subscribe
// after subscribe
// 1
// 2
// 3
// done
```

### asap

跟 `async` 相似，但會比 `async` 先出現

```js
import { Scheduler } from 'rxjs';
import { Observable } from 'rxjs/Observable';

import { observeOn } from 'rxjs/operator/observeOn';

const source$ = new Observable(observer => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
});

console.log('before subscribe');

source$::observeOn(Scheduler.asap)
  .subscribe(
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );

console.log('after subscribe');
// before subscribe
// after subscribe
// 1
// 2
// 3
// done
```

```js
import { Scheduler } from 'rxjs';
import { Observable } from 'rxjs/Observable';

import { range } from 'rxjs/observable/range';

import { observeOn } from 'rxjs/operator/observeOn';
import { _do } from 'rxjs/operator/do';

const timeStart = Date.now();
const source$ = Observable::range(1, 5)
  ::_do(value => console.log(`processing value ${value}`))
  ::observeOn(Scheduler.asap)

console.log('before subscribe');

source$.subscribe(
  value => console.log(`next ${value}`),
  error => console.error(error),
  () => console.log(`Total time: ${Date.now() - timeStart}ms`)
);

console.log(`after subscribe`);
// before subscribe
// processing value 1
// processing value 2
// processing value 3
// processing value 4
// processing value 5
// after subscribe
// next 1
// next 2
// next 3
// next 4
// next 5
// Total time: 92ms
```

### async

比 `asap` 還晚出現

```js
import { Scheduler } from 'rxjs';
import { Observable } from 'rxjs/Observable';

import { observeOn } from 'rxjs/operator/observeOn';

const source$ = new Observable(observer => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
});

console.log('before subscribe');

source$::observeOn(Scheduler.async)
  .subscribe(
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );

console.log('after subscribe');
// before subscribe
// after subscribe
// 1
// 2
// 3
// done
```

```js
import { Scheduler } from 'rxjs';
import { Observable } from 'rxjs/Observable';

import { range } from 'rxjs/observable/range';

import { observeOn } from 'rxjs/operator/observeOn';
import { _do } from 'rxjs/operator/do';

const timeStart = Date.now();
const source$ = Observable::range(1, 5)
  ::_do(value => console.log(`processing value ${value}`))
  ::observeOn(Scheduler.async)

console.log('before subscribe');

source$.subscribe(
  value => console.log(`next ${value}`),
  error => console.error(error),
  () => console.log(`Total time: ${Date.now() - timeStart}ms`)
);

console.log(`after subscribe`);
// before subscribe
// processing value 1
// processing value 2
// processing value 3
// processing value 4
// processing value 5
// after subscribe
// next 1
// next 2
// next 3
// next 4
// next 5
// Total time: 88ms
```

### queue

```js
import { Scheduler } from 'rxjs';
import { Observable } from 'rxjs/Observable';

import { observeOn } from 'rxjs/operator/observeOn';

const source$ = new Observable(observer => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
});

console.log('before subscribe');

source$::observeOn(Scheduler.queue)
  .subscribe(
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );

console.log('after subscribe');
// before subscribe
// 1
// 2
// 3
// done
// after subscribe
```

```js
import { Scheduler } from 'rxjs';
import { Observable } from 'rxjs/Observable';

import { range } from 'rxjs/observable/range';

import { observeOn } from 'rxjs/operator/observeOn';
import { _do } from 'rxjs/operator/do';

const timeStart = Date.now();
const source$ = Observable::range(1, 5)
  ::_do(value => console.log(`processing value ${value}`))
  ::observeOn(Scheduler.queue)

console.log('before subscribe');
source$.subscribe(
  value => console.log(`next ${value}`),
  error => console.error(error),
  () => console.log(`Total time: ${Date.now() - timeStart}ms`)
);

console.log('after subscribe');
// before subscribe
// processing value 1
// next 1
// processing value 2
// next 2
// processing value 3
// next 3
// processing value 4
// next 4
// processing value 5
// next 5
// Total time: 7ms
// after subscribe
```

## Creation (建立)

### ajax

```js
import { ajax } from 'rxjs/observable/dom/ajax';
import { map } from 'rxjs/operators';

ajax('https://web-go-demo.herokuapp.com/__/text-list')
  .map(event => event.response)
  .subscribe(data => console.log(data));
  // Array [...]
```

### bindCallback

將回呼 API 轉換成返回 Observable 函式。

```js
import { Observable } from 'rxjs/Observable';

import { bindCallback } from 'rxjs/observable/bindCallback';

const source$ = Observable::bindCallback(
  (value, callback) => callback(`Hello ${value}`)
);

source$('World')
  .subscribe(value => console.log(value));
  // Hello World
```

### bindNodeCallback

將 Node.js 風格的回呼 API 轉換成返回 Observable 函式。

```js
import { readdir } from 'fs';

import { Observable } from 'rxjs/Observable';

import { bindNodeCallback } from 'rxjs/observable/bindNodeCallback';

const source$ = Observable::bindNodeCallback(readdir);

source$(process.cwd())
  .subscribe(value => console.log(value));
  // $ ls -a
```

### create

Creates a new Observable that will execute the specified function when a Subscriber subscribes to it.

建立一個新的 Observable，當 Subscriber 訂閱時，它將執行指定的函式。

```js
import * as Rx from 'rxjs/Rx';

Rx.Observable.create(observer => {
    observer.next('Hello');
    observer.next('World');
  })
  .subscribe(value => console.log(value));
  // Hello
  // World
```

### defer

Creates an Observable that, on subscribe, calls an Observable factory to make an Observable for each new Observer.

建立一個 Observable，在 subscribe 上，調用一個 Observable 工廠為每個新的 Observer 做一個 Observable。

```js
import { Observable } from 'rxjs/Observable';

import { defer } from 'rxjs/observable/defer';
import { of } from 'rxjs/observable/of';

Observable::defer(() => Observable::of(1, 2, 3))
  .subscribe(value => console.log(value + 1));
  // 2
  // 3
  // 4
```

### empty

Creates an Observable that emits no items to the Observer and immediately emits a complete notification.

建立一個不向 Observer 發送項的 Observable，並立即發出一個完整的通知。

```js
import { Observable } from 'rxjs/Observable';

import { empty } from 'rxjs/observable/empty';

Observable::empty()  // 直接完成
  .subscribe(
    result => console.log(result, 'Next...'),
    error => console.error(error),
    () => console.log('Complete!')
  );
  // Complete!
```

### from

Creates an Observable from an Array, an array-like object, a Promise, an iterable object, or an Observable-like object.

從陣列，像陣列的物件，Promise，可迭代物件或像 Observable 的物件建立一個 Observable。

```js
import { Map } from 'immutable';

import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

const map1 = Map({ a: 1, b: 2, c: 3 });
const map2 = map1.set('b', 4);

Observable::from(map2)
  .subscribe(value => console.log(value));
  // ["a", 1]
  // ["b", 4]
  // ["c", 3]
```

### fromEvent

Creates an Observable that emits events of a specific type coming from the given event target.

建立一個 Observable，發出來自給定事件目標的特定類型的事件。

```js
import { Observable } from 'rxjs/Observable';

import { fromEvent } from 'rxjs/observable/fromEvent';

Observable::fromEvent(document, 'click')  // 點擊頁面
  .subscribe(value => console.log(value.pageX, value.pageY));
  // 打印出點擊的座標
```

```js
import { Observable } from 'rxjs/Observable';

import { fromEvent } from 'rxjs/observable';

const ele = document.querySelector('#name');  // an input field (一個輸入框)

Observable::fromEvent(ele, 'input')
  ::map(event => event.target.value)
  .subscribe(value => console.log(value));
```

### fromEventPattern

Creates an Observable from an API based on addHandler/removeHandler functions.

基於 addHandler/removeHandler 函式從 API 建立一個 Observable。

```js
import { Observable } from 'rxjs/Observable';

import { fromEvent } from 'rxjs/observable/fromEvent';

const addClickHandler = handler => document.addEventListener('click', handler);
const removeClickHandler = handler => document.removeEventListener('click', handler);

Observable::fromEventPattern(
    addClickHandler,
    removeClickHandler
  )
  .subscribe(value => console.log(value.screenX, value.screenY));
  // xxx, xxx
```

### fromPromise

將 Promise 轉化為 Observable。

```js
import { Observable } from 'rxjs/Observable';

import { fromPromise } from 'rxjs/observable/fromPromise';

const promise = new Promise(resolve => resolve('Hello World'));

Observable::fromPromise(promise)
  .subscribe(value => console.log(value));
  // Hello World
```

### interval

Creates an Observable that emits sequential numbers every specified interval of time, on a specified IScheduler.

建立一個 Observable，它在指定的 IScheduler 上每隔指定的時間間隔發出序列號。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

Observable::interval(1000)
  .subscribe(value => console.log(value));
  // 0
  // 1
  // 2
  // 3
  // ...
```

### never

Creates an Observable that emits no items to the Observer.

建立一個不向 Observer 發射任何項目的 Observable。

```js
import { Observable } from 'rxjs/Observable';

import { never } from 'rxjs/observable/never';

import { startWith } from 'rxjs/operator/startWith';

const info = () => console.log('這不會被呼叫');

Observable::never()
  ::startWith(11)
  .subscribe(value => console.log(value), info, info);
  // 11
```

### of

Creates an Observable that emits some values you specify as arguments, immediately one after the other, and then emits a complete notification.

建立一個 Observable，它發射一些所指定為參數的值，立即一個接一個的發射，然後發射一個完整的通知

```js
import { of } from 'rxjs/observable';

of('foo', 'bar', 'baz').subscribe(value => console.log(value));
// foo
// bar
// baz
```

```js
import { of } from 'rxjs/observable';

of({ a: 'A' }, [2, 'b'], () => 'C')
  .subscribe(value => console.log(value));
  // {a: "A"}
  // [2, "b"]
  // function () {
  //   return 'C';
  // }
```

### range

Creates an Observable that emits a sequence of numbers within a specified range.

建立一個 Observable，它發射指定範圍內的一系列數字。

```js
import { range } from 'rxjs/observable';

range(1, 5).subscribe(value => console.log(value));
  // 1
  // 2
  // 3
  // 4
  // 5
```

```js
import { range } from 'rxjs/observable';
import { map, count } from 'rxjs/operators';

range(1, 5)
  .pipe(
    map(value => value * 2),
    count(value => value % 3 === 0)
  )
  .subscribe(value => console.log(value));
  // 1
```

### repeat

Returns an Observable that repeats the stream of items emitted by the source Observable at most count times.

返回一個 Observable，它在大多數計數時間內重複由來源 Observable 發射的資料流

```js
import { of } from 'rxjs/observable';
import { repeat } from 'rxjs/operators';

of('foo', 'bar')
  .pipe(
    repeat(2)
  )
  .subscribe(value => console.log(value));
  // foo
  // bar
  // foo
  // bar
```

```js
import { of } from 'rxjs/observable';
import { repeat } from 'rxjs/operators';

of('foo', 'bar')
  .pipe(
    repeat(2),
    repeat(3)  // 2 * 3 = 6, equivalent to `repeat(6)`
  )
  .subscribe(value => console.log(value));
  // foo
  // bar
  // foo
  // bar
  // foo
  // bar
  // foo
  // bar
  // foo
  // bar
  // foo
  // bar
```

### throw

Creates an Observable that emits no items to the Observer and immediately emits an error notification.

建立一個 Observable，它不向 Observer 發送任何項目，並立即發出錯誤通知。

```js
import { Observable } from 'rxjs/Observable';

import { _throw } from 'rxjs/observable/throw';

Observable::_throw('This is an error!')
  .subscribe(
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );
  // This is an error!
```

### timer

Creates an Observable that starts emitting after an `initialDelay` and emits ever increasing numbers after each `period` of time thereafter.

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';

Observable::timer(1000)
  .subscribe(value => console.log(value));
  // 0
```

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';

Observable::timer(1000, 2000)
  .subscribe(value => console.log(value));
  // 一秒後打印
  // 0
  // 再來都是兩秒後打印
  // 1
  // 2
```

## Transformation (轉化)

### buffer

緩衝所有輸出值，直到被發射出去。反覆執行...

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';
import { fromEvent } from 'rxjs/observable/fromEvent';

import { buffer } from 'rxjs/operator/buffer';

Observable::interval(1000)
  ::buffer(Observable::fromEvent(document, 'click'))  // 點擊頁面
  .subscribe(value => console.log(value));
  // 發射值
  // [...]
```

### bufferCount

緩衝所有輸出值，直到指定的數字被履行，然後再發射出去。反覆執行...

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { bufferCount } from 'rxjs/operator/bufferCount';

const source$ = Observable::interval(1000);

source$::bufferCount(3)
  .subscribe(value => console.log(value));
  // [0, 1, 2]
  // 下個間隔
  // [3, 4, 5]
  // ...

source$::bufferCount(3, 1)
  .subscribe(value => console.log(value));
  // [0, 1, 2]
  // [1, 2, 3]
  // [2, 3, 4]
  // 下個間隔
  // [3, 4, 5]
  // [4, 5, 6]
  // [5, 6, 7]
  // ...
```

### bufferTime

緩衝所有輸出值，直到到達指定的時間點，然後再發射出去。反覆執行...

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { bufferTime } from 'rxjs/operator/bufferTime';

const source$ = Observable::interval(1000);

source$::bufferTime(2000)
  .subscribe(value => console.log(value));
  // [0]
  // 下個間隔
  // [1, 2]
  // 下個間隔
  // [3, 4]
  // ...

source$::bufferTime(2000, 1000)
  .subscribe(value => console.log(value));
  // [0]
  // [0, 1, 2]
  // 下個間隔
  // [1, 2, 3]
  // [2, 3, 4]
  // 下個間隔
  // [3, 4, 5]
  // [4, 5, 6]
  // ...
```

### bufferToggle

### bufferWhen

### concatMap

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { concatMap } from 'rxjs/operator/concatMap';

Observable::of('Hello', 'Goodbye')
  ::concatMap(value => Observable::of(`${value} World!`))
  .subscribe(value => console.log(value));
  // Hello World!
  // Goodbye World!
```

### concatMapTo

Projects each source value to the same Observable which is merged multiple times in a serialized fashion on the output Observable.

將每個來源值投射到在輸出的 Observable 上以序列化方式多次合併的相同的 Observable

```js
import { of } from 'rxjs/observable';
import { concatMapTo } from 'rxjs/operators';

of('INCREMENT', 'DECREMENT')
  .pipe(
    concatMapTo(
      of('Counter'),
      (outerValue, innerValue) => `[${innerValue}] ${outerValue}`
    )
  )
  .subscribe(value => console.log(value));
  // [Counter] INCREMENT
  // [Counter] DECREMENT
```

### expand

### groupBy

Groups the items emitted by an Observable according to a specified criterion, and emits these grouped items as GroupedObservables, one GroupedObservable per group.

按照指定的標準對 Observable 發出的項目進行分組，並將這些分組的項目作為 GroupedObservables 發送，每組一個 GroupedObservable

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable';

import { groupBy } from 'rxjs/operator';

const groupedData = [];

Observable::from([1, 2, 3, 4, 5, 6])
  ::groupBy(
    group => group % 2,  // group
    value => value + 2  // map
  )
  .subscribe(result => {
    const group = { key: result.key, data: [] };
    result.subscribe(value => group.data.push(value));
    groupedData.push(group);
  });

console.log(groupedData);
// [ { key: 1, data: [ 3, 5, 7 ] }, { key: 0, data: [ 4, 6, 8 ] } ]
```

### map

對來源的每個值進行應用投射。

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

import { map } from 'rxjs/operator/map';

Observable::from([1, 2, 3, 4, 5])
  ::map(value => value + 10)
  .subscribe(value => console.log(value))
  // 11
  // 12
  // 13
  // 14
  // 15

Observable::from([
    { name: 'Joe', age: 30 },
    { name: 'Frank', age: 20 },
    { name: 'Ryan', age: 50 }
  ])
  ::map(value => value.name)
  .subscribe(value => console.log(value))
  // Joe
  // Frank
  // Ryan
```

### mapTo

將發射值映射到所賦予的常數。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { mapTo } from 'rxjs/operator/mapTo';

Observable::interval(1000)
  ::mapTo('Hello World!')
  .subscribe(value => console.log(value));
  // 每秒打印 Hello World!
```

### mergeMap

將來源的值先映射到內部個觀察者，再將其合併發射出去。即，先 `map` 再 `mergeAll`。

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { mergeMap } from 'rxjs/operator/mergeMap';

Observable::of('Hello')
  ::mergeMap(value => Observable::of(`${value} World!`))
  .subscribe(value => console.log(value));
  // Hello World!
```

### partition

### pluck

### scan

Applies an accumulator function over the source Observable, and returns each intermediate result, with an optional seed value.

在源 Observable 上應用累加器函式，並返回每個中間結果，並具有可選的種子值。

```js
import { of } from 'rxjs/observable';
import { filter, map, scan } from 'rxjs/operators';

of(2, 5, 10)  // 2, 5, 10
  .pipe(
    filter(value => value % 2 === 0),  // 2, 10
    map(value => value * 2),  // 2 * 2, 10 * 2
    scan((acc, value) => acc + value, 0)  // 0 + 4, 4 + 20
  )
  .subscribe(value => console.log(value));
  // 4
  // 24
```

```js
import { Subject } from 'rxjs';

import { startWith, scan } from 'rxjs/operator';

const subject = new Subject();

subject::startWith(0)
  ::scan((acc, curr) => acc + curr)
  .subscribe(value => console.log(value));
  // 0

subject.next(1);  // 1 <= 0 + 1
subject.next(2);  // 3 <= 1 + 2
subject.next(3);  // 6 <= 3 + 3
```

累積物件

```js
import { Subject } from 'rxjs';

import { scan } from 'rxjs/operator';

const subject = new Subject();

subject::scan((acc, curr) => Object.assign({}, acc, curr), {})
  .subscribe(value => console.log(value));

subject.next({ primary: 'JavaScript' });  // { primary: 'JavaScript' }
subject.next({ accent: 'TypeScript' });  // { primary: 'JavaScript', accent: 'TypeScript' }
```

### switchMap

Projects each source value to an Observable which is merged in the output Observable, emitting values only from the most recently projected Observable.

將每個源值投射到一個 Observable，將其合併到輸出 Observable 中，僅從最近預計的 Observable 發射值。

```js
import { Observable } from 'rxjs/Observable';

import { timer, interval } from 'rxjs/observable';

import { switchMap } from 'rxjs/operator';

Observable::timer(0, 5000)
  ::switchMap(() => Observable::interval(500))
  .subscribe(value => console.log(value));
  // 0
  // 1
  // 2
  // 3
  // 4
  // 5
  // 6
  // 7
  // 8
  // 0 - 重複
  // 1
  // 2
  // ...
```

### window

Branch out the source Observable values as a nested Observable whenever `windowBoundaries` emits.

將每個 windowBoundaries 發出的源 Observable 值作為嵌套 Observable 分支。

```js
import { Observable } from 'rxjs/Observable';

import { timer, interval } from 'rxjs/observable';

import { window, scan, mergeAll } from 'rxjs/operator';

const source = Observable::timer(0, 1000);

const window$ = source::window(Observable::interval(3000))

const count = window$::scan(acc => acc + 1, 0);

count.subscribe(value => console.log(`Window ${value}:`));
window$::mergeAll().subscribe(value => console.log(value));
// Window 1:
// 0
// 1
// 2
// Window 2:
// 3
// 4
// 5
// Window 3:
// 6
// 7
// 8
// ...
```

### windowCount

### windowTime

### windowToggle

### windowWhen

## Filtering (過濾)

### audit

Ignores source values for a duration determined by another Observable, then emits the most recent value from the source Observable, then repeats this process.

忽略由另一個 Observable 確定的持續時間的源值，然後從源 Observable 發出最新值，然後重複此過程

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable';

import { take, audit } from 'rxjs/operator';

Observable::interval(1000)
  ::take(10)
  ::audit(() => Observable::interval(1500))
  .subscribe(value => console.log(value));
  // 1
  // 3
  // 5
  // 7
```

### debounce

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';
import { timer } from 'rxjs/observable/timer';

import { debounce } from 'rxjs/operator/debounce';

Observable::of(1, 2, 3)
  ::debounce(() => Observable::timer(1000))
  .subscribe(value => console.log(value));
  // 3
```

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';
import { timer } from 'rxjs/observable/timer';

import { debounce } from 'rxjs/operator/debounce';

Observable::interval(1000)
  ::debounce(value => Observable::timer(value * 200))
  .subscribe(value => console.log(value));
  // 0
  // 1
  // 2
  // 3
  // 4
```

### debounceTime

Emits a value from the source Observable only after a particular time span has passed without another source emission.

只有在特定的時間跨度已經過去之後才能從源 Observable 發射一個值，而不會有其他源發射

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable';

import { debounceTime } from 'rxjs/operator';

Observable::of(1, 2, 3)
  ::debounceTime(1000)
  .subscribe(value => console.log(value));
  // 3
```

```js
import { Observable } from 'rxjs/Observable';

import { fromEvent } from 'rxjs/observable';

import { map, debounceTime } from 'rxjs/operator';

const input = document.querySelector('#text');

Observable::fromEvent(input, 'keyup')
  ::map(event => event.target.value)
  ::debounceTime(500)
  .subscribe(value => console.log(value));
```

### distinct

Returns an Observable that emits all items emitted by the source Observable that are distinct by comparison from previous items.

返回一個 Observable，它發射由來源 Observable 發射的所有項目，這些項目與以前的項目相比是不同的

```js
import { of } from 'rxjs/observable';
import { distinct } from 'rxjs/operators';

of('foo', 'bar', 1, 2, 3, 2, 1, 'bar', 'foo')
  .pipe(
    distinct()
  )
  .subscribe(value => console.log(value));
  // foo
  // bar
  // 1
  // 2
  // 3
```

### distinctUntilChanged

### filter

過濾給予的條件值，再將值發射出去。

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

import { filter } from 'rxjs/operator/filter';

Observable::from([1, 2, 3, 4, 5])
  ::filter(num => num % 2 === 0)  // 過濾掉非偶數的數值
  .subscribe(value => console.log(value));
  // 2
  // 4
```

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

import { filter } from 'rxjs/operator/filter';

Observable::from([
    { name: 'Joe', age: 31 },
    { name: 'Bob', age: 25 }
  ])
  ::filter(person => person.age > 30)  // 過濾掉大於 30 歲的人
  .subscribe(value => console.log(`Over 30: ${value.name}`));
  // Over 30: Joe
```

### first

僅發射來源 Observable 射出的第一個值 (或滿足某個條件的第一個值)。

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

import { first } from 'rxjs/operator/first';

const source$ = Observable::from([1, 2, 3, 4, 5]);

source$::first()
  .subscribe(value => console.log(value));
  // 1

source$::first(value => value === 3)
  .subscribe(value => console.log(value));
  // 3

source$::first(
    value => value % 2 === 0,
    (result, index) => `First even: ${result} is at index: ${index}`
  )
  .subscribe(value => console.log(value));
  // First even: 2 is at index: 1

source$::first(
    value => value > 5,
    value => `Value: ${value}`,
    'Default Value'
  )
  .subscribe(value => console.log(value));
  // Default Value
```

### ignoreElements

Ignores all items emitted by the source Observable and only passes calls of `complete` or `error`.

忽略源 Observable 發射的所有項目，只傳遞 `complete` 或 `error` 的呼叫。

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

import { ignoreElements } from 'rxjs/operator/ignoreElements';

Observable::from([1, 2, 3])
  ::ignoreElements()
  .subscribe(
    value => console.log(value),
    error => console.error(error),
    () => console.log('done')
  );
  // done
```

### last

Returns an Observable that emits only the last item emitted by the source Observable.

返回一個僅發出源 Observable 發射的最後一個項目的 Observable。

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

import { last } from 'rxjs/operator/last';

const source$ = Observable::from([1, 2, 3, 4, 5]);

source$::last()
  .subscribe(value => console.log(value));
  // 5

source$::last(value => value % 2 === 0)
  .subscribe(value => console.log(value));
  // 4

source$::last(
    value => value % 2 === 0,
    (result, index) => `Last even: ${result} is at index: ${index}`
  )
  .subscribe(value => console.log(value));
  // Last even: 4 is at index: 3
```

### sample

Emits the most recently emitted value from the source Observable whenever another Observable, the `notifier`, emits.

當 `notifier` 發射另一個 Observable 時，從源 Observable 發射最近發射的值。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { sample } from 'rxjs/operator/sample';

Observable::interval(1000)
  ::sample(Observable::interval(2000))
  .subscribe(value => console.log(value));
  // 0
  // 2
  // 4
  // 6
  // ...
```

### single

Returns an Observable that emits the single item emitted by the source Observable that matches a specified predicate, if that Observable emits one such item.

返回一個 Observable，它發出源 Observable 發射的與指定謂詞匹配的單個項，如果該 Observable 發射一個這樣的項。

```js
import { Observable } from 'rxjs/Observable';

import { from } from 'rxjs/observable/from';

import { single } from 'rxjs/operator/single';

Observable::from([1, 2, 3, 4, 5])
  ::single(value => value === 4)
  .subscribe(value => console.log(value));
  // 4
```

### skip

Returns an Observable that skips the first `count` items emitted by the source Observable.

返回一個 Observable，它忽略源 Observable 發射的第一個 `count` 項。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { skip } from 'rxjs/operator/skip';

Observable::interval(1000)
  ::skip(3)
  .subscribe(value => console.log(value));
  // 3
  // 4
  // 5
  // ...
```

### skipUntil

Returns an Observable that skips items emitted by the source Observable until a second Observable emits an item.

返回一個 Observable，它跳過源 Observable 發射的項目，直到第二個 Observable 發射一個項目。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';
import { timer } from 'rxjs/observable/timer';

import { skipUntil } from 'rxjs/operator/skipUntil';

Observable::interval(1000)
  ::skipUntil(Observable::timer(3000))
  .subscribe(value => console.log(value));
  // 2
  // 3
  // 4
  // ...
```

### skipWhile

Returns an Observable that skips all items emitted by the source Observable as long as a specified condition holds true, but emits all further source items as soon as the condition becomes false.

返回一個 Observable，它可以忽略源 Observable 發射的所有項目，只要指定的條件成立，只要條件變為 false，就會發射所有其他的源項目。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { skipWhile } from 'rxjs/operator/skipWhile';

Observable::interval(1000)
  ::skipWhile(value => value < 3)
  .subscribe(value => console.log(value));
  // 3
  // 4
  // 5
  // ...
```

### take

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { take } from 'rxjs/operator/take';

Observable::of(1, 2, 3, 4, 5)
  ::take(3)
  .subscribe(value => console.log(value));
  // 1
  // 2
  // 3
```

### takeUntil

Emits the values emitted by the source Observable until a `notifier` Observable emits a value.

發出源 Observable 發出的值，直到 `notifier` Observable 發出一個值。

```js
import { Observable } from 'rxjs';
import { interval, timer } from 'rxjs/observable';
import { takeUntil } from 'rxjs/operator';

Observable::interval(1000)
  ::takeUntil(Observable::timer(5000))
  .subscribe(value => console.log(value));
  // 0
  // 1
  // 2
  // 3
```

### takeWhile

Emits values emitted by the source Observable so long as each value satisfies the given `predicate`, and then completes as soon as this `predicate` is not satisfied.

只要每個值滿足給定的 `predicate`，就發出源 Observable 發出的值，然後一旦這個 `predicate` 不滿足就立即完成。

```js
import { Observable } from 'rxjs';
import { of } from 'rxjs/observable';
import { takeWhile } from 'rxjs/operator';

Observable::of(2, 2, 4, 4, 6, 6, 8, 8, 6, 6, 4, 4, 2, 2)
  ::takeWhile(value => value === 2)
  .subscribe(value => console.log(value));
  // 2
  // 2
```

### throttle

Emits a value from the source Observable, then ignores subsequent source values for a duration determined by another Observable, then repeats this process.

從源 Observable 中發出一個值，然後忽略由另一個 Observable 確定的持續源值，然後重複此過程。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { throttle } from 'rxjs/operator/throttle';

Observable::interval(1000)
  ::throttle(() => Observable::interval(3000))
  .subscribe(value => console.log(value));
  // 0
  // 4
  // 8
  // 12
  // ...
```

### throttleTime

Emits a value from the source Observable, then ignores subsequent source values for a duration determined by another Observable, then repeats this process.

從源 Observable 發射一個值，然後忽略由另一個 Observable 確定的持續時間的後續源值，然後重複這個過程。

```js
import { Observable } from 'rxjs';
import { interval } from 'rxjs/observable';
import { throttleTime } from 'rxjs/operator';

Observable::interval(1000)
  ::throttleTime(5000)
  .subscribe(value => console.log(value));
  // 0
  // 5
  // 10
  // 15
  // 20
  // ...
```

## Combination (組合)

### combineAll

當外部的觀察者完成時，輸出內部觀者的最新值。

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';
import { of } from 'rxjs/observable/of';

import { mapTo } from 'rxjs/operator/mapTo';
import { combineAll } from 'rxjs/operator/combineAll';

const source$ = Observable::timer(2000);

source$::mapTo(
    Observable::of('Hello', 'World')
  )
  ::combineAll()
  .subscribe(value => console.log(value));
  // ["Hello"]
  // ["World"]

source$::mapTo(
    Observable::of('Hello', 'Goodbye')
  )
  ::combineAll(value => `${value} Friend!`)
  .subscribe(value => console.log(value));
  // Hello Friend!
  // Goodbye Friend!
```

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { take } from 'rxjs/operator/take';
import { map } from 'rxjs/operator/map';
import { combineAll } from 'rxjs/operator/combineAll';

Observable::interval(1000)
  ::take(3)
  ::map(value => Observable::interval(value + 500)::take(2))
  ::combineAll()
  .subscribe(value => console.log(value));
  // [0, 0, 0]
  // [1, 0, 0]
  // [1, 1, 0]
  // [1, 1, 1]
```

### combineLatest

當任何的觀察者發射一個值時，從每個值發射出最新的值。

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';
import { combineLatest } from 'rxjs/observable/combineLatest';

const sourceOne$ = Observable::timer(1000, 4000);
const sourceTwo$ = Observable::timer(2000, 4000);
const sourceThree$ = Observable::timer(3000, 4000);

Observable::combineLatest(sourceOne$, sourceTwo$, sourceThree$)
  .subscribe((values) => {
    const [valueOne, valueTwo, valueThree] = values;
    console.log(`${valueOne}, ${valueTwo}, ${valueThree}`);
  });
  // 0, 0, 0
  // 1, 0, 0
  // 1, 1, 0
  // 1, 1, 1
  // 2, 1, 1
  // 2, 2, 1
  // 2, 2, 2
  // ...
```

使用函式投射

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';
import { combineLatest } from 'rxjs/observable/combineLatest';

const sourceOne$ = Observable::timer(1000, 4000);
const sourceTwo$ = Observable::timer(2000, 4000);
const sourceThree$ = Observable::timer(3000, 4000);

Observable::combineLatest(
    sourceOne$, sourceTwo$, sourceThree$,
    (valueOne, valueTwo, valueThree) => `${valueOne}, ${valueTwo}, ${valueThree}`
  )
  .subscribe(values => console.log(values));
  // 0, 0, 0
  // 1, 0, 0
  // 1, 1, 0
  // 1, 1, 1
  // 2, 1, 1
  // 2, 2, 1
  // 2, 2, 2
  // ...
```

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';
import { interval } from 'rxjs/observable/interval';

import { combineLatest } from 'rxjs/operator/combineLatest';

const sourceOne$ = Observable::timer(1000, 4000);
const sourceTwo$ = Observable::timer(2000, 4000);
const sourceThree$ = Observable::timer(3000, 4000);

Observable::interval(1000)
  ::combineLatest(sourceOne$, sourceTwo$, sourceThree$)
  .subscribe((values) => {
    const [valueOne, valueTwo, valueThree] = values;
    console.log(`${valueOne}, ${valueTwo}, ${valueThree}`);
  });
  // 2, 0, 0
  // 3, 0, 0
  // 4, 0, 0
  // 4, 1, 0
  // 5, 1, 0
  // 5, 1, 1
  // 6, 1, 1
  // 6, 1, 1
  // 7, 1, 1
  // 8, 1, 1
  // 8, 2, 1
  // 9, 2, 1
  // 9, 2, 2
  // ...
```

### concat

Creates an output Observable which sequentially emits all values from every given input Observable after the current Observable.

建立一個輸出 Observable，它從當前 Observable 後的每個給定輸入 Observable 中順序發出所有值。

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';
import { concat } from 'rxjs/observable/concat';

Observable::concat(
    Observable::of(1, 2, 3),
    Observable::of(4, 5, 6)
  )
  .subscribe(value => console.log(value));
  // 1
  // 2
  // 3
  // 4
  // 5
  // 6
```

```js
import { Observable } from 'rxjs/Observable';

import { concat } from 'rxjs/observable/concat';
import { interval } from 'rxjs/observable/interval';
import { of } from 'rxjs/observable/of';

Observable::concat(
    Observable::interval(1000),
    Observable::of('This', 'Never', 'Runs')  // 這個不會被執行
  )
  .subscribe(value => console.log(value));
  // 每秒打印
  // 0
  // 1
  // 2
  // 3
  // ...
```

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { concat } from 'rxjs/operator/concat';

Observable::of(1, 2, 3)
  ::concat(Observable::of(4, 5, 6))
  .subscribe(value => console.log(value));
  // 1
  // 2
  // 3
  // 4
  // 5
  // 6
```

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { delay } from 'rxjs/operator/delay';
import { concat } from 'rxjs/operator/concat';

Observable::of(1, 2, 3)
  ::delay(2000)
  ::concat(Observable::of(4, 5, 6))
  .subscribe(value => console.log(value));
  // 延遲兩秒
  // 1
  // 2
  // 3
  // 4
  // 5
  // 6
```

### concatAll

Converts a higher-order Observable into a first-order Observable by concatenating the inner Observables in order.

通過按順序連接內部 Observable，將高階 Observable 轉換為一階 Observable。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';
import { of } from 'rxjs/observable/of';

import { map } from 'rxjs/operator/map';
import { concatAll } from 'rxjs/operator/concatAll';

Observable::interval(1000)
  ::map(value => Observable::of(value + 10))
  ::concatAll()
  .subscribe(value => console.log(value));
  // 每秒打印
  // 10
  // 11
  // 12
  // 13
  // ...
```

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { map } from 'rxjs/operator/map';
import { concatAll } from 'rxjs/operator/concatAll';

Observable::interval(1000)
  ::map(value => new Promise(resolve => resolve(value)))
  ::concatAll()
  .subscribe(value => console.log(value));
  // 每秒打印
  // 0
  // 1
  // 2
  // 3
  // ...
```

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';
import { interval } from 'rxjs/observable/interval';

import { take } from 'rxjs/operator/take';
import { concatAll } from 'rxjs/operator/concatAll';

Observable::of(
    Observable::interval(1000)::take(5),  // 0, 1, 2, 3, 4 (完成)
    Observable::interval(500)::take(2),  // 0, 1 (完成)
    Observable::interval(2000)::take(1)  // 0 (完成)
  )
  ::concatAll()
  .subscribe(value => console.log(value));
  // 0
  // 1
  // 2
  // 3
  // 4
  // 0
  // 1
  // 0
```

### forkJoin

```js
import { Observable } from 'rxjs/Observable';

import { forkJoin } from 'rxjs/observable/forkJoin';
import { of } from 'rxjs/observable/of';
import { interval } from 'rxjs/observable/interval';

import { delay } from 'rxjs/operator/delay';
import { take } from 'rxjs/operator/take';

const p = value => new Promise(resolve => setTimeout(() => resolve(`Resolved: ${value}`), 5000));

Observable::forkJoin(
    Observable::of('Hello'),
    Observable::of('World')::delay(1000),
    Observable::interval(1000)::take(1),
    Observable::interval(1000)::take(2),
    p('RESULT')
  )
  .subscribe(value => console.log(value));
  // 五秒後印出
  // [ "Hello", "World", 0, 1, "Resolved: RESULT" ]
```

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';
import { forkJoin } from 'rxjs/observable/forkJoin';

import { mergeMap } from 'rxjs/operator/mergeMap';

const p = value => new Promise(resolve => setTimeout(() => resolve(`Resolved: ${value}`), 5000));

Observable::of([1, 2, 3, 4, 5])
  ::mergeMap(q => Observable::forkJoin(...q.map(p)))
  .subscribe(value => console.log(value));
  // 五秒後印出
  // [ "Resolved: 1", "Resolved: 2", "Resolved: 3", "Resolved: 4", "Resolved: 5" ]
```

### merge

Creates an output Observable which concurrently emits all values from every given input Observable.

建立一個輸出 Observable，它從每個給定的輸入 Observable 同時發射所有的值。

```js
import { Observable } from 'rxjs/Observable';

import { merge } from 'rxjs/observable/merge';
import { interval } from 'rxjs/observable/interval';

import { mapTo } from 'rxjs/operator/mapTo';

Observable::merge(
    Observable::interval(3000)::mapTo('FIRST'),
    Observable::interval(2000)::mapTo('SECOND'),
    Observable::interval(1000)::mapTo('THIRD')
  )
  .subscribe(value => console.log(value));
  // THIRD
  // SECOND
  // THIRD
  // FIRST
  // THIRD
  // SECOND
  // ...
```

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { merge } from 'rxjs/operator/merge';

Observable::interval(2000)
  ::merge(Observable::interval(1000))
  .subscribe(value => console.log(value));
  // 0
  // 0
  // 1
  // 2
  // 1
  // 3
  // ...
```

### mergeAll

Converts a higher-order Observable into a first-order Observable which concurrently delivers all values that are emitted on the inner Observables.

將高階 Observable 轉換為一階 Observable，它同時傳遞在內部 Observables 上發出的所有值。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { take } from 'rxjs/operator/take';
import { map } from 'rxjs/operator/map';
import { delay } from 'rxjs/operator/delay';
import { mergeAll } from 'rxjs/operator/mergeAll';

const source$ = Observable::interval(1000)::take(3);

source$::map(() => source$::delay(1000)::take(2))
  ::mergeAll(2)
  .subscribe(value => console.log(value));
  // 0
  // 1
  // 0
  // 1
  // 0
  // 1
```

### race

Returns an Observable that mirrors the first source Observable to emit an item from the combination of this Observable and supplied Observables.

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';

import { race } from 'rxjs/operator/race';
import { mapTo } from 'rxjs/operator/mapTo';

Observable::timer(5000)
  ::race(
    Observable::timer(2000)::mapTo('foo'),
    Observable::timer(1000)::mapTo('bar'),
    Observable::timer(3000)::mapTo('baz')
  )
  .subscribe(value => console.log(value));
  // bar
```

### startWith

Returns an Observable that emits the items you specify as arguments before it begins to emit items emitted by the source Observable.

返回一個 Observable，在開始發射來自 Observable 射出的項目之前發射所指定為參數的項目。

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { startWith } from 'rxjs/operator/startWith';

Observable::of(1, 2, 3)
  ::startWith(0)
  .subscribe(value => console.log(value));
  // 0
  // 1
  // 2
  // 3
```

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { startWith } from 'rxjs/operator/startWith';
import { scan } from 'rxjs/operator/scan';

Observable::of('World!', 'Goodbye', 'World!')
  ::startWith('Hello')
  ::scan((acc, value) => `${acc} ${value}`)
  .subscribe(value => console.log(value));
  // Hello
  // Hello World!
  // Hello World! Goodbye
  // Hello World! Goodbye World!
```

### withLatestFrom

Combines the source Observable with other Observables to create an Observable whose values are calculated from the latest values of each, only when the source emits.

將源 Observable 與其他 Observables 組合，以建立一個 Observable，其值根據每個的最新值計算，只有當源發出時。

```js
import { Observable } from 'rxjs/Observable';

import { interval } from 'rxjs/observable/interval';

import { withLatestFrom } from 'rxjs/operator/withLatestFrom';
import { map } from 'rxjs/operator/map';

const sourceOne$ = Observable::interval(2000);
const sourceTwo$ = Observable::interval(1000);

sourceOne$::withLatestFrom(sourceTwo$)
  ::map(([valueOne, valueTwo]) => `sourceOne$: ${valueOne}, sourceTwo$: ${valueTwo}`)
  .subscribe(value => console.log(value));
  // 兩秒後打印
  // sourceOne$: 0, sourceTwo$: 0
  // sourceOne$: 1, sourceTwo$: 2
  // sourceOne$: 2, sourceTwo$: 4
  // ...
```

### zip

Combines multiple Observables to create an Observable whose values are calculated from the values, in order, of each of its input Observables.

組合多個 Observables 以建立一個 Observable，其值根據每個輸入 Observable 的順序計算。

```js
import { Observable } from 'rxjs/Observable';

import { zip } from 'rxjs/observable/zip';
import { of } from 'rxjs/observable/of';
import { delay } from 'rxjs/operator/delay';

Observable::zip(
    Observable::of('Foo'),
    Observable::of('Bar')::delay(1000),
    Observable::of('Baz')::delay(2000)
  )
  .subscribe(value => console.log(value));
  // [ 'Foo', 'Bar', 'Baz' ]
```

## Multicasting (組播)

### multicast

### publish

Returns a ConnectableObservable, which is a variety of Observable that waits until its connect method is called before it begins emitting items to those Observers that have subscribed to it.

返回一個 ConnectableObservable，它是各種 Observable 等待，直到它的連接方法被調用之前，它開始向已經訂閱的觀察者發送項目。

```js
import Observable from 'rxjs/Observable';
import { interval } from 'rxjs/observable';
import { publish } from 'rxjs/operator';

import { _do } from 'rxjs/operator/do';

const source$ = Observable::interval(1000)
  ::_do(() => console.log('----------'))
  ::publish();

source$.subscribe(value => console.log(`Subscriber One: ${value}`));
source$.subscribe(value => console.log(`Subscriber Two: ${value}`));

setTimeout(() => source$.connect(), 3000);
// ----------
// Subscriber One: 0
// Subscriber Two: 0
// ----------
// Subscriber One: 1
// Subscriber Two: 1
// ----------
// Subscriber One: 2
// Subscriber Two: 2
// ----------
// Subscriber One: 3
// Subscriber Two: 3
// ...
```

### share

Returns a new Observable that multicasts (shares) the original Observable.

返回一個新的 Observable 來組播 (共享) 原始的 Observable

```js
import { Observable } from 'rxjs/Observable';

import { timer } from 'rxjs/observable/timer';

import { _do } from 'rxjs/operator/do';
import { mapTo } from 'rxjs/operator/mapTo';
import { share } from 'rxjs/operator/share';

const source$ = Observable::timer(1000)
  ::_do(() => console.log('***SIDE EFFECT***'))
  ::mapTo('***RESULT***');

source$.subscribe(value => console.log(value));
source$.subscribe(value => console.log(value));
// ***SIDE EFFECT***
// ***RESULT***
// ***SIDE EFFECT***
// ***RESULT***

const sourceShared$ = timer$::share();

sourceShared$.subscribe(value => console.log(value));
sourceShared$.subscribe(value => console.log(value));
// **SIDE EFFECT***
// ***RESULT***
// ***RESULT***
```

## Error Handling (錯誤處理)

### catch

Catches errors on the observable to be handled by returning a new observable or throwing an error.

透過返回一個新的 observable 或拋出一個錯誤來捕獲 observable 中的錯誤

```js
import { Observable } from 'rxjs/Observable';

import { _throw } from 'rxjs/observable/throw';
import { of } from 'rxjs/observable/of';

import { _catch } from 'rxjs/operator/catch';

Observable::_throw('An error!')
  ::_catch(error => Observable::of(`Error message: ${error}`))  // return a new observable (返回一個新的 observable)
  .subscribe(value => console.log(value));
  // Error message: An error!
```

```js
import { Observable } from 'rxjs/Observable';

import { _throw } from 'rxjs/observable/throw';

import { _catch } from 'rxjs/operator/catch';

Observable::_throw('An error!')
  ::_catch(error => { throw `Error message: ${error}`; })  // throw an error (拋出一個錯誤)
  .subscribe(value => console.log(value));
  // Error message: An error!
```

### retry

Returns an Observable that mirrors the source Observable with the exception of an `error`. If the source Observable calls `error`, this method will resubscribe to the source Observable for a maximum of `count` resubscriptions (given as a number parameter) rather than propagating the `error` call.

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable';

import { map, retry } from 'rxjs/operator';

Observable::of('foo', 'bar', 'error')
  ::map(value => {
    if (value === 'error') throw 'throw an error';
    return value;
  })
  ::retry(3)
  .subscribe(value => console.log(value));
  // foo
  // bar
  // foo - retry
  // bar
  // foo - retry 2 times
  // bar
  // foo - retry 3 times
  // bar
  // throw an error
```

### retryWhen

Returns an Observable that mirrors the source Observable with the exception of an error. If the source Observable calls error, this method will emit the Throwable that caused the error to the Observable returned from notifier. If that Observable calls complete or error then this method will call complete or error on the child subscription. Otherwise this method will resubscribe to the source Observable.

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable';

import { map, retryWhen } from 'rxjs/operator';

Observable::of('foo', 'bar', 'error')
  ::map(value => {
    if (value === 'error') throw 'throw an error';
    return value;
  })
  ::retryWhen(() => Observable::of('bar'))
  .subscribe(value => console.log(value));
```

## Utility (公用)

### do

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';

import { _do } from 'rxjs/operator/do';
import { map } from 'rxjs/operator/map';

Observable::of(1, 2, 3, 4, 5)
  ::_do(value => console.log(`BEFORE MAP: ${value}`))
  ::map(value => value + 10)
  ::_do(value => console.log(`AFTER MAP: ${value}`))
  .subscribe(value => console.log(value));
  // BEFORE MAP: 1
  // AFTER MAP: 11
  // 11
  // BEFORE MAP: 2
  // AFTER MAP: 12
  // 12
  // BEFORE MAP: 3
  // AFTER MAP: 13
  // 13
  // BEFORE MAP: 4
  // AFTER MAP: 14
  // 14
  // BEFORE MAP: 5
  // AFTER MAP: 15
  // 15
```

### delay

Delays the emission of items from the source Observable by a given timeout or until a given Date.

延遲發射 Observable 來源中的項目，依照由給定的超時或直到給定的日期。

```js
import { Observable } from 'rxjs/Observable';

import { of } from 'rxjs/observable/of';
import { merge } from 'rxjs/observable/merge';

import { mapTo } from 'rxjs/operator/mapTo';
import { delay } from 'rxjs/operator/delay';

const source$ = Observable::of(null);

Observable::merge(
    source$::mapTo(1),
    source$::mapTo(2)::delay(1000),
    source$::mapTo(3)::delay(3000)
  )
  .subscribe(value => console.log(value));
  // 1
  // 2 (延遲 1 秒)
  // 3 (延遲 4 秒)
```

### delayWhen

Delays the emission of items from the source Observable by a given time span determined by the emissions of another Observable.

延遲來自源 Observable 的物品的排放在給定時間跨度由另一個 Observable 的排放決定

```js
import { range, interval } from 'rxjs/observable';
import { delayWhen } from 'rxjs/operators';

range(0, 10)
  .pipe(
    delayWhen(value => interval(Math.floor((Math.random() * 10))))
  )
  .subscribe(value => console.log(value));
  // random: 0 ~ 9
```

### dematerialize

### let

### materialize

### observeOn

### toPromise

## Conditional and Boolean (附條件和布林值)

### defaultIfEmpty

Emits a given value if the source Observable completes without emitting any next value, otherwise mirrors the source Observable.

如果來源 Observable 完成而不發射任何下一個值，則發射給定的值，否則反映來源 Observable

```js
import { of } from 'rxjs/observable';
import { defaultIfEmpty } from 'rxjs/operators';

of()
  .pipe(
    defaultIfEmpty('The source is empty!')
  )
  .subscribe(value => console.log(value));
  // The source is empty!

of(1, 2, 3)
  .pipe(
    defaultIfEmpty('The source is empty!')
  )
  .subscribe(value => console.log(value));
  // 1
  // 2
  // 3
```

### every

Returns an Observable that emits whether or not every item of the source satisfies the condition specified.

返回一個 Observable，它發射一個來源的每個項目是否滿足指定的條件

```js
import { of } from 'rxjs/observable';
import { every } from 'rxjs/operators';

of(1, 2, 3, 4, 5)
  .pipe(
    every(value => value % 2 === 0)  // Is each value an even number? (每個值都是偶數嗎？)
  )
  .subscribe(value => console.log(value));
  // false
```

```js
import { of } from 'rxjs/observable';
import { every } from 'rxjs/operators';

of(2, 4, 6, 8, 10)
  .pipe(
    every(value => value % 2 === 0)  // Is each value an even number? (每個值都是偶數嗎？)
  )
  .subscribe(value => console.log(value));
  // true
```

### find

Emits only the first value emitted by the source Observable that meets some condition.

只發射符合一些條件的來源 Observable 發射的第一個值

```js
import { of } from 'rxjs/observable';
import { find } from 'rxjs/operators';

of(1, 5, 9, 13, 17, 21, 25)
  .pipe(
    find(value => value % 3 === 0)
  )
  .subscribe(value => console.log(value));
  // 9
```

```js
import { of } from 'rxjs/observable';
import { find } from 'rxjs/operators';

of(1, 5, 9, 13, 17, 21, 25)
  .pipe(
    find(value => value % 2 === 0)
  )
  .subscribe(value => console.log(value));
  // undefined
```

### findIndex

Emits only the index of the first value emitted by the source Observable that meets some condition.

只發射符合一些條件的來源 Observable 發射的第一個值的索引

```js
import { of } from 'rxjs/observable';
import { findIndex } from 'rxjs/operators';

of(1, 5, 9, 13, 17, 21, 25)
  .pipe(
    findIndex(value => value % 3 === 0)
  )
  .subscribe(value => console.log(value));
  // 2
```

### isEmpty

If the source Observable is empty it returns an Observable that emits true, otherwise it emits false.

如果來源 Observable 為空，則返回一個發射 true 的 Observable，否則發射 false

```js
import { of, from, empty } from 'rxjs/observable';
import { isEmpty } from 'rxjs/operators';

of()
  .pipe(
    isEmpty()
  )
  .subscribe(value => console.log(value));
  // ture

from([])
  .pipe(
    isEmpty()
  )
  .subscribe(value => console.log(value));
  // ture

empty()
  .pipe(
    isEmpty()
  )
  .subscribe(value => console.log(value));
  // ture
```

## Mathematical and Aggregate (運算和合計)

### count

Counts the number of emissions on the source and emits that number when the source completes.

計算來源所發射的數量，並在來源完成時發射該數量

```js
import { range } from 'rxjs/observable';
import { count } from 'rxjs/operators';

range(0, 10)
  .pipe(
    count(value => value % 3 === 0)
  )
  .subscribe(value => console.log(value));
  // 4
```

### max

The Max operator operates on an Observable that emits numbers (or items that can be compared with a provided function), and when source Observable completes it emits a single item: the item with the largest value.

Max 操作子在發射數字的 Observable（或可以與提供的函式進行比較的項目）上進行操作，當 Observable 來源完成時，它會發射一個單項：具有最大值的項目

```js
import { range } from 'rxjs/observable';
import { max } from 'rxjs/operators';

range(0, 10)
  .pipe(
    max()
  )
  .subscribe(value => console.log(value));
  // 9
```

```js
import { of } from 'rxjs/observable';
import { max } from 'rxjs/operators';

of(
  { name: 'Jojo', age: 18 },
  { name: 'Anna', age: 25 },
  { name: 'Joanna', age: 28 }
)
  .pipe(
    max((v1, v2) => v1.age < v2.age ? -1 : 1)
  )
  .subscribe(({ name }) => console.log(name));
  // Joanna
```

### min

The Min operator operates on an Observable that emits numbers (or items that can be compared with a provided function), and when source Observable completes it emits a single item: the item with the smallest value.

Min 操作子在發射數字的 Observable（或可以與提供的函式進行比較的項目）上進行操作，當 Observable 來源完成時，它將發射單個項目：具有最小值的項目

```js
import { range } from 'rxjs/observable';
import { min } from 'rxjs/operators';

range(0, 10)
  .pipe(
    min()
  )
  .subscribe(value => console.log(value));
  // 0
```

```js
import { of } from 'rxjs/observable';
import { min } from 'rxjs/operators';

of(
  { name: 'Jojo', age: 18 },
  { name: 'Anna', age: 25 },
  { name: 'Joanna', age: 28 }
)
  .pipe(
    min((v1, v2) => v1.age < v2.age ? -1 : 1)
  )
  .subscribe(({ name }) => console.log(name));
  // Jojo
```

### reduce

Applies an accumulator function over the source Observable, and returns the accumulated result when the source completes, given an optional seed value.

在源 Observable 上應用累加器函式，並在源完成時返回累積結果，給定可選的種子值

```js
import { of } from 'rxjs/observable';
import { filter, map, reduce } from 'rxjs/operators';

of(2, 'foo', 5, 'bar', 10)  // 2, 'foo', 5, 'bar', 10
  .pipe(
    filter(value => !isNaN(value)),  // 2, 5, 10
    map(value => value * 2),  // 2 * 2, 5 * 2, 10 * 2
    reduce((acc, value) => acc + value, 0)  // 0 + 4, 4 + 10, 14 + 20
  )
  .subscribe(value => console.log(value));
  // 34
```
