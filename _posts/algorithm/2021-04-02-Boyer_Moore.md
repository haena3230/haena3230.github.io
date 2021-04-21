---
title: Boyer - Moore 법 (문자열 검색)
categories: [algorithm]
comments: true
---

KMP 법보다 효율이 더 우수하여 실제로 많이 사용합니다.

- 문자열을 찾을 곳 : Text
- 찾고자 하는 문자열 : String

- 찾고자 하는 `String`보다 `Text`의 길이가 더 길어야합니다.
- 찾고자 하는 문자열 내의 패턴 분석을 먼저 진행합니다.

  → 어떤 문자가 들어가있는지 저장(`pattern`)

- `Text[0]`, `String[0]` 부터 시작하지만, String의 마지막 문자부터 앞으로 검색합니다.

  → 문자열 길이 StringLen, Count = 0

  → `text[StringLen]` —비교— `문자열[StringLen]`

1. 일치할때

   → 다음 순서의 문자를 비교하며 `Count++` 해줍니다. ([StringLen-1])

   **→ StringLen == Count 이면 문자열 찾음!**

2. 불일치 할때

3. 불일치한 문자가 `pattern` 에 속한 문자라면, 다음 인덱스로 넘어가서 다시 마지막 문자부터 비교를 수행합니다.

4. 불일치한 문자가 `pattern` 에 속하지 않은 문자라면, StringLen - Count 크기만큼 이동하여 마지막 문자부터 비교를 수행합니다.

### pattern에 속하지 않은 문자일 경우, 문자열을 순차적으로 비교하지 않고, 계산된 크기만큼 순서를 뛰어 넘을 수 있어 효율적입니다.
