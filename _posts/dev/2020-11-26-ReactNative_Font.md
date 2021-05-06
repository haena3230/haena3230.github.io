---
title: React Native Font
categories: [dev]
comments: true
---

# React Native Font 적용하기[Android]

## 1. 폰트 파일 다운받기

RN 프로젝트를 진행하면서, 폰트를 적용 하는 방법을 찾아 보았습니다. 구글의 `nato sans KR` 글꼴을 사용하였으며, 사용하실 글꼴 파일을 다운로드 받아 사용하면 됩니다.

- [nato sans KR download](<[https://fonts.google.com/specimen/Noto+Sans+KR](https://fonts.google.com/specimen/Noto+Sans+KR)>)

## 2. 폰트파일 추가하기

원하는 폰트 파일을 다운받았다면, 프로젝트에 파일을 추가해야 합니다. Android 는`'android/app/src/main/assets/font'` 의 폴더에 파일을 추가하면 됩니다.

## 3. 경로 설정하기

1. React Native 버전이 0.6 이상이라면 root 경로에 `react-native.config.js` 파일을 추가하여 다음과 같이 입력합니다.

```jsx
module.exports = {
  assets: [".src/assets/fonts/"],
};
```

2. React Native 버전이 0.6 미만 이라면 `package.jcon` 파일에 다음과 같이 추가합니다.

```jsx
"rnpm": {
    "assets": [
	"./assets/fonts/"
    ]
},
```

## 3 적용하기

1. React Native가 글로벌로 설치되어 있다면

```jsx
react-native link
```

2. 아니라면

```jsx
npx react-native link
```

위의 명령어를 통해 연결 과정을 거친 후 재 빌드해야 폰트 인식이 됩니다. 프로젝트에서 `font-family` 를 통해 다운받은 폰트를 사용할 수 있습니다.

저의 경우 새로운 폰트에 패딩이 적용되어 있는 것인지 적용 후 뷰들을 조금씩 수정해야 했습니다..

## 적용 방식

style.ts 파일에 사용할 글꼴을 정리하여 사용했습니다.

```jsx
// 예시
export const Styles = StyleSheet.create({
  // Thin
  m_font: {
    fontFamily:'NotoSansKR-Regular',
    fontSize: DWidth > 480 ? 20 : 16,
    color:Color.g4_color
  },
  ...
});
```
