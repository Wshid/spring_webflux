# [CH02_03] Redis 소개
- 가장 인기 있는 Key-Value 구조의 저장소

## Redis 특징
- oss기반의 key-value db
- memory에 저장, 빠른 응답 속도
- single thread로 구현되어 요청을 빠르게 처리 및 데이터 정합성 보장
- 다양한 데이터 타입 지원
  - `String, Hash, List, Set Sorted set, Stream, HyperLogLog`
- Master-Slave 구조를 통한 데이터 복제(Replication 지원)
- RDB(Redis Database)와 AOP(Append Only File) 방식으로 **영속성**부여 가능
  - RDB: Redis 데이터의 snapshot을 **일정 시간 간격**으로 디스크에 저장
  - AOP: 모든 데이터 변경 작업을 **로그**로 기록

## Redis의 구조
- Single thread로 동작. I/O Multiplexing 이용
- 모든 작업들이 **task queue**를 통해서 **순차적**으로 처리
  - **데이터 정합성**, **일관성** 등을 보장
- Event Driven Model 사용
- TCP 프로토콜을 지원하며,
  - RESP(REdis Serialization Protocol) 사용
- Redis Database와 Append Only File 등으로 영속성 지원

## Redis Key-Value
- Redis는 In-memory 기반으로 **고성능 Key-Value 저장소**를 제공
- database, cache, message broker로 사용 가능
- Key-Value 쌍으로 데이터 저장
  - Key는 Unique해야 하며, **최대 512MB** 지원
  - e.g. 
    - collection_name:key_0/field_name_0 -> value_a
    - collection_name:key_0/field_name_1 -> value_b
    - ...

## Redis Value
- String
  - 텍스트, 숫자, 이진데이터
- List
  - 순서가 있는 문자열 리스트
- Set
  - 순서가 없는 문자열 리스트
- Hash
  - Key-Value 쌍의 모음
- Sorted set
  - `Set`과 유사하나, 각각의 `score`를 부여하여 sort
- Stream
  - 실시간으로 이벤트 기록
  - 생성된 이벤트를 **구독**할 수 있는 스트림
- HyperLogLog
  - 확률적인 데이터 구조
  - 고유한 요소의 갯수를 추정하는 사용
  - 높은 메모리 효율
  - estimated cardinality

### Redis String
- 문자열, 직렬화된 객체, 이진 배열등의 **기본적인 값** 저장
- **캐싱**에 자주 사용됨
- **카운터**를 구현하고 **비트 연산**을 제공
- 연산
  - SET: key에 string 저장
  - SETNX: key에 값이 없는 경우에만 저장
  - GET
  - MGET: 하나의 요청으로 여러 key 조회
  - INCRBY: 특정 key에 저장된 값을 주어진 수만큼 증가
    - 음수가 주어지면 감소도 가능함
- 예시
  ```sql
  SET person:1:name xxx
  GET person:1:name
  SETNX person:1:name woo
  MGET person:1:name person:2:name
  INCRBY person1:age 10
  ```

### Redis LIST
- 문자열로 구성된 Linked List
  - stack, queue
- 최대 길이: `2^32 - 1`
- 연산
  - LPUSH
  - RPUSH
  - LPOP
  - RPOP
  - LLEN: list 길이 반환

### Redis Set
- 문자열로 구성된 순서 없는 collection
  - unique
  - 관계 표현
- 최대 길이: `2^32 - 1`
- 연산
  - SADD
  - SREM: set에서 값 제거
  - SISMENBER: 포함 여부
  - SCARD: set의 크기
  - SMEMBERS: set의 모든 value 반환
    - 추가, 삭제 등의 대부분의 작업은 `O(1)`이나,
    - SMEMBERS는 `O(n)`

### Redis Hash
- field-value 쌍의 collection
  - 기본 객체 표현
  - counter의 모음 저장
- java의 map과 유사
- 최대 길이: `2^32 - 1`
- 연산
  - HSET: field value 순으로 나열하여 저장 가능
    ```sql
    HGET person:1 name taewoo age 20 gender M
    ```
  - HGETALL
  - HLEN
  - HINCRBY: field의 value 증가
    ```sql
    HINCRBY person:1 age 10
    ```
  - HGET
  - HDEL
