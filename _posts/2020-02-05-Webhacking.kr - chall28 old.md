# 문제

- 파일을 업로드할 수 있는 폼이 주어진다.
- flag.php를 읽으라는 힌트가 주어졌다.



# 풀이

- `.htaccess`파일이 업로드된다.
- php 파일을 일반 텍스트 파일처럼 읽으려면 `.htaccess`파일을 이용하여 php engine을 꺼야한다.
  - `php_flag engine off`
  - 를 내용으로한 `.htaccess`를 업로드 하자
- flag.php를 실행하면 끝



# 알게된 것
