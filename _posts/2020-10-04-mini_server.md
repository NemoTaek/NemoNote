---
title: "mini server"
tags:
  - http
  - request
  - response
  - get
  - post
---

[HTTP 트랜젝션 해부](https://nodejs.org/ko/docs/guides/anatomy-of-an-http-transaction/)  

먼저 사이트를 하나 놓고 시작하겠다.  
이 사이트는 node.js 공식 홈페이지에 있는 HTTP 요청할때 사용하는 처리 과정을 설명해 놓은 사이트이다.  
<br>

- 서버 생성  

  `createServer`를 사용해서 웹 서버 객체를 생성한다.  
  
  ```
  const http = require('http');
  const server = http.createServer((request, response) => {
    // 코드 작성
  });
  ```
  
- 포트 설정  

  listen 메소드로 포트를 설정하여 서버 객체에서 호출한다.  
  
  ```
  // port 번호와 ip는 각자 준비한 것을 쓰면 된다.
  server.listen(PORT, ip, () => {
    console.log(`http server listen on ${ip}:${PORT}`);
  });
  ```
  
- 헤더, 메소드, url 생성  

  `const { headers, method, url } = request;` 로 가져온다.  
  
- request body  

  request 객체는 ReadableStream 인터페이스를 구현하고 있다.  
  이 스트림의 'data'와 'end' 이벤트에 이벤트리스너를 등록해서 데이터를 받을수 있다.  
  data 이벤트에서 발생시킨 chunck는 Buffer이고, chunk는 문자열 데이터이다.  
  
  ```
  let body = [];
  request.on('data', (chunk) => {
    body.push(chunk);
  }).on('end', () => {
    body = Buffer.concat(body).toString();
    // 여기서 'body'에 전체 요청 바디가 문자열로 담겨있습니다.
  });
  ```
  
- 오류 처리  

  ```
  request.on('error', (err) => {
    // 여기서 stderr에 오류 메시지와 스택 트레이스를 출력합니다.
    console.error(err.stack);
  });
  ```

여기까지는 request 객체에 대한 설명이다.  
<br>
<hr>
<br>
여기부터는 response 객체에 대한 설명이다.  
<br>
<br>

- HTTP 상태코드와 응답 헤더 설정  

  상태코드는 보통 100번대가 정보확인, 200번대가 통신 성공, 300번대가 리다이렉트, 400번대가 클라이언트 오류, 500번대가 서버 오류에 해당한다. 여기서 자주 사용하는 상태코드를 보면 다음과 같다.  
  + 200(OK): 기존에 있는 URL에 대한 요청이 성공함  
  + 201(Create): 신규 URL에 대한 리소스 생성이 성공함  
  + 202(Accept): 요청은 접수 되었으나, 리소스 처리는 완료되지 않음  
  + 400(Bad Request): 요청이 정상적이지 않음, API에서 정의되지 않은 요청이 들어옴  
  + 403(Forbidden): 접근 금지, 권한 밖의 접근을 시도함  
  + 404(Not Found): 요청한 URI에 해당되는 리소스가 존재하지 않음  
  + 500(Internal Server Error): 서버 내부 오류  
  + 502(Bad Gateway): 게이트웨이 오류  
  
  상태코드 설정은 `response.statusCode = 200` 이런식으로 작성하면 된다.  
  응답 헤더 설정은 `response.setHeader('Content-Type', 'application/json')` 이런식으로 작성하면 된다.  
  
  원한다면 명시적으로도 헤더 데이터를 전송할 수 있다.  
  ```
  response.writeHead(200, {
    'Content-Type': 'application/json',
    'X-Powered-By': 'bacon'
  });
  ```
  <br>
- response body  

  response 객체는 WritableStream 인터페이스를 구현하고 있다.  
  응답 body는 일반적인 스트림 메소드 `response.write()` 를 사용하여 작성한다.  
  <br>
- 오류 처리  

  ```
  request.on('error', (err) => {
    // 여기서 stderr에 오류 메시지와 스택 트레이스를 출력합니다.
    console.error(err.stack);
  });
  ```
  <br>
- GET 방식  

  ```
  if(request.method === 'GET'){
     if(request.url === '/'){
       response.writeHead(200, headers);
       response.end(JSON.stringify(result));
     }
     else{
       response.writeHead(404, headers);
       response.end();
     }
   }
  ```
  <br>
- POST 방식  

  ```
  if(request.method === 'POST'){
    if(request.url === '/'){
      let body = '';
      request.on('data', (chunk) => {
        body += chunk;
      }).on('end', () => {
        const obj = JSON.parse(body);
        result.results.push(obj);
        response.writeHead(201, headers);
        response.end(JSON.stringify(obj));
      });
    }
    else{
      response.writeHead(404, headers);
      response.end();
    }
  }
  ```
