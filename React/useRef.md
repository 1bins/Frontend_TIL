# useRef

## 정의
`useRef`는 **컴포넌트의 생명주기 동안 유지되는 변경 가능한 참조값**을 저장할 수 있다.
```js
const ref = useRef(initialValue);
```
- `ref.current`로 값을 읽거나 수정할 수 있다.
- **값이 바뀌어도 컴포넌트를 리렌더링하지 않는다.**
- **DOM 접근**과 **렌더링과 무관한 값 저장**에 주로 사용된다.

<br>

## 🧪 예제로 이해하기
### DOM 요소에 접근할 때
```js
const inputRef = useRef(null);

useEffect(() => {
  inputRef.current?.focus();
}, []);
```
#### 🎈 DOM 요소를 참조해서 스타일이나 클래스 조작할 때는?
`useLayoutEffect`를 사용하는 것이 더 안전하다.
- `useLayoutEffect`는 렌더링 후 DOM이 업데이트되기 직전의 시점에 동기적으로 실행.
```js
function MyComponent() {
  const divRef = useRef(null);

  useLayoutEffect(() => {
    if (divRef.current) {
      divRef.current.classList.add('add');
    }
  }, []);

  return <div ref={divRef}>Hello</div>;
}
```

### 클로저 문제를 피하고 최신 값을 유지할 때
`setInterval`, `setTimeout` 등에서 오래된 상태값을 참조하지 않도록 하기 위해 사용한다.

```js
const [count, setCount] = useState(0);
const countRef = useRef(count);

// 최신 count 값을 계속 저장
useEffect(() => {
  countRef.current = count;
}, [count]);

useEffect(() => {
  const id = setInterval(() => {
    setCount(countRef.current + 1); // 항상 최신 값 사용
  }, 1000);
  return () => clearInterval(id);
}, []);
```