# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 37</title>
  </head>
  <body>
  <?php
    $db = dbconnect();
    $query = "select flag from challenge where idx=37";
    $flag = mysqli_fetch_array(mysqli_query($db,$query))['flag'];
    $time = time();
  
    $p = fopen("./tmp/tmp-{$time}","w");
    fwrite($p,"127.0.0.1");
    fclose($p);
  
    $file_nm = $_FILES['upfile']['name'];
    $file_nm = str_replace("<","",$file_nm);
    $file_nm = str_replace(">","",$file_nm);
    $file_nm = str_replace(".","",$file_nm);
    $file_nm = str_replace("/","",$file_nm);
    $file_nm = str_replace(" ","",$file_nm);
  
    if($file_nm){
      $p = fopen("./tmp/{$file_nm}","w");
      fwrite($p,$_SERVER['REMOTE_ADDR']);
      fclose($p);
    }
  
    echo "<pre>";
    $dirList = scandir("./tmp");
    for($i=0;$i<=count($dirList);$i++){
      echo "{$dirList[$i]}\n";
    }
    echo "</pre>";
  
    $host = file_get_contents("tmp/tmp-{$time}");
  
    $request = "GET /?{$flag} HTTP/1.0\r\n";
    $request .= "Host: {$host}\r\n";
    $request .= "\r\n";
  
    $socket = fsockopen($host,7777,$errstr,$errno,1);
    fputs($socket,$request);
    fclose($socket);
  
    if(count($dirList) > 20) system("rm -rf ./tmp/*");
  ?>
  <form method=post enctype="multipart/form-data" action=index.php>
  <input type=file name=upfile><input type=submit>
  </form>
  <a href=./?view_source=1>view-source</a>
  </body>
  </html>
  ```

- 파일 업로드가 가능하다.

- 페이지 request시 서버에 있는 /tmp/time 파일의 내용을 호스트로 해서 플래그값을 전송해준다.

  

# 풀이

- 특별한 내용이 없다.

- 몇초뒤 timestamp값을 이름으로 갖는, 내 아이피 주소가 내용인 파일을 생성해서 업로드 하고

- 내 nc에 7777 포트를 열어놓은뒤

- 문제페이지를 새로고침 해준다.

- 내가 올려놓은 파일의 timestamp 시간이 되었을 때 플래그가 수신된다.

  

# 알게된 것

### file_get_contents()

- 파일의 내용을 스트링으로 읽어들인다.

### $_FILES

- POST 방식 업로드에서 사용되는 배열(대충)

- 배열요소

  >   Array
  > (
  >     [file1] => Array
  >         (
  >             [name] => MyFile.txt (comes from the browser, so treat as tainted)
  >             [type] => text/plain  (not sure where it gets this from - assume the browser, so treat as tainted)
  >             [tmp_name] => /tmp/php/php1h4j1o (could be anywhere on your system, depending on your config settings, but the user has no control, so this isn't tainted)
  >             [error] => UPLOAD_ERR_OK  (= 0)
  >             [size] => 123   (the size in bytes)
  >         )
  >
  > ​    [file2] => Array
  > ​        (
  > ​            [name] => MyFile.jpg
  > ​            [type] => image/jpeg
  > ​            [tmp_name] => /tmp/php/php6hst32
  > ​            [error] => UPLOAD_ERR_OK
  > ​            [size] => 98174
  > ​        )
  > )