# closure 파헤치기

📌 *들어가기 전에*,

<img src="https://i.namu.wiki/i/-B6mlCu66RkrhbSDhvB5lKAoomwWZXsPvCSFxK20aBGaOLhGv9Byx5lKGhY87YWkkPmA67GT3T8hGP402olemQ.webp" width="160"/>

"자바스크립트의 함수는 일급 객체다"
<br> 함수도 숫자나 문자열처럼 값으로 다룰 수 있다.
<br> 변수에 저장이 가능하고, 다른 함수에 인자로 넘길 수 있고, 다른 함수에서 반활할 수도 있다.

## 클로저란?
클로저는 함수가 생성될 때 외부 변수(렉시컬 환경)를 기억하고, 그 함수가 외부 스코프에서 실행되더라도 그 변수에 접근할 수 있게 해주는 기능이다.
<br>
[🎦 클로저, 죽은 함수의 망령](https://www.youtube.com/watch?v=WbVVKY9CDP0)
> 함수가 자신이 생성된 환경을 '기억'하는 것.

<br>

## 🧪 예제로 이해하기
### 기본 예제
```js
function outer() {
    let count = 0;
    
    function inner() {
        count++;
        console.log(count);
    }
    
    return inner;
}

const fn = outer(); // outer() 호출 -> inner 함수 리턴됨
fn(); // 1
fn(); // 2
```
- `outer()`함수가 실행되면 `count`라는 지역 변수가 생기고,
- 그 안에서 `inner()`함수가 정의되고, 리턴됨.
- `inner()`는 외부 함수인 `outer()`의 `count`변수에 접근할 수 있음.
<br> **클로저** 덕분에 `count`를 "기억"하고 있기 때문.
- 그래서 `fn()`을 여러 번 호출해도 `count`는 유지됨.

### 실무 사용 예제
```js
function createCounter() {
    let count = 0;
    
    return {
        increase: function () {
            count++;
            console.log(count);
        },
        decrease: function () {
            count--;
            console.log(count);
        }
    };
}

const counter = createCounter();
counter.increase(); // 1
counter.increase(); // 2
counter.decrease(); // 1
```
- `count`는 외부에서 직접 접근 못 하고, 오직 `increase`나 `decrease`로만 조작 가능함
- **모듈화, 컴포넌트 상태 관리** 등에서 이런 패턴을 사용함!

<br>

## 🔥 클로저가 언제 유용할까?
1. 데이터 은닉 (private 변수 만들기)
<br>: 외부에서 직접 접근 못하게 하고 함수로만 조작할 수 있도록 할 때


2. 상태 유지가 필요할 때
<br>: 위 예시처럼 특정 값을 계속 기억하면서 로직을 처리해야 할 때


3. 콜백이나 이벤트 핸들러에서 사용
<br>: 특정 컨텍스트를 계속 유지할 필요가 있을 때도 클로저가 유용

<br>


## 🎯 클로저 관련 문제
### Q. 자바스크립트의 클로저에 대해 설명
> 클로저는 함수가 자신이 선언될 당시의 렉시컬 환경을 기억해서, 외부 함수의 지역 변수에 접근할 수 있게 해주는 개념이다.
> <br> 예를 들어, 어떤 함수 내부에서 변수를 선언하고 그 내부 함수를 리턴하면, 그 내부 함수는 외부 함수가 종료된 이후에도 해당 변수에 접근할 수 있다.
> <br> 데이터 은닉이나 상태 유지에 유용하게 쓰일 수 있다.