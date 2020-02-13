# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    include "./inc.php";
    if($_GET['view_source']) view_source();
    error_reporting(E_ALL);
    ini_set("display_errors", 1);
  ?><html>
  <head>
  <title>Challenge 41</title>
  </head>
  <body>
  <?php
    if(isset($_FILES['up']) && $_FILES['up']){
      $fn = $_FILES['up']['name'];
      $fn = str_replace(".","",$fn);
      $fn = str_replace("<","",$fn);
      $fn = str_replace(">","",$fn);
      $fn = str_replace("/","",$fn);
  
      $cp = $_FILES['up']['tmp_name'];
      copy($cp,"./{$upload_dir}/{$fn}");
      $f = @fopen("./{$upload_dir}/{$fn}","w");
      @fwrite($f,$flag);
      @fclose($f);
      echo("Done~");
    }
  ?>
  <form method=post enctype="multipart/form-data">
  <input type=file name=up><input type=submit value='upload'>
  </form>
  <a href=./?view_source=1>view-source</a>
  </body>
  </html>
  ```

- 파일을 업로드 받고

- 파일이름 몇가지를 필터링하고

- 완성된 파일명으로 어떤 디렉토리 아래에 플래그를 담은 파일을 저장한다.

  

# 풀이

- 업로드 디렉토리를 알아내거나 우회해야할 것 같다.

- 파일명을 ../../처럼 입력할수있다면 루트디렉토리에 플래그를 저장할 수 있어서 업로드 디렉토리를 몰라도 되지만 마침표와 /가 필터링..

- 이것저것 입력해보다가 우연히 바로 업로드 버튼을 눌렀는데 php 에러메시지가 나온다. 

  - 이거구나
  - 에러를 발생시키자

- 리눅스 파일명으로 될수없는 문자를 검색해서 나온 특수문자를 다 입력했는데 되네??...

  - 스페이스도 다 되네??...

- 로컬 리눅스에서 파일 생성해보면서 테스트하는데 파일명 막 입력하다가 빡이나서 샷건 몇번쳤는데 에러가 떴다.

  - 파일명이 너무 깁니다.
  - 이거다.

- 파일명을 길게 입력하니 에러와함께 디렉토리 명이 떠버렸다.

  

# 알게된 것

### Copy 에러유도

- 파일명 길게
- *입력할수없는 파일명*  // 나는 안됬음
- 파일크기 엄청크게 //안해봤음