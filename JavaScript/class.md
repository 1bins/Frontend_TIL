# class

📌 *들어가기 전에*,

`class`는 객체를 생성하기 위한 템플릿으로, 객체가 가져야 할 상태(state)와 동작(behavior)을 정의한다.
<br>클래스를 사용하면 코드 재사용성이 좋아지고, 객체지향 프로그래밍을 좀 더 명확히 구현할 수 있다.

## 클래스의 기본 구조
```js
class Person {
    // 필드의 초기화
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // 메서드 정의
    greet() {
        console.log(`안녕하세요, 저는 ${this.name}이고 ${this.age}살 입니다.`);
    }
}

// 인스턴스 생성
const person1 = new Person('철수', 25);
person1.greet();
```
1. `constructor` 메서드
   - 객체가 생성될 때 자동으로 실행되는 **생성자 함수**
   - 초기화할 값(속성)을 넣어준다.


2. `this` 키워드
   - 생성된 인스턴스를 가리킴.
   

3. 메서드
    - 클래스 안에서 정의된 함수는 자동으로 `propertype`에 등록됨, 메모리를 아낄 수 있음.

<br>

## 클래스 상속
```js
class Animal {
    constructor(name) {
        this.name = name;
    }    
    
    speak() {
        console.log(`${this.name}이(가) 소리를 냅니다.`);
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
}
const dog = new Dog('별이', '장모치와와');
```
클래스를 상속할때 부모로 받은 필드인 `name`을 `countructor()` 파라미터에 넣어주고, `super()`키워드를 사용한다.

<br>

## 🚀 클래스 고급 문법
### static 메서드
인스턴스마다 다르게 가질 필요가 없는 **공통 속성**이나 **유틸리티 메서드**를 만들 때 사용.
<br>실무에서는 주로 유틸리티 클래스(수학 계산기, 로그인 서비스, 데이터 포맷 변환)등에 활용한다.
```js
class Example {
    static a = 10;
    
    static showA() {
        // this / 클래스명 둘다 사용 가능
        console.log(Example.a);
        console.log(this.a);
    }
}

// 인스턴스에서는 사용 불가
const utils = new Example();
console.log(utils.a); // ❌ undefined

// 🛠️ 유틸리티 메서드 - 인스턴스를 생성하지 않고, 바로 메서드를 호출한다
class Calculator {
    static add(a, b) {
        return a + b;
    }

    static subtract(a, b) {
        return a - b;
    }
}

console.log(Calculator.add(2, 3));  // 5
console.log(Calculator.subtract(5, 3)); // 2
```

### private 필드 '#'
웹 애플리케이션에서 상태 관리를 할 때, **비공개 데이터**가 필요할 때 많이 사용된다.
<br>로그인 세션에서 사용자 정보를 `private`으로 관리하여 외부에서 수정할 수 없게 보호하는 방식으로 활용한다.
```js
class Counter {
    #count = 0; // private 필드
    
    increment() {
        this.#count++;
        console.log(`현재 카운트: ${this.#count}`);
    }
    
    #logSecret() { // private 메서드
        console.log('이건 외부에서 접근 불가');
    }
}

const c = new Counter();
c.increment(); // 현재 카운트 1
// console.log(c.#count); // ❌ SyntaxError
// c.#logSecret(); //❌ SyntaxError
```

<br>

## 🧪 응용해서 사용해보기
```js
// 클래스 생성
class AuthService {
    static async login(username, password) {
        try {
            const res = await axios.post('api/login', {
                username,
                password,
            });
            
            const token = res.data.token;
            localStorage.setItem('token', token);
        }
        catch(err) {
            const message =
                    err.response?.data?.message ||
                    '로그인 중 에러가 발생했습니다';
            console.log(message);
        }
    }
}

// 기본 사용 예제
async function handleLogin() {
   try {
      const result = await AuthService.login('admin', '1234');
      console.log('로그인 성공!', result.token);

      // 예: 페이지 이동
      window.location.href = '/dashboard';

   } catch (err) {
      alert('로그인 실패: ' + err.message);
   }
}

handleLogin(); // 함수 실행
```