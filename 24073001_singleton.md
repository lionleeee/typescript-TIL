### 싱글턴 패턴

클래스 인스턴스가 전역에서 딱 하나이고 싶을 때 사용

```ts
class Person{
static instance ; // <- 이거 먼저 선언
  constructor (name, age)
  {
    if(Person.instance){ // <-이 조건에 의해 기존에 만든 객체 리턴
    return Person.Instance;
    }
    this.name =name ;
    this.age = age;
    Person.instance = this; 
  }
}


//위와 같이 작성하면 객체를 새로 찍어도 하나만 생성됨
new Person() == new Person()
```

- 싱클턴 패턴을 오남용 하는 경우에는 안티패턴으로 취급하는 경우가 많음.
- 싱글턴 패턴은 구체적으로 구현을 하기에 확장성이 부족하고 테스트하기 어려움
