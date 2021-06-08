---
title: Code_Spliting_React
categories: [Web]
comments: true
---

## 파일 분리 작업

- 프로젝트를 완성하여 사용자에게 제공하기 위해서는 `빌드작업`을 거쳐 `배포`가 필요하다.
- 빌드 작업을 통해 파일 크기를 최소화하거나 코드의 트랜스파일 작업 등을 하게된다.

→ `**웹팩(webpack)**`이라는 도구가 담당한다.

→ 웹팩에서 별도의 설정을 하지 않으면 모든 자바스크립트 파일이 하나의 파일로 합쳐지고 모든 CSS 파일도 하나의 파일로 합쳐진다.

## CRA(create-react-app)

- CRA로 빌드할 경우 최소 두 개 이상의 자바스크립트 파일이 생성된다.
- CRA의 기본 웹팩 설정에 SplitChunks라는 기능이 적용되어 node_modules에서 불러온 파일, 일정 크기 이상의 파일, 여러 파일간에 공유된 파일을 자동으로 따로 분리시켜 캐싱의 효과를 보여준다.

## 코드 비동기 로딩

- SPA에서 별도로 설정하지 않으면 당장 필요하지 않은 컴포넌트의 정보들도 모두 불러오면서 파일 크기가 매우 커진다

→ 로딩이 오래걸리고, 트래픽도 많이 나오게 된다.

→ 코드 비동기 로딩을 통해 필요한 함수, 객체, 컴포넌트 등을 `**필요한 시점**`에 불러 사용할 수 있다.

### 1. Dynamic import 문법

- import() 함수 형태로 메서드 안에서 사용하면 파일을 따로 분리시켜 저장한다.

→ 실제 함수가 필요한 지점에 파일을 불러와서 함수를 사용

```jsx
import notify from './notify'

const App=()=>{
	const onClick =()=>{
		notify();
	};
	return
	...
}
```

```jsx
const App=()=>{
	const onClick=()=>{
		import('./notify').then(result=>result.default());
	};
	return
	...
}
```

### 2. state 사용

- 클래스형 컴포넌트로 변환 필요
- 매번 state 선언 필요

### 3. 유틸함수 React.lazy, 컴포넌트 Suspense 사용

- 주의 : 서버사이드 렌더링을 지원하지 않음
- 컴포넌트를 렌더링하는 시점에서 비동기적으로 로딩할 수 있게 해준다.

```jsx
const SplitMe = React.lazy(() => import("./SplitMe"));
```

- Suspense는 컴포넌트를 로딩하도록 발동시키고, fallback props로 로딩이 끝나지 않았을 때 보여줄 UI를 설정할 수 있다.

```jsx
import React,{Suspense} from 'react';

...

<Suspense fallback={<div>loading...</div>}>
	<SplitMe />
</Suspense>

...
```

### 4. Loadable Components

- 코드 스플리팅을 편하게 하도록 도와주는 서드파티 라이브러리
- 서버 사이드 렌더링 지원

```jsx
const SplitMe = loadable(() => import("./SplitMe"), {
  fallback: <div>loading...</div>,
});
```

- preload()

→ 사용자에게 더 좋은 경험 제공 가능

```jsx
...
const onMouseOver=()=>{
	SplitMe.preload();
};

...
<p onMouseOver={onMouseOver}>
	Hello
</p>
...
```

- 타임아웃, 로딩 UI 딜레이, 서버사이드 렌더링 호환 ...
- 공식문서

[Delay - Loadable Components](https://www.smooth-code.com/open-source/loadable-components/docs/delay/)
