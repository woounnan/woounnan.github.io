# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
  if($_GET['view_source']) view_source();
    $db = dbconnect();
    if($_POST['lid'] && isset($_POST['lphone'])){
      $_POST['lid'] = addslashes($_POST['lid']);
      $_POST['lphone'] = addslashes($_POST['lphone']);
      $result = mysqli_fetch_array(mysqli_query($db,"select id,lv from chall59 where id='{$_POST['lid']}' and phone='{$_POST['lphone']}'"));
      if($result['id']){
        echo "id : {$result['id']}<br>lv : {$result['lv']}<br><br>";
        if($result['lv'] == "admin"){
        mysqli_query($db,"delete from chall59");
        solve(59);
      }
      echo "<br><a href=./?view_source=1>view-source</a>";
      exit();
      }
    }
    if($_POST['id'] && isset($_POST['phone'])){
      $_POST['id'] = addslashes($_POST['id']);
      $_POST['phone'] = addslashes($_POST['phone']);
      if(strlen($_POST['phone'])>=20) exit("Access Denied");
      if(preg_match("/admin/i",$_POST['id'])) exit("Access Denied");
      if(preg_match("/admin|0x|#|hex|char|ascii|ord|select/i",$_POST['phone'])) exit("Access Denied");
      mysqli_query($db,"insert into chall59 values('{$_POST['id']}',{$_POST['phone']},'guest')");
    }
  ?>
  <html><head><title>Challenge 59</title></head><body>
  <form method=post>
  <table border=1>
  <tr><td></td><td>ID</td><td>PHONE</td><td></td></tr>
  <tr><td>JOIN</td><td><input name=id></td><td><input name=phone></td><td><input type=submit></td></tr>
  <tr><td>LOGIN</td><td><input name=lid></td><td><input name=lphone></td><td><input type=submit></td></tr>
  </form>
  <br>
  <a href=./?view_source=1>view-source</a>
  </body></html>
  ```
  
- 가입과 로그인이 있음

- 폰번호와 id를 입력받음

- 로그인하면 lv이 출력됨



# 풀이

- lv을 admin으로 설정해야 하는게 핵심
- 가입할 때 번호 입력을 쿼터로 안감싸고 있음
  - 번호입력을 통해 sql 인젝션이 가능
  - 싱글쿼터 우회 같은 내용이 아님
- `번호+lv(admin) --`을 인젝션 해야함
  - 하지만 admin 문자열을 필터링 하고있음
  - chr 같은 조잡한 우회도 필터링하고있음
    - 어차피 문자열 길이제한 때문에 이용 못할듯
  - concat, mid, reverse 다 찾아봤지만 아무리 최소로 줄여도 길이제한에 걸림 ㅠㅠ..
  - 포기하고 더 검색하는데
  - 내장함수 인자로 속성명을 넣어도 된다는걸 알게되었다.
  - 바로 시도해보기
  - 로컬에서 테스트해보니 insert구문에서`reverse(id)` 식으로 추가할 경우 id에 해당하는 insert되는 값을 저 reverse의 id가 사용하게 된다
- 끝

# 알게된 것

### 쿼리 전송시 속성명을 value로 넣기

- 굳이 'admin' 이렇게 안넣고, id pw 같은 속성명을 넣어도 동작
- 예시
  - `insert into myTable values('123', id)` //id, pw라고 가정
    - id는 `'123'`이므로 pw에도 `'123'`이 저장된다



### quota escape bypass

- escape 함수 종류

  - mysqli:::`real_escape_string`/`escape_string`/ `mysqli_real_escape_string`
    - 모두 같은 함수
    - 지원되는 문자가 확장됬다.
      - ㅁ`'`, `"`, `\`, `Null`,`0x1a(EOF)`, `\n`, `\r`
  - addslashes
    - 지원되는 문자
      - '`, `"`, `\`, `Null`

- Bypass

  1. escape 함수에 숫자형 데이터가 올경우, 숫자로 간주해서 스킵된다고 한다.(자세한건 나중에.. 지금은 이해가 힘들다)

  2. multibyte encoding
     - 쿼터 앞에 멀티바이트 문자를 넣어서 인코딩시 백슬래시랑 합쳐지게 만드는 방법
  3. indirect SQL Injection
     - php 상에서 escape 시켰지만 DB에 저장될 때는 쿼터로 저장되는 특징
     - 예시
       - input: guest`'`
       - php: guest`\'`
       - DB: guest`'`
     - DB에 저장된 데이터를 쿼리로 재사용시 문제가 발생할 수 있음



### Other bypass

- 등호
  - `id like 'admin'`
  - `id in 'admin'`
  - `instr(id, 'admin')`
- 함수
  - `concat('gu', 'est')`
    - 문자열 합치기
  - `reverse('tseug')`
    - 거꾸로 변환
  - `mid('admin', 2)`
    - 중간에서 짜름
    - 위의 사용은 `'dmin'`을 리턴
- preg_match
  - `preg_match('/admin/')`
    - 대소문자 구분함
    - `adMin` 식으로 우회가능
    - `/admin/i`일 경우 대소문자 구분 안하므로 이 방법으로 우회 불가능