# 웹 접근성, `<aria>`

📌 *들어가기 전에*,

<img src="https://lh5.googleusercontent.com/proxy/OKYXhKxOZEe0j388jzlv9Hg27Zjb6mflxOomLDtlq9F2kiscSXX9yMMtN0pu8dcXj2LO1TUwdqfhGwckFK8xxwOPGiDPxj2Bkw" width="200"/>

우연히, 한 회사의 채용 공고를 보게 되었다. 웹 접근성에 대한 내용이 필수 요소였다.
<br>내가 아는 웹 접근성은 시멘틱 태그, 이미지 alt속성, 헤딩(h1~h6) 태그 순차적 사용이 전부였는데.
<br>웹서치를 하다보니 `aria`태그에 관한 설명이 있었다.
<br>오늘은 이것에 대해 공부해봐야겠다!

## aria 태그?
### 정의
웹 접근성을 높이기 위한 **WAI-ARIA**(Web Accessibility Initiative - Accessible Rich Internet Applications)(웹 접근성 계획 - 접근 가능한 풍부한 인터넷 애플리케이션)의 속성(attribute)들을 말한다.
<br>이 속성들은 시각장애인 등 보조 기술을 사용하는 사용자들이 웹 애플리케이션을 더 잘 이해하고 사용할 수 있도록 도와준다.

### 목적
기본 HTML 요소로는 표현하기 어려운 **동적인 UI 요소**(예: 드롭다운, 탭, 모달 등)에 **역할, 상태, 속성**을 정의하여 보조 기술(스크린 리더 등)이 그것을 인식하게 해준다.

<br>

## 주요 aria 속성
### `role`
요소의 역할을 명확히 정의
<br>스크린 리더는 `div`태그를 버튼으로 인식한다.
```
<div role="button">Click me</div>
```


### `aria-label`
요소의 이름을 설명
<br>시각적으로는 `X`지만, 스크린 리더는 "닫기"로 읽힘
```
<button aria-label="닫기">X</button>
```

### `aria-hidden`
요소를 스크린 리더에서 무시함
```
<div aria-hidden="true">이건 스크린 리더가 무시함</div>
```

### `aria-expanded`
드롭다운, 아코디언 등 **동적 콘텐츠**가 확장되거나 축소될 때, 그 상태를 나타내는 데 사용.
<br>해당 요소가 열려 있는지, 닫혀 있는지를 보조 기술(스크린 리더 등)에 알려준다.
```
<button aria-expanded="false">메뉴 열기</button>
<div id="accordion-content" hidden>
    ...아코디언 자리
</div>

<script>
    const button = document.querySelector('button');
    const content = document.querySelector('#accordion-content');

    button.addEventListener('click', () => {
        const isExpanded = button.getAttribute('aria-expanded') === 'true';
        
        content.hidden = isExpanded;
        button.setAttribute('aria-expanded', !isExpanded);
    });
</script>
```

### `aria-current`
사용자가 현재 활성화된 항목을 식별할 수 있도록 돕는 속성.
<br>주로 **탭, 네비게이션 메뉴, 페이지 내 내비게이션 링크** 등에서 사용되어, 보조 기술(스크린 리더)이 현재 위치를 명확하게 알려주도록 한다.
```
<nav>
    <ul>
        <li><a href="/home" aria-current="page">홈</a></li>
        <li><a href="/about">소개</a></li>
        <li><a href="/contact">연락처</a></li>
    </ul>
</nav>
```
#### `aria-current`에 들어갈 수 있는 속성
- `page`: 현재 페이지를 나타냄


- `step`: 단계별 프로세스에서 현재 단계를 나타낼 때 사용


- `location/date/time`: 현재 위치/날짜/시간을 나타냄.