# ArgumentResolver, HttpMessageConverter
## ArgumentResolver란
> 핸들러 어댑터에서 **컨트롤러가 필요로하는 객체를 만들고 값을 바인딩하여 전달**하기 위해 호출하는 인터페이스

아래와 같은 애노테이션들은 모두 ArgumentResolver로 동작합니다.
 
- @RequestParam: 쿼리 파라미터 값 바인딩
- @ModelAttribute: 쿼리 파라미터 및 폼 데이터 바인딩
- @CookieValue: 쿠키값 바인딩
- @RequestHeader: 헤더값 바인딩
- @RequestBody: 바디값 바인딩   

## HttpMessageConverter란
> ArgumentResolver나 ReturnValueHandler가 **컨트롤러 파라미터에 @RequestBody 애노테이션이 있거나 파라미터 타입이 HttpEntity일 경우 각각 역직렬화와 직렬화를 하기위해** 호출하는 인터페이스

<br>
<img width="100%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/a1b9f16d-50da-4a1d-99d6-725d373ac7ee">