# 문제

- 소스

  ```php
  <?php
    if($_GET['view_source']) highlight_file(__FILE__);
    $db = mysqli_connect() or die();
    mysqli_select_db($db,"chall30") or die();
    $result = mysqli_fetch_array(mysqli_query($db,"select flag from chall30_answer")) or die();
    if($result[0]){
      include "/flag";
    }
  ?>
  ```

- 파일 업로드 폼이 있고 자신의 개인 디렉토리를 알려준다.

  - 업로드시 개인디렉토리에 저장된다.

- 소스보기를 누르면 자신의 개인디렉토리에 있는 index.php가 실행된다.

  


#  풀이

- 업로드시 몇가지가 필터링 된다.
  
  - php 파일을 업로드 해도 텍스트로 인식된다.
  - 파일명 `index.php`
  - 파일명 `.htaccess`
    - 파일명으로 필터링 되는게 아니었다
    - 파일 내용에 php 인식 확장자 바꾸는 내용이 있으면 필터링 되는것 같다.
  
- 다른 방법은?

  - 소스를 가만히 들여다보고있으니  mysql_connect가 좀 허전하다는 것을 느낀다.

    - 값이 설정되어있지 않다.

  - [mysql_connect - PHP document]()를 보면 값이 설정되있지 않을 때 기본값이 설정된다.

    > The username. Default value is defined by [mysql.default_user](https://www.php.net/manual/en/mysql.configuration.php#ini.mysql.default-user). In [SQL safe mode](https://www.php.net/manual/en/ini.core.php#ini.sql.safe-mode), this parameter is ignored and the name of the user that owns the server process is used.

  - 이 기본값은 `php.ini`(/etc/에 존재하는 설정파일)로 설정가능한데 이것은 또한 `.htaccess`로 설정 가능하다.

- 하지만... 안되죠~~

  - mysql 원격접속 허용해놓고(구름 ide에서는 접속잘됨)
  - htaccess를 인젝션했으나
  - 안된다..
  - 원인 모름
  - 문제환경과 동일하게
    - 로컬에서 확인해보려니
      - 내부에서 외부아이피로 접속하는건 또 안된다..
    - 구름에서 확인해보려니
      - php 설치가 안된다.
      - 앙 기모찌해

# 알게된 것

### 업로드 우회

- MIME 타입 우회
- 파일 대소문자 우회
- 확장자 추가 우회
  - `xxx.txt.php`
- null injection 우회
  - `xxx.jpg\0.php`
- `php3`등 다른 확장자 이용
- `.htaccess` 조작
  - 인식 확장자 바꾸기

### PHP.ini  configuration

># PHP | php.ini File Configuration
>
>At the time of PHP installation, **php.ini** is a special file provided as a default configuration file. It’s very essential configuration file which controls, what a user can or cannot do with the website. Each time PHP is initialized, the **php.ini** file is read by the system. Sometimes you need to change the behavior of PHP at runtime, then this configuration file is to use.
>
>All the settings related to register global variables, upload maximum size, display log errors, resource limits, the maximum time to execute a PHP script and others are written in a file as a set of directives which helps in declaring changes.
>
>**Note:** Whenever some changes are performed in the file, you need to restart our web server.
>
>
>
>
>
>**To check file path use the following program:**
>
>*filter_none*
>
>
>
>
>
>*brightness_4*
>
>```
>`<?php ``echo` `phpinfo(); ``?> `
>```
>
>**Note:** Keys in the file are case-sensitive, keyword values are not spaces and lines starting with a semicolon are ignored. The file is well commented. The Boolean values are represented by **On/Off, 1/0, True/False, Yes/No**.
>
>The file contains a set of directives with a set of respective values assigned to it. The values can be string, a number, a PHP constant, INI constants, or an expression, a quoted string or a reference to a previously set variable. Expression in the INI file is limited to bitwise operators or parentheses. Settings with a particular hostname will work under that particular host only.
>
>**Environment variables of php.ini file:**
>
>- **memory_limit:** This setting is done to show the maximum amount of memory a script consumes.
>
>**Important settings or common parameters of the php.ini file:**
>
>1. **enable_safe_mode = on** Its default setting to ON whenever PHP is compiled. Safe mode is most relevant to CGI use.
>2. **register_globals = on** its default setting to ON which tells that the contents of EGPCS (Environment, GET, POST, Cookie, Server) variables are registered as global variables. But due to a security risk, the user has to ensure if it set to OFF for all scripts.
>3. **upload_max_filesize** This setting is for the maximum allowed size for uploaded files in the scripts.
>4. **upload_tmp_dir = [DIR]** Don’t uncomment this setting.
>5. **post_max_size** This setting is for the maximum allowed size of POST data that PHP will accept.
>6. **display_errors = off** This setting will not allow showing errors while running PHP project in the specified host.
>7. **error_reporting = E_ALL & ~E_NOTICE:** This setting has default values as E_ALL and ~E_NOTICE which shows all errors except notices.
>8. **error_prepend_string = [“”]** This setting allow you to make different color of messages.
>9. **max_execution_time = 30** Maximum execution time is set to seconds for any script to limit the time in production servers.
>10. **short_open_tags = Off** To use XML functions, we have to set this option as *off.*
>11. **session.save-handler = files** You don’t need to change anything in this setting.
>12. **variables_order = EGPCS** This setting is done to set the order of variables as Environment, GET, POST, COOKIE, SERVER. The developer can change the order as per the need also.
>13. **warn_plus_overloading = Off** This setting issues a warning if + used with strings in a form of value.
>14. **gpc_order = GPC** This setting has been GPC Deprecated.
>15. **magic_quotes_gpc = on** This setting is done in case of many forms are used which submits to themselves or others and display form values.
>16. **magic_quotes_runtime = Off** If magic_quotes_sybase is set to On, this must be Off, this setting escape quotes.
>17. **magic_quotes_sybase = Off** If this setting is set to off it should be off, this setting escape quotes.
>18. **auto-prepend-file = [filepath]** This setting is done when we need to automatically *include()* it at the beginning of every PHP file.
>19. **auto-append-file = [filepath]** This setting is done when we need to automatically *include()* it at the end of every PHP file.
>20. **include_path = [DIR]** This setting is done when we need to require files from the specified directories. Multiple directories are set using colons.
>21. **ignore_user_abort = [On/Off]** This settings control what will happen when the user click any stop button. The default value is on this setting doesn’t work on CGI mode it works on only module mode.
>22. **doc_root = [DIR]** This setting is done if we want to apply PHP to a portion of our website.
>23. **file_uploads = [on/off]** This flag is set to *ON* if file uploads are included in the PHP code.
>24. **mysql.default_host = hostname** This setting is done to connect to MySQL default server if no other server host is mentioned.
>25. **mysql.default_user = username** This setting is done to connect MySQL default username, if no other name is mentioned.
>26. **mysql.default_password = password** This setting is done to connect MySQL default password if no other password is mentioned.

- [참조 - PHP.ini  configuration](https://www.geeksforgeeks.org/php-php-ini-file-configuration/)

- [참조2 - PHP.ini  document - default values](https://www.php.net/manual/en/mysql.configuration.php#ini.mysql.default-password)



### PHP configuration in .htaccess

- 다음과 형태로 설정 가능하다.

  ```php
  PHP_FLAG register_globals ON
  
  PHP_VALUE mysql.default_charset UTF8
  ```

- 실습

  - mysql 기본값 설정

    ```php
    PHP_VALUE mysql.default_host 219.248.167.137
    PHP_VALUE mysql.default_port 7777
    PHP_VALUE mysql.default_user test
    PHP_VALUE mysql.default_password test
    ```



### Mysql 원격 접속

- 유저 생성 및 권한설정

  ```php
  >grant all privileges on test.* to test@'%';
  >flush privileges; // 또는 쉘에서 mysqladmin -u root -p reload
  #kill [mysql pid]  //kill 안하니까 종료가 안됨..
  #service mysql restart
  ```

- ip 허용

  - ` vi /etc/mysql/mariadb.conf.d/50-server.cnf `
    - bind ip ~~~
      - 0.0.0.0으로 바꾸던가
      - 주석처리 하던가

- 공유기 포트포워딩

- 접속

  - `mysql -utest -h[내 ip] --port[포트] -p`