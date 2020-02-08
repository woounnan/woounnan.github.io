# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 27</title>
  </head>
  <body>
  <h1>SQL INJECTION</h1>
  <form method=get action=index.php>
  <input type=text name=no><input type=submit>
  </form>
  <?php
    if($_GET['no']){
    $db = dbconnect();
    if(preg_match("/#|select|\(| |limit|=|0x/i",$_GET['no'])) exit("no hack");
    $r=mysqli_fetch_array(mysqli_query($db,"select id from chall27 where id='guest' and no=({$_GET['no']})")) or die("query error");
    if($r['id']=="guest") echo("guest");
    if($r['id']=="admin") solve(27); // admin's no = 2
  }
  ?>
  <br><a href=?view_source=1>view-source</a>
  </body>
  </html>
  ```
  
  

# 풀이

- no이 2번인 admin값을 조회하면 된다.
- 나는 or 2>1 order by no desc --로 실행했다.
- 주의할점
  - 스페이스 우회를 %0a로 하면 안된다.
  - %09로 하니 된다.
  - 왜이럴까... 하



# 알게된 것

### ByPass

- 스페이스 우회
  - %0a : line feed
  - %09 : horizental tab
  - 그밖에..
    - %0c(form feed), %0d(carriage return)도 된다고 한다.
- equals 우회
  - no = 1
    - no like 1
- [참고 - 몇가지 우회정리글](https://ar9ang3.tistory.com/7)

### MySQL

- SQL
  
- 기타
  
  - 비번 잃어버린 경우
  
    ```shell
    # killall mysqld
    # mysqld_safe  --skip-grant-tables &
    ```
  
    

### MariaDB

- 어머니는 MySQL이다

- 오픈소스이다

- 대충 개요까지만..

- 기타

  - PHP/MariaDB 연동

    - 권한부여 및 비밀번호 설정   *※필수※*

      `GRANT ALL ON [database name].* TO [username]@[location] IDENTIFIED BY '[password]';`

    - 그 후, mysqli_connect 진행

    - [참조](https://www.ionos.com/community/hosting/mysql/connect-to-a-mysqlmariadb-database-with-php-on-a-cloud-server-running-linux/)