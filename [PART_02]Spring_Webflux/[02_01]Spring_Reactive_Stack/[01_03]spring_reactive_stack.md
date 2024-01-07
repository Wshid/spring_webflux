# [CH01_03] Spring Reactive Stack

## Spring Reactive Stack
- ![image](https://github.com/Wshid/spring_webflux/assets/10006290/45911eab-cc29-4a2f-b6ac-7ec4bd95dca5)

### ReactiveAdapterRegistry
  - Reactor뿐 아니라 다양한 비동기 라이브러리 활용할 수 있도록 함
  - 모든 비동기 라이브러리를 Publisher로 치환하여 Spring Webflux를 사용할 수 있도록 함

### Reactor Netty
- Netty, Reactor를 쓸 수 있도록

### Spring Data Reactive
- Reactor를 기반으로 Netty Client를 구현
- Kotlin Coroutine 사용 가능

### Spring Security Reactive
- Reactor, Kotlin Coroutine 사용 가능

## 강의 순서
- Netty
  - 어떻게 더 적은 메모리로 더 적은 thread를 활용할 수 있을까의 키를 가짐
- Reactor
- Spring webflux
- Server sent event
  - 어떻게 통신할 것인가
- Websocket
- Spring security reactive