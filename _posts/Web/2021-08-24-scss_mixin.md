---
title: scss의 mixin을 활용한 반응형 웹
categories: [Web]
comments: true
---

CSS로 반응형 웹을 작성할 때 우리는 미디어 쿼리를 사용합니다. 

보통 웹 애플리케이션이 복잡해지면 하나의 CSS 파일에 모든 스타일을 작성하기 보다는 여러 파일로 나눠 작성하게 됩니다.

이 경우, 매 파일마다 미디어 쿼리를 새로 작성해야 하는 불편함이 생깁니다.

특히, CSS에는 변수가 없어서 디바이스별 `min-width`나 `max-width`의 픽셀 값을 외우고 있다가 적용해야 합니다.

**SCSS를 이용해서 간편하게 미디어 쿼리를 적용하는 방법**을 살펴보겠습니다.

## Sass/SCSS의 변수와 믹스인(Variables & mixins)

먼저, **SCSS는 Sass의 상위호환(Superset)으로 CSS와 호환될 수 있는 문법**을 가지고 있습니다.

CSS와 문법이 좀 다른 Sass에 비해 SCSS는 거의 완벽히 CSS 문법과 호환이 되어 편리합니다.

### 변수(Variables)

변수를 사용할 수 있습니다!

```jsx
$font-large: 3rem;
$font-medium: 1.6rem;
$font-default: 1.2rem;
$font-small: 0.8rem;
```

변수는 다음과 같이 사용 가능합니다, 

```jsx
body {
  font-size: #{$font-large};
}
```

### 믹스인(Mixins)

믹스인은 CSS를 묶어서 재사용할 수 있는 모듈로 만들어 줍니다.

```jsx
@mixin card-view {
  font-size: 22px;
  border: 1px solid grey;
  border-radius: 4px;
}
```

이렇게 선언한 믹스인은 다음과 같이 가져다 쓸 수 있습니다.

```jsx
div {
  @include card-view;
}
```

컴파일하면 아래처럼 바뀝니다.

```jsx
div {
  font-size: 22px;
  border: 1px solid grey;
  border-radius: 4px;
}
```

## Sass/SCSS 미디어쿼리 작성

 믹스인과 변수의 개념을 활용해 미디어쿼리를 작성해 보겠습니다.

### 변수 작성

먼저 변수를 선언하겠습니다.

우리는 모바일, 태블릿, 데스크톱 세 환경을 대응하도록 변수를 선언하겠습니다.

**_variables.scss**

```jsx
// Breakpoints
$breakpoint-mobile: 335px;
$breakpoint-tablet: 758px;
$breakpoint-desktop: 1024px;
```

Scss 파일명 앞에 언더바 `_` 가 보이시나요? 언더바를 붙여서 작성해야 파일단위로 분리되어 컴파일 되지 않습니다.

사실 `_variables.scss` 는 변수만 따로 저장해놓을 파일이기에 별도의 CSS 파일로 컴파일될 필요가 없습니다.

### 믹스인 작성

다음으로 미디어쿼리를 이용하기 쉽도록 믹스인으로 작성하겠습니다.

**_mixin.scss**

`@import "./variables";`

**불러올 때는 언더바나 확장자를 제외해도 정상 작동합니다.**

**_mixin.scss**

```jsx
@import "./variables";

@mixin mobile {
  @media (min-width: #{$breakpoint-mobile}) and (max-width: #{$breakpoint-tablet - 1px}) {
    @content;
  }
}

@mixin tablet {
  @media (min-width: #{$breakpoint-tablet}) and (max-width: #{$breakpoint-desktop - 1px}) {
    @content;
  }
}

@mixin desktop {
  @media (min-width: #{$breakpoint-desktop}) {
    @content;
  }
}
```

### 믹스인을 활용한 미디어 쿼리

자, 그럼 믹스인으로 작성한 미디어 쿼리를 사용해볼까요?

**main.scss**

```jsx
@import "../../Styles/mixins";

@include mobile {
  .img-card {
    width: 100px;
  }
}

@include tablet {
  .img-card {
    width: 200px;
  }
}

@include desktop {
  .img-card {
    width: 300px;
  }
}
```

이렇게 믹스인을 사용하면 기기별로 손쉽게 미디어쿼리를 이용할 수 있습니다.

[다음의 블로그를 참고했습니다.](https://leonkong.cc/posts/scss-media-query.html)