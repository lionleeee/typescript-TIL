## Promise와 Observable의 차이점

Promise와 Observable은 비슷한 듯 하나, 약간의 차이점을 가지고 있다

### Promise

Promise는 일종의 약속 객체
"미래의 어떤 시점에 특정 형태의 결과를 제공" 이라는 것을 구현해 놓은 객체라고 생각하면 된다.
한번에 하나의 값에 대해서만 비동기 처리를 한다.
한번 완료된 즉, async한 값을 resolve 하고나면 더이상 사용이 불가능하고 취소 할 수가 없다.
그래서 객체를 사용 할 때 사용하는 함수명도 "then"이다.

Promise는 정의가 되는 순간에 호출되기 때문에` then 메서드가 호출되는 시간은executor에 영향을 주지 않는다`

`new Promise에는 resolve와 reject를 인수로 갖는 함수가 전달되는데, 이 함수를 executor(실행자, 실행함수)라 한다.`
- executor(실행자, 실행함수)는 new Promise에 전달되는 함수이다.
- executor는 새로운 Promise 객체 인스턴스가 만들어질 때 자동으로 실행된다.
- executor의 인수 resolve와 reject는 JavaScript에서 자체 제공하는 콜백함수이다.
- executor에서는 인수로 넘겨준 콜백(resolve, reject) 중 하나를 반드시 호출해야 한다.
- resolve(value) — 일이 성공적으로 끝난 경우 그 결과를 나타내는 value와 함께 호출
- reject(error) — 에러 발생 시 에러 객체를 나타내는 error와 함께 호출


```js
const welcomePromise = new Promise(resolve => {
  console.log("In Promise executor fn");

  resolve("Welcome!");
});

console.log("Before calling the then method");

welcomePromise.then(console.log);
```

### Observable
Observable은 단어 뜻과 같이, 지속적인 관찰 가능한 객체라는 의미
"변경 될 때마다, 변경된 값을 비동지적으로 제공" 이라는 것을 구현해 놓은 객체라고 생각하면 된다
다수의 async 값들을 emit 할 수 있다. 즉, 다수의 값, 이벤트 스트림을 다루기 위해 사용한다.
Observable갸ㅐㄱ체를 사용할 때 사용되는 함수명도 "subscribe"이다.
주기적으로 값의 변경값을 받아보고 더이상 구독이 필요 없을 때, unsubscribe하는 것도 중요하다
#### 기본적으로 get요청은 Observable로 떨어진다

Observable은 rxjs에서 제공한다는 것이다.
이 말은 즉, map, filter, switchMap 등과 같은 pipe로 사용 할 수 있고 subscribe하기 전에 값을 트랜스폼 할 수도 있다.


`Observable은 구독을 할 때까지 함수가 호출되지 않음`
```js
import { Observable } from "rxjs";

const welcomeObservable$ = new Observable(observer => {
  console.log("In Observable producer fn");

  observer.next("Welcome!");
  observer.complete();
});

console.log("Before calling the subscribe method");

welcomeObservable$.subscribe(console.log);
```

`observable로 작업하는 것을 선호하는 경우 from 함수를 사용하여 Promise 객체를 Observable 인스턴스로 변환할 수 있다.`
```js
import { from } from "rxjs";

function getWelcomePromise() {
  return new Promise(resolve => {
    console.log("In Promise executor fn");

    resolve("Welcome!");
  });
}

const wrappedPromise1$ = from(getWelcomePromise());

console.log("Before calling the subscribe method");

wrappedPromise1$.subscribe(console.log);
```


![image](https://github.com/user-attachments/assets/754ae8e4-6ff3-4696-a10d-50073309b1b4)
![image](https://github.com/user-attachments/assets/bf92c601-3ff9-45c6-af76-264c1efd5fcc)


##### 생성 시점 지연 시키기
`defer함수`를 사용하여 생성되는 시점을 구독을 하는 순간으로 지연 시킬 수 있다.
```js

import { defer } from "rxjs";

function getWelcomePromise() {
  return new Promise(resolve => {
    console.log("In Promise executor fn");

    resolve("Welcome!");
  });
}

//여기서 함수를 호출했지만 defer에 의해 지연됨
const wrappedPromise2$ = defer(() => getWelcomePromise());

console.log("Before calling the subscribe method");

wrappedPromise2$.subscribe(console.log);
```

##### observable의 동기과 비동기
```js
import { Observable } from 'rxjs';

// 동기 Observable 예제
const syncObservable$ = new Observable(subscriber => {
  console.log("Observable started");
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  subscriber.complete();
});

// 구독
syncObservable$.subscribe({
  next(value) { console.log(`Received value: ${value}`); },
  complete() { console.log('Observable completed'); }
});
// 출력
// Observable started
// Received value: 1
// Received value: 2
// Received value: 3
// Observable completed
```

```js
import { Observable } from 'rxjs';

// 비동기 Observable 예제
const asyncObservable$ = new Observable(subscriber => {
  console.log("Observable started");
  
  setTimeout(() => {
    subscriber.next(1);
    subscriber.next(2);
    subscriber.next(3);
    subscriber.complete();
  }, 1000); // 1초 지연 후 값 방출

});

// 구독
asyncObservable$.subscribe({
  next(value) { console.log(`Received value: ${value}`); },
  complete() { console.log('Observable completed'); }
});

// 출력
// Observable started
// (1초 후)
// Received value: 1
// Received value: 2
// Received value: 3
// Observable completed
```

#### 연산자
Rxjs에는 많은 연산자 파이프가 존재함. 이를 사용해서 observable에 적용하여 원하는 스트림을 맞출 수 있음
- filter : 특정 조건많을 만족하는 것들만 걸러서 리턴
- first : 첫번째 값 1개만 발행하고 더이상 값을 발행하지 않는(complete 함수를 호출)
- last : 마지막 값 하나만 발행 마찬가지로 complete함수를 호출
- take : 정해진 개수만큼 구독하고 구독을 해제
- takeUntil : 특정 이벤트가 발생할 때까지 구독
- distinct : 내부에서 자체 구현한 Set 자료 구조로 이미 발행된 값을 중복 없이 저장했다가 같은 값이 전달되면 발행하지 않음.
-- 해당 구조는 배열 형태이며 해당 배열의 indexOf함수를 사용하여 얻은 index값이 -1과 다른지 검사하여 값을 유무를 확인
-- 객체는 참조로 식별되기에 새로 만든 객체가 안에 값이 같아도 참조가 다르기에 서로 다른 객체로 인식됨
-- 따라서 단순히 distinct 함수를 호출하면 모든 객체가 출력되므로 이를 방지하기 위해 객체의 (키-값)으로 구분하여 key를 사용하여 조건처리 -> id기준으로 거를 수 있도록
- switchMap : 새 옵저버블을 반환하고 구독하는 연산자였다면 새 옵저버블을 반환하고 구독하기 전에 연산자에서 완료되지 않은 작업이 있다면 해당 옵저버블의 구독을 해제하고 새 옵저버블을 구독한다

... 등등 많은 연산자들이 있다.

### 요약
- Promise는 항상 비동기식이지만 observable은 동기와 비동기 둘 다 가능하다
- Promise는 단일 값만 제공할 수 있지만 observable은 값의 스트림(0개에서 ~n개)
- observable의 연산자를 사용하면 편하게 개발이 가능하다

