# Lab: Method-based access control can be circumvented
**Access Control / 권한 상승 · Practitioner**

## TL;DR
권한 검증을 post신호에서만 해서 change request method를 통해 get신호로 바꾸어 일반 유저 계정을 관리자 계정으로 승격시켰다.

## 취약점
관리자 기능에 접근을 특정신호에서만 통제한다. 같은 동작을 다른 메서드에서 하면 권한 검증을 안해서 일반 유저도 관리자가 될 수 있다.

## 익스플로잇
1. 먼저 관리자 계정으로 로그인해서 다른 사람의 권한을 상승시켜본다.
2. post 신호가 오는것을 보고, 그 신호를 리피터로 보낸다.
3. 일반 유저 계정으로 로그인 한 후에, 관리자의 쿠키를 유저의 쿠키로 바꾼다.
4. send를 눌러서 401이 response에 뜨는것을 확인한다.
5. change request method를 한다.
6. 302가 뜨며 wiener 계정의 권한 상승을 확인한다.

## 핵심 요청
```http
GET /admin-roles?username=wiener&action=upgrade HTTP/1.1
Host: .web-security-academy.net
Cookie: session=
```

## 영향
권한 검증을 한 메서드에만 의존하면, 다른 메서드로 신호를 보내서 우회가 가능하다.권한 없는 유저가 관리자가 될 수 있다.

## 대응
접근 통제를 모든 메서드에서 동일하게 검증하도록 한다.
허용되지 않은 메서드는 막는다.
민감한 기능들은 요청자 권한을 서버에서 검증하도록한다.
