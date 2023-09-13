# 주소창에 naver.com을 치면 일어나는 일

1. 주소창에 도메인(naver.com)을 입력
2. 브라우저가 naver.com의 **`IP 주소`** 를 찾기 위해 **`캐시에서`** **`DNS 기록`** 이 있는지 확인
3. 없다면 DNS 서버가 DNS 쿼리로 naver.com을 **`호스팅`** 하는 서버의 IP 주소를 찾음
4. 브라우저가 해당 서버와 **`TCP 연결`** 을 시작
5. 브라우저가 웹서버에 **`HTTP 요청`** 을 보냄
6. 웹서버 -> was -> Filter
7. 디스패처 서블릿이 요청을 받아서 핸들러 매핑에게 핸들러를 조회(@RequestMapping)
8. Interceptor -> 핸들러 어댑터 -> 핸들러 실행
9. 디스패처 서블릿으로 ModelAndView 반환
10. viewResolver 호출
11. View 반환
12. 서버가 응답메시지 만들어서 응답
13. 브라우저에서 뷰 렌더링