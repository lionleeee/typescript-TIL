### type
```ts
type Animal = {
    name: string;
    legs?: number;
};
```

legs같은 nullable이나 union 타입의 변수는 덜 정확한(?) 타입으로 사용 할 때 완전 정확한 타입으로 변할 수 있다.
이러한 과정을 type narrowing이라고 하며 이를 통해 타입 에러를 피할 수 있다


만약 아래와 같이 사용하면 에러가 발생 할 수 있다
```ts
function addLeg(animal: Animal) {
  return animal.legs = animal.legs + 1;  💥
}
```
legs가 undefined일 수도 있기 때문


#### 1 조건문을 사용한 타입 확인
```ts
function addLeg(animal: Animal) {
    if (animal.legs) {
        animal.legs = animal.legs + 1;
    }
}
```
if문을 만나기 전까지 legs는 number | undefined 타입
if문 후에는 number타입으로 좁혀지게 됨

#### 2. typeof 타입가드
```ts
function double(item: string | number) {
    if (typeof item === "string") {
        return item.concat(item);  // item이 string 타입
    } else {
        return item + item;     // item이 number 타입
    }
}
```
item 인자는 if문전에는number혹은 string 타입
if문안에서는 string으로 좁혀지고, else문에서는 number로 좁혀지게 됨


#### 3. instanceof 타입 가드
우리는 원시타입만 사용하는게 아닌 조금 더 복잡한 클래스를 더 자주 사용함
```ts
class Person {
    constructor(
        public firstName: string,
        public surname: string
    ) {}
}
class Organisation {
    constructor(public name: string) {}
}
type Contact = Person | Organisation;

function sayHello(contact: Contact) {
    console.log("Hello " + contact.firstName);
 // 💥 contact는 firstName을 가지고 있지 않은 Organisation일 수도 있기 때문에 타입 에러가 발생
}

```
클래스 타입을 이용할 때는 instanceof 타입가드 이용
```ts
function sayHello(contact: Contact) {
    if (contact instanceof Person) {
        console.log("Hello " + contact.firstName);
    }
}
```


#### 4. in 타입가드
클래스를 사용하지 않다고 하면 이 방법을 통해서 검사를 할 수 있음
```ts
interface Person {
    firstName: string;
    surname: string;
}
interface Organization {
    name: string;
}
type Contact = Person | Organisation;

function sayHello(contact: Contact) {
    if ("firstName" in contact) {
        console.log("Hello " + contact.firstName);
    }
}

```

