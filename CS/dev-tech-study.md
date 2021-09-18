# Dev-Tech Study Repository

> 좋은 개발자로 성장하기 위해  부족한 기초 개념을 정리하는 저장소입니다.

### Process

- 

### Thread

- 

### Restful API

- 개념 설명
- 통신 과정 설명

### HTTP Protocol

- request 구성
- response code 설명
- 500대 코드 에러 처리
- Token 말고 다른 인증 경험?

### HTTP와 HTTPS의 차이

### SSL

### 웹 통신의 흐름(동작방식)

### GET

### POST

### GET과 POST의 차이 

### 쿠키

### 세션

### 쿠키와 세션을 사용하는 이유?

### 동기와 비동기의 개념

### 동기와 비동기의 차이점

### 멀티스레드

### 메모리 관리 전략

### OSI 참조모델

### cache

- redis,memcached

### webserver

- nginx, tomcat
- was와 웹서버 차이

### DNS

### Authentication

### Xss

### 스케줄링 알고리즘

### 캐시 알고리즘

### 데드락

### OOP

### TDD

- 테스트의 필요성
- 종류 (단위, 통합까지)

### DB Key

검색 또는 정렬할 때 Tuple을 구분할 수 있는 기준이 되는 속성

- PK : 기본키, not null, unique
- FK : 외래키, 다른 릴레이션의 PK(기본키)를 그대로 속성으로 참조하는 키

### DB 정규화

### 트랜잭션

- 트랜잭션시 주의점

### 트랜잭션 ACID

### DB Index

- 성능과 구현시 고려할 사항

### Sql Injection

조작이나 변조된 SQL 쿼리가 DB에 전달되어 비정상적 명령을 수행하여 공격하는 방법

##### **인증 우회**

로그인시 비밀번호 입력 input 창에 다른 쿼리를 입력시켜 조작하는 방법을 말합니다.

예를 들어 1234qwer란 비밀번호가 있을때 `1234qwer; delete * account from id='1' ;` 를 입력한다면 뒤에 delete문이 실행될 수도 있으며, 기본 쿼리에 where절 뒤에 or로 `'1' = '1'` 을 추가하여 조건을 true로 만들어 조작할 수 있는 방법이 있습니다.

**데이터 노출**

시스템상 발생하는 에러 메세지를 활용해 공격하는 방법을 말합니다. get 방식 요청시 url 쿼리 스트링을 추가해 에러를 발생시키고 이때 에러를 통해 서비스의 DB구조를 유추해 해킹하는 방식을 말합니다.

**SQL Injection 방어**

- input 값 받을 때 validation check
- sql 서버 에러 발생시 에러 메세지 숨기기

### CSRF

### CORS

### DB를 사용하는 이유

### SPA

### middleware

### SSR

### CSR

### CI/CD

### Master DB

- post, update, delete

### Slave DB

- get
- master DB를 바라본다

## JavaScript

### Hoisting

### Closure

### This

### Promise

### 함수형프로그래밍



## Python Django

### 클래스 상속 및 메소드 실행방식

### GIL

### GIL 왜 성능문제가 발생하는지

### GC는 어떻게 작동하는지

### Celery란? 사용 유무

### 웹서비스 응답이 느릴때 해결 방안

### python 메모리 릭이 발생하는 경우

### Decorator?

### Generator

### 로그인 인증

### JWT

### JWT 변조 공격 대처

### Bcrypt 작동 원리 

### 유닛 테스트, 통합 테스트 경험 유무

## Python Data Structure

### List

- 

### Duple

- 

### Dictionary

- 

### Set

- 

### Stack

- 

### Queue

- 

### Map

- 

### Hash

- 

