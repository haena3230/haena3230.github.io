---
title: Start Express
categories: [dev]
comments: true
---

대부분의 미들웨어가 지원되며, 커뮤니티 환경이 잘 구성되어 있는 express를 통해 두번째 `Gombang` 프로젝트의 API를 만들어 보는 것으로 결정했다.

Express VS koa Note

[Express vs koa](https://www.notion.so/haena3230/Express-vs-koa-798ed2a29e954a52867592b6779999b4)

## 환경

`express-generator` 를 통해 간단하게 서버 프로젝트를 생성하고 테스트를 위해 `Postman` 을 설치했다.

- 설치 : `npm i -g express-generator`
- 생성 : `express [project name]`

---

## Express 구조

### bin/www 파일

- `bin` 폴더: 서버의 로직을 작성하는 곳

  - `www` 파일: 서버를 실행하는 스크립트

  http 모듈에 express 모듈 연결 및 접속 포트 지정

```jsx
var app = require("../app");
var debug = require("debug")("learn-express:server"); // 콘솔에 로그를 남기는 모듈
var http = require("http");
```

```jsx
// process.env(환경변수) 객체에 PORT 속성이 있다면 이것을, 없으면 3000번 포트 사용
var port = normalizePort(process.env.PORT || "3000");

// 서버가 실행될 포트 설정
app.set("port", port);
```

```jsx
var server = http.createServer(app);
server.listen(port);
server.on("error", onError); // 이벤트 등록
server.on("listening", onListening);
```

### App.js 파일

- 핵심적인 서버 역할. 주로 미들웨어 위치.

```jsx
var app = express(); // express 패키지를 호출하여 app 변수 객체 생성
```

- app.set() 메서드: 익스프레스 앱 설정

```jsx
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

- app.use() 메서드: 미들웨어 연결

```jsx
/* 주요 미들웨어 */
app.use(logger("dev")); // morgan: 요청 정보를 콘솔에 기록
app.use(express.json()); // 요청 들어온 본문을 JSON으로 해석
app.use(express.urlencoded({ extended: false })); // 요청 들어온 본문을 querystring을 사용하여 해석
app.use(cookieParser()); // cookie-parser: 요청에 동봉된 쿠키 해석
app.use(express.static(path.join(__dirname, "public"))); // 정적 파일이 담긴 폴더 설정

/* 라우팅 미들웨어 */
app.use("/", indexRouter);
app.use("/users", usersRouter);

/* 404(Not Found) 처리 미들웨어 */
app.use(function (req, res, next) {
  next(createError(404));
});

/* 에러 처리 미들웨어 */
app.use(function (err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get("env") === "development" ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render("error");
});
```

- bin/www 파일에서 사용하기 위한 app 객체 모듈화

```jsx
module.exports = app;
```

### 미들웨어

- 요청과 응답의 중간에 위치
- `app.use()` 메서드에 의해 호출됨
- 미들웨어 내에서 `next()` 메서드를 호출해야 다음 미들웨어로 넘어감

```jsx
app.use(
  "/",
  function (req, res, next) {
    console.log("첫 번째 미들웨어");
    next();
  },
  function (req, res, next) {
    console.log("두 번째 미들웨어");
    next();
  },
  function (req, res, next) {
    console.log("세 번째 미들웨어");
    next();
  }
);
```

### morgan

- 요청 정보를 콘솔에 기록하는 미들웨어
- 클라이언트가 접속 시 콘솔에 `GET / 200 12.345ms` 같은 로그 출력

```jsx
/*** app.js ***/

// ...
var logger = require("morgan");

// ...
app.use(logger("dev")); // 주요 인자: dev, short, common, combined 등
```

- logger() 메서드의 인자: dev, short, common, combined 등
  - 이 인자를 어떻게 주느냐에 따라 콘솔에 출력되는 로그가 달라짐
  - 개발 시: short나 dev
  - 배포 시: common이나 combined
- 파일이나 데이터베이스에 로그를 남기려면 morgan 대신 winston 모듈 등을 사용

### body-parser

- 요청 들어온 본문(JSON, URL-encoded, Raw(버퍼 데이터), Text 등) 해석
- multipart/form-data 같은 폼을 통해 전송된 데이터는 해석하지 못함
- 폼 데이터나 AJAX 요청 데이터를 처리
- Express 4.16.0 버전부터 body-parser의 일부 기능이 익스프레스에 내장됨

```jsx
/*** app.js ***/

var express = require("express");

/* Express 4.16.0 버전 이후 내장된 body-parser 부분 */
app.use(express.json());
// app.use(bodyParser.raw()); // 본문이 버퍼 데이터일때 해석
// app.use(bodyParser.text()); // 본문이 텍스트 데이터일때 해석
app.use(express.urlencoded({ extended: false })); // false: querystring 모듈 사용, true: qs 모듈(querystring을 확장한 모듈) 사용

/* 위의 코드 작동 예시 */
// JSON 형식으로 { name: 'pengsu', study: 'nodejs' }를 본문으로 보낸다면 req.body 객체에 그대로 들어감
// URL-encoded 형식으로 name=pengsu&study=nodejs를 보낸다면 req.body 객체에 { name: 'ing-yeo', study: 'nodejs' }가 들어감
```

### cookie-parser

- 요청에 동봉된 쿠키 해석
  - 해석된 쿠키들은 req.cookies 객체에 들어감
  - 예: `name=pengsu` 쿠키를 보냈다면 req.cookies는 `{ name: 'pengsu' }`가 됨
- 서명된 쿠키를 사용하면 클라이언트에서 수정했을 때 에러가 발생하므로 위험을 방지할 수 있음
  - `cookieParser()` 메서드의 첫번째 인자에 문자열을 넣어 서명

```jsx
/*** app.js ***/

// ...
var cookieParser = require("cookie-parser");

// ...
app.use(cookieParser("secret code")); // 특정 문자열로 쿠키에 서명
```

### static

- 정적인 파일 제공
- 메서드의 인자로 정적 파일들이 담겨있는 폴더를 지정하면 됨 (기본: public 폴더)
  - 예: **public**/stylesheets/style.css 파일을 [http://localhost:3000/stylesheets/styles.css](http://localhost:3000/stylesheets/styles.css) 경로로 접근하도록 설정
  - 서버의 폴더 경로와 요청 경로가 다르므로 서버의 구조를 쉽게 파악할 수 없도록 함

```jsx
/*** app.js ***/

// 정적 파일을 제공하는 폴더를 public 폴더로 지정 (/public/abc.png -> abc.png)
app.use(express.static(path.join(__dirname, "public")));
```

```jsx
// public 폴더 경로를 img로 지정 (/public/abc.png => /img/abc.png)
app.use("/img", express.static(path.join(__dirname, "public")));
```

- static 미들웨어는 정적 파일을 발견하면 응답으로 해당 파일을 전송
  - 이 경우 응답을 보냈기 때문에 다음에 나오는 라우터가 실행되지 않음
  - 파일을 찾지 못했다면 요청을 라우터로 넘김
  - 자체적으로 정적 파일 라우터 기능을 수행하므로 쓸데없는 미들웨어 작업을 하지 않도록 최대한 위쪽에 배치하는 것이 좋음
  - morgan 미들웨어 보다는 아래 쪽에서 호출하는 것이 좋음

### express-session

- 세션 관리
- Express-generator로는 설치되지 않으므로 `npm i express-session` 명령어로 직접 설치
- express-session은 req 객체 안에 req.session 객체를 생성
  - 이 객체에 값을 대입하거나 삭제하여 세션을 변경
  - 세션을 한 번에 삭제: `req.session.destroy()` 메서드 호출
  - 현재 세션의 아이디: `req.sessionID` 객체

```jsx
/*** app.js ***/

// ...
var session = require("express-session");
// ...

// ...
/* cookie-parser 미들웨어 */
app.use(cookieParser());

/* express-session 미들웨어 */
// express-session은 cookie-parser 뒤에 놓는 것이 안전
app.use(
  session({
    resave: false, // 세션 수정사항이 없더라도 세션을 다시 저장할 것인지?
    saveUninitialized: false, // 세션에 저장할 내역이 없더라도 세션을 저장할 것인지? (방문자 추적 용도)
    secret: "secret code", // 필수 항목. 클라이언트에 세션 쿠키를 보낼때 사용할 서명 값. cookie-parser의 secret과 같게 설정해야 함.
    cookie: {
      httpOnly: true, // 클라이언트에서 쿠키를 확인하지 못하도록 함
      secure: false, // https가 아닌 환경에서도 사용할 수 있도록 함 (배포 시에는 true 권장)
    },
  })
);
```

---

## 라우팅

- 요청 경로에 따라 어떻게 처리할 것인지 설정
- `Router` 객체를 사용하여 구현

### 기본적인 라우팅

- 라우팅 미들웨어는 첫 번째 인자로 주소를 받아서 특정 주소에 해당하는 요청이 왔을 때만 동작

```jsx
app.use("/", function (req, res, next) {
  console.log("/ 주소의 요청일 때 실행됩니다. HTTP 메서드는 상관 없습니다.");
  next();
});
```

- use() 메서드 대신 get(), post(), put(), patch(), delete() 같은 HTTP 메서드를 사용할 수도 있음
  - `use() 메서드`: 모든 HTTP 메서드에 대해 요청 주소만 일치할 때 실행
  - `get()`, `post()`, `put()`, `patch()`, `delete()` 메서드: 주소뿐만 아니라 HTTP 메서드까지 일치하는 요청일 때만 실행

```jsx
app.use("/", function (req, res, next) {
  console.log("/ 주소의 요청일 때 실행됩니다. HTTP 메서드는 상관 없습니다.");
  next();
});
app.get("/", function (req, res, next) {
  console.log("GET 메서드 / 주소의 요청일 때만 실행됩니다.");
  next();
});
app.post("/data", function (req, res, next) {
  console.log("POST 메서드 /data 주소의 요청일 때만 실행됩니다.");
  next();
});
```

### router 객체를 이용한 라우팅

router 객체를 만든 후 `app.js` 파일에서 이들을 미들웨어로 사용하여 라우팅

```jsx
/*** app.js ***/

// ...
// routes 폴더에 있는 js 파일(router 객체)을 require
var indexRouter = require("./routes/index");
var usersRouter = require("./routes/users");
// ...

// ...
// 주소가 /로 시작하면 routes/index.js를 호출
app.use("/", indexRouter);

// 주소가 /users로 시작하면 routes/users.js를 호출
app.use("/users", usersRouter);
```

```jsx
/*** routes/index.js ***/

var express = require("express");
var router = express.Router(); // router 객체 생성

// '/' 주소로 GET 요청 시 살행
router.get("/", function (req, res, next) {
  res.render("index", { title: "Express" });
});

module.exports = router; // 라우터 모듈
```

```jsx
/*** routes/users.js ***/

var express = require("express");
var router = express.Router();

// '/users' 주소로 GET 요청 시 실행
router.get("/", function (req, res, next) {
  res.send("respond with a resource");
});

module.exports = router;
```

router 하나에 여러 개의 미들웨어를 여러 개 장착할 수 있음

```jsx
router.get("/", middleWare1, middleWare2, middleWare3);
```

라우터에 연결된 나머지 미들웨어들을 건너뛸 때: `next('route')` 메서드 사용

```jsx
router.get(
  "/",
  function (req, res, next) {
    next("route"); // 연결된 미들웨어들을 건너뜀
  },
  function (req, res, next) {
    console.log("실행되지 않습니다.");
    next();
  },
  function (req, res, next) {
    console.log("실행되지 않습니다.");
    next();
  }
);
COPY;
```

### 경로 매개변수

- 주소에 콜론(`:`)을 명시하여 경로 매개변수 지정

```jsx
// /users/1 이나 /users/pengsu 로 접속하면 이 라우터에 걸림
router.get("/users/:id", function (req, res) {
  console.log(req.params.id, req.query);
});
```

- `req.params`: 경로 매개변수 값이 들어있는 객체
- `req.params.VAR_NAME`: 이름이 VAR_NAME인 경로 매개변수 값

### 쿼리스트링

- 쿼리스트링: 키-값의 쌍으로 이루어진 값
- `req.query`: 쿼리 스트링 값이 들어있는 객체

```jsx
router.get("/:user", function (req, res, next) {
  console.log(req.params); // 경로 매개변수 정보
  console.log(req.query); // 쿼리스트링 정보
  res.send("respond with a resource");
});

// `http://localhost:3000/pengsu?id=1&limit=5` 이라는 주소의 요청이 들어왔을 때
// req.params 객체는 `{ user: 'pengsu' }`,
// req.query 객체는 `{ id: '1', limit: '5' }`가 됨
```

---

## 라우팅 완료 후 응답하기

### 기본적인 응답

- `res.send(buffer | str | html | json)`: 버퍼 데이터, 문자열, HTML 코드, JSON 등 전송
- `res.sendFile(filePath)`: 파일을 응답으로 전송
- `res.json(json)`: JSON 데이터를 응답으로 전송
- `res.redirect(addr)`: 응답을 다른 라우터로 전송 (예: 로그인 후 메인 화면으로 돌아가기)
- `res.render('templateFile', { var })`: 템플릿 엔진 렌더링
  - views 폴더 안에 pug 확장자를 가진 파일들이 템플릿

### 상태 코드 응답

- 라우팅 성공 시 기본적으로 200 HTTP 상태 코드 응답(단, redirect는 302)
- 상태 코드를 바꾸려면 `status()` 메서드 사용

```
res.status(404).send('Not Found'); // 404: Not FoundCOPY
```

### 하나의 요청엔 하나만 응답

- 하나의 요청엔 하나의 응답만을 보내야 함
- 두 번 이상 보내면 `Error [ERR_HTTP_HEADERS_SENT]: Cannot set headers after they are sent to the client` 에러 발생

### 라우터가 요청을 처리하지 못한 경우

- 요청을 처리할 수 있는 라우터가 없을 때 다음 미들웨어에서 새로운 에러 생성
- 다음 미들웨어에서 상태코드를 404로 설정한 뒤 에러를 에러 처리 미들웨어로 넘김

```jsx
/* 404 (Not Found) 처리 미들웨어 */
app.use(function (req, res, next) {
  next(createError(404));
});

/* 에러 처리 미들웨어 */
app.use(function (err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get("env") === "development" ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render("error");
});
```

---

## 에러 처리 미들웨어

```jsx
/* 에러 처리 미들웨어(핸들러) */
app.use(function (err, req, res, next) {
  /* res.locals 객체에 값을 지정 => pug/html 렌더링 시 값이 전달됨 */
  res.locals.message = err.message;
  // res.locals.error 객체는 개발환경인 경우에만 값을 지정. 배포환경에서는 값이 지정되지 않음. (보안 강화 목적)
  res.locals.error = req.app.get("env") === "development" ? err : {};

  /* render the error page */
  res.status(err.status || 500);

  /* views/error.pug 파일 렌더링. 이 때 res.locals 속성에 대입한 값이 전달됨. */
  res.render("error");
});
```

---

## 실행

`npm start` 명령어를 실행한 후 웹 브라우저로 [http://localhost:3000으로](http://localhost:3000%EC%9C%BC%EB%A1%9C/) 접속

### `nodemon`

참고로 `nodemon` 패키지를 설치한 후 `nodemon start` 명령어를 실행하면 소스코드가 바뀌어도 서버가 자동으로 재시작되어 매우 편리하다.

- `nodemon`설치 명령어: `npm i -g nodemon`
- 실행 명령어 `nodemon --watch src/ src/index.js`

→ `src/` 디렉토리에서 코드변화가 감지하면 재시작을 하도록 설정하고, src/index.js 를 실행시켜준다.

- scripts에 추가하기

개발을 할 때마다 이 명령어를 작성하는것이 번거로울 수도 있으니 이 명령어를 npm scripts 에 추가해주면 된다.

### **`packages.json` - 하단**

```jsx
  (...)
  "scripts":{
    "start": "node src",
    "start:dev": "nodemon --watch src/ src/index.js"
  }
}

```

서버를 실행 할 때는 `yarn start` 를 하면 되고, 개발모드로 킬 때는 `yarn start:dev` 를 입력하면 된다.

### 다음 단계 Restful API Note

[Restful API Note]()
