---
title: React Native ImageViewer
categories: [ReactNative]
comments: true
---

# Image Viewer

모바일에서 이미지를 제스처를 통해 확대하여 볼 수 있는 방법이 필요하여 라이브러리를 찾아보게 되었습니다. 그 중에서도 두 가지의 라이브러리를 사용해 보았습니다.

- `react-native-image-pan-zoom`
- `react-native-image-zoom-viewer`

두번째 라이브러리는 첫 번째 라이브러리를 보완하여 만들어진 라이브러리 인 것같습니다. 첫 번째 라이브러리인 `react-native-image-pan-zoom` 를 설치하고나서 안드로이드 스튜디오에 문제가 발생하여 삭제와 조치 후 두 번째 라이브러리인 `react-native-image-zoom-viewer` 를 사용했습니다.

- `react-native-image-zoom-viewer`

## 설치

```jsx
npm i react-native-image-zoom-viewer
```

## 사용 예제

공식 예제는 `react native` 에서 기본으로 제공하는 컴포넌트인 `Modal` 컴포넌트를 사용했지만, 프로젝트에 모달을 사용할 일이 많아 생성 해 두었던 `react-native-modal` 라이브러리가 존재했으므로, 적용은 이 라이브러리를 활용했습니다.

image url은 prop을 사용하여 local의 source를 사용할 수 있지만, 서버와 통신하여 내려 받은 이미지를 활용할 계획이었기 때문에 url 그대로 사용해 주었습니다.

배열로 삽입하면 여러 사진을 넘겨 가며 볼 수 있고, `enableSwipeDown` 과 `onSwipeDown` prop을 활용하여 밑으로 쓸어내리는 제스처를 취했을 때, Madal을 종료 할 수 있도록 했습니다.

## 실행 영상

[![ReactNative_ImageViewer_ex](http://img.youtube.com/vi/1st__gNFSE4/0.jpg)](https://youtu.be/1st__gNFSE4)

## 참고

[공식 npm 참고](https://www.npmjs.com/package/react-native-image-zoom-viewer)
