# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 50</title>
  </head>
  <body>
  <h1>SQL INJECTION</h1>
  <form method=get>
  id : <input name=id value='guest'><br>
  pw : <input name=pw value='guest'><br>
  <input type=submit>&nbsp;&nbsp;&nbsp;<input type=reset>
  </form>
  <?php
    if($_GET['id'] && $_GET['pw']){
      $db = dbconnect();
      $_GET['id'] = addslashes($_GET['id']); 
      $_GET['pw'] = addslashes($_GET['pw']);
      $_GET['id'] = mb_convert_encoding($_GET['id'],'utf-8','euc-kr');
      foreach($_GET as $ck) if(preg_match("/from|pw|\(|\)| |%|=|>|</i",$ck)) exit();
      if(preg_match("/union/i",$_GET['id'])) exit();
      $result = mysqli_fetch_array(mysqli_query($db,"select lv from chall50 where id='{$_GET['id']}' and pw=md5('{$_GET['pw']}')"));
      if($result){
        if($result['lv']==1) echo("level : 1<br><br>");
        if($result['lv']==2) echo("level : 2<br><br>");
      } 
      if($result['lv']=="3") solve(50);
      if(!$result) echo("Wrong");
    }
  ?>
  <hr><a href=./?view_source=1>view-source</a>
  </body>
  </html>
  ```

- id, pw를 입력받는다.

- 제출시 해당 계정의 lv이 출력되고

- lv3인 계정으로 로그인해야 클리어된다.




#  풀이

- 

  




# 알게된 것

### mb_convert_encoding() in php

- 사용법

  - ``` php
    mb_convert_encoding($string, $to_encoding, $from_encoded)
    ```

- 취약점



### 

### 문자열 인코딩 방식

- SBCS
  - Single byte Character Set, 아스키코드
  - 1바이트로 영어 및 특수문자만 표현
  - euc-kr, cp949 등
- MBCS
  - Multi Byte, 아스키 + 다른 문자
  - 1바이트 또는 2바이트 그 이상이 될 수 있다.
  - 문자 집합표마다 다른 문자를 나타낸다
- WBCS
  - Wide Byte, 유니코드
  - 2바이트로, 모든 문자를 표시할 수 있다.
  - UTF-8 등



### Efficient blind sql injection from rubiya

- 효율적으로 플래그를 알아내는 방법이다.
- 대상 플래그 문자 하나를 이진수로 표현한 뒤, 각 자리수를 비교하여 값이 무엇인지 확인하는 방법이다.
- 예시
  - `select substr(lpad(bin(ascii(substr('flag',1,1))),7,0),1,1)=0`
    - `'f'`를 이진수로 만든 뒤, 이진수의 첫째자리수가 0인지 비교한다.
    - 맞으면 0을 저장하고(틀리면 1을 저장) 다음 자리수를 비교한다.
    - 끝에 자리수까지 7번만 비교를 수행하면 `'f'`의 이진수값을 얻어낼 수 있고, 이것을 문자로 변환하면 된다.