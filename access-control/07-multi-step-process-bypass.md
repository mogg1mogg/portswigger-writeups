# Lab: Multi-step process with no access control on one step
**Access Control / 권한 상승 · Practitioner**

## TL;DR
권한 변경 요청이 여러단계인데, 마지막 단게에서만 접근통제가 잘 이루어지지 않아 최종요청만 조작해서 일반유저를 관리자로 승격

## 취약점
여러단계 검증중 마지막 단계에서만 요청자의 권한을 검증하지않음. 공격자가 최종검증만 손대면 우회가능

## 익스플로잇
1. 관리자 계정으로 로그인한다.
2. 일반 유저의 계정의 권한을 상승시켜보고 그 후에 뜨는 확인버튼을 누른다.
3. 마지막 단계의 요청을 리피터로 보낸다.
4. 관리자계정을 로그아웃하고 일반 유저계정으로 로그인한다.
5. 일반유저의 세션을 복사해서 리피터의 세션칸에 붙여넣기한 후에 send한다.
6. 끝

## 핵심 요청
```http
POST /admin-roles HTTP/2
Host: .web-security-academy.net
Cookie: session=
Content-Type: application/x-www-form-urlencoded

action=upgrade&confirmed=true&username=wiener
```

## 영향
아무리 여러단계로 검증을 거쳐도 한단계에서 검증이 제대로 이루어지지 않으면, 공격자가 중간의 과정을 건너뛰고 요청을 위조해서 보내서 권한상승이 가능하다.

## 대응
모든 검증 단계에서 권한을 확인한다.
