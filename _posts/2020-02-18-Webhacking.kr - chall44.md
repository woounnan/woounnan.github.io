# 문제

- 소스

  ```php
  <?php
    if($_GET['view_source']){ highlight_file(__FILE__); exit; }
  ?><html>
  <head>
  <title>Challenge 44</title>
  </head>
  <body>
  <?php
    if($_POST['id']){
      $id = $_POST['id'];
      $id = substr($id,0,5);
      system("echo 'hello! {$id}'"); // You just need to execute ls
    }
  ?>
  <center>
  <form method=post action=index.php name=htmlfrm>
  name : <input name=id type=text maxlength=5><input type=submit value='submit'>
  </form>
  <a href=./?view_source=1>view-source</a>
  </center>
  </body>
  </html>
  ```

- 하나의 입력값이 주어지고

- 입력된 값은 리눅스 `echo` 명령의 인자값으로 전다로디어`hello! XXX`로 출력된다.

  




#  풀이

- 새로운 명령을 실행하기 위해선 세미콜론을 넣어야 하지만 쿼터에 의해서 문자로 인식된다.
  - 쿼터를 넣어서 세미콜론을 넣어주고 ls 해주자
  - `';ls'`
- 플래그 파일명이 나오고 웹페이지로 해당 파일에 들어가면 플래그 값이 나온다




# 알게된 것

