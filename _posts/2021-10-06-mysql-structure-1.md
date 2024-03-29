---
layout: post
title:  "MySQL 구조 (1)"
categories: mysql
tags: mysql architecture, mysql structure
---

## MySQL 구조
![Mysql 구조]({{site.url}}/assets/images/db/mysql-architecture.png)
## MySQL 엔진
- 클라이언트 접속 / 쿼리 요청을 처리하는 커넥션 핸들러
- SQL 파서, 전처리기 쿼리 최적화 실행을 위한 옵티마이저로 구성됨.


### MySQL 쿼리 실행구조
실행구조 그림 
![쿼리 실행 구조]({{site.url}}/assets/images/db/query-execute.jpeg)

- 쿼리파서 : SQL -> mysql 엔진이 인식할 수 있는 토큰으로 분리해 트리형태의 구조로 만들어냄
- 전처리기 : 파서 트리를 기반으로 구조적인 문제점 파악. 테이블 이름, 칼럼이름, 권한, 내장함수 매핑/객체의 존재여부 파악.
- 옵티마이저 : 사용자의 요청으로 들어온 쿼리 문장을 *저렴한* 비용으로 가장 빠르게 처리할지 를 결정하는 역할. - DBMS의 두뇌.
- 실행엔진 (쿼리실행) : 옵티마이저에서 나온 결정을 실행함.
- 스토리지엔진 (핸들러) : 디스크에 저장/읽어옴

### MySQL 쓰레드 구조
- MySQL은 프로세스 기반이 아니라 쓰레드 기반으로 작동하며, `performance_schema` 데이터베이스의 Thread 테이블을 통해 확인할 수 있습니다.
- 포그라운드 쓰레드와 백그라운드 쓰레드로 나뉨.
  - 포그라운드 쓰레드
    - 접속된 클라이언트 갯수만큼 존재하며, 주로 클라이언트가 요청한 쿼리 문장을 처리한다.
    - 데이터를 MySQL의 버퍼나 캐시로부터 가져오며, 없을 경우 직접 디스크의 데이터나 인덱스파일로부터 데이터를 읽어와 작업을 처리한다.
      - MYISAM 스토리지엔진은 쓰기 작업까지 포그라운드 쓰레드가 처리하지만, InnoDB 스토리지엔진은 데이터 버퍼나 캐시까지만 포그라운드가 처리하고, 나머지는 버퍼로부터 디스크까지 기록하는 작업은 백그라운드 쓰레드가 처리한다.

  - 백그라운드 쓰레드
    - 역할
      - Insert buffer 를 병합
      - 로그를 디스크로 기록
      - InnoDB 버퍼 풀의 데이터를 디스크에 기록.
      - 데이터를 버퍼로 읽어옴.
      - 잠금이나 데드락을 모니터링

## 스토리지 엔진 (핸들러)
- 실제 데이터를 디스크 스토리지에 저장/로드 부분을 담당
![InnoDB 구조]({{site.url}}/assets/images/db/innodb-strucuture.png)

### InnoDB 버퍼풀
- InnoDB 스토리지 엔진에서 가장 핵심적인 역할을 한다.
- 디스크의 데이터나 인덱스 정보를 버퍼에 담아두는 공간이다.
- 일반적인 어플리케이션에서 `insert, update, delete` 같은 쿼리들은 파일의 이곳저곳에 위치한 레코드를 변경하기 떄문에 랜덤한 디스크 작업이 발생하는데, 버퍼풀이 변경된 데이터에 대해서 디스크 작업 횟수를 줄이는 역할을 한다.

#### 버퍼풀의 구조
- InnoDB 엔진은 버퍼풀이라는 메모리 공간을 페이지 크기의 조각으로 쪼개서 스토리지엔진이 데이터를 필요로 할 때 해당 데이터 페이지를 읽어서 각 조각에 저장한다. 

- 버퍼풀의 페이지 크기 조각을 관리하기 위해 3개의 자료구조를 관리한다.
  - LRU, Flush, Free

#### LRU (Least Recently Used) [LRU와 MRU 가 결합된 형태]
- ![LRU 구조]({{site.url}}/assets/images/db/innodb-lru.png)
- LRU 의 목적은 디스크로부터 한번 읽어온 페이지를 최대한 오래 InnoDB 버퍼풀의 메모리에 상주시켜 디스크 읽기를 최소화하는 것이다.

#### InnoDB 엔진에서 데이터를 찾는 과정
1. 필요한 레코드가 버퍼풀에 있는지 검사.
  - InnoDB 어댑티브 해시 인덱스를 이용해 페이지 검색
  - 해당 테이블의 인덱스를 이용해 버퍼풀에서 페이지를 검색
  - 버퍼풀에 이미 데이터 페이지가 있다면 해당 페이지의 포인터 MRU(New) 방향으로 승급.
2. 디스크에서 필요한 데이터 페이지를 버퍼풀에 적재하고, 적재된 페이지에 대한 포인터를 LRU 헤더 부분에 추가
3. 버퍼풀의 LRU 헤더 부분에 적재된 데이터 페이지가 실제로 읽히면 MRU 헤더 부분으로 이동
4. 버퍼풀에 상주하는 데이터 페이지는 사용자 쿼리가 얼마나 최근에 조회되었는지에 따라 Age가 부여되며, 버퍼풀에 상주하는 동안 쿼리에서 오랫동안 사용되지 않으면 나이가 오래되고 결국 페이지는 버퍼풀에서 삭제된다. 다시 버퍼풀에서 사용되면 나이가 초기화되어 MRU 헤더 부분으로 이동된다.
5. 필요한 데이터가 자주 조회되었다면 해당 페이지의 인덱스 키를 어댑티브 해시 인덱스에 추가된다.


