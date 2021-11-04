## PostgreSQL Command Line

### 실행

- `$ pg_ctl -D /usr/local/var/postgres start`
- `$ pg_ctl -D /usr/local/var/postgres start && brew services start postgresql` 

### 중지

- `$ pg_ctl -D /usr/local/var/postgres stop`
- `$ pg_ctl -D /usr/local/var/postgres stop && brew services stop postgresql` 

### 접속

- psql postgres (default super user)

### 테이블 삭제

- dropdb 테이블명

### DB 접속하기 

- \c `db명`

### psql 종료

- \q

### DB 목록

- \l

### 유저 목록

- \du

### DB 관계 정보

- \d
- \d+

### DB 시스템 테이블 정보

- \dS

### DB 테이블 정보

- \dt

### DB View 정보

- \dv

### DB, user, port 정보

- \conninfo

### 사용자 권한 등록

- grant all privileges on database `DB명` to `유저명`;

### DB 생성

- create database <name> encoding 'utf-8';

### 유저 생성

- create user `유저명` with password '비밀번호';

### 현재 사용자 조회

- select current_user;

### 권한 부여

- alter user `유저명` with **superuser**;
- alter user `유저명` with **createdb**;
- alter user `유저명` with **createrole**;
- alter user `유저명` with **replication**;
- alter user `유저명` with **bypassrls**;

### 외부 접속 허용

/usr/local/var/postgres 이동

- `$ vi postgresql.conf`
  - listen_addresses = 'localhost' -> '*' 변경
  - post 주석 해제
  - `$ pg_ctl -D /usr/local/var/postgres restart`
- `$ pg_hba.conf`
  - IPv4 -> host	all	 all	 0.0.0.0/0	 md5 추가
  - `$ pg_ctl -D /usr/local/var/postgres restart`
- 