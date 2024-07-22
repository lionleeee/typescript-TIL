## typescript에서  satisfies는 언제 쓸까

satisfies는 Union 타입이 있을 때 사용.
```ts
type T {
 name: number | string;
age: number | string;
};
const obj ={
namee : 'jh' // <-1. 오타가 있을 경우
age :28
};

const obj  T ={ // <-2. 이런식으로 타이핑을 해서 오타를 잡을 수 있음
name : 'jh' // <- 3. 여기에 오타가 있다고 오류가 나옴
age :28
};

//4. 하지만 타이핑을 했을 경우문자에 charAt 메소드, 숫자에 fixied 메소드를 사용하지 못함
//5. T 타이핑을 지우면 사용이 가능하지만, 지우면 오타를 못잡고,, 사용하면 메소드를 사용 못함..
obj.name.charAt(1);
obj.age.toFixed (1);
```

여기에서 name,age에 메소드를 사용하지 못하는 이유는 number와 string의 유니온 타입이라서 사용을 못하는 거임
이럴 경우 T로 타이핑을 하는 것이 아닌 뒤에 satisfies를 사용
타입스크립트의 타입 추론은 그대로 사용하면서 추가적으로 타입 검사를 한번 더 할 수 있음

```ts
type T {
 name: number | string;
age: number | string;
};

const obj   ={ 
name : 'jh' 
age :28
} satisfies T // <-- 이렇게 사용

obj.name.charAt(1);
obj.age.toFixed (1);
```


## :question: 질문

그냥 유니온을 사용안하면 안되냐??
```ts
type T {
 name:  string; // 이렇게?
age: number;
};
```

그럼 이렇게 생긴 친구들을 만날 경우 어떻게 할래?

```ts

type T {
 [key in 'name | 'age] : string | number;
};

const obj   ={ 
name : 'jh' 
age :28
} satisfies T // <-- 이렇게 요론거 쓰면 짱

obj.name.charAt(1);
obj.age.toFixed (1);

```

