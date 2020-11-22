---
title: "인증"
tags:
  - Cookie
  - Session
  - token
  - OAuth
  - SNS
  - login
---

쿠키, 세션, 토큰은 모두 보안을 위해 사용되는 도구들이다. 그렇다면 왜 이것들을 사용할까?  
네트워크 특징인 `connectionless`와 `stateless` 특성 때문이다.  
connectionless는 클라이언트와 서버가 요청과 응답을 한번씩 주고받으면 연결을 끊어버리는 특성이다. 클라이언트가 request를 서버로 보내면 서버는 request에 맞네 response를 보내고 연결을 끊는다.  
stateless는 요청과 응답으로 연결이 끊기면 상태정보를 유지하지 않는 특성이다.  
바로 이런 특성때문에 쿠키, 세션, 토큰을 사용해서 사용자 인증에 대한 정보를 유지해야 하는 것이다.(만약 안하면 페이지 몇번 왔다갔다 할때마다 로그인 해야할수도..?)  
그렇다면 이제 각각을 알아보자.  

- 쿠키  
  쿠키는 **클라이언트에 저장**되는 key, value로 이루어진 데이터이다.  
  서버의 요청 응답으로 인해 쿠키가 저장되면 다음 요청은 쿠키에 담긴 정보를 이용해 참조한다.  
  클라이언트에 저장되기 때문에 방문했던 사이트에 로그인하고 종료한 후에 다시 접속할 경우 자동으로 로그인 되어있는 것도 클라이언트에 쿠키를 저장해놓기 때문에 가능한 일이다.  
  그리고 만료 시점도 설정할 수 있다. 하지만 브라우저가 종료되어도 만료 시점이 지나지 않으면 자동으로 삭제되지 않기때문에 삭제를 원하면 직접 삭제해주어야 한다.  
  
  동작 방식은 다음과 같다.  
  1. 클라이언트에서 로그인 요청  
  2. 서버에서 쿠키 생성 후 클라이언트로 전송  
  3. 클라이언트가 서버로 요청할 때 쿠키를 전송  
  4. 쿠키를 이용해 인증을 진행  
  
  하지만 사용자 정보를 오직 클라이언트만 관리하므로 http 공격을 당했을 경우 쿠키가 공개되어 정보 노출이 될 수 있다. 그래서 쿠키만 사용할 경우에는 민감한 정보가 없는 곳에 사용하는 것이 좋다.  
  
- 세션  
  세션은 쿠키 기반이지만 쿠키와는 다르게 **서버에 저장**하여 관리한다.  
  서버는 세션id를 클라이언트마다 부여하여 클라이언트가 종료되기 전까지 유지한다.  
  
  동작 방식은 다음과 같다.  
  ![쿠키 및 세션 방식](https://t1.daumcdn.net/cfile/tistory/994BEA345B53368401)  
  
  1. 서버는 클라이언트에게 고유한 세션id를 부여하고, 세션 저장소에 저장한 후 클라이언트로 전송  
  2. 클라이언트는 발급받은 세션id를 쿠키에 저장하고, 요청을 보낼 때마다 쿠키를 전송  
  3. 서버는 쿠키에 담겨있는 세션id와 세션 저장소에 있는 세션id를 대조한 후 일치하면 데이터를 전송  
  
  여기서 세션id를 쿠키라고 봐도 무방하며, 세션id는 그 자체가 유의미한 값을 가지지 않기 때문에 중간에 http 공격을 당해도 정보 노출의 위험이 덜해진다.  
  하지만 이 역시 세션 하이재킹 공격이 가능하기 때문에 세션 만료기한 설정을 하여 예방할 수 있다.  
  그리고 세션을 서버에서 관리하기 때문에 사용자가 많아질수록 서버의 부하가 커져 관리하기 힘들 수 있다.  

- 토큰  
  토큰을 사용한다면 보통 JWT(JSON Web Token)을 사용할 것이다.  
  토큰은 인증에 필요한 정보를 암호화시킨 것이다.  
  위의 쿠키와 세션과 유사한 점은 사용자는 토큰을 http 헤더에 실어 서버로 보내게 된다는 것이고, 차이점은 유저의 정보가 서버에 저장되지 않는다는 점이다.  
  
  토큰을 만들기 위해서는 3가지(Header, Payload, Verify Signature)가 필요하다.  
  Header: 위 3가지 정보를 암호화할 방식, 타입 등이 들어간다.  
  Payload: 서버에 보낼 데이터가 들어간다. 일반적으로 유저의 고유id, 유효기간이 들어간다.  
  Verify Signature: base64방식으로 인코딩한 Header, Payload와 Secret Key를 더한후 서명된다.  
  그래서 토큰위 최종 결과물은 Encoded Header.Encoded Payload.Verify Signature가 된다.  
  
  여기서 Header와 Payload는 마음만 먹으면 디코딩하여 정보를 확인할 수 있지만 Verify Signature는 서버에서 설정한 Secret Key를 모르면 디코딩 할 수 없기 때문에 안전하다고 볼 수 있다.  
  
  동작 방식은 다음과 같다.  
  ![토큰 방식](https://t1.daumcdn.net/cfile/tistory/995EC2345B53368912)  
  
  1. 클라이언트가 로그인 요청  
  2. 서버에서 유저의 고유한 id와 다른 인증 정보들과 함께 payload에 담음  
  3. Access Token의 유효기간 및 옵션을 설정  
  4. Secret Key를 이용해 Access Token을 발급  
  5. 발급된 Access Token은 쿠키나 로컬 스토리지에 저장하여 요청을 보낼 때마다 같이 보냄  
  6. 서버는 Access Token을 Secret Key를 이용해 복호화하여 검증하고 일치하면 데이터를 전송  
  
  토큰이 세션 방식보다 좋아보이기는 하지만 여전히 단점이 있다.  
  이미 토큰이 발행되면 변경이 불가능하다는 점이다.  
  만약 해킹당했을 경우, 쿠키와 세션은 지워버리면 그만이지만 토큰은 그렇지 못하기에 한번 공격받으면 속수무책으로 유효기간이 만료될 때 까지는 계속 공격당해야 한다.  
  그래서 이를 방지하기 위해 Refresh Token이라는 것이 있다.  
  
  Refresh Token은 긴 유효기간을 가지면서, 토큰이 만료되었을 때 새로 발급해주는 열쇠가 된다.  
  예를들어 Access Token의 유효기간이 1시간이고 Refresh Token의 유효기간이 2주라 하면 API 요청을 1시간 하면 만료가 될 것이다. 여기서 Refresh Token의 유효기간 전까지 Access Token을 새롭게 바로 발급받을 수 있게 해준다.  
  
  동작 방식은 기존 토큰 방식에 Access Token + Refresh Token을 같이 발급하는 것이다.  
  ![리프레쉬 토큰 방식](https://t1.daumcdn.net/cfile/tistory/99DB8C475B5CA1C936)  
  
  7. 시간이 지나 Access Token이 만료가 되면 서버는 해당 토큰이 완료됨을 확인하고 권한없음을 응답으로 보냄  
  8. 클라이언트는 Access Token과 Refresh Token을 같이 서버로 보냄  
  9. 서버는 받은 Access Token이 조작되지 않았는지 확인한 후, Refresh Token과 사용자의 DB에 저장되어있던 Refresh Token을 비교  
  10. 그 값이 일치하고 유효기간도 지나지 않았다면 새로운 Access Token을 발급  
  
  비록 과정이 복잡하고 구현도 어려울 것이지만 보안을 위해서라면 해야하는게 맞는 것 같다.  
  
- OAuth  
  OAuth는 외부 서비스의 인증 빛 권한부여를 관리하는 범용적인 프로토콜이다.  
  현재 대부분이 사용하는 OAuth는 OAuth2.0 버전이다. 1.0과의 차이점은 모바일에서도 사용이 용이하고, https가 도입되고, Access Token의 만료기간이 생겼다는 점이다.  
  
  동작 방식은 다음과 같다.  
  ![OAuth 인증](https://t1.daumcdn.net/cfile/tistory/9945F13F5B6EECC02A)  
  
  여기서 Authorization Server는 권한을 관리하는 서버이고, Resource Server는 OAuth 2.0을 관리하는 서버(페이스북, 구글 네이버 등)의 자원을 관리하는 서버이다.(OAuth 2.0 관리 서버의 자체 API로 보면 됨)  
  
  1. 사용자가 개발 서버에 인증 요청을 하면, 서버는 Authorization Request를 통해 사용자에게 인증할 수단(페이스북, 구글, 네이버 동의 로그인 url)을 보냄  
  2. 사용자는 해당 request를 통해 인증을 진행하고 인증을 완료했다는 신호로 권한 증서(Authorization Grant)를 url에 실어서버에 보냄  
  3. 서버는 권한 증서를 Authorization Server에 보냄  
  4. Authorization Server는 권한 증서를 확인 후 사용자가 맞다면 서버에게 Acces Token과 Refresh Token 그리고 사용자 정보를 발급  
  5. 서버는 해당 토큰을 DB에 저장하거나 사용자에게 넘김  
  6. 사용자가 Resource Server에 자원이 필요하면 서버는 Access Token을 담아 Resource Server에 요청  
  7. Resource Server는 Access Token이 유효한지 확인 후 서버에게 자원을 보냄  
  
- SNS 로그인  
  SNS 로그인을 제공하는 페이스북, 구글, 네이버 등은 모두 OAuth 2.0 프레임워크를 통해 로그인 API를 제공한다.  
  SNS 로그인은 OAuth 2.0 + 서버인증으로 구성된다.  
  
  SNS 로그인 동작 방식은 다음과 같다.  
  ![SNS 로그인 인증](https://t1.daumcdn.net/cfile/tistory/99115C3F5B6EECBF37)  
  
  1. 사용자가 서버에게 로그인 요청을 하면 서버는 사용자에게 특정 쿼리를 붙인 SNS 로그인 URL을 사용자에게 보냄  
  2. 사용자는 해당 URL로 접근하여 로그인을 진행한 후 권한 증서를 담아 서버에게 보냄  
  3. 서버는 해당 권한 증서를 SNS의 Authorization Server로 요청  
  4. Authrization Server는 권한 증서를 확인한 후 Access Token, Refresh Token, 사용자 정보를 발급  
  5. 받은 고유 id를 key값으로 해서 DB에 사용자가 있다면 로그인, 없다면 회원가입을 진행  
  6. 로그인이 완료되었으면 세션, 쿠키, 토큰 인증 방식을 통해 사용자의 인증을 처리  
  
지금까지 인증에 관하여 알아보았습니다!
  