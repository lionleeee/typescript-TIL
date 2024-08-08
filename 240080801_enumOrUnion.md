### enum 보다는 Union

```ts
enum Fruit {
  Apple,
  Banana,
  Orange,
}
// 아래는 union type
type Fruit = "apple" | "banana" | "orange";
```

enum은 컴파일을 하고나면 `객체`가 생성이 된다. 즉, 런타임에 영향을 줌
타입스크립트의 큰 사용 이유는 정적 타입을 사용하기 위함인데, 정적 타입 정보가 런타임에 영향을 준다

### const enum을 사용!
```ts
const enum Fruit {
  Apple,
  Banana,
  Orange,
}
```
const enum을 사용하면 컴파일 후에도 객체가 생성이 되지 않는다.
하지만, enum은 값을 입력하기 위해서 import를 해야한다.
import라는 한줄의 코드를 더 작성해야 해서 union보다 생산성이 떨어진다.

### union 사용 팁

1. 객체의 키를 유니온 타입으로 변환하기
```ts
   const person = {
 name: "John",
 age: 30,
 job: "developer"
};

type PersonKeys = keyof person;
// 결과: "name" | "age" | "job"
   ```

2. 객체으 ㅣ값을 유니온 타입으로 변환하기
```ts
const person = {
 name: "Johtyn",
 age: 30,
 job: "developer"
};

type PersonValues = typeof person[keyof typeof person];

// 결과: string | number
```

3. 제네릭을 사용한 키와 값 추출하기

아래 코드에서 `getObjectKeys`는 어떤 객체든 키를 배열 형태로 반환
이 배열을 이용해 키의 타입을 union타입으로 변환 할 수 있다.
```ts

function getObjectKeys<T>(obj: T): (keyof T)[] {
 return Object.keys(obj) as (keyof T)[];
}

const person = {
 name: "John",
 age: 30,
 job: "developer"
};

const personKeys = getObjectKeys(person);
// 결과: ["name", "age", "job"]

type PersonKeys = typeof personKeys[number];
// 결과: "name" | "age" | "job"
```

4. 객체의 키와 값을 타입으로 활용하기
```ts
   const person = {
 name: “John”,
 age: 30,
 job: “developer”
};

type Person = typeof person;
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
 return obj[key];
}
const name = getValue(person, "name"); // "John"
const age = getValue(person, "age"); // 30

   ```

#### 요약
- enum을 쓸꺼면 const enum을 사용해라
- 그래도 union이 더 좋다
