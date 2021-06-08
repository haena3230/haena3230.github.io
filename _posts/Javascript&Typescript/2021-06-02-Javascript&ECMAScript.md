---
title: Typescript : Javascript와 ECMAScript의 차이점
categories: [Javascript$Typescript]
comments: true
---

👀 들어가기 전에 알아보기

## 컴파일러

고급언어로 쓰인 프로그램이 컴퓨터에서 수행되기 위해서 컴퓨터가 이해할 수 있는 언어로 바꿔주어야 한다. 이러한 일을 하는 프로그램을 컴파일러라고 한다. 컴파일러는 소스를 한꺼번에 한 번만 읽고 분석한다.

## 인터프리터

컴파일러는 소스를 한 번에 기계어로 변환시키지만, 인터프리터는 코드 한 구문씩 기계어로 해석해나가면서 실행하는 프로그램이다.

## 스크립트 언어

스크립트 언어는 컴파일 과정을 요구하지 않는 언어이다. 예를 들면 C나 JAVA 같은 경우 작동하기 전에 컴파일이 필요하지만, 스크립트 언어인 JavaScript나 PHP는 컴파일이 필요하지 않다. 일반적으로 컴파일된 프로그램은 한 번에 기계어로 번역된 후 실행되기 때문에 인터프리터 언어보다 빠르다.

👉 JavaScript는 각 브라우저의 엔진에 따라 다양한 방식으로 실행 전에 컴파일이 된다. 따라서 보통 JavaScript를 인터프리터 언어라고 하지만 사실은 컴파일언어이다.

⚡️ ECMAScript와 JavaScript의 관계

## ECMAScript

ECMA는 European Computer Manufacturer's Association의 줄임말이며 정보와 통신 시스템을 위한 국제적 표준화 기구이다. (현재는 Ecma 인터내셔널로 이름을 바꿨다고 함). ECMAScript는 JavaScript와 같은 스크립트 언어의 표준을 말한다. JavaScript는 ECMAScript를 기반으로 하며 ECMAScript언어 중 가장 인기 있는 언어로 알려져 있다.

**채용 사이트의 자격요건에서 종종 'ES6문법에 익숙하신 분'이라는 문구를 볼 수 있다.**

ES6(=ES2015)이란 ECMAScript 6이라는 의미로, ECMA-262 표준의 제6판이다. ECMAScript 사양의 주요 변경 사항 및 개선사항을 명세한다. ECMA-262는 ECMA에 의해 제정된 ECMA스크립트 언어 규격을 칭하는 기술규격의 이름이다. 예를 들어 JSON에 대한 기술규격 이름은 ECMA-404이다.

## JavaScript

JavaScript는 맨 처음에는 모카(Mocha), 그리고 LiveScript로 알려졌으나 Netscape가 Java의 인기에 영향을 받아 최종적으로 JavaScript로 이름을 바꾸었다. 당시 썬 마이크로시스템즈 Java의 유명세를 얻어보고자 마케팅적 영향을 노려서 바꿨을 뿐 Java와 직접적인 연관은 없다. 이 언어가 처음 개발되었을 때 주요 목적은 Netscape 웹 브라우저에서 동적인 요소를 구현하기 위함이었다. 그 후 다른 많은 웹 브라우저들 또한 JavaScript를 탑재하기 시작했고, 다양한 브라우저에서 JavaScript가 공통되게 작동할 수 있도록 표준 규격이 필요해졌다.

이 때문에 ECMA에서 스크립트 표준을 만들게 됐다. JavaScript는 ECMAScript 사양을 준수하는 범용 스크립팅 언어이며 이 같은 표준 스크립트 언어를 ECMAScript라고 한다.

ECMAScript는 스크립팅 언어를 어떻게 만들어야 하는지를 설명하는 일종의 설명서라고 생각하면 되고,

JavaScript는 ECMAScript를 사양을 바탕으로 만들어진 언어인 것이다.

👏 기타

## JavaScript 엔진

JavaScript엔진은 JavaScript코드의 인터프리터이다. JavaScript엔진은 웹 브라우저에서 찾아볼 수 있다. 대표적으로 Google Chrome의 V8이 있으며, Mozilla Firefox의 SpiderMonkey도 있다.

이들 엔진은 각자 퍼포먼스가 다르고, 지원되는 ECMAScript도 다르다. ECMAScript가 새로운 버전을 발표하면 이에 맞춰서 JavaScript 엔진도 사양을 준수하도록 점진적으로 업데이트를 해준다. 즉, 브라우저마다 ECMAScript를 지원하는 범위가 각자 다르기 때문에, 각 브라우저마다 호환성 문제(Cross Browser Issues)가 발생한다. 이러한 문제를 위해 바벨(babel)이라는 오픈소스 JavaScript 트랜스 파일러를 사용한다. 바벨은 ES6 사양 기준으로 작성된 코드를 이전 버전과 호환되는 JavaScript버전으로 변환해준다. 주요 브라우저는 ES5까지 지원하기 때문에 바벨이 ES5코드로 변경해주어 호환성 문제를 해결 할 수 있게 해준다.
