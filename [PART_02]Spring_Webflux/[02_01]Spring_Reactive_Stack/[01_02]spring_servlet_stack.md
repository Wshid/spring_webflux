# [CH01_02] Spring Servlet Stack

## Spring Framework의 구성

### Spring web stack
- Reactive Stack / Servlet Stack
- Spring Webflux / Spring MVC
- ![image](https://github.com/Wshid/spring_webflux/assets/10006290/f061a78b-da65-4a9e-b5d1-53e6d0e59ccf)

### Spring Servlet Stack

#### Servlet Containers(Tomcat, Jetty, JBoss)
- Java Servlet을 실행하고 관리
- request-per-thread 모델 사용
  - 요청시 thread 할당
  - 여러 client 요청 동시 처리
- Connector를 통해 HTTP 통신 수행
- Filter
  - 각 요청, 응답의 내용 헤더를 변환
- client의 요청을 servlet의 `service` 메소드에 넘겨주고 그 결과를 응답으로 전달
- Servlet의 `init`과 `destroy` 메소드로 **생명주기**를 담당

#### Servlet API
- Java EE Servlet에서 정의(현재 Jakarta EE Servlet에서 지원)
- `init`
  - Servlet 초기화 시 사용
  - Servlet 객체 생성시 사용
  - Servlet Container에 등록
- `service`
  - client 요청에 따라 비즈니스 로직 실행 및 응답 반환
- `destroy`
  - Servlet 종료시 수행
  - 리소스 해제 등을 맡음

##### HttpServlet
- Servlet 기반으로 HTTP 프로토콜 지원
- GET, POST, ... 등을 처리할 수 있음

##### HTTPServlet Service
- request로부터 method 추출
- method에 따라 `doGet, doHead, doPost, ...` 등 수행 가능

#### Spring MVC

##### DispatcherServlet
- `doService`: `doDispatch`라는 내부 메서드 수행
- client의 요청을 적절한 Controller에게 전달
- Front Controller Pattern 구현
- `HttpServlet`을 상속하여 모든 요청을 `doDispatc(request, response)` 호출하게 변경
- `HandlerMapping, HandlerAdapter, ViewResolver`등과 상호작용
- ![image](https://github.com/Wshid/spring_webflux/assets/10006290/0e308996-7c12-4b03-bd16-af603606ae7e)


#### Spring Security
- Authentication, Authorization
- 보안 작업을 수행하는 Servlet filter 제공
- `SecurityContextHolder`를 이용하여 `authentication` 설정


##### SecurityContextHolder
- 기본적으로 `MODE_THREADLOCAL` 전략 사용
- SecurityContext를 ThreadLocal에 저장
- thread가 계속 변경되는 reactive 환경에서는 다르게 활용

#### Spring Data
- 동기 blocking, thread당 하나의 db 요청 처리
- Spring JDBC
  - JDBC를 사용하여 RDB 연결
- Spring JPA
  - Java Persistence API를 사용하여 RDB 연결

### Spring Servlet Stack 정리
- thread-per-request 모델 사용, 요청이 들어올때마다 thread 처리
- SecurityContext를 threadLocal에 저장
- thread당 하나의 db 요청 처리
- 요청이 들어올 때마다 할당된 thread는 아래와 같은 긴 활동 주기를 가짐
  - spring mvc flow
  - 비즈니스 로직 수행
  - response 생성
  - securityContext 관리
  - db 요청
  - http 요청

## C10K 문제(Client 10K)
- 혹시 1만개 혹은 그 이상의 요청이 동시에 들어온다면?
- Servlet container의 thread pool 크기 제한
- 동기 blocking한 연산들로 인해 `작업을 완료, 반환하는 스레드 수 < 요청 수`
- **thread 부족 및 해소되지 않는 상황 발생**

### thread pool을 사용하지 않는다면?
- 매번 새로운 스레드 생성
- 스레드 생성/제거 비용이 비싼편
- thread마다 할당되는 stack memory로 인해 메모리 부족 문제 발생
- 더 많은 스레드가 cpu를 점유하기 위해 context switching을 함
- **thread를 가능한한 최소한으로 사용하는 방법 필요**
