# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Chellenge 39</title>
  </head>
  <body>
  <?php
    $db = dbconnect();
    if($_POST['id']){
      $_POST['id'] = str_replace("\\","",$_POST['id']);
      $_POST['id'] = str_replace("'","''",$_POST['id']);
      $_POST['id'] = substr($_POST['id'],0,15);
      $result = mysqli_fetch_array(mysqli_query($db,"select 1 from member where length(id)<14 and id='{$_POST['id']}"));
      if($result[0] == 1){
        solve(39);
      }
    }
  ?>
  <form method=post action=index.php>
  <input type=text name=id maxlength=15 size=30>
  <input type=submit>
  </form>
  <a href=?view_source=1>view-source</a>
  </body>
  </html>
  
  ```

- 특징

  - 싱글쿼터가 열려있다.
  - 싱글쿼터가 ''로 필터된다.
  - `\`가 삭제된다.
  - 입력값이 15자로 제한된다.
  - 15자가 넘을경우 잘린다.

# 풀이

- 조건은 상관없이 쿼리만 실행되면 된다.

  - 싱글쿼터를 닫아야 한다
  
- 필터로 다 필터되어 문자삽입으로 우회하여 싱글쿼터를 닫을 경우의 수는 딱히 생각나지 않는다.

- 글자수 제한이 눈에 들어온다.

  - 14자를 채우고 마지막에 싱글쿼터를 넣으면

  - 싱글쿼터가 두개로 변환되어 16자가 된다.

  - 그럼 글자수 제한으로 뒤에있는 싱글쿼터가 잘려나가고

  - 싱글쿼터 하나만 남아, 정상적인 쿼리실행이 이뤄질 것이다.

  - 끝

    

# 알게된 것

### SELECT에서 속성이 아닌 숫자를 넣을 경우

- 조건이 어떻든 상관없이 해당 숫자값이 리턴된다.
- 위의 문제에서도 조건을 맞출 필요가 없는 이유이다.