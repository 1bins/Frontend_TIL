# type alias

## type alias란?
> 내가 원하는 새로운 타입을 정의할 수 있다.
```
type Text = string;
const name: Text = '문자열';

type Num = number;
const numb: Num = 25;

// 문자열, 숫자열 뿐만 아니라 객체로 정의할 수도 있다.
type Student = {
    name: string;
    age: number;
};
const student: Student = {
    name: '1bins',
    age: 30
}
```

<br>

## Union Types
> 'or': 모든 가능한 케이스 중에 발생할 수 있는 딱 하나를 담을 수 있는 타입을 만들고 싶을 때 사용,
> <br> 실무 활용도 매우 높음!
### 예제
```
type Direction = 'left' | 'right' | 'up' | 'down';
function move(direction:Direction) {
    console.log(direction);
}
move('up'); // Direction에 있는 4개 중 하나를 추천해준다.
```
### 실전 예제
```
type SuccessState = {
    result: 'success';
    response: {
        body: string;
    }
};
type FailState = {
    result: 'fail';
    reason: string;
}
type LoginState = SuccessState | FailState;

function loginSuccess():LoginState {
    return {
        result: 'success'
        response: {
            body: 'logged in!'
        }
    }
}
```
### discriminated(차별화하는/구분하는)
그렇다면, 출력하는 함수 `printLoginState`에서 성공과 실패를 구분하고자 할때 어떻게 사용할까?
```
function printLoginState(state: LoginState) {
    // 'response' in state ↓
    // 성공, 실패state 타입에 result를 넣어줌으로써 명확한 조건 구분 가능
    if (state.result === 'success') {
      console.log(`🎉 ${state.response.body}`)
    } else {
      console.log(`😭 ${state.reason}`)
    }
}
```