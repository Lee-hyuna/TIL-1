# component-scan / annotation-config / annotation-driven 차이점

1. ## <context:component-scan/>

   ```xml
   <context:component-scan base-package="org.controller"/>
   ```

   위의 선언은 특정 패키지(위에서는 org.controller) 안의 클래스들을 스캔하고 ,

   빈 인스턴스를 생성한다. 

   아래와 같은 정확한 어노테이션이 존재해야지 빈을 생성할수있다. 

   - **@Component**
   - **@Repository**
   - **@Service**
   - **@Controller**
   - **@Autowired**

   ​

2. ## <mvc:annotation-driven/>

   스프링 MVC 컴포넌트들의 디폴트 설정을 적용.

   - **HandlerMapping 및 HandlerAdapter 등록.**
   - **@NumberFormat, @DateTimeFormat, @Valid**
   - **Xml 및 JSON 읽고 쓰기**

   ​

3. ## <context:annotation-config/>

   - **@Required**
   - **@Autowired**
   - **@Resource, @PostConstruct, @PreDestory**
   - **@Configuration**

   위 어노테이션 사용하기 위해서 필요.



## Reference

[HAMA 블로그](http://hamait.tistory.com/322)
