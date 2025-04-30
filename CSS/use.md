# @import 대신 @use를 쓰세요

📌 *들어가기 전에*,

Vite로 토이 프로젝트를 진행하고 있었다. 어느 때처럼 기본 SCSS를 설정하고 @import를 통해 이용하고 있었는데 터미널에서 에러가 발생했다.
<br>`Sass @import rules are deprecated and will be removed in Dart Sass 3.0.0.`
<br>`@import`가 Dart Sass 3.0.0 버전부터는 **완전히 제거될 예정**이라고 했다.

## 대체자 `@use`
```scss
@import './variables.scss';
// =>
@use './variables';

// 작성 방법
.color {
  color: variables.$color;
}
```
### 파일명이 너무 길 경우 `as`를 써보자!
```scss
@use './variables' as v;

.color {
  color: v.$color;
}

// v 쓰기도 너무 귀찮다? => *을 써도 된다
// 💩 권장하진 않습니다?
@use './variables' as *;
.color {
  color: $color;
}
```

<br>

## 왜 바뀌었나?
기존 `@import`는 여러가지 문제점이 있다. 
### 같은 파일을 여러 번 불러오면 중복 적용됨
```scss
// a.scss
@import 'common';

// b.scss
@import 'common';

// main.scss => common.scss가 두 번 적용된다.
@import 'a';
@import 'b';
```

### 전역 스코프로 스타일이 퍼짐
```scss
// variables.scss
$primary-color: red;

// theme.scss
$primary-color: blue;

@import 'variables';
@import 'theme';

body {
  color: $primary-color; // ❓ red일까 blue일까?
}
```

`@use`를 사용하게 되면, 변수명을 사용해서 접근하므로 명확해진다.
```scss
@use 'variables' as v;
@use 'theme' as t;

body {
  color: v.$primary-color; // 명확함
}
```