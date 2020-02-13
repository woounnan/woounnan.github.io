# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 7</title>
  </head>
  <body>
  <?php
  $go=$_GET['val'];
  if(!$go) { echo("<meta http-equiv=refresh content=0;url=index.php?val=1>"); }
  echo("<html><head><title>admin page</title></head><body bgcolor='black'><font size=2 color=gray><b><h3>Admin page</h3></b><p>");
  if(preg_match("/2|-|\+|from|_|=|\\s|\*|\//i",$go)) exit("Access Denied!");
  $db = dbconnect();
  $rand=rand(1,5);
  if($rand==1){
    $result=mysqli_query($db,"select lv from chall7 where lv=($go)") or die("nice try!");
  }
  if($rand==2){
    $result=mysqli_query($db,"select lv from chall7 where lv=(($go))") or die("nice try!");
  }
  if($rand==3){
    $result=mysqli_query($db,"select lv from chall7 where lv=((($go)))") or die("nice try!");
  }
  if($rand==4){
    $result=mysqli_query($db,"select lv from chall7 where lv=(((($go))))") or die("nice try!");
  }
  if($rand==5){
    $result=mysqli_query($db,"select lv from chall7 where lv=((((($go)))))") or die("nice try!");
  }
  $data=mysqli_fetch_array($result);
  if(!$data[0]) { echo("query error"); exit(); }
  if($data[0]==1){
    echo("<input type=button style=border:0;bgcolor='gray' value='auth' onclick=\"alert('Access_Denied!')\"><p>");
  }
  elseif($data[0]==2){
    echo("<input type=button style=border:0;bgcolor='gray' value='auth' onclick=\"alert('Hello admin')\"><p>");
    solve(7);
  }
  ?>
  <a href=./?view_source=1>view-source</a>
  </body>
  </html>
  ```
  
- get 파라미터로 val을 입력받음

- val은 sql 쿼리문에 사용되고 결과로 받아온 값이 lv=2 면 클리어



# 풀이

- lv값을 바로 2로 줘보자
  - val = `5%3`
  - 하지만 클리어되지 않았다.
  - DB에 lv2값이 없는듯하다.
- union select로 2값을 만들자
  - `0)union(select(5%3))#`
  - 주의할점 #(주석)을 urlencode 해서 넘겨야 한다.
- 끝

# 알게된 것

### Union Select 괄호로 호출

- `union(select(1))`
  - 단 인자가 하나일 때만 가능하다

### url로 파라미터 넘길때 수동으로 URL Encoding을 해주자

- `~~~~ #`
  - 이렇게 넘기면 안되고
  - `~~~~ %23`
    - 이렇게 인코딩을 해줘야 한다.