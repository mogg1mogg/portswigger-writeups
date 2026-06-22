# Lab: Insecure direct object references

**Category:** Access Control / IDOR  
**Difficulty:** Apprentice  
**Platform:** PortSwigger Web Security Academy

## TL;DR
사이트에 있는 고객센터 대화의 transcirpt가 숫자로 순차적으로 이름지어지고, 주인이 나인지 아닌지 확인 안해서 파일명 숫자를 낮춰서 유저와 고객센터와의 대화내역을 탈취하였고, 거기에 우연히 있던 비밀번호 또한 탈취했습니다.

## 취약점
파일 요청하는 사람이 소유자인지 확인하지 않고, 또한 파일 명 또한 예측하기 쉬운것

## 익스플로잇 과정
1. 로그인 한 후에 고객센터로 간다.
2. 고객센터에 글을 남긴다.
3. transcript를 발급받는다.
4. transcript의 번호가 2인것을 확인하고, 버프스위트에 type extension이 txt인 신호를 리피터로 보낸다.
5. 리피터에서 최상단의 url에서 숫자 2를 1로 바꾼다.
6. send를 누르면 response에서 carlos가 고객센터와 나눈 대화가 나온다.
7. 고객센터에서의 대화를 바탕으로 로그인하면 성공

## 핵심 요청
\`\`\`http
GET /download-transcript/2.txt HTTP/2
\`\`\`

## 영향 (Impact)
프라이버시가 침해당하고, 개인정보를 타인이 조회할 수도 잇다.

## 대응 (Remediation)
실제로 그 transcript의 소유자인지 검증하고, transcript같은것들의 번호는 암호화하거나 무작위로한다.
