# [CH04_03] webHandler

## WebHandler
- Filter, Codec, ExceptionHandler 등을 지원
- ServerWebExchange를 제공

## ServerWebExchange
- getRequest
  - ServerHttpRequest 제공
- getResponse
  - ServerHttpResponse 제공
- getAttributes
  - 요청 처리중에 추가/변경 가능한 Map 제공
  - 상태 전달/공유시 사용
- getSession
  - WebSessions publisher 반환
    - Session 정보를 담음
- getPrincipal
  - Principal Publisher 반환
    - Security 정보를 담음
- getFormData
  - Context-Type=`application/x-www-form-urlencoded`인 경우
  - `MultiValueMap`을 담고 있는 publisher 제공
- getMultipartData
  - Context-Type=`multipart/form-data`인 경우
  - body를 `MultiValueMap` 형태로 제공
- getApplicationContext
  - Spring 환경에서 구동된 경우 applicationContext 반환
  - Spring 환경이 아니라면 `null`

## WebHandler 생성
- ServerWebExchange로부터 `request, response` 획득
- `request`의 `getQueryParam`으로
- `ReactorHttpHandlerAdapter`로 감싸서 `HttpServer`의 `handle`에 전달

## WebHttpHandlerBuilder
- Builder에 주어진
  - `Filters`, `ExceptionHandlers`, `CodecConfigurer`, `ApplicationContext`등을 이용하여
  - 하나의 `HttpWebHandlerAdapter`를 생성

## HttpWebHandlerAdapter
```java
public class HttpWebHandlerAdapter extends WebHandlerDecorator implements HttpHandler
```
