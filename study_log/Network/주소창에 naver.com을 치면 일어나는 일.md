> **Q. 웹 브라우저의 요청과 흐름**   
> A.      
> 1. 클라이언트가 URL 요청
> 2. DNS를 조회해서 HTTP 요청 메시지를 만들어서 요청
> 3. 웹서버 -> was -> Filter
> 4. 디스패처 서블릿이 요청을 받아서 핸들러 매핑에게 핸들러를 조회(@RequestMapping)
> 5. Interceptor -> 핸들러 어댑터 -> 핸들러 실행
> 6. 디스패처 서블릿으로 ModelAndView 반환
> 7. viewResolver 호출
> 8. View 반환
> 9. 응답메시지 만들어서 응답
> 10. 브라우저에서 뷰 렌더링