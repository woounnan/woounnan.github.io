# 문제

- 소스

  ```php
  php
    <?php
      include "../../config.php";
      if($_GET['view_source']) view_source();
    ?><html>
    <head>
    <title>Challenge 49</title>
    </head>
    <body>
    <h1>SQL INJECTION</h1>
    <form method=get>
    level : <input name=lv value=1><input type=submit>
    </form>
    <?php
      if($_GET['lv']){
        $db = dbconnect();
        if(preg_match("/select|or|and|\(|\)|limit|,|\/|order|cash| |\t|\'|\"/i",$_GET['lv'])) exit("no hack");
        $result = mysqli_fetch_array(mysqli_query($db,"select id from chall49 where lv={$_GET['lv']}"));
        echo $result[0] ;
        if($result[0]=="admin") solve(49);
      }
    ?>
    <hr><a href=./?view_source=1>view-source</a>
    </body>
    </html>
  ```

- lv 값을 입력받는다.

- 입력한 lv값으로 select 하는데 받아온 값의 id속성이 admin 이면 클리어

#  풀이

- `id='admin'` 조건을 만들어야 한다.
  - 안되는거
    - `order by asc`
    - `'admin'`
    - `or`
    - `space`, `tab`
  - 되는거
    - `%0d`
    - `||`
    - `id = 0x~~~`
- 끝

# 알게된 것

### ?