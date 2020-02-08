# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 24</title>
  </head>
  <body>
  <p>
  <?php
    extract($_SERVER);
    extract($_COOKIE);
    $ip = $REMOTE_ADDR;
    $agent = $HTTP_USER_AGENT;
    if($REMOTE_ADDR){
      $ip = htmlspecialchars($REMOTE_ADDR);
      $ip = str_replace("..",".",$ip);
      $ip = str_replace("12","",$ip);
      $ip = str_replace("7.","",$ip);
      $ip = str_replace("0.","",$ip);
    }
    if($HTTP_USER_AGENT){
      $agent=htmlspecialchars($HTTP_USER_AGENT);
    }
    echo "<table border=1><tr><td>client ip</td><td>{$ip}</td></tr><tr><td>agent</td><td>{$agent}</td></tr></table>";
    if($ip=="127.0.0.1"){
      solve(24);
      exit();
    }
    else{
      echo "<hr><center>Wrong IP!</center>";
    }
  ?><hr>
  <a href=?view_source=1>view-source</a>
  </body>
  </html>
  ```

  

# 풀이

- $REMOTE_ADDR값을 검사하므로 이 값을  127.0.0.1로 만들어야 함
- cookie값에 REMOTE_ADDR을 추가
- 이 값을 str_replace 수행후에 127.0.0.1이 되도록 하는 값으로 설정



# 알게된 것

### extract

- 딕셔너리를 key, value 와 매칭시켜 변수로 만들어줌
  - 예를들어
    - a['abcd'] = 1234라면, abcd = 1234 와 같은 변수를 생성해줌

### htmlspecialchars

- 특정 특수문자를 html 엔티티(? 뭔지모름)로 변경해줌

  | 특수 문자        | 변환된 문자 |
  | ---------------- | ----------- |
  | **&**(앰퍼샌드)  | &amp;       |
  | **""**(겹따옴표) | &quot;      |
  | **''**(홑따옴표) | &#039;      |
  | **<**(미만)      | &lt;        |
  | **>**(이상)      | &gt;        |

  