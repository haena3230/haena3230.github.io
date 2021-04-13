[원본](https://www.notion.so/haena3230/Pagination-1016e5d3f2b74b38bae506e21f229cb8) 포스트를 블로그로 이동했습니다.

# Pagination in Apollo Client

**ContestBox** 프로젝트를 진행하던 도중, 데이터 로딩 시간을 단축시키기 위해 ScrollView에서 페이지네이션 기능을 제공했다.

**GraphQL** 의 **Apollo Client** 를 사용했으며, **Cursor** , **Offset**, **Relay Style** 기반의 세 가지 **Pagination** 방식 중 Relay style Pagination을 선택했습니다.

## Pagination

서버에서 데이터를 가져 올 때, 모든 데이터를 한번에 가져오게 되면 데이터 로딩 시간이 길어지게 되어 **특정한 정렬 기준** 과 **지정된 개수** 를 통해 데이터를 가져오는 것이 필요합니다. 이를 **Pagination** 이라고 표현하며, 보통 두 가지의 방식을 사용합니다.

1. 오프셋 기반 페이지네이션(Offset based Pagination)
2. 커서 기반 페이지네이션(Cursor based Pagination)

## 1. Offset based Pagination

Offset 기반 페이지네이션은 원하는 데이터가 **몇 번째** 에 있다는 사실에 집중합니다.

페이지에서 1 2 3 4 5 버튼을 통해 페이지를 구분하는 방식이 보통 offset 기반의 페이지네이션 입니다. 이러한 기능을 구현하기 위해 다음의 단계를 거쳐 데이터를 보여줍니다.

1. 데이터를 전부 스캔한다.
2. offset값 이후의 데이터를 잘라낸다.
3. 잘라낸 데이터 중 표시할 게시물 갯수만큼 잘라서 리턴한다.

### Offset 기반 단점

- 데이터 변화가 있을 경우 **중복 데이터**가 노출된다.

예를 들어 1 페이지에서 10 개의 데이터를 보여주고 사용자가 1페이지를 구경하는 사이, 5개의 새로운 데이터가 업데이트 되는 상황이 발생한다면, 사용자가 2페이지로 넘어갔을 때 1페이지에서 봤던 5개의 데이터를 다시 보게 됩니다.

이것은 지정된 갯수만 순회하여 자르는 방식을 사용했기 때문입니다. offset 값이 클수록 쿼리의 퍼포먼스는 떨어질 수 있습니다.

### Offset 기반 사용 케이스

위와 같은 단점이 있기 때문에 다음과 같은 조건을 가졌을 때 사용할 수 있습니다.

- 데이터의 변화가 거의 없어 중복 데이터가 노출 될 염려가 없는 경우
- 일반 유저에게 노출되는 정보가 아니어서 중복 데이터가 노출되어도 문제가 없는 경우
- 데이터가 많지 않아 퍼포먼스 걱정이 필요 없는 경우

### docs

[Offset-based pagination](https://www.apollographql.com/docs/react/pagination/offset-based/)

## 2. Cursor based Pagination

데이터가 **몇 번째** 에 있다는 사실에 집중하는 Offset 기반 페이지네이션과는 달리 Cursor 기반은 원하는 데이터가 **어떤 데이터의 다음** 에 있는지에 집중합니다. 마지막 데이터를 기준으로, 해당 데이터 다음의 n개의 데이터를 요청하는 방식입니다.

1. cursor를 정하기 위해 DB의 unique하고, 순서가 있는 컬럼을 준비한다. **id**
2. 데이터 요청 시 **cursor**(마지막 데이터 id)와 **데이터 개수**를 활용한다.

### Cursor 기반 사용 케이스

- 업데이트의 빈도가 많은 경우
- 단일 방향으로 이동하는 데이터 세트로 작업하는 경우

### docs

[Cursor-based pagination](https://www.apollographql.com/docs/react/pagination/cursor-based/)

## 3. Relay style Pagination

클라이언트에서 graphQL을 다루기 위해 지원하는 라이브러리는 크게 세 가지가 있다.

1. Relay
2. Apollo
3. urql

graphQL 클라이언트 참고

[GraphQL Client](https://www.notion.so/haena3230/GraphQL-Client-9dab0ee6db3c465284f639bed3619007)

**cursor** 기반의 Apollo Client에서 제공하는 pagination 이지만, **Relay**에서 제공하는 규칙을 가진 것을 Relay style Pagination 이라고 한다.

### Naming Rules

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5ef3f51-2400-45ba-8d0e-1f1436531ffa/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5ef3f51-2400-45ba-8d0e-1f1436531ffa/Untitled.png)

참고 - [https://medium.com/graphql-seoul/graphql-pagination-구현하기-relays-cursor-based-connection-pattern-72ab0daceed4](https://medium.com/graphql-seoul/graphql-pagination-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-relays-cursor-based-connection-pattern-72ab0daceed4)

다음과 같은 이미지과 같이 Relay에서는 **Connection Model**에 대한 각 스펙에 네이밍 규칙을 정의 해 놓고 있다.

**Edges** 의 **node** 와 **cursor**를 통해 객체를 표현하며, **PageInfo** 를 통해 Pagination 정보를 표현한다.

```jsx
const COMMENTS_QUERY = gql`
  query Comments($cursor: String) {
    comments(first: 10, after: $cursor) {
      edges {
        node {
          author
          text
        }
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
`;
```

### docs

[Cursor-based pagination](https://www.apollographql.com/docs/react/pagination/cursor-based/#relay-style-cursor-pagination)

### 실제 구현 방식 Note

[GraphQL relay-style cursor pagination](https://www.notion.so/haena3230/GraphQL-relay-style-cursor-pagination-c1cfbba042134dbaa822d5661218394a)
