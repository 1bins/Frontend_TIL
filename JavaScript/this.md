# 자바스크립트에서 this란?

📌 *들어가기 전에*,

`this`를 사용할 때 마다 정확한 방식을 알지 못해서, 매번 구글링으로 의미를 다시 확인하였었다.
<br> 
이번 작성을 통해 완벽히 숙지해야겠다.

## this란?
자바스크립트에서 this는 현재 실행 중인 코드에서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다.
> this는 **함수 호출 방식**에 의해 **동적**으로 결정된다.

<br>

## 🧪 예제로 이해하기
### 전역 컨텍스트에서
```
console.log(this); // 브라우저에서는 window 객체
```

### 일반 함수 호출
```
function show() {
    console.log(this);
};
show(); // window (또는 strict mode에선 undefined)
```

### 객체의 메서드로 호출
```
const user = {
    name: 'Alice',
    sayHi() {
        console.log(this.name);
    }
};
user.sayHi(); // Alice
```

### 생성자 함수
```
function Person(name) {
    this.name = name;
}
const p = new Person('Bob');
console.log(p.name); // Bob
```

### 화살표 함수
화살표 함수의 `this`는 **정의된 시점**의 스코프에서 **가장 가까운(상위 스코프)** `this`를 그대로 사용한다 = 렉시컬 this
```
const user = {
    name: 'Dana',
    sayHi() {
        const inner = () => {
            console.log(this.name); // this는 sayHi(가장 가까운, 상위 스코프)의 this, 즉 user
        }
        inner();
    }
};
user.sayHi(); //Dana
```

### call, apply, bind
```
function greet() {
    console.log(this.name);
}
const person = { name: 'Charlie' };
greet.call(person); // Charlie
```

### setTimeout
```
const person = {
    name: "Jane",
    sayHi() {
        setTimeout(function(){
            console.log(this.name);
        }, 1000);
    }
}
person.sayHi(); // undefined (this는 전역 객체)

// 해결법 1: bind(this)
    ...
        setTimeout(function () {
            console.log(this.name);
        }.bind(this), 1000)
    ...

// 해결법 2: 화살표 함수 사용
    ...
        setTimeout(() => {
            console.log(this.name);
        }, 1000); // Jane
    ...
```

<br>

## 🎯 this 관련 문제
### Q. 다음 코드에서 `this`는 무엇을 가리킬까?
```
const obj = {
  value: 100,
  getValue: function() {
    return this.value;
  }
};

const extracted = obj.getValue;
console.log(extracted());
```
> 정답: `undefined` 또는 전역 객체의 `value`

`getValue`가 객체에서 분리되어 호출되면 `this`는 **전역 객체**를 가리킴