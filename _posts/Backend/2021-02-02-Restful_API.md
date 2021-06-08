---
title: Restful API
categories: [Backend]
comments: true
---

두번째 `Gombang` 프로젝트에서 `express` 를 통해 `Restful API`를 생성하는 것으로 백엔드 서버의 기초 연습을 시작했다.

### 이전 포스트 Express 시작하기 Note

[Express 시작하기](https://www.notion.so/haena3230/Express-b9047c63444343888e419417dc51fcb6)

---

## 라우팅

프로젝트를 생성하면 `app.js` 파일에 기본적으로 `/` 및 `/users` 주소에 대한 라우팅이 설정되어 있음.

```jsx
/*** app.js ***/

// ...

// 라우터 객체
var indexRouter = require("./routes/index");
var usersRouter = require("./routes/users");

// ...

// 라우팅
app.use("/", indexRouter);
app.use("/users", usersRouter);
```

라우팅이 어떻게 작동하는지는 `routes/index.js` 및 `routes/users.js` 파일에 구현되어 있음.

## GET

```jsx
/*** routes/users.js ***/

var express = require("express");
var router = express.Router();

const users = [
  { id: 1, name: "Node.js" },
  { id: 2, name: "npm" },
  { id: 3, name: "Pengsu" },
];

// 모든 유저 정보를 제공하는 라우팅
router.get("/", function (req, res, next) {
  res.json(users);
});

// 경로 매개변수를 사용한 라우팅: 특정 유저 정보 제공
router.get("/:id", function (req, res, next) {
  user = users.find((u) => u.id === parseInt(req.params.id));
  res.send(user);
});

module.exports = router;
```

작동 테스트 방법: 웹 브라우저에서 [http://localhost:3000/users](http://localhost:3000/users) 또는 [http://localhost:3000/users/1](http://localhost:3000/users/1) 접속 확인

## POST

```jsx
router.post("/", function (req, res, next) {
  const user = {
    id: users.length + 1,
    name: req.body.name,
  };
  users.push(user);
  res.send(user);
});
```

작동 테스트 방법: Postman 사용

- Request 종류: Post
- 접속 주소: `localhost:3000/users`
- Body 탭에서 종류는 raw, JSON 선택 후 다음과 같이 입력

  ```json
  {
    "name": "Postman"
  }
  ```

- Send 버튼을 클릭하여 Body 내용에 id와 name이 요청대로 들어있는지 확인

## PUT

```jsx
router.put("/:id", function (req, res, next) {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) {
    return res.status(404).send("ID was not found.");
  }

  user.name = req.body.name; // users 값도 바뀌네? 왜죠...
  res.send(user);
  console.log(users);
});
```

작동 테스트 방법: Postman 사용

- Request 종류: PUT
- 접속 주소: `localhost:3000/users/1`
- Body 탭에서 종류는 raw, JSON 선택 후 다음과 같이 입력

  ```json
  {
    "name": "Postman"
  }
  ```

- Send 버튼을 클릭하여 Body 내용에 name이 요청대로 수정되었는지 확인

## DELETE

```jsx
router.delete("/:id", function (req, res, next) {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) {
    return res.status(404).send("ID was not found.");
  }

  const index = users.indexOf(user);
  users.splice(index, 1);

  res.send(user);
  console.log(users);
});
```

작동 테스트 방법: Postman 사용

- Request 종류: DELETE
- 접속 주소: `localhost:3000/users/1`
- Body 탭의 종류는 none으로 설정
- Send 버튼을 클릭하여 Body 내용에 삭제된 user 정보가 맞는지 확인

### HTTP 상태 코드

- 2xx

  - 200 : OK
  - 201 : Create
  - 202 : Accept
  - 204 : No content

- 4xx
  - 400 : Bad Request
  - 401 : Unauthorized
  - 403 : Forbidden
  - 404 : Not found
  - 405 : Method not allowed
  - 409 : Conflict
  - 429 : Too many Request
- 5xx : Server Error

제작한 API와 연결할 DB(MySQL)를 Docker를 통해 연결했다.

### Container(Docker)Note

[Container(Docker)](https://www.notion.so/haena3230/Container-Docker-f7bb07a2d07e4cdbaf6f5224ec13f1ba)
