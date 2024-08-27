# typeof
`객체 데이터를 객체 타입으로 변환해주는 연산자`<br/>
객체, 함수, 클래스 등을 타입으로 변환시켜 준다.<br/>
`객체에 쓰인 타입 구조`를 사용하여 `독립된 타입` 만들어 준다
```ts
const obj = {
   red: 'apple',
   yellow: 'banana',
   green: 'cucumber',
};

// 위의 객체를 타입으로 변환하여 사용하고 싶을때
type Fruit = typeof obj;
/*
type Fruit = {
    red: string;
    yellow: string;
    green: string;
}
*/

```
### 함수도 타입으로 변환이 가능하다
```ts
function fn(num: number, str: string): string {
   return num.toString();
}

type fn_Type = typeof fn;
// type fn_Type = (num: number, str: string) => string
```

### 클래스도 타입으로 변환이 가능하다
```ts
class Classes {
   constructor(public name: string, public age: number) {}
}

type class_Type = Classes;
// type Class_Type = { name: string, age, number }

const cc: Class_Type = {
   name: '임꺾정',
   age: 18888,
};

```


#  keyof
`객체 형태의 타입을, 따로 속성[키]만 뽑아서 유니온 타입으로 만들어주는 연산자
```ts
type Type = {
   name: string;
   age: number;
   married: boolean;
}

type Union = keyof Type;
// type Union = name | age | married

const a:Union = 'name';
const b:Union = 'age';
const c:Union = 'married';

```
### 객체의 속성[키]를 상수 타입으로 만들고 싶을 경우
```ts
const obj = { red: 'apple', yellow: 'banana', green: 'cucumber' } as const; // 상수 타입을 구성하기 위해서는 타입 단언을 해준다.

// 위의 객체에서 red, yellow, green 부분만 꺼내와 타입으로서 사용하고 싶을떄
type Color = keyof typeof obj; // 객체의 key들만 가져와 상수 타입으로


//Color = 'red' | 'yellow' | 'green';
let ob2: Color = 'red';
let ob3: Color = 'yellow';
let ob4: Color = 'green';
```

### 객체의 [값]만 뽑아서 상수 타입으로 만들고 싶을 경우
```ts
const obj = { red: 'apple', yellow: 'banana', green: 'cucumber' } as const;

type Key = typeof obj[keyof typeof obj]; // 객체의 value들만 가져와 상수 타입으로

//key = 'apple' | 'banana' | 'cucuber';
let ob2: Key = 'apple';
let ob3: Key = 'banana';
let ob4: Key = 'cucumber';
```
