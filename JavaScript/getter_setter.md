# Getter & Setter

📌 *들어가기 전에*,

`getter`와 `setter`를 쓰는 이유는 무엇일까?
1) 내부에 있는 데이터를 직접적으로 수정하는 것이 아니기 때문에 (비교적)안전함
2) object 안의 데이터가 복잡한 경우 함수를 써서 데이터를 가져오는 것이 더 유리함

## Getter & Setter 정의
### Getter
- 객체의 프로퍼티를 **읽을 때 실행되는 함수**
- `get` 키워드를 사용
- 매개변수를 설정할 수 없음, `return`값 필수

### Setter
- 객체의 프로퍼티를 **설정할 때 실행되는 함수**
- `set` 키워드를 사용
- 하나 이상의 매개변수 설정 필요

<br>

## 🧪 예제로 이해하기
### 객체에서 사용해보기
```js
const person = {
    firstName: "kim",
    lastName: "jisoo",
    get fullName() {
        return `${this.firstName} ${this.lastName}`
    },
    set fullName(name) {
        // 구조분해할당
        [this.firstName, this.lastName] = name.split(" ");
    }
};

// getter
console.log(person.fullName); // Kim Jisoo

// setter
person.fullName = "Lee Minho";
console.log(person.firstName); // Lee
```

### `class`에서 사용해보기
```js
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }
    
    get area() {
        return this.width * this.height;
    }
    
    set size(sizeObj) {
        const { width, height } = sizeObj;
        this.width = width;
        this.height = height;
    }
}

const rect = new Rectangle(10, 5);
console.log(rect.area); // 50

rect.size = { width: 20, height: 2 };
console.log(rect.area); //40
```

<br>

## 🎯 실무 사용 예제
### 비밀번호를 직접 접근 못 하게 숨기기
```js
class User {
    #password; // private field (외부에서 직접 접근 불가)
    
    constructor(username, password) {
        this.username = username;
        this.#password = password;
    }
    
    get maskedPassword() {
        return '*'.repeat(this.#password.length);
    }
    
    set newPassword(pw) {
        if (pw.length < 6) {
            console.log("비밀번호는 6자 이상이어야 합니다.");
        } else {
            this.#password = pw;
            console.log("비밀번호가 변경되었습니다.");
        }
    }
    
    // 실제 비밀번호 확인 메서드는 따로 제공
    checkPassword(input) {
        return this.#password === input;
    }
}

const user = new User("frontend_dev", "secure123");

// ❌ 외부에서는 private 필드 직접 접근 불가
console.log(user.#password); // syntaxError

// getter 간접적으로 마스킹된 값만 접근 가능
console.log(user.maskedPassword); // *********

// sette로 간접적으로 비밀번호 변경
user.newPassword = "123"; // 비밀번호는 6자 이상이어야 합니다.
user.newPassword = "newpass9"; // 비밀번호가 변경되었습니다.

console.log(user.checkPassword("newpass9")); // true
```
- `#password`: 외부에 노출되지 않도록 비공개 상태 유지
- `get maskedPassword`: 실제 값은 숨기고, 사용자에게 읽기 인터페이스만 제공
- `set newPassword`: 유효성 검사 포함된 쓰기 인터페이스 제공

### 제품 가격 관리
```js
class Product {
    #price;
    
    constructor(price) {
        this.#price = price;
    }
    
    get price() {
        return this.#price;
    }
    
    set price(newPrice) {
        if (newPrice > 0) {
            this.#price = newPrice;
        } else {
            console.log("가격은 -가 될수 없습니다.");
        }
    }
}

const product = new Product(100);
console.log(product.price); // 100

product.price = 500;
console.log(product.pirce); // 500
```