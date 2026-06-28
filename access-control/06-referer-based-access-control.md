# Lab: Referer-based access control
**Access Control / 권한 상승 · Practitioner**

## TL;DR
서버가 referer헤더로 권한을 판단해서 일반 유저 계정으로 referer헤더에 어드민 요청을 보내서 자기 자신을 권한 상승시킴

## 취약점
권한 검증을 referer헤더에 의존해서 공격자가 임의로 위조가능하다. 

## 익스플로잇
1. admin계정으로 로그인 한다.
2. 일반 유저의 계정을 어드민패널에서 권한 상승시켜보고 요청을 잡는다.
3. 권한 상승 요청을 리피터로 보낸후에 일반 유저 계정으로 로그인한다.
4. 일반 유저의 세션을 리피터에 있는 세션칸에 붙여넣기한다.
5. send하면 일반유저 스스로 권한상승 완료

## 핵심 요청
```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: .web-security-academy.net
Cookie: session=
Referer: https://.web-security-academy.net/admin
```

## 영향
클라이언트가 보내는 헤더로 권한을 판단하면, 위조가 가능하다. 일반유저가 모든 권한을 지니게 될 수도 있다.

## 대응
권한 검증을 referer같은 헤더에 의존하지 않는다.
