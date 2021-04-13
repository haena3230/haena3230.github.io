[원본](https://www.notion.so/haena3230/GraphQL-Client-9dab0ee6db3c465284f639bed3619007) 포스트를 블로그로 이동했습니다.

# GraphQL Client

이전까지의 프로젝트에서는 **REST**방식으로 서버와 통신하며, **Redux**를 활용하였는데, **ContestBox** 프로젝트를 진행하며 클라이언트 시스템에서 필요한 데이터만을 효율적으로 가져다 쓸 수 있는 **GraphQL** 기반의 API를 활용하여 적용해 보았다.

### GraphQL Concept Note

[graphQL](https://www.notion.so/haena3230/graphQL-9be1a4005ce143238db940e588ce0b05)

클라이언트에서 GraphQL을 다루기 위한 라이브러리로 크게 세 가지가 있다.

1. Relay
2. Apollo Client
3. urql

결론적으로 대중적인 프레임 워크를 위한 바인딩을 제공하며, 쉽고 유연하다는 장점을 가지고 있는 **Apollo Client**를 선택하여 경험해 보았다.

## 1. Relay

Facebook이 직접 만든 GraphQL 클라이언트로, 2015년에 GraphQL와 함께 오픈 소스로 공개되었다. Relay는 성능이 크게 최적화되어있으며, 가능한 한 네트워크 전송을 최소화하도록 되어있다. 흥미로운 점으로, Relay는 본래 **라우팅** 프레임워크로서 탄생되었고 데이터를 불러오는 역할은 이후에 추가되었다. 즉, 강력한 데이터 관리 솔루션으로 진화하여, 자바스크립트 어플리케이션에서 사용할 수 있는 GraphQL API 인터페이스로 거듭났다.

하지만 **Learning Curve** 가 상당한 복잡한 프레임워크로, GraphQL을 바로 사용하기에는 적합한 선택이 아닐 수 있다.

## 2. Apollo Cilent

Apollo Client는 커뮤니티 주도 하에 개발되어, 이해하기 **쉽고 유연**하며 **강력**한 GraphQL 클라이언트를 추구한다. 현재 React, Angular, Ember, Vue와 같은 **대중적인 프레임워크**를 위한 바인딩을 제공하는 자바스크립트 클라이언트가 존재하며, **iOS와 Android**를 위한 초기 버전의 클라이언트도 존재한다. Apollo는 서비스 개발에 바로 사용될 수 있는 수준의 클라이언트이며, 캐싱, 옵티미스틱 UI, 구독 지원 등과 같은 편리한 기능들을 제공한다.

## 3. urql

Relay와 Apollo에 비하면 **최신 프로젝트**이며, 보다 **역동적**인 접근을 취하는 GraphQL 클라이언트이다. React에 크게 초점을 두고 개발되었지만, 그 핵심에서는 **간단함**과 **확장성**을 목표로 하고 있다. 효율적인 GraphQL 클라이언트를 만들기 위한 기본적인 구조만이 주어지지만, "Exchanges"를 통하여 GraphQL 동작과 결과를 처리하기 위한 완전한 제어를 제공한다. urql에서 제공하는 퍼스트 파티 Exchange인 @urql/exchange-graphcache를 사용하면 Apollo와 거의 동등한 기능을 제공하지만, 보다 적은 용량(Footprint)와 집중화된 API만으로도 가능해진다.

# Apollo Client

### 응답 구조

- RESTful API 는 하나의 Endpoint 에서 돌려줄 수 있는 응답의 구조가 정해져 있는 경우가 많다.
- 반면, GraphQL 은 사용자가 응답의 구조를 자신이 원하는 방식으로 바꿀 수 있다.

→ HTTP 요청의 횟수를 줄일 수 있고, 요청 Size를 줄일 수 있다.

### 제공기능

- 내장캐시

API 호출의 반복을 줄이기 위해 Redux에 데이터를 저장하는 과정이 필요했는데, Apollo Client에서 제공해주는 내장 캐시를 사용하여 그 과정을 생략할 수 있는 장점이 있다.

- loading, error

라이브러리에서 상태를 체크하여 반환해주는 기능을 제공해 주어 직접 요청 상태를 확인하고 반환하는 로직을 구현하지 않고 상태에 따라 뷰만 바꿔주면 되는 장점이 있다.

- variables 옵션

필요한 데이터들을 가져올 쿼리를 작성하고 React **useQuery** Hook의 구성 옵션인 **variables**를 통해 변수를 편리하게 작성할 수 있다.

## EndPoint

- RESTful은 다양한 Endpoint가 존재 하는 반면, gql은 단 하나의 Endpoint가 존재한다.
- Postman과 같은 문서화 과정 없이도 서버와 클라이언트가 효율적으로 작업할 수 있다.

### Query 작성

```jsx
export const GET_LISTS = gql`
  query($categories: [ID!], $search: String, $sort: ContestsSortType) {
    contests(categories: $categories, search: $search, sort: $sort) {
      edges {
        node {
          id
          title
          hits
          categories {
            id
            label
          }
          application {
            status
            period {
              endAt
            }
          }
        }
      }
    }
  }
`;
```

### react hook

**useQuery**, **useMutation** 등의 **hook** 을 사용하여 데이터를 로드, 업로드

```jsx
const { loading, error, data } = useQuery(GET_LISTS, {
  variables: { search: search, sort: sortStatus },
});
```

### Cache update

쿼리 결과를 **cache** 에 저장하여 필요시 바로 읽어와 속도가 장점이 있지만, 캐시된 데이터가 최신 상태인지 확인해야 할 때도 있다.

- **polling**

폴링은 쿼리가 지정된 간격으로 주기적으로 실행되도록하여 서버와의 거의 실시간 동기화를 제공한다. **useQuery**가 반환 하는 **startPolling**및 **stopPolling**함수를 사용하여 동적으로 폴링을 시작하고 중지 할 수도 있다.

```jsx
const { loading, error, data } = useQuery(GET_DOG_PHOTO, {
  variables: { breed },
  pollInterval: 500,
});
```

- **refetching**

사용자의 특정 작업에 대한 응답으로 쿼리 결과를 새로 고칠 수 있다.

```jsx
...
const { loading, error, data, refetch } = useQuery(GET_DOG_PHOTO, {
    variables: { breed }
  });
...
<button onClick={() => refetch()}>Refetch!</button>
...
```

mutation 시에 업데이트 하기 위해서는 **update**와 같은 **useMutation** 제공 **options** 등을 사용할 수 있다.

### 공식문서

[Get started](https://www.apollographql.com/docs/react/get-started/)

### 적용 Github Repository

[바로가기](https://github.com/contestbox/android)
