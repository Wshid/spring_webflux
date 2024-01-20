# [CH02_02] MongoDB 소개

## MongoDB 특징
- opensource기반의 NoSQL DB
- 사전 정의된 스키마 없이 유연하게 데이터 저장
- 데이터를 여러 노드에 분산 저장/처리 가능
  - 샤딩을 기본적으로 지원
  - c.f. Mysql에서는 app단에서 샤딩이 필요함
- 다양한 종류의 index 지원
- `4.0`부터 **multi document transacton**을 통해 ACID 준수

## MongoDB BSON
- Binary 형식의 JSON
- JSON 보다 빠른 인코딩/디코딩
- 필드의 순서에 따라 **인코딩 결과**가 달라짐
- 다양한 타입 지원
  - document id: ObjectId
  - Binary Data: BinData
  - 날짜: Date, ISODate
  - 정규표현식
- MongoDB 문서들이 저장되는 포맷

## MongoDB BSON 인코딩
- 첫 줄은 전체 document의 크기를 가리킴
  - `E6`이기 때문에 `230 Byte`
- 데이터 타입, 필드명, 길이(Data Type에 따라 Optional), 값으로 구성

## Mongodb Document
- 하나의 개체
- RDB에 Row와 유사
- field와 value로 구성되며,
  - 중첩된 **하위 Document**나 **배열**을 포함할 수 있음
- `BSON` 형식으로 저장되며, `BSON`에서 지원하는 데이터 타입 사용

## MongoDB Collection
- MongoDB Document들을 저장하는 단위
- `RDB`의 `Table`에 해당
- 하나의 Collection내에서 `Document`마다 다른 필드 및 타입을 가질 수 있음
- RDB와 다르게 Column내 다른 타입을 가진 값이 존재할 수 있다는 의미

## MongoDB Database
- MongoDB Collection을 저장하는 컨테이너
- Document를 추가할 Collection이 없다면
  - 자동으로 Collection 생성

## MongoDB Transaction
- 4.0 버전 이후부터 `multi-document` 트랜잭션 지원
  - 여러 개의 문서와 컬렉션 사이에 작업 수행 가능
  - ACID 보장
- 여러 `shard`에 영향을 주는 transaction의 경우,
  - **쿼리가 `broadcast`되므로 성능에 크게 영향을 줌**
  - transaction이 많다면 오히려 mysql을 고려하는게 좋음
- 1번 재시도 후 실패시 client에 에러 전파
- 이미 특정 document를 업데이트 하는 순간,
  - 다른 update 요청이 발생하면
  - 새로운 요청에서 `WriteConflictException`이 발생함
