---
title: Apollo Client -  Error Handling
categories: [problem]
comments: true
---

`ContestBox` 프로젝트를 진행하며, `GraphQL` 을 통해 서버와 교류했고, `React Native` 에서 사용할 수 있는 `Apollo Client` 를 활용했다. `End Point` 에서 쉽게 서버상의 문제인지 확인할 수 있었고, 오류가 발생했을때 문제가 무엇인지 정확하게 파악할 필요성이 있었다.

Apollo Client Note

[Apollo Client](https://www.notion.so/haena3230/Apollo-Client-9dab0ee6db3c465284f639bed3619007)

## 오류 유형

- GraphQL 오류 : 성공적인 데이터와 함께 나타날 수있는 GraphQL 결과의 오류
- 서버 오류 : 성공적인 응답이 형성되지 못하게하는 서버 내부 오류
- 트랜잭션 오류 : `update` 변이와 같은 트랜잭션 작업 내부의 오류
- UI 오류 : 구성 요소 코드에서 발생하는 오류
- Apollo 클라이언트 오류 : 코어 또는 해당 라이브러리 내의 내부 오류

## Apollo 오류 정책

- `none`

  : Apollo Client 1.0의 작동 방식과 일치하는 기본 정책입니다. GraphQL 오류는 네트워크 오류와 동일하게 처리되며 모든 데이터는 응답에서 무시됩니다.

- `ignore`

  : Ignore를 사용하면 GraphQL 오류와 함께 반환되는 데이터를 읽을 수 있지만 오류를 저장하거나 UI에보고하지 않습니다.

- `allall`

  :

  정책을 사용하는 것이 서버에서 가능한 한 많은 데이터를 표시하면서 잠재적 인 문제를 사용자에게 알리는 가장 좋은 방법입니다. UI에서 사용할 수 있도록 데이터와 오류를 모두 저장합니다.

## 설정 예시

```jsx
const MY_QUERY = gql`
  query WillFail {
    badField
    goodField
  }
`;

function ShowingSomeErrors() {
  const { loading, error, data } = useQuery(MY_QUERY, { errorPolicy: "all" });

  if (loading) return <span>loading...</span>;
  return (
    <div>
      <h2>Good: {data.goodField}</h2>
      <pre>
        Bad:{" "}
        {error.graphQLErrors.map(({ message }, i) => (
          <span key={i}>{message}</span>
        ))}
      </pre>
    </div>
  );
}
```

## 네트워크 오류

Apollo Link를 사용할 때 네트워크 오류를 처리하는 능력은 훨씬 더 강력합니다. 이를 수행하는 가장 좋은 방법 `@apollo/client/link/error`은를 사용하여 서버 오류, 네트워크 오류 및 GraphQL 오류를 포착하고 처리하는 것입니다. 다른 링크와 결합하려면 [링크 체인 작성을](https://www.apollographql.com/docs/react/api/link/introduction/#composing-a-link-chain) 참조하십시오 .

## 예시

```jsx
import { onError } from "@apollo/client/link/error";

const link = onError(({ graphQLErrors, networkError }) => {
  if (graphQLErrors)
    graphQLErrors.map(({ message, locations, path }) =>
      console.log(
        `[GraphQL error]: Message: ${message}, Location: ${locations}, Path: ${path}`
      )
    );

  if (networkError) console.log(`[Network error]: ${networkError}`);
});
```

### [옵션](https://www.apollographql.com/docs/react/data/error-handling/#options)

오류 링크는 오류 발생시 호출되는 함수를 사용합니다. 이 함수는 다음 키를 포함하는 객체와 함께 호출됩니다.

- operation : 오류가 발생한 작업
- response : 링크 체인의 아래쪽에서 반환 된 결과
- graphQLErrors : GraphQL 엔드 포인트의 오류 배열
- networkError : 링크 실행 또는 서버 응답 errors중 GraphQL 결과 필드의 일부로 전달되지 않은 오류
- forward : 체인의 다음 링크에 대한 참조입니다. return forward(operation)콜백을 호출 하면 요청을 재 시도하여 구독 할 업스트림 링크에 대한 새 관찰 가능 항목을 반환합니다.

### [오류 무시](https://www.apollographql.com/docs/react/data/error-handling/#ignoring-errors)

조건부로 오류를 무시 `response.errors = null;`하려면 오류 처리기 내에서 설정할 수 있습니다 .

```jsx
onError(({ response, operation }) => {
  if (operation.operationName === "IgnoreErrorsQuery") {
    response.errors = null;
  }
});
```
