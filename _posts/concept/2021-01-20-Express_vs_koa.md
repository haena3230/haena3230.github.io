---
title: Express vs Koa
categories: [concept]
comments: true
---

# Express vs Koa

Node.js를 활용해 API를 제작하기 위한 방법을 찾던 중 두 가지 프레임워크를 접해 비교해 보았다.

## 요약

- Express 만든 팀이 2016년에 Koa를 만들었다.
- Express는 뼈대로 설치되는 모듈이 많고, 미들웨어를 붙일 때 꼭 Express 기반이어야 한다.
- Koa는 뼈대로 설치되는 모듈이 적고, customizing이 자유롭다.
- Koa는 태생이 es6 기반이기 때문에 async/await를 지원하여 try-catch 에러 처리가 필요없다.
- Express는 community가 매우매우매우 강력하다.

## 비교(1) 기본 Setup

KSetup 방식은 똑같다.

- **Express**

```
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;
const server = app.listen(PORT, () => {
	console.log(`Express is listening to <http://localhost>:${PORT});
});
```

- **Koa**

```
const koa = require('koa');
const app = express();
const PORT = process.env.PORT || 3000;
const server = app.listen(PORT, () => {
	console.log(`Koa is listening to <http://localhost>:${PORT});
});
```

## 비교(2) Middleware

Middleward 사용 방식도 비슷하다. app.use() 함수를 이용한다. 다만 Express는 req, res context를 이용하는 대신 Koa는 ctx object를 사용한다. 또한 Koa는 middleware를 정의할 때 async/await 개념을 적용할 수 있다.

- **Express**

```
app.use((req, res, next) => {
	console.log(`Time : ${Date.now()}`);
    next();
});
```

- **Koa**

```
app.use(async (ctx, next) => {
	console.log(`Time: ${Date.now()}`);
    await next();
});
```

## 비교(3) Hello world example & 성능

기본적으로 callback을 멈추고 에러를 처리하는 성능이 더 좋기 때문에 Koa가 가볍다.

- **Express** → 38510 req/sec

```
const express = require('express');
const app = express();
const port = 3000;
app.get('/', (req, res) => res.send('Hello world!'));
app.listen(port, () => console.log(`Example app listening on port ${port}!`));
```

- **Koa** → 50933 req/sec

```
const Koa = require('koa');
const app = new Koa();
app.use(async ctx => ctx.body = 'Hello world' });
app.listen(3000);
```

## 정리

- Express : 가장 기본적인 표준으로 인식되고 있음. 대부분의 미들웨어가 지원되며 가벼움.
- Koa : Async/await 가 지원된다. 최근 핫한 프레임워크.

---

Express 시작하기 Note

[Express 시작하기](https://www.notion.so/haena3230/Express-b9047c63444343888e419417dc51fcb6)
