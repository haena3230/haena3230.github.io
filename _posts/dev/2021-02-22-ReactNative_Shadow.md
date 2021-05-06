---
title: React Native에서 OS별 Shadow 넣는 방법
categories: [dev]
comments: true
---

react native 에서 OS에 따라 그림자 효과를 줄 수 있는 방법이 다르다.

### 안드로이드

- elevation 사용

0~5 까지의 값을 넣을 수 있으며, 숫자가 커질수록 shadow가 커진다.

### IOS

그림자의 세세한 효과를 지정할 수 있다.

- shadowColor
- shadowOffset
- shadowOpacity
- shadowRadius

Plafrom.select를 사용하여 Platfrom에 따라 스타일을 지정해 줄 수 있다.

```jsx
...Platform.select({
      ios: {
        shadowColor: "rgb(50, 50, 50)",
        shadowOpacity: 0.5,
        shadowRadius: 5,
        shadowOffset: {
          height: -1,
          width: 0,
        },
      },
      android: {
        elevation: 3,
      },
    })
```
