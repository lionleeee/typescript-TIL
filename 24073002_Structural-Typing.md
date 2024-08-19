
### 구조적 타이핑

명시적 선언이나 이름을 기반으로 하는 명목적 타입(nominal type)인 java, C#과는 다름, 런타임에 타입을 체크하는 덕 타이핑과도 다름


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

* 덕 타이핑
  - 두 객체가 서로 같은 변수와 메서드를 가진다면 두 객체는 동일한 타입으로 간주 하는 것
  - 객체의 실제 타입은 중요하지 않음
  - 런타임 시에 타입 검사를 함
    
* 구조적 타이핑
  - 객체의 실제 구조(프로퍼티와 메서드)에 기반하여 타입을 결정하는 방식
  - 객체가 특정 타입의 구조를 가지고 있다면, 그 객체는 해당 타입으로 간주
  - 객체 간 속성이 동일하다면 서로 호환되며, 실제 타입은 중요하지 않고 구조만 중요
  - 컴파일 시에 타입 체크를 함
 
  두 타입 모두 객체가 가진 속성을 기반으로 타입을 검사한다는 공통점이 있지만 타입을 검사하는 시점에 차이가 있다.
