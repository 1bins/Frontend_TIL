# 완벽 `promise` 숙지하기

📌 *들어가기 전에*,

<img src="https://velog.velcdn.com/images/kmh060020/post/f015f8c3-c604-4fba-a190-a5d655387d11/image.png" />

모든 TIL에서 대부분의 이유를 차지하겠지만, `promise`라는 걸 알았지만 딥하게 설명하는 데에는 어려움이 컸다.
<br>
이번 기회를 통해서, 정복해보자!

## 간단한 개념 요약
> `promise`는 **미래에 완료될 수도 있고 실패할 수도 있는 비동기 작업**을 나타내는 객체이다.

### 상태
`promise`는 총 3가지 상태를 가질 수 있다.

- `pending`: 대기 상태. 아직 작업이 완료되지 않음
- `fulfilled`: 작업이 성공적으로 완료됨
- `rejected`: 작업이 실패함

## 🧪 예제로 이해하기
```js
const promise = new Promise((resolve, reject) => {
    const success = true;
    
    if (success) {
        resolve("작업 성공!");
    } else {
        reject("작업 실패!");
    }
});

promise
    .then(result => {
        console.log(result); // 작업 성공!
    })
    .catch(error => {
        console.log(error); // 작업 실패!
    });
```

### 흐름 정리
1. `new Promise()`로 생성 => `resolve` or `reject`실행
2. `then()`으로 성공, `catch()`로 실패 처리
3. `.finally()`는 성공/실패 관계없이 마지막에 실행됨

<br>

## 🔷 좀 더 알아보기
### promise.all()
여러 개의 `promise`를 **동시에 실행**하고, **모두 성공했을 때 결과를 한 번에 반환**해주는 메서드
<br> **하나라도 실패하면 전체가 실패 처리됨**

#### 1. 여러 API 병렬 호출 (성능 최적화)
순차적으로 처리하면 느리지만, `Promise.all`로 병렬 처리하면 속도가 줄어듦
```js
const getUser = axios.get('/api/user');
const getNotifications = axios.get('/api/notifications');
const getMessages = axios.get('/api/messages');

Promise.all([getUser, getNotifications, getMessages])
    .then(([userRes, notiRes, msgRes]) => {
        setUser(userRes.data);
        ...
    })
    .catch(console.error);

    
// async/await로 작성    
async function fetchAlldata() {
    try {
        const [userRes, notiRes, msgRes] = await Promise.all([
            axios.get('api/user'),
            ...
        ]);
        
        setUser(userRes.data);
        ...
    } catch (error) {
        ...
    }
}
```

#### 2. 이미지 여러 장 preload 할 때
```js
const loadImage = (src) => new Promise((resolve, reject) => {
    const img = new Image();
    img.src = src;
    img.onload = () => resolve(src);
    img.onerror = reject;
});

Promise.all([
    loadImage('/img1.png'),
    loadImage('/img2.png'),
    loadImage('/img3.png')
]).then(() => {
    console.log('모든 이미지 로드 완료');
});
```

### promise.allSettled()
모든 `promise`의 결과가 성공/실패로 끝날 때까지 기다리는 메서드
<br> 성공하거나 실패한 모든 `promise` 결과를 **배열로 반환해줌**
<br> **실패한 `promise`가 있어도 에러로 처리되지 않음** (각각의 상태를 알 수 있음)

#### 예시 상황
여러 API를 병렬로 요청할 때, 하나가 실패해도 나머지는 계속 진행해야 하는 경우.
<br> 상품 목록을 가져오고 각 상품에 대한 상세 정보도 동시에 가져올 때, 상품 목록은 필수값이고 각 상품의 상세 정보는 실패해도 문제없이 진행되는 경우.
```js
async function fetchProductData() {
  try {
    const [productListResult, productDetailsResult] = await Promise.allSettled([
      axios.get('/api/products'),
      axios.get('/api/product/details')
    ]);

    const productList = productListResponse.status === 200
      ? productListResponse.data
      : [];

    const productDetails = productDetailsResponse.status === 200
      ? productDetailsResponse.data
      : null;

    console.log('상품 목록:', productList);
    console.log('상품 상세:', productDetails);
  } catch (err) {
      console.error('전체 실패:', err);
  }
}
```

<br>

## 🎯 promise 관련 문제
### Q. `promise`란 무엇이고 왜 필요한가?
> 비동기 처리 방식 중 하나로, 콜백 함수보다 가독성과 유지보수가 좋다.
> <br> `then`, `catch`, `finally` 메서드를 통해 비동기 흐름을 직관적으로 관리할 수 있고, `promise.all`이나 `promise.race`같은 유틸 메서드로 복잡한 비동기 로직도 쉽게 처리할 수 있다.


### Q. `promise`와 `async/await`의 차이는?
> `async/await`는 `promise`를 더 직관적으로 사용할 수 있게 해주는 문법이다.
> <br> 내부적으로는 `promise`를 사용하며, `try...catch`구문과 함께 사용하면 에러 처리도 간편해진다.


<br>

## 📝 직렬/병렬처리 총 정리
| 구분                     | 내용                                                                      |
|------------------------|-------------------------------------------------------------------------|
| `promise.all()`        | 여러 `promise`를 **병렬 처리**하고, 모든 요청이 성공해야 결과를 처리                           |
| `promise.allSettled()` | 여러 `Promise`를 **병렬 처리**하고, **실패**하더라도 계속 진행하며 각 `Promise`의 성공/실패 여부를 확인 |
| `axios`와 `async/await`  | 기본적으로 직렬 처리, `promise.all()`을 활용하여 병렬처리, `await`만 사용하여 순차 처리 가능         |

