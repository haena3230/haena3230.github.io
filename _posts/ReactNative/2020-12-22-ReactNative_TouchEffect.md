---
title: React Native Touch Effect
categories: [ReactNative]
comments: true
---

`View` 를 만들 때, 버튼 요소가 필요하다면 `Button` 컴포넌트를 사용할 수 있습니다. 하지만 `React Native` 에는 버튼 컴포넌트 외에도 터치 효과를 주는 다양한 컴포넌트가 존재합니다.

- **TouchableHighlight**

터치가 되었을 때 색을 어둡게 하거나 underlayColor prop를 활용하여 특정 색을 활성화 시킬 수 있습니다.

- **TouchableOpacity**

사용자가 터치 했을 때 버튼을 불투명하게 합니다.

- **TouchableNativeFeedback**

터치 했을 때 잔물결이 일어나며, 이런 잔물결을 사용자에게 피드백을 준다는 뜻으로 사용이됩니다.

- **TouchableWithoutFeedback**

터치 했을 때 잔물결이 일어나지 않습니다.

이러한 컴포넌트를 활용하면, 조금 더 쉽고 빠르게 효과적인 버튼을 제작할 수 있으며, 컴포넌트의 prop들을 활용하면 더 효과적입니다.

하나의 예시로, 앱 개발 시 제가 가장 자주 사용하는 `TouchableOpacity` 컴포넌트는 다음과 같은 `prop` 을 활용할 수 있습니다.

- onPress
- onLongPress
- style
- tvParallaxProperties
- hasTVPreferredFocus
- nextFocusDown
- nextForcusForward
- nextFocusLeft
- nextFocusRight
- nextFocusUp

```jsx
// TouchableOpacity 컴포넌트 사용 예제(인라인 스타일)
<TouchableOpacity
        onPress={...}
        style={{
            ...
        }}>
...
</TouchableOpacity>
```

[React Native TouchableOpacity Docs](<[https://reactnative.dev/docs/touchableopacity](https://reactnative.dev/docs/touchableopacity)>)
