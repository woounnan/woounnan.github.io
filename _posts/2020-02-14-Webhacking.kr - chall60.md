# 문제

- 소스

  ```php
   <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
    login_chk();
    echo "Your idx is {$_SESSION['idx']}<hr>";
    if(!is_numeric($_COOKIE['PHPSESSID'])) exit("Access Denied<br><a href=./?view_source=1>view-source</a>");
    sleep(1);
    if($_GET['mode']=="auth"){
      echo("Auth~<br>");
      $result = file_get_contents("./readme/{$_SESSION['idx']}.txt");
      if(preg_match("/{$_SESSION['idx']}/",$result)){
        echo("Done!");
        unlink("./readme/{$_SESSION['idx']}.txt");
        solve(60);
        exit();
      }
    }
    $p = fopen("./readme/{$_SESSION['idx']}.txt","w");
    fwrite($p,$_SESSION['idx']);
    fclose($p);
    if($_SERVER['REMOTE_ADDR']!="127.0.0.1"){
      sleep(1);
      unlink("./readme/{$_SESSION['idx']}.txt");
    }
  ?>
  <html><head><title>Challenge 60</title></head><body><a href=./?view_source=1>view-source</a></body></html>
  
  ```
  
- PHPSESSID 쿠키값이 숫자여야 통과

- get 으로 mode 값을 auth로 전달되는 경우와

  - 내 세션idx에 해당하는 파일의 내용을 읽어들여서 있으면 solve

- 아닌 경우

  - 내 세션idx에 해당하는 파일을 생성한다.

- 두가지로 분기된다

#  풀이

- 레이스 컨디션 어택을 해야한다.
  - 시스템해킹에서 많이 해본 스타일..
- 페이지를 두개띄워놓고
  - 세션idx파일이 삭제되기전에 다른 페이지를 새로고침 하면 clear
  - 특이점
    - 브라우저를 다른것으로 실행하고, 쿠키값이 달라야 클리어가 되는것 같다.
    - 이유는 모르겠다.

# 알게된 것

