---
title: Typescript : Javascript [A-Z]
categories: [Javascript$Typescript]
comments: true
---

`Array.from()` 메서드는 유사 배열 객체(array-like object)나반복 가능한 객체(iterable object)를 얕게 복사해새로운Array 객체를 만듭니다.

```jsx
console.log(Array.from("foo"));
// expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], (x) => x + x));
// expected output: Array [2, 4, 6]
```

[문서](<[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)>)

# String.fromCharCode

`String.fromCharCode()` 메소드는 UTF-16 코드 유닛의 시퀀스로부터 문자열을 생성해 반환합니다.

```jsx
console.log(String.fromCharCode(189, 43, 190, 61));
// expected output: "½+¾="
```

### 알파벳 A부터 Z까지 배열 만들기

```jsx
const alphabet = Array.from({ length: 26 }, (v, i) =>
  String.fromCharCode(i + 65)
);
```
