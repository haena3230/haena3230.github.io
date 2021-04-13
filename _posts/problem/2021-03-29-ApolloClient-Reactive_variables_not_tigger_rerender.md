---
title: Apollo Client - Reactive variables not trigger re render after updating the value
categories: [problem]
comments: true
---

# The Problem

`Relay style pagination`을 적용한 후

1. makeVar
2. useState

의 방법을 사용하여 변수를 업데이트 해도 `re-rendering`이 `trigger` 되지 않고, `refetching` 시에만 `re-rendering` 되는 문제가 발생했다.

relay-style pagination note

[GraphQL relay-style cursor pagination](https://www.notion.so/haena3230/GraphQL-relay-style-cursor-pagination-c1cfbba042134dbaa822d5661218394a)

makeVar note

[Apollo Client : makeVar](https://www.notion.so/haena3230/Apollo-Client-makeVar-16e76f86e48043acaa9e498920884291)

# The Explanation

해결 방법을 찾기 위해

1. `makeVar`를 전역 변수로 사용하는 방법
2. `fetchMore`을 활용방법
3. query의 `variables 구조`를 바꿔보는 방법

을 적용해 보았다.

### 1. makeVar 확장

makeVar를 활용해 client에서 사용할 수 있는 전역변수로 연결할 수 있다

1. query에 `@client`변수 추가
2. 변수 정의(makeVar)
3. cache의 `field`에 `read`할 수 있도록 추가

```tsx
// test 변수 추가
export const GetTest=gql`
	query GetTest{
		test @client
		...
	}
`

// makeVar 정의, cache에 규칙 지정
export const cache: InMemoryCache = new InMemoryCache({
  typePolicies: {
    Query: {
      fields: {
        getColor()
          return colorVar();
        }
      }
    }
  }
});
export const colorVar = cache.makeVar('red')
```

reactive variables note

[Apollo Client Variables](https://www.notion.so/haena3230/Apollo-Client-Variables-106586c3e4f84ef0ab8f0a746d5ed838)

→ 변수는 업데이트 되지만 re-rendering이 trigger되지 않는 문제는 똑같이 발생했다. 그리고 굳이 전역으로 다룰 필요가 없으므로 다른 방법을 찾았다.

### 2. ferchMore 활용

이벤트가 발생했을때 `cursor`가 없는 variables를 담아 `fetchMore`()을 호출하는 함수를 작성했다.

원하는 것 처럼 동작했지만 pagination을 위해 사용하는 fetchMore이기 때문에 적절한 또 다른 방법이 있을것이라 생각하고 찾아 보았다

### 3. variables에 사용하는 변수 구조 변경

variables에 들어가는 변수를 makeVar 혹은 useState를 통해 작성했다. 이 때, 변수 자체를 사용하지 않고 변수 내부의 객체를 연결할 땐 trigger되지 않았지만, 변수 자체를 사용했을 땐 상태 변화 시 자동으로 trigger, re-rendering이 되는 것을 확인했다.

```tsx
// useState 작성
interface testProps {
  id: string;
  label: string;
}
const [test, setTest] = useState<testProps>([]);

// 상태변수 내부의 객체를 variables로 사용 -> 상태변화 감지  x
const { loading, error, data, fetchMore, refetch } = useQuery(GET_LISTS, {
  variables: {
    conditions: test.id,
  },
  fetchPolicy: "cache-and-network",
});

// 상태변수 자체를 variables로 사용 -> 상태 변화 감지 후 trigger 성공
const { loading, error, data, fetchMore, refetch } = useQuery(GET_LISTS, {
  variables: {
    conditions: test,
  },
  fetchPolicy: "cache-and-network",
});
```

## The Solution

3번 방법으로 해결

useState의 변수 자체를 variables로 사용하지 않고, 변수 내의 객체를 variables로 사용하여 상태 변화가 일어났을때 캐치하여 reload 하지 못했다.

가장 간단하게 사용할 수 있는 방법이기 때문에 선택했지만, api call을 정상적으로 한번씩만 하는지 체크해 볼 필요성이 있다.

## 참고

From Apollo Client 3.2.0 you can use the useReactiveVar hook to get the data :

→ 3.2.0 버전부터 hook를 활용하는 `useReactiveVar` 를 통해 read를 할 수 있다

```tsx
import { useReactiveVar } from "@apollo/client";
import { colorVar } from "cache"

// to read
const color = useReactiveVar(colorVar);

// to update
colorVar(your preferred color)
```
