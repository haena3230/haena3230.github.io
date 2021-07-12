---
title: Javascript의 복사
categories: [Javascript_Typescript]
comments: true
---

JavaScript는 배열 객체를 복사하는 개념에 두 가지가 있습니다. `원시타입 복사`와 `참조타입 복사`를 구분하고,
`얕은 복사`인 `Shallow copy`와 `깊은 복사`인 `Deep copy` 개념을 정리해 두기 위한 포스트 입니다.

## JavaScript 복사

일반 문자열, 숫자, 불린 같은 경우는 원시타입 복사로,

```jsx
test = 2;
test_copy = test;
```

간단하게 복사와 사용이 가능합니다.
객체(배열, 일반 객체, 함수)도 마찬가지이긴 하지만, 복사된 값을 조작할 때 차이가 있습니다.

## JavaScript 객체 복사

다음과 같이 배열과 같은 객체를 복사한다면, 값을 복사하는 게 아니라 `참조(메모리의 주소)`를 복사합니다.
동일한 객체를 가리키기 때문에 값이 같이 바뀌게 됩니다.

```jsx
array = [1, 2, 3, 4];
array_copy = array;

array.splice(0, 1);

console.log(array, array_copy);
// [2,3,4,5] , [2,3,4,5]
```

## Shallow copy(얕은 복사)

얕은 복사는 가장 상위 객체만 새로 생성되고 내부 객체들은 참조 관계인 경우를 의미합니다.
하나의 리스트일 경우에는 위와 달리 따로 변경이 가능하지만, 리스트 내의 리스트가 변경될 경우 같이 값이 변하게 됩니다.

가장 대표적인 얕은 복사 연산자로 `spread operator`와 `assign`가 있습니다.

```jsx
array = [1, [1, 2]];
array_copy = [...array];

array.push(5);
console.log(array, array_copy);
// [1,[1,2],5] [1,[1,2]]

array[1].push(7);
console.log(array, array_copy);
// [1,[1,2,7],5] [1,[1,2,7]]
```

## Deep copy(깊은복사)

깊은 복사의 경우 내부 객체까지 모두 새로 생성된 것으로, 다른 배열 객체의 값 변화가 있어도 영향을 받지 않습니다.

### 1. JSON객체의 메소드 이용하기

`JSON.stringify` 는 자바스크립트 `객체`를 `JSON문자열`로 변환시킵니다.

반대로 `JSON.parse`는 `JSON문자열`을 자바스크립트 `객체`로 변환시킵니다.

JSON문자열로 변환했다가 다시 객체로 변환하기에, 객체에 대한 참조가 없어진 것입니다.

하지만 이 방법에는 2가지 문제점이 있는데요.

다른 방법에 비해서 성능적으로 느리다는 점과, JSON.stringify 메소드가 function을 undefined로 처리한다는 점입니다.

```jsx
const cloneObj = (obj) => JSON.parse(JSON.stringify(obj));

const original = {
  a: 1,
  b: {
    c: 2,
  },
};

const copied = cloneObj(original);

original.a = 1000;
original.b.c = 2000;

console.log(copied.a); // 1
console.log(copied.b.c); // 2
```

```jsx
const cloneObj = (obj) => JSON.parse(JSON.stringify(obj));

const original = {
  a: 1,
  b: {
    c: 2,
  },
  d: () => {
    console.log("hi");
  },
};

const copied = cloneObj(original);

console.log(copied.d); // undefined
```

### 2. Lodash의 deepclone 함수 사용하기

Lodash는 많은 사람들이 사용해오고 안정성이 증명된 라이브러리입니다. Lodash는 많은 메소드들을 제공하는데요. 그 중 하나인 deepclone메소드를 사용하면 깊은복사가 가능합니다.

```jsx
const clonedeep = require("lodash.clonedeep");

const original = {
  a: 1,
  b: {
    c: 2,
  },
  d: () => {
    console.log("hi");
  },
};

const deepCopied = clonedeep(original);

original.a = 1000;
original.b.c = 2000;

console.log(deepCopied.a); // 1
console.log(deepCopied.b.c); // 2
console.log(deepCopied.d()); // 'hi'
```
