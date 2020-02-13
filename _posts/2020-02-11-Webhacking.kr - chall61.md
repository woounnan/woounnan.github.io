# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
  if($_GET['view_source']) view_source();
    $db = dbconnect();
    if(!$_GET['id']) $_GET['id']="guest";
    echo "<html><head><title>Challenge 61</title></head><body>";
    echo "<a href=./?view_source=1>view-source</a><hr>";
    $_GET['id'] = addslashes($_GET['id']);
    if(preg_match("/\(|\)|select|from|,|by|\./i",$_GET['id'])) exit("Access Denied");
    if(strlen($_GET['id'])>15) exit("Access Denied");
    $result = mysqli_fetch_array(mysqli_query($db,"select {$_GET['id']} from chall61 order by id desc limit 1"));
    echo "<b>{$result['id']}</b><br>";
    if($result['id'] == "admin") solve(61);
    echo "</body></html>";
  ?>
  ```
  
- get 파라미터로 id를 입력받음

- 필터와 길이 체크를 하고

- id값을 속성명으로 select해서 받은 결과값으로 admin 판단



# 풀이

- select가 막혀있으므로 union 불가

- 실제 테이블 속성 개수에 맞게 값을 넣어볼까?

  - union select 1,1, 'admin' 하듯이 결과가 나지 않을까?
    - 일단 쉼표가 필터링 되고
    - 싱글쿼터도 필터링

- 핵심은 id 속성명에 해당하는 값을 출력시키는 것

  - 로컬에서 테스트해보다가 reverse('nimda')를 넣어보니 해당 값이 속성명이고 함수실행 결과가 해당 속성이 갖고있는 값으로 출력되었다.
  - 속성명을 지정할 수도 있겠구나
  - 검색

- as를 이용하여 속성명 설정 가능

  - `select 'admin' as id from myTable;`

    - 출력

      ```mysql
      id
      -----
      'admin'
      ```

    - 하지만 위 문제에서 길이가 초과됨

  - as 키워드 생략 가능

    - `select 'admin' id from myTable`
    - 요호

- 싱글쿼터가 필터되므로 헥스로 admin값을 입력하면 되겠다.

  - `0x61646d696e`

- 끝

# 알게된 것

### As  in sql

- 질의시 출력되는 속성명과 값을 설정 가능하다

- 입력

  `select 'admin' as id from myTable`

  - 출력

    ```mysql
    id
    -----
    'admin'
    ```

### input string using hex

- 위 문제와 같이 쿼터가 필터되고 입력값이 문자로 인식되지 않을 때
  - hex값을 이용하여 문자열을 넣을 수 있다.
- 사용
  - `0x61646d696e`
    - 요것은 'admin'으로 저장된다.