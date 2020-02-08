# 문제

```php
<?php
include "../../config.php";
if($_GET['view_source']) view_source();
if(!$_COOKIE['user']){
  $val_id="guest";
  $val_pw="123qwe";
  for($i=0;$i<20;$i++){
    $val_id=base64_encode($val_id);
    $val_pw=base64_encode($val_pw);
  }
  $val_id=str_replace("1","!",$val_id);
  $val_id=str_replace("2","@",$val_id);
  $val_id=str_replace("3","$",$val_id);
  $val_id=str_replace("4","^",$val_id);
  $val_id=str_replace("5","&",$val_id);
  $val_id=str_replace("6","*",$val_id);
  $val_id=str_replace("7","(",$val_id);
  $val_id=str_replace("8",")",$val_id);

  $val_pw=str_replace("1","!",$val_pw);
  $val_pw=str_replace("2","@",$val_pw);
  $val_pw=str_replace("3","$",$val_pw);
  $val_pw=str_replace("4","^",$val_pw);
  $val_pw=str_replace("5","&",$val_pw);
  $val_pw=str_replace("6","*",$val_pw);
  $val_pw=str_replace("7","(",$val_pw);
  $val_pw=str_replace("8",")",$val_pw);

  Setcookie("user",$val_id,time()+86400,"/challenge/web-06/");
  Setcookie("password",$val_pw,time()+86400,"/challenge/web-06/");
  echo("<meta http-equiv=refresh content=0>");
  exit;
}
?>
<html>
<head>
<title>Challenge 6</title>
<style type="text/css">
body { background:black; color:white; font-size:10pt; }
</style>
</head>
<body>
<?php
$decode_id=$_COOKIE['user'];
$decode_pw=$_COOKIE['password'];

$decode_id=str_replace("!","1",$decode_id);
$decode_id=str_replace("@","2",$decode_id);
$decode_id=str_replace("$","3",$decode_id);
$decode_id=str_replace("^","4",$decode_id);
$decode_id=str_replace("&","5",$decode_id);
$decode_id=str_replace("*","6",$decode_id);
$decode_id=str_replace("(","7",$decode_id);
$decode_id=str_replace(")","8",$decode_id);

$decode_pw=str_replace("!","1",$decode_pw);
$decode_pw=str_replace("@","2",$decode_pw);
$decode_pw=str_replace("$","3",$decode_pw);
$decode_pw=str_replace("^","4",$decode_pw);
$decode_pw=str_replace("&","5",$decode_pw);
$decode_pw=str_replace("*","6",$decode_pw);
$decode_pw=str_replace("(","7",$decode_pw);
$decode_pw=str_replace(")","8",$decode_pw);

for($i=0;$i<20;$i++){
  $decode_id=base64_decode($decode_id);
  $decode_pw=base64_decode($decode_pw);
}

echo("<hr><a href=./?view_source=1 style=color:yellow;>view-source</a><br><br>");
echo("ID : $decode_id<br>PW : $decode_pw<hr>");

if($decode_id=="admin" && $decode_pw=="nimda"){
  solve(6);
}
?>
</body>
</html>
```



# 풀이

- 문제소스의 val_id, pw를 admin, nimda로 변경하며 인코딩

- 인코딩된 값을 쿠키에 넣어주면 됨

- base64엔 특수문자가 없기때문에 인코딩, 디코딩시에 str_replace로

  변환되어도 값이 중복되지 않는다.



# 알게된 것

### Base64

- 데이터를 공통 ASCII영역의 문자로만 나타내기 위해 변환시키는 인코딩

- 8비트로 표현되는 데이터를 3개씩 나열해서 6bit 로 쪼갠 뒤

  각각을 문자코드표에 맞춰 변환시킴

- 6bit씩 안나눠지는 경우, 빈 bit를 0으로 채움

  - a를 예로들자
    - a의 base64인코딩 값은 YQ==
    - 왜 == 두개가 나왔냐?
    - a는 8비트이므로 6비트 + 2비트, 총 2개의 문자를 만들수있음
    - 하지만 base64는 24비트 단위이기 때문에
    - 남은 2개의 문자를 0으로 채웠고, 모든 비트가 0인 문자는 =이므로
    - ==가 짜잔

- 문자코드표

  | 값   | 문자 | 값   | 문자 | 값   | 문자 | 값   | 문자 |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | 0    | A    | 16   | Q    | 32   | g    | 48   | w    |
  | 1    | B    | 17   | R    | 33   | h    | 49   | x    |
  | 2    | C    | 18   | S    | 34   | i    | 50   | y    |
  | 3    | D    | 19   | T    | 35   | j    | 51   | z    |
  | 4    | E    | 20   | U    | 36   | k    | 52   | 0    |
  | 5    | F    | 21   | V    | 37   | l    | 53   | 1    |
  | 6    | G    | 22   | W    | 38   | m    | 54   | 2    |
  | 7    | H    | 23   | X    | 39   | n    | 55   | 3    |
  | 8    | I    | 24   | Y    | 40   | o    | 56   | 4    |
  | 9    | J    | 25   | Z    | 41   | p    | 57   | 5    |
  | 10   | K    | 26   | a    | 42   | q    | 58   | 6    |
  | 11   | L    | 27   | b    | 43   | r    | 59   | 7    |
  | 12   | M    | 28   | c    | 44   | s    | 60   | 8    |
  | 13   | N    | 29   | d    | 45   | t    | 61   | 9    |
  | 14   | O    | 30   | e    | 46   | u    | 62   | +    |
  | 15   | P    | 31   | f    | 47   | v    | 63   | /    |

- 한가지 의문

  - 문자코드표에는 0이 A인데, 위에선 왜 =로 바꾼거지?





