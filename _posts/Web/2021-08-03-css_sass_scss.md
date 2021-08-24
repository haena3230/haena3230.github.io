---
title: CSS / Sass / SCSS
categories: [Web]
comments: true
---

프론트엔드 개발을 하다보면 다양하게 style을 다루게 되는데, CSS, SCSS, 그리고 SASS라는 것의 차이점과 사용하는 이유에 대해 명확하게 정리하기 위해 작성했습니다..

## 요약

결론은 SCSS와 SASS는 **CSS를 편리하게 이용할 수 있도록 도와주며 추가 기능도 있는 확장판 ( CSS를 확장하는 스크립팅언어 ) 입니다**.

그리고 Sass(SCSS)를 **CSS pre-processor** (전 처리기)라고도 하는데 이는 Sass자체로 브라우저에 적용하는 것이 아니라 CSS를 확장해서 쉽고 편리하게 쓰기 위해 쓰는 스크립팅 언어이기 때문에 컴파일을 해서 일반 CSS의 문법으로 바꾼 뒤 적용합니다.

- html 코드

```
<ul class='list'>
	<li>1</li>
	<li>2</li>
	<li>3</li>
</div>
```

이 html코드를 꾸밀 때SCSS와 SASS, 그리고 CSS의 차이를 코드로 살펴보면 다음과 같습니다.

- CSS 코드

```
.list {
  width: 100px;
  float: left;
  }
li {
  color: red;
  background: url("./image.jpg");
  }
li:last-child {
  margin-right: -10px;
  }

```

- SCSS 코드

```
.list {
  width: 100px;
  float: left;
  li {
    color: red;
    background: url("./image.jpg");
    &:last-child {
      margin-right: -10px;
    }
  }
}
```

- SASS 코드

```
.list
  width: 100px
  float: left
  li
    color: red
    background: url("./image.jpg")
    &:last-child
      margin-right: -10px

```

셋 다 같은 것을 적용했지만 약간씩 코드가 다릅니다.

---

### 1. SASS, SCSS 사용 이유

CSS가 복잡한 언어는 아니지만 작업이 크고 고도화 될수록 불편함이 생기게 됩니다.

이에 SCSS와 Sass는 불필요한 선택자(Selector)의 과용과 연산 기능의 한계, 구문(Statement)의 부재 등 프로젝트가 커지면서 복잡해지는 CSS 작업을 쉽게 해주며 가독성과 재사용성을 높여주어 유지보수가 쉬워지게 도와줍니다.

그리고 CSS의 태생적 한계를 보완하기 위해 Sass는 다음과 같은 추가 기능과 유용한 도구들을 제공합니다.

- 변수의 사용
- 조건문과 반복문
- Import
- Nesting
- Mixin
- Extend/Inheritance

### 2. Sass와 SCSS 차이

SCSS는 Sassy CSS(멋진 CSS)의 줄임말이고 SASS는 Syntactically Awesome Style Sheets (문법적으로 짱 멋진 스타일시트)의 줄임말입니다. 

우선 코드에 대한 대략적인 차이는 위의 코드에서 눈으로 확인 할 수 있습니다. 문법을 포함한 여러 차이점이 있지만 간단히 요약하자면 **SASS보다 SCSS가 뒤에 나왔고 여러가지 문법의 차이가 있지만 SCSS가 더 넓은 범용성과 CSS의 호환성 등의 장점으로 SCSS를 사용하기를 권장하고 있습니다.**

- **공식 문법**: 공식 레퍼런스는 SCSS 문법을 기준으로 모든 문법을 설명하고 예시를 보여줍니다.
- **더 넓은 사용자**: 다수의 라이브러리, 프레임워크가 SCSS 문법을 활용하는 등 새로운 문법이 더욱 널리 쓰입니다.
- **CSS 호환성**: 친근한 CSS 문법은 Sass와 CSS 사이의 심리적 틈을 줄여주고, 기능적으로도 확장성을 높입니다.
- **여러 줄 쓰기 지원**: Sass 문법은 들여쓰기와 줄 바꿈이 문법의 중요한 요소이기 때문에, 반대로 여러 줄 쓰기를 지원하지 않습니다.

[다음의 블로그를 참고했습니다.]([https://velog.io/@jch9537/CSS-SCSS-SASS](https://velog.io/@jch9537/CSS-SCSS-SASS))

[더 자세한 내용을 참고할 수 있습니다.]([https://react.vlpt.us/styling/01-sass.html](https://react.vlpt.us/styling/01-sass.html))