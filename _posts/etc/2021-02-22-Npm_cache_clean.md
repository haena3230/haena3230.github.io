---
title: npm - cache clean
categories: [etc]
comments: true
---

## The Problem

express 프로젝트 실행시 'This is probably not a problem with npm. There is likely additional logging output above.' 오류와 함께 빌드가 실패하는 문제가 발생했다.

## The Explanation

- npm은 node.js에서 사용하는 패키지 관리자인데, 많은 편리한 플러그인들이 있어 유용하게 사용할 수 있다. python의 pip와 유사하다.
- npm cache는 일반적으로 npm-cache/\_cache 폴더에 저장된다. 이 디렉토리는 모든 HTTP 요청 데이터와 패키지 관련 데이터를 저장하는 캐시이다.
- npm도 버전이 다양한데, apt-get 등으로 설치할 경우에 버전이 제각각으로 설치되어, 나중에 꼬이게 되는 문제가 발생한다. 로컬에서 npm 버전을 업그레이드하거나, CI 서버에서 빌드를 할 때 등 npm의 cache를 지워서 해결한다.

## The Solution

- package-lock.json파일과 node_modules폴더를 삭제한 후, npm 캐시를 정리

```jsx
npm cache clean --force
```

- 다시 npm install 로 모듈 설치 후 실행
