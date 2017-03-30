### 심플하게 스프링 스케줄러 사용하기

Spring Quartz를 사용할때

@Autowired가 안먹어서, 직접 bean을 찾아 등록해줘야 하는

번거로움이 있었다.

스프링 3버전 이상부터 간단히 스케줄러를 사용할 수 있다.



- **context-scheduler.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:task="http://www.springframework.org/schema/task" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-4.2.xsd
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd">

    <context:component-scan base-package="com.iyl.stock.service" />
 
    <!-- job bean -->
    <bean id="scheduleJob" class="com.iyl.stock.schedule.Schedule" />
    
    <task:scheduled-tasks> <!-- scheduled job list -->
        <task:scheduled ref="scheduleJob" method="executeJob" cron="0/30 * * * * ?"/>
        <!-- add more job here -->
    </task:scheduled-tasks>
    
</beans>
```

- **Schedule.java**

```java
@Component
public class Schedule {

    @Autowired
    private ScheduleService scheduleService;

    public void executeJob() throws Exception {
        this.scheduleService.insertPush();
    }
}
```

xml을 더 간단하게 하려면 다음을 추가하고.

- **context-schedule.xml**

> <task:annotation-driven />

java파일의 함수 위에 다음과 같은 어노테이션과 크론표현식을 추가해주면 끝.

- **Schedule.java**

> ​      @Scheduled(cron="0 19 16 * * ?")



아직 성능상이나 비효율적인 부분은 공부해야겠지만...

너무 허무할 정도.



단, 서비스를 Autowired하기 위해서

<context:component-scan base-package>를 통해 

bean을 한 번 더 스캔 후 등록하는데.

나는 context-common.xml에서 위 작업을 이미 해놓고 있어서..

이렇게 해도 성능상이나, 그 외 문제는 없을지 체크가 필요한듯..



## Reference

[SPRING 3.X 스케쥴러 사용하기](http://darkhorizon.tistory.com/316)
