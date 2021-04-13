---
title: React Native Error Boundaries
categories: [concept]
comments: true
---

# React Native Error Boundaries

앱을 실행하고 동작할 때 사용자에게 적절한 `**에러 화면**`을 보여주고, 적절하게 `**처리 방법**`을 유도하지 않는다면, 사용자가 앱에대한 신뢰성을 잃고 이탈 할 가능성이 크다. 따라서 적절한 처리 화면과 처리 방법의 로직을 구현하는 것이 중요하다. 적절한 에러 처리를 위해 `**React**` 의 `**생명주기**`를 참고할 수 있다.

React Lifecycle Note

[React 생명주기](https://haena3230.github.io/2021-01/React_Lifecycle)

### React Error Boundaries Docs

[에러 경계 (Error Boundaries) - React](https://reactjs-kr.firebaseapp.com/docs/error-boundaries.html)

# 처리방법

1. Error Boundaries
2. component stack trace
3. try catch

# Error Boundaris

어느 부분에 `에러처리 경계`를 지정할지는 개발자에게 달려있다. 서버사이드 프레임워크가 종종 충돌을 처리하는 것처럼 “무언가 잘못되었다는” 메시지를 유저에게 보여주려면 `최상위` 라우트 컴포넌트를 감싸면된다. 또한 `개별 위젯`을 에러 경계로 감싸서 어플리케이션의 나머지 부분이 충돌나지않도록 할 수 있다.

예를 들어 `페이스북 메신저`는 사이드바의 콘텐츠, 정보 패널, 대화 기록, 그리고 메시지 입력을 구분된 에러 경계로 감싸두었다. 만약 이러한 UI 영역 중 하나라도 충돌하는 경우 나머지는 대화형으로 유지된다.

또한 `프로덕션 환경`에서 처리되지 않은 예외에 대해 알아볼 수 있도록 `JS 에러 리포팅 시스템`을 사용하거나 직접 빌드하고, 고치는 것이 좋다.

# Component Stack Trace

`React 16`은 개발 모드에서 렌더링 중에 발생한 모든 에러를 `콘솔에 출력` 한다. 에러 메시지와 자바스크립트 스택에 더불어, 컴포넌트 스택 추적을 제공하여 컴포넌트 트리의 정확히 어디에서 에러가 발생했는 지 확인할 수 있다.

![https://reactjs-kr.firebaseapp.com/static/error-boundaries-stack-trace-f1276837b03821b43358d44c14072945-acf85.png](https://reactjs-kr.firebaseapp.com/static/error-boundaries-stack-trace-f1276837b03821b43358d44c14072945-acf85.png)

만약 Create React App을 사용하지 않는다면 `Babel 설정`에 수동으로 [이 플러그인](https://www.npmjs.com/package/babel-plugin-transform-react-jsx-source)을 추가할 수 있다. 이 기능은 `개발 모드`를 위해 구현하였으며 `**프로덕션 모드에서는 반드시 비활성화 하여야**` 한다.

# try catch

`error boundaries` 는 이벤트 핸들러 내의 오류를 잡지 못한다. 따라서 이를 해결하기 위해 `try` / `catch` 문을 사용할 수 있다.
