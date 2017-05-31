# @RequestBody, @ResponseBody

JAVA Server에 request/response를 보낼 시에, content-type을 변환해서 송신할 수 있는 기능을 제공해준다.

> 예를들어 웹페이지에서 json으로 request한 파라미터들을 java에서 받으려면 java object로의 변환이 필요하며 마찬가지로 response 시에도 java object에서 json으로 변환이 필요하다.

이러한 작업들을 해주는 어노테이션이 바로 `@RequestBody` 와 `@ResponseBody` 이다. 컨트롤러에 두 어노테이션을 추가해주면, JSON이나 key/value 방식 xml 등으로 송수신 할 수 있다.



## 설정 방법

- spring-servlet.xml 설정파일

````xml
<!-- 방법 1 : 직접 정의 -->
<beans:bean id="jacksonMessageConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
  <beans:property name="supportedMediaTypes">
    <beans:list>
      <beans:value>application/json;charset=UTF-8</beans:value>
    </beans:list>
  </beans:property>
</beans:bean>

<beans:bean name="handlerAdapter" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
  <beans:property name="messageConverters">
    <beans:list>
      <!-- Json 컨버터 -->
      <beans:ref bean="jacksonMessageConverter" />
      ...
    </beans:list>
  </beans:property>
</beans:bean>

<!-- 방법 2 : spring 3.1 이상에서 annotaion-driven을 사용하면 자동 등록 -->
<annotation-driven />
````

-  ByteArrayHttpMessageConverter(*)

 HTTP 메시지와 byte 배열 사이의 변환을 처리한다. 컨텐츠 타입은  application/octet-stream이다.

- StringHttpMessageConverter(*)

 HTTP 메시지와 String 사이의 변환을 처리한다. 컨텐츠 타입은  text/plain;charset=ISO-8859-1이다.

- FormHttpMessageConverter(*)

 HTML 폼 데이터를 MultiValueMap으로 전달받을 때 사용된다. 지원하는 컨텐  츠 타입은 application-x-www-form-urlencorded이다.

- SourceHttpMessageConverter(*)

 HTTP 메시지와 javax.xml.transform.Source 사이 변환을 처리한다. 컨텐츠 타  입은 application/xml 또는 text/xml이다.

- MarshallingHttpMessageConverter(*)

 스프링의 Marshaller와 unMarshaller를 이용해서 XML HTTP 메시지와 객체 사  이의 변환을 처리한다. 컨텐츠 타입은 application/xml 또는 text/xml이다.

- MappingJacksonHttpMessageConverter(*)

 Jackson 라이브러리를 이용해서 JSON HTTP 메시지와 객체 사이의 변환을 처리한다. 컨텐츠 타입은 applicaion/json이다.



## Reference

http://devbox.tistory.com/entry/Spring-RequestBody-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98%EA%B3%BC-ReponseBody-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98%EC%9D%98-%EC%82%AC%EC%9A%A9