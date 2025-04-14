# 타입스크립트의 `type`들

📌 *들어가기 전에*,

자바스립트의 타입에는 `Primitive`타입과 `Object`타입이 있다.
- Primitive: `number`, `string`, `boolea`, `bigint`, `symbol`, `null`, `undefined`
- Object: `function`, `array`, ...

## Types
### number
```
const num: nubmer = 5;
```

### string
```
const str: stiring = 'hello';
```

### boolean
```
const bool: boolean = false;
```

### undefined
단독으로 사용하지 않음.
<br> 값이 비었는지 안 비었는지 아직 결정되지 않음, 보편적으로 null보다 undefined를 많이 이용함.
```
let age: number | undefined;
age = undefined;
age = 1;
function find(): number | undefined {
    return undefined;
}
```

### null
단독으로 사용하지 않음.
<br>
값이 비었음.
```
let person: string | null;
person = null;
person = '아이유';
```
### unknown
💩 타입을 알 수 없음
```
let notSure: unknown;
notSure = 'he';
notSure = 1;
notSure = true;
```

### any
💩 어떤 타입이든 다 담음
```
let anything: any;
anthing = 0;
anything = 'hello';
```

### void
함수가 아무런 값도 리턴하지 않을 때 사용,
<br> 함수에 굳이 표시하지 않고 생략해도 됨(회사 스타일 가이드에 따라 결정)
```
function print(): void {
    console.log('hello');
}
```

### never
이 함수를 호출하면 절대 리턴할 계획이 없으니까 감안해!
<br> Error 던지거나 무한한 반복문 작성시 사용
```
function throwError(message: string): never {
    // error 던질때
    throw new Error(message);
    
    // 무한한 반복일때
    while (true) {}
}
```

### object
💩 점 표기법으로 객체의 프로퍼티에 접근할 수 없기 때문에 사용X
<br> => 대안: 객체 리터럴 타입
```
function acceptSomeObject (obj: object) {}
acceptSomeObject({ name: 'ellie' });
acceptSomeObject({ animal: 'dog' });
```
