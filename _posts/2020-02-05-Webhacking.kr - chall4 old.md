# 문제

- 소스

  ```php
  <?php
    include "../../config.php";
    if($_GET['view-source'] == 1) view_source();
  ?><html>
  <head>
  <title>Challenge 4</title>
  <style type="text/css">
  body { background:black; color:white; font-size:9pt; }
  table { color:white; font-size:10pt; }
  </style>
  </head>
  <body><br><br>
  <center>
  <?php
    sleep(1); // anti brute force
    if((isset($_SESSION['chall4'])) && ($_POST['key'] == $_SESSION['chall4'])) solve(4);
    $hash = rand(10000000,99999999)."salt_for_you";
    $_SESSION['chall4'] = $hash;
    for($i=0;$i<500;$i++) $hash = sha1($hash);
  ?><br>
  <form method=post>
  <table border=0 align=center cellpadding=10>
  <tr><td colspan=3 style=background:silver;color:green;><b><?=$hash?></b></td></tr>
  <tr align=center><td>Password</td><td><input name=key type=text size=30></td><td><input type=submit></td></tr>
  </table>
  </form>
  <a href=?view-source=1>[view-source]</a>
  </center>
  </body>
  </html>
  ```

- sha1 해쉬값이 주어지고, 그값을 생성해낸 초기 plaintext 숫자를 입력하면 클리어 된다.



# 풀이

- 레인보우 테이블을 만들어서 해당 값을 찾아내자
- 끝



# 알게된 것

### MD5

- 출력은 128bit

### SHA

- sha1 은 40글자이다. //확실하지 않음

### 레인보우 테이블

- 정의

  > 레인보우 테이블은 해시 함수([MD5](https://namu.wiki/w/MD5), [SHA-1](https://namu.wiki/w/SHA-1), [SHA-2](https://namu.wiki/w/SHA-2) 등)을 사용하여 만들어낼 수 있는 값들을 **왕창** 저장한 표이다. 물론 해시 함수는 입력이 무제한이라서 모든 내용을 넣는 게 아니고, 이를테면 영어 소문자와 숫자 조합으로 일정 길이까지의 모든 문자열에 대해서 계산한다거나 하는 것이다. 

### tqdm

- percentage를 나타내줄 수 있는 파이썬 라이브러리

- for문의 in 부분을 tqdm.tqdm() 으로 감싸준다

  `for x in tqdm.tqdm(range(10)):`




### 코어 개수에 따른 계산속도 차이 비교해보기

- 90000개의 sha1 루틴
  - 4코어일 때
    - 26.2초
  - 됬다.. 4코어가 제일 빠르다..