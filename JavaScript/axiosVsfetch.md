# fetch vs axios

📌 *들어가기 전에*,

`NEXT.js` 학습 중에 NEXT에서는 axios대신 fetch를 사용하는 게 권장된다고 했다.
<br> 두 메서드의 차이점을 정확히 이해해보고자 작성한다.

## 개념 정리
### fetch
- Promise 기반이며, 네트워크 오류 시에만 reject되고 HTTP 상태 코드가 200번대가 아니어도 resolve됨.
- 응답 데이터를 직접 `.json()`같은 메서드를 호출해 변환해야 함.
- POST 요청 시 데이터를 `JSON.stringify()`로 변환 해줘야 함
```js
// GET요청
try {
  const res = await fetch('/api/data');
  if (!res.ok) {
    throw new Error('');
  }
  
  const data = await res.json();
  console.log(data);
} catch (e) {
  // ...
}

// POST 요청
try {
  const response = await fetch('api/post', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      name: '홍길동',
      age: 30
    })
  });
  
  if (!response.ok) {
    throw new Error('');
  }
} catch (e) {
  // ...
}
```


### axios
- HTTP 상태 코드가 200~299 범위가 아니면 자동으로 에러를 던짐.
- 응단 데이터를 바로 `res.data`로 사용할 수 있음.
```js
try {
  const res = await axios.get('/api/data');
  console.log(res.data);
} catch (e) {
  // ...
}
```