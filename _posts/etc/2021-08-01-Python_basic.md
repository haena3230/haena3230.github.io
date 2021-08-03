---
title: Python basic grammer
categories: [etc]
comments: true
---

갑작스럽게 cos pro를 해 볼 기회가 생겨 파이썬으로 코딩테스트를 준비했습니다. 그과정에서 간단한 문법적 요소들을 정리했습니다.

### array에 추가

- append()

→ 끝에 객체 추가

- extend()

→ 입력한 iterable 자료형의 항목 각각을 array의 끝에 하나씩 추가

- insert(i,x)

→ 인덱스 i 에 x 객체 추가

### list 값 삭제

- del list(index)

### list 값 변경

- list[index] = chg

### 길이

- 문자형 길이 len()
- 숫자 길이

→ 문자열로 변경 후 len(str(x))

### for문

- 기본구조

→ for 변수 in 리스트:

- 튜플 대입 for문

```python
list =  [(1,2), (2,3)]
for (first, last ) in list:
	print(first+last)

3
5
```

- continue 만나면 for문 위로
- range : 숫자 리스트를 자동으로 만들어 주는 함수

```python
marks = [90, 25, 67, 45, 80]
for number in range(len(marks)):
    if marks[number] < 60:
        continue
    print("%d번 학생 축하합니다. 합격입니다." % (number+1))
```

### 사칙연산

- 덧셈

* 뺄셈

- 곱셈

\*\* 거듭 제곱

/ 나누기

// 나누기 연산 후 소수점 이하의 수를 버리고 정수 부분만 구함

% 나누기 연산 후 몫이 아닌 나머지를 구함

### 유니코드 값으로

- ord("A")

### 배열 나누기

```python
test = [1,2,3,4,5,6]
left = test[:2] # -> [1, 2] (인덱스 2-1 까지)
right = test[3:] # -> [4, 5, 6] ( 인덱스 3부터 )

```

### zip 함수

```python
>>> numbers = [1, 2, 3]
>>> letters = ["A", "B", "C"]
>>> for pair in zip(numbers, letters):
...     print(pair)
...
(1, 'A')
(2, 'B')
(3, 'C')
```

### 리스트에 특정 값이 있는지 체크하기

```python
if item in list:
    print('리스트에 값이 있습니다.')
else:
    print('리스트에 값이 없습니다.')
```

### 리스트에 특정 값이 없는지 체크하기

```python
if item not in list:
    print('리스트에 값이 없습니다.')
else:
    print('리스트에 값이 있습니다.')
```

### 반복문 enumerate

- 반복문 사용 시 몇 번째 반복문인지 확인이 필요할 수 있습니다. 이때 사용합니다.
- 인덱스 번호와 컬렉션의 원소를 tuple형태로 반환합니다.

```
>>> t = [1, 5, 7, 33, 39, 52]
>>>for pin enumerate(t):
...     print(p)
...
(0, 1)
(1, 5)
(2, 7)
(3, 33)
(4, 39)
(5, 52)

```

- tuple형태 반환을 이용하여 아래처럼 활용할 수 있습니다.

```
>>>for i, vin enumerate(t):
...     print("index : {}, value: {}".format(i,v))
...
index : 0, value: 1
index : 1, value: 5
index : 2, value: 7
index : 3, value: 33
index : 4, value: 39
index : 5, value: 52
```

### 배열 뒤집기

- reserve()

→ 뒤집음

```python
list = [0,10,20,40]

list.reverse()

print(list)

# [40, 20, 10, 0]
```

- reserved()

→ 뒤집은 리스트 반환

```python
seqList = [1, 2, 4, 3, 5]

print(list(reversed(seqList)))

# [5, 3, 4, 2, 1]

array=[0, 10, 20, 40]

for i in reversed(array):

print(i)

'''

40

20

10

0

'''
```

- [::-1]

문자열을 [::-1] 이라는 인덱스로 호출하면,

아주 단순하게 해당 문자열을 뒤집은 결과를 반환한다.

만약 [3:0:-1] 이라는 인덱스로 호출하면,

3번 인덱스부터 1번 인덱스까지(0번 까지가 아님) 역순으로 출력해준다.

[3::-1] 과 같이 출력하면 3번 인덱스부터 0번 인덱스까지 역순으로 출력해준다.

```python

s = 'abcde'
print(s[3:0:-1])  # dcb

s **=** 'abcde'
**print**(s[3::**-**1])  *# dcba*

```

[4::-1] 은 4번 인덱스부터 0번 인덱스까지 역순으로 출력해주는데,

이 때 4번 인덱스가 마지막 인덱스라면 생략할 수 있다.

```python
s **=** 'abcde'
**print**(s[4::**-**1])  *# edcba*
**print**(s[::**-**1])  *# edcba*
```

그래서 결과적으로 [::-1] 인덱스를 주면 전체 문자열을 역순으로 출력해주는 것이다.

### 정렬

- sort()

→ 리스트 정렬함

- sorted()

→ 정렬된 리스트 반환

```python
>>> students = [
        ('홍길동', 3.9, 2016303),
        ('김철수', 3.0, 2016302),
        ('최자영', 4.3, 2016301),
]
>>> sorted(students, key=lambda student: student[2], reverse=True)
[('홍길동', 3.9, 2016303), ('김철수', 3.0, 2016302), ('최자영', 4.3, 2016301)]
```

정렬 기준 : 학번

reverse = True : 내림차순, False : 오름차순

### 절대값

- abs()

### 소수점 첫번째 자리까지 표현

- "{.1f}".format(test)
