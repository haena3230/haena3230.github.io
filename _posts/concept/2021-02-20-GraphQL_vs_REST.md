---
title: GraphQL vs REST
categories: [concept]
comments: true
---

# GraphQL vs REST

`GraphQL` 은 Server API 를 구성하기 위해 Facebook 에서 만든 `Query Language` 이다. `ContestBox` 프로젝트를 진행하며 새롭게 접했으며, 프론트엔드 개발을 맡았기 때문에 `Apollo Client`를 활용하여 적용했다.

## Apollo Client Note

[Apollo Client](https://www.notion.so/haena3230/Apollo-Client-9dab0ee6db3c465284f639bed3619007)

### Server API

- Server API (혹은 Server-side web API) 는 적절한 요청을 하였을 때,그에 맞는 응답을 되돌려주는 창구 (Endpoint) 를 Web 을 통해 노출한 것을 말한다.
- 이 Server API 를 만드는 방법론 중 하나로 `REST`라는 것이 있으며,이 방법론은 많은 Server API 들을 구성하기 위해 사용되어왔고, 또 현재도 많이 사용되고 있다.

### REST

- REpresentational State Transfer
- 모든 Resource (자료, User, …) 들을 하나의 `Endpoint` 에 연결해놓고,각 Endpoint 는 그 Resource 와 관련된 내용만 관리하게 하자는 방법론이다.
- REST 의 조건을 만족하는 API 를 `RESTful API` 라고 부르고, 이런 방식으로 API 를 작성하는 것을 RESTful 하다고 한다.
- RESTful API 는 Resource 마다 하나의 Endpoint 를 가지고, 그 Endpoint 에서 그 Resource 에 대한 (거의) 모든 것을 담당한다.
- 하나의 Endpoint 에서 돌려줄 수 있는 응답의 구조가 정해져 있는 경우가 많다.

### graphQL

- GraphQL 은 Graph Query Language 의 줄임말이다.
- Query Language 는 정보를 얻기 위해 보내는 질의문(`Query`)을 만들기 위해 사용되는 Computer 언어의 일종이다. GraphQL 은 이런 Query Language 중에서도 Server API 를 통해 정보를 주고받기 위해 사용하는 Query Language 이다.
- 정보를 사용하는 측에서 원하는 대로 정보를 가져올 수 있고, 보다 편하게 정보를 수정할 수 있도록 하는 표준화된 Query language 를 만들게 되었다.
- 전체 API 를 위해서 단 `하나의 Endpoint` 만을 사용한다.
- 사용자가 응답의 `구조`를 자신이 원하는 방식으로 바꿀 수 있다.

### GraphQL 작업 유형(operation type)

- query: 데이터를 받아올 때 사용한다.
- mutation: 데이터를 생성, 수정, 삭제할 때 사용한다.
- subscription: 웹소켓을 사용해 실시간 양방향 통신을 구현할 때 사용한다.

### GraphQL vs RESTful

1. GraphQL
   - 서로 다른 모양의 다양한 요청들에 대해 응답할 수 있어야 할 때
   - 대부분의 요청이 CRUD(Create-Read-Update-Delete) 에 해당할 때
2. RESTful
   - HTTP 와 HTTPs 에 의한 Caching 을 잘 사용하고 싶을 때
   - File 전송 등 단순한 Text 로 처리되지 않는 요청들이 있을 때
   - 요청의 구조가 정해져 있을 때
