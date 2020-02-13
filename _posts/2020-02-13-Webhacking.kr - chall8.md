# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 11</title>
  <style type="text/css">
  body { background:black; color:white; font-size:10pt; }
  </style>
  </head>
  <body>
  <center>
  <br><br>
  <?php
    $pat="/[1-3][a-f]{5}_.*$_SERVER[REMOTE_ADDR].*\tp\ta\ts\ts/";
    if(preg_match($pat,$_GET['val'])){
      solve(11);
    }
    else echo("<h2>Wrong</h2>");
    echo("<br><br>");
  ?>
  <a href=./?view_source=1>view-source</a>
  </center>
  </body>
  </html>
  ```
  
- get 파라미터 val을 정규표현식으로 검색한다.

- 해당 패턴이 존재하면 클리어

# 풀이

- 정규표현식이 오랜만이라 php online 사이트에서 하나씩 패턴을 확인해가면서 시도했다.
- 예상치못한 에러
  - php online에서 하는데 패턴에 맞게 작성했는데 자꾸 failed가 떴다.
  - 열받아서 그냥 문제에다 입력해봤더니
  - 잘된다.

# 알게된 것

### Regular expression

- 표현
  - `a{2}`
    - a를 2번 반복
    - `aa`
  - `.`
    - 아무 문자
  - `a*`
    - `*`앞의 문자가 0번이상 반복된 모든 문자
    - , `a`, `aa`, `aaa`