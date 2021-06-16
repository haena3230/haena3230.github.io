---
title: Android - Keystore
categories: [etc]
comments: true
---

`cotestbox` 앱을 개발하면서, `firebase` 와 연동하여 `google login` 기능을 활용하고자 했지만, [DEVELOPER ERROR] 에러가 발생하여 해결하는 과정에서 `keystore` 과정이 자주 헷갈려 확실히 정리해두기 위해 작성했습니다.

## Keystore란?

말그대로 열쇠를 뜻하며, 안드로이드 앱을 개발하고 서명하기위해 필요한 암호화된 파일 입니다.

## Keystore가 필요한 이유

암호화된 파일을 통해 증명하지 않으면, 누군가 APK를 변조하여 자신의 앱 인것 처럼 나의 의도와는 다른 기능으로 활용할 수 있습니다.

## Android Keystore 종류

- 디버깅용 키스토어
- 릴리즈용 키스토어 (업로드용)
- 구글 사이닝 키스토어

## 디버깅용 키스토어

개발 용도의 목적으로 서명할 떄 사영하는 키스토어로, 일반적으로 내가 사용하고 있는 JDK가 가지고 있는 키스토어를 사용해 서명하게 됩니다. 이러한 경우는 개발 환경마다 고유한 파일이며, JDK에서 제공하는 범용 키스토어입니다.

## 릴리즈용 키스토어

실제 서비스를 위한 키스토어로, 나 이외의 사용자가 인증 또는 서명할 수 없도록 합니다. 해당 키스토어는 주인이 암호 또는 일련의 과정을 더하고 해당 암호를 분실하면 안됩니다.

## 구글 사이닝 키스토어

기존에 안드로이드 서명 및 배포 방식에서 없던 방법으로, 최종 앱에 안전하게 서명하는 기능을 구글에서 해주는 대신 릴리즈용 서명 키스토어는 Play Console에 업로드할 때만 사용합니다.

## 프로젝트 적용

개발 용으로 테스트하기 위함이었으므로, 디버깅용 키스토어를 발급해서 사용했습니다.

- AndroidStudio의 signingReport를 통해 확인

```jsx
cd android && gradlew signingReport
```

- .android파일의 keystore를 통해 확인

```jsx
keytool -list -v -alias androiddebugkey -keystore %USERPROFILE%\.android\debug.keystore
```
