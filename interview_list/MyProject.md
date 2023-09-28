# 프로젝트 경험 관련
```
Q. 필터와 인터셉터의 차이는?
A. 필터는 디스패처 서블릿에 요청이 전달되기 전에 필터링되며 웹 컨텍스트에 해당되고
인터셉터는 컨트롤러를 호출하기 전에 가로채는 기술이며 스프링 컨텍스트에 해당됩니다.
인터셉터는 스프링의 기술이기 때문에 스프링에서 관리하는 빈들을 사용할 수 있습니다. 

Q. XSS는 뭔가요?
A. Cross-Site Scripting의 약어로 공격하려는 사이트에 스크립트를 넣는 기법이며 클라이언트의 쿠키나 세션의 탈취에 목적이 있습니다.

Q. XSS를 필터로 막는다면 어떤 메커니즘으로 막을 수 있는건가요?
A. Lucy-xss-servlet-filter 라이브러리로 form data 대해서 방어하고 request body로 넘어오는 JSON 데이터 같은 경우는 클라이언트로 내보내는 단계에서 HttpMessageConverter를 활용해서 xss 방지 처리해줍니다.

Q. Form 데이터와 JSON 데이터 상에 어떤 차이가 있길래 Lucy-xss-servlet-filter는 Form 데이터로 넘어오는 파라미터에 대해서만 방어를 할 수 있을까요?
A. 형식과 전송방식에 차이가있습니다. Form 데이터는 application/x-www-form-urlencoded Content-Type으로 전송되고 JSON 데이터는 application/json로 전송됩니다.

Q. 그렇다면 안에 있다면 가질 수 있는 이점이 몇가지 있을것 같은데 생각나시는게 있을까요? 예를 들어주세요
A. 의존성 주입 같은 스프링 기술들을 사용할 수 있습니다.
의존성 주입: service를 주입 받아서 비즈니스 로직을 처리하거나 repository를 주입 받아서 로그 내용을 저장할 수도 있고 @Value 애노테이션을 사용하여 프로퍼티 값을 주입 받을 수도 있습니다.
또한 @ExceptionHandler 로 처리되어 있는 커스텀 예외를 처리할 수 있습니다.

Q. ArgumentResolver는 어떻게 작동할까요?
A. 
    1. HandlerMethodArgumentResolver 구현체에서 supportsParameter() 메서드를 통해 해당 인자를 처리할 수 있는지 여부를 판단합니다.
    2. supportsParameter()메서드에서 true를 반환할 경우 resolveArgument() 메서드가 호출되고 실제로 인자를 해석하고 값을 반환하는 역할을 수행합니다.
    3. 반환된 값은 스프링이 컨트롤러 메서드를 호출할 때 인자로 사용됩니다.
```
