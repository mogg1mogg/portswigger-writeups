# Lab: User ID controlled by request parameter with data leakage in redirect

**Category:** Access Control / <세부유형>  
**Difficulty:** <Apprentice / Practitioner>  
**Platform:** PortSwigger Web Security Academy

## TL;DR
id부분을 carlos로 바꾸고 리피트 했을때 response에서 302가 떴지만, carlos의 api키가 나왔다.

## 취약점
id만 바꾸니깐 유저의 정보를 보내주었다.

## 익스플로잇 과정
1. my account부분을 눌러서 로그인을 한다.
2. 로그인을 햇을때 잡히는 신호를 리피터에다가 보낸다.
3. 리피터에서 id부분을 carlos로 바꾸고 send한다.
4. carlos의 api키를 response에서 확인할 수 잇따.

## 핵심 요청
```http
GET /my-account?id=carlos HTTP/2
```

## 영향 (Impact)
302를 검색해보니깐 임시로 다른 URL로 이동했음라는것인데, 이또한 안전하지 않다. 권한이 없어도 302가 뜬 response에서 carlos의 api키를 탈취했다.

## 대응 (Remediation)
이부분은 잘 몰라서 ai의 힘을 빌렸습니다.
- 인가 검증을 데이터 조회보다 **먼저** 수행
- 인가 실패 시 민감 데이터를 응답 본문에 **절대 포함하지 말 것**
- 리다이렉트라도 body에 데이터가 새지 않게 처리
