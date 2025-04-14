# call / apply / bind

📌 *들어가기 전에*,

`this`키워드를 공부하다 보니 자연스레 `call` `apply` `bind`의 존재에 대해 알게 되었다.

## 간단한 개념 요약
> call/apply/bind는 함수의 `this`를 원하는 객체로 바꿔서 실행하거나, 바인딩할 때 사용하는 메서드이다.

| 메서드     | 설명                   |    즉시 실행    |  인자 전달 방식  |
|---------|----------------------|:-----------:|:----------:|
| `call`  | `this`를 정의하고 함수를 호출  |      O      |   하나씩 나열   |
| `apply` | `this`를 정의하고 함수를 호출  |      O      |   배열로 전달   |
| `bind`  | `this`를 정의한 새 함수를 반환 |  X(나중에 실행)  |   하나씩 나열   |

<br>

## 🧪 예제로 이해하기
```
const user = {
    name: "Alice",
};

function say(greeting, puctuation) {
    console.log(`${greeting}, ${this.name}${puctuation}`);
}
```

### call
인자를 하나씩 나열
```
say.call(user, "Hello', "!"); // Hello, Alice!
```

### apply
인자를 배열로 전달
```
say.apply(user, ["Hi", "!!"]); // Hi, Alice!!
```

### bind
`boundSay`라는 새 함수로 반환, 인자를 하나씩 나열
```
const boundSay = say.bind(user, "Hey", "~~~");
boundSay(); // Hey, Alice~~~
```

<br>

## 🎯 call/apply/bind 관련 질문
### Q. `bind`를 써야 하는 경우가 있을까?
> `bind`는 함수를 즉시 실행하지 않고, `this`가 영구적으로 바인딩된 **새로운 함수를 반환**한다.
> <br>
> 주로 **이벤트 핸들러나 콜백 함수에서 `this`가 의도치 않게 바뀌는 걸 방지**하기 위해 사용한다.


### Q. 실무에서 `call`이나 `apply`를 쓰는 경우
> 재사용 가능한 유틸 함수나 콜백 내부에서 context를 명확하게 제어해야 할 때 사용.

예를 들어, 공통 로직을 가진 함수에서 `this`에 따라 동작을 다르게 하거나, 외부 라이브러리에서 제공하는 콜백이 `this`를 이상하게 바인딩 할 때 `call`이나 `apply`로 제어한다.
```
function logInfo(level, msg) {
  console.log(`[${level}] ${this.name}: ${msg}`);
}

const logger = { name: "AppLogger" };

// call을 사용해 this를 logger로 바인딩
logInfo.call(logger, "INFO", "시작합니다");

// apply는 인자를 배열로 전달
logInfo.apply(logger, ["ERROR", "문제가 발생했습니다"]);
```