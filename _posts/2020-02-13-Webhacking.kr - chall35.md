# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 35</title>
  <head>
  <body>
  <form method=get action=index.php>
  phone : <input name=phone size=11 style=width:200px>
  <input name=id type=hidden value=guest>
  <input type=submit value='add'>
  </form>
  <?php
  $db = dbconnect();
  if($_GET['phone'] && $_GET['id']){
    if(preg_match("/\*|\/|=|select|-|#|;/i",$_GET['phone'])) exit("no hack");
    if(strlen($_GET['id']) > 5) exit("no hack");
    if(preg_match("/admin/i",$_GET['id'])) exit("you are not admin");
    mysqli_query($db,"insert into chall35(id,ip,phone) values('{$_GET['id']}','{$_SERVER['REMOTE_ADDR']}',{$_GET['phone']})") or die("query error");
    echo "Done<br>";
  }
  
  $isAdmin = mysqli_fetch_array(mysqli_query($db,"select ip from chall35 where id='admin' and ip='{$_SERVER['REMOTE_ADDR']}'"));
  if($isAdmin['ip'] == $_SERVER['REMOTE_ADDR']){
    solve(35);
    mysqli_query($db,"delete from chall35");
  }
  
  $phone_list = mysqli_query($db,"select * from chall35 where ip='{$_SERVER['REMOTE_ADDR']}'");
  echo "<!--\n";
  while($r = mysqli_fetch_array($phone_list)){
    echo htmlentities($r['id'])." - ".$r['phone']."\n";
  }
  echo "-->\n";
  ?>
  <br><a href=?view_source=1>view-source</a>
  </body>
  </html>
  ```

- phone번호와 id값을 get으로 입력받음

  - id값은 default `'guest'`

- 입력받은 값을 검사한 뒤 문제없으면 DB에 접속 IP에 해당하는 id phone 행을 추가

- 주석에 추가된 데이터 리스트를 보여줌

# 풀이

- phone이 쿼터로 안닫혀있으므로 인젝션 가능하다.
- 작성된 데이터 쌍에서 id값을 바꾸는 방법은 안보임
  - insert 문법 특징으로 여러개 데이터 쌍을 추가할 수 있는 것을 이용
- 정상적인 쿼리를 보내도 query error가 발생
  - 쿼터가 막혀있음을 추측
    - 이를테면 magic_quota_gpc
  - 헥스로 admin과 ip를 치환하자
- 끝

# 알게된 것

### Insert 여러개 데이터 추가

- `insert into myTable values('guest1', 1), ('guest2', 2);`
  - 위처럼 행에 해당하는 데이터 집합을 괄호단위로 추가하면 됨