## MySQL DB 엔진

대표적인 두 엔진이 있다.

#### MyISAM

- 트랜잭션을 보장하지 않음.
- 풀텍스트 인덱싱(전체 문장을 카타로그로 저장하여, 인덱싱함)
- 복구 기능을 제공
- 테이블과 인덱스를 각각 분리된 파일로 관리
- 테이블 단위의 lock 사용


- Select 검색은 빠르지만, Insert/Update 속도가 느림



#### InnoDB

- 트랜잭션을 보장.
- 풀텍스트 인덱싱 지원 안함.


- Commit/rollback/장애복구/row-level locking, 외래키 등 기능 지원ㅇ
- Row-level Lock(행 단위 락)
- 다수의 사용자 동시접속, 대용량 데이터 처리할때 최고의 퍼포먼스가 나도록 설계
- 메인 메모리 안에 데이터 캐싱과 인덱싱을 위한 버퍼 풀을 관리
- 테이블과 인덱스를 테이블 스페이스에 함께 저장





#### 무엇이 좋을까?

- MyISAM을 사용 할 때

 1) 블로그라던지, 게시판 처럼 한사람이 글을 쓰면 다른 많은 사람들이 글을 읽는 방식에 최적의 성능을 발휘
 2) 소규모 웹서비스에 적절

1.INSERT 와 SELECT 구문을 주로 사용하는 경우 
2.ROLLBACK 트랜잭션을 사용하지 않는 경우 
3.테이블을 대규모로 동시에 읽고 쓰지 않는 경우 
4.InnoDB 가 제공하는 특별 기능을 사용하지 않는 경우 
5.FULLTEXT 인덱스를 사용하는 경우 
6.공간적인 컬럼 타입을 사용하는 경우 



## Reference

DB 엔진 비교 http://ojava.tistory.com/25

DB 엔진 차이 http://www.mysqlkorea.com/gnuboard4/bbs/board.php?bo_table=community_03&wr_id=1702

풀텍스트 인덱싱 http://brownbears.tistory.com/5