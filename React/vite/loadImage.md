# 이미지 경로 불러오기

📌 *들어가기 전에*,

Vite는 CRA와 달리 `require()`가 작동하지 않는다.
<br>CRA는 `Webpack`을 번들러로 가지고 있어 `require()`가 사용가능 했지만,
Vite의 경우는 번들러가 `Vite + ESBuild`이기 때문이다.

## 동적 이미지
### 1. 이미지 import 방식으로 가져오기
```ts
import logo from './logo.png';
```

### 2. images.ts로 라이브러리 만들어서 가져오기
```tsx
// images.ts
const IMAGES = {
    introImages: {
        character: new URL("@/assets/images/intro/character.png", import.meta.url).href,
        btnText: new URL("@/assets/images/intro/button_txt.png", import.meta.url).href,
    }
}

export default IMAGES;

// home.tsx
import IMAGES from './src/library/images.ts';
const { introImages } = IMAGES;

<img src={introImages.character} alt="대체 텍스트"/>
```

## 정적 이미지
이미지가 public 폴더 안에 있을 경우
```tsx
<img src="/logo.png" alt="로고 이미지" />
```