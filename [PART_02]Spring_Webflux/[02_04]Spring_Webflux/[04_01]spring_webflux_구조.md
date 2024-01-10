# [CH04_01] Spring Webflux 기본 구조

## AutoConfiguration으로 알아보는 의존 그래프
- Spring Webflux를 위해 필요한 컴포넌트
- `ReactiveWebServerFactoryConfiguration.EmbeddedNetty`
- `NettyReactiveWebServerFactory`
  - 내부적으로 web server를 가짐
  - httpserver는 `reactor.netty`를 통해 제공 받음

## Reactor Netty
- Reactor 기반으로 Netty wrapping
- Netty의 성능
- Reactor의 조합성, 편의성 모두 제공
- 예시
  ```java
  Consumer<HttpServerRoutes> routesConsumer = routes -> routes.get("/hello", (request, response) -> {
    var data = Mono.just("Hello World!");
    return response.sendString(data);
  });

  HttpServer.create()
            .route(routesConsumer)
            .port(8080)
            .bindNow()
            .onDispose()
            .block();
  ```

## RxJava, Mutiny, Coroutine을 지원하는 Spring Webflux
- **ReactiveAdapterRegistry**
  - ReactorAdapter
    - ReactorRegister
  - RxJava3Register
  - registry에 등록
- RxJava와 관련된 `ReactiveAdapter`를 활용하여 사용

### ReactiveAdapter
- ReactiveAdapterRegistry는
  - 내부적으로 각각 라이브러리의 Publisher와 매칭되는 변환함수를 `Adapter` 형태로 저장
- `getAdapter`를 통해서
  - 변환하고자 하는 Publisher를 넘기고
  - 해당 adapter의 `toPublish`를 이용하여, `reactive streams`의 Publisher로 변경
- 여러 라이브러리의 `Publisher`들을
  - `reactive streams`의 `Publisher`로 변환 가능
- `Spring Webflux`에서는
  - `reactive streams`의 `Publisher`를 기준으로 구현을 한다면
  - `ReactiveAdapter`를 통해서 **여러 라이브러리** 지원 가능

## Spring WebFlux & Spring MVC
- @Controller
- Reactive clients
- Tomcat, Jetty, Undertow
