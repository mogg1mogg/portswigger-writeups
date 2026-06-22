# Lab: User ID controlled by request parameter, with unpredictable user IDs

**Category:** Access Control / IDOR  
**Difficulty:** Apprentice  
**Platform:** PortSwigger Web Security Academy

## TL;DR
게정 페이지가 id=uuid를 검증 안하고 막 줘서 carlos가 올린 블로그에서 uuid를 찾아서 끼워넣어서 api키를 찾음

## 취약점
로그인 여부만 확인하고 소유권을 확인하지 않음. uuid같은것을 랜덤으로 해놓았지만, 블로그같은곳에 노출되서 무력화됨

## 익스플로잇 과정
1. 내 계정에 로그인해서 api키가 나온다는것을 확인하고, 버프 스위트에서 유저 고유의 uuid가 있는것을 확인한다.
2. calros의 블로그를 확인하고, response에 노출된 uuid를 확인한다.
3. 리피터에서 carlos의 uuid로 교체 후 요청하면
4. your api키 이러면서 나온다.
5. 제출하면 끝

## 핵심 요청
\`\`\`http
GET /my-account?id=ebba883f-c20b-448b-a545-3cf86021d435 HTTP/2
\`\`\`

## 영향 (Impact)
사이트 만든 사람은 uuid를 어렵게 만들어놨으니 보안상 안전할것이라 생각햇겟지만, 다른 페이지에서 노출 당해버려서 무용지물이 됨. api키가 털림.

## 대응 (Remediation)
소유권을 서버에서 검증하고, uuid같은것을 불필요한 위치에 노출시키지 않는다.
