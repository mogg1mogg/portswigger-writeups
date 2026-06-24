# Lab: URL-based access control can be circumvented
**Access Control / 헤더 우회 · Practitioner**

## TL;DR
프론트 엔드에서 어드민 경로 접근만 차단하고, 백엔드에 다른 조치가 없어서, x-orginal-url을 통해 경로를 덮어써서 관리자 패널에 접근하고 carlos를 삭제함.

## 취약점
접근 통제를 프론트엔드에 있는 경로검사에만 의존하고, 백엔드에는 아무런 조치를 취하지 않았다. 프론트엔드에는 x-original-url로 실제 경로를 재작성할 수 있어서 우회가 가능하다.

## 익스플로잇
1. 어드민 패널 버튼을 누르고 접근 권한이 막힌 것을 본다.
2. 접근을 막은 요청을 리피터로 보낸 후에, http부분을 1.1로 바꾸고, x-original-url헤더를 사용해서 우회한다.(http부분이 2이면 x-original-url이 먹히지 않는다.)
3. 어드민 패널에 접근한 것을 response에서 확인한 후에, 유저목록과 carlos 삭제 링크를 확인한다.
4. x-original-url의 값을 /admin/delete로 바꾸고, 위에 http부분에는 carlos로 바꿔준다.
5. 삭제완료
## 핵심 요청
```http
GET /?username=carlos HTTP/1.1
Host: .web-security-academy.net
X-Original-URL: /admin/delete
Cookie: session=
```

## 영향
프론트 엔드에서 헤더 하나만 조작하면 백엔드에 접근 가능함. 권한이 없어야하는 유저가 권한 남용이 가능하다. 

## 대응
접근통제를 프론트엔드만 하지말고 백엔드에도 한다. 민감한 요청을 받았을땐 권한을 검토하게한다. 비표준 헤더를 신뢰하지 못하게 한다.
