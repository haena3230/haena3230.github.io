---
title: Javascript의 map & reduce 메서드
categories: [Javascript_Typescript]
comments: true
---

map과 reduce 메서드에 대해 알아보겠습니다. 알아두면 다양한 곳에 활용할 수 있으며, 맵리듀스라고 이름지어진 프레임워크도 있습니다.

## map

map 메서드는 다음과 같이 사용합니다.

`배열.map((요소, 인덱스, 배열) => { return 요소 });`

map의 기본 원리는 간단합니다. 반복문을 돌며 배열 안의 요소들을 1대1로 짝지어 줍니다.

```jsx
const oneTwoThree = [1, 2, 3];
let result = oneTwoThree.map((v) => {
  console.log(v);
  return v;
});
// 콘솔에는 1, 2, 3이 찍힘
oneTwoThree; // [1, 2, 3]
result; // [1, 2, 3]
oneTwoThree === result; // false
```

반복문으로 요소를 순회(1, 2, 3 순서로)하면서 각 요소를 어떻게 짝지어줄지 알려줍니다. 함수가 그냥 return v를 하기 때문에 같은 값을 그대로 짝짓습니다. 알아둘 점은, map을 실행하는 배열과 결과로 나오는 배열이 다른 객체라는 것입니다. 기존 배열을 수정하지 않고 새로운 배열을 만들어냅니다. 단, 배열 안에 객체가 들어있는 경우, 객체는 공유됩니다.

이번에는 각 요소에 1씩 더한 값을 반환해보겠습니다.

```jsx
result = oneTwoThree.map((v) => {
return v + 1;
});
result; // [2, 3, 4]
규칙적인 배열만 반환할 수 있는게 아니라, 함수 안에 적어준대로 반환할 수 있기 때문에 자유도가 높습니다.

result = oneTwoThree.map((v) => {
if (v % 2) {
return '홀수';
}
return '짝수';
});
result; // ['홀수', '짝수', '홀수']
```

정리하자면, map은 배열을 1대1로 짝짓되 기존 객체를 수정하지 않는 메서드입니다.

## reduce, reduceRight

### reduce 메서드는 다음과 같이 사용합니다.

`배열.reduce((누적값, 현잿값, 인덱스, 요소) => { return 결과 }, 초깃값);`

이전값이 아니라 누적값이라는 것에 주의하셔야 합니다. 누적값이기 때문에 다양하게 활용할 수 있습니다. 먼저 흔한 덧셈 예시를 살펴봅시다.

```jsx
result = oneTwoThree.reduce((acc, cur, i) => {
  console.log(acc, cur, i);
  return acc + cur;
}, 0);
// 0 1 0
// 1 2 1
// 3 3 2
result; // 6
```

acc(누적값)이 초깃값인 0부터 시작해서 return하는대로 누적되는 것을 볼 수 있습니다. 초깃값을 적어주지 않으면 자동으로 초깃값이 0번째 인덱스의 값이 됩니다.

```jsx
result = oneTwoThree.reduce((acc, cur, i) => {
  console.log(acc, cur, i);
  return acc + cur;
});
// 1 2 1
// 3 3 2
result; // 6
```

### reduceRight는 reduce와 동작은 같지만 요소 순회를 오른쪽에서부터 왼쪽으로 한다는 점이 차이입니다.

```jsx
result = oneTwoThree.reduceRight((acc, cur, i) => {
  console.log(acc, cur, i);
  return acc + cur;
}, 0);
// 0 3 2
// 3 2 1
// 5 1 0
result; // 6
```

### map과 filter과 같은 함수형 메서드를 모두 reduce로 모두 구현할 수 있습니다.

```jsx
result = oneTwoThree.reduce((acc, cur) => {
  acc.push(cur % 2 ? "홀수" : "짝수");
  return acc;
}, []);
result; // ['홀수', '짝수', '홀수']
```

초깃값을 배열로 만들고, 배열에 값들을 push하면 map과 같아집니다. 이를 응용하여 조건부로 push를 하면 filter와 같아집니다. 다음은 홀수만 필터링하는 코드입니다.

```jsx
result = oneTwoThree.reduce((acc, cur) => {
  if (cur % 2) acc.push(cur);
  return acc;
}, []);
result; // [1, 3]
```

이와 같이 sort, every, some, find, findIndex, includes도 다 reduce로 구현 가능합니다.

map과 reduce 외에도, 배열의 메서드인 sort, filter, every, some, find, findIndex, includes 정도는 알아두시면 좋습니다. 오늘 reduce만 있어도 다른 메서드들을 다 구현할 수 있다는 것을 배웠기 때문에, 다른 메서드를 까먹으면 reduce로 구현하시면 됩니다!

[참고 포스트](https://www.zerocho.com/category/JavaScript/post/5acafb05f24445001b8d796d)
