
### 구조적 타이핑

명시적 선언이나 이름을 기반으로 하는 명목적 타입(nominal type)인 java, C#과는 다름, 런다임에 타입을 체크하는 덕 타이핑과도 다름


명시적 타이핑
```ts
declare class Duck{
  quack(): void;
}

interface IQuack{
  quack(): void;
}

function quackquack(duck : Duck){ // <-사용하는 클래스를 넣음
  duck.quack();
}

quackquack(new Duck());

```


구조적 타이핑
```ts
declare class Duck{
  quack(): void;
}

interface IQuack{
  quack(): void;
}


function quackquack(duck : IQuack){ // <- 일단 다른 객체도 넣을 수 있음
//타입스크립트는 클래스나 메소드나 같은 메소드, 속성을 가지고 있으면 같은 객체로 침
  duck.quack();
}

quackquack(new Duck());

```
