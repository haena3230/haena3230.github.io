---
title: Android - Unsupported Modules Detected
categories: [problem]
comments: true
---

## The Problem

React Native 프로젝트를 진행하며, 새로운 라이브러리를 추가할 일이 생겨 `yarn add` 명령어를 통해 라이브러리를 추가했습니다. 그 이후에 갑자기 `Android Studio`를 통해 `Gradle` 동기화가 진행이 되지 않고 다음과 같은 에러 메시지가 나타났습니다.

** Unsupported Modules Detected: Compilation is not supported for following modules: [프로젝트명] Unfortunately you can't have non-Gradle Java modules and Android-Gradle modules in one project. **

## The Solution

안드로이드 스튜디오 4.0.1 버전 기준이며, 저는 다음과 같은 방법으로 해결할 수 있었습니다.

1. 프로젝트 닫기
2. 안드로이드 스튜디오 IDE 닫기
3. [프로젝트]/.idea 폴더 삭제
4. 프로젝트 내의 모든 .iml 파일 삭제
5. 안드로이드 스튜디오 재 실행 후 프로젝트 import

[참고](<[https://stackoverflow.com/questions/30142056/error-unfortunately-you-cant-have-non-gradle-java-modules-and-android-gradle](https://stackoverflow.com/questions/30142056/error-unfortunately-you-cant-have-non-gradle-java-modules-and-android-gradle)>)
