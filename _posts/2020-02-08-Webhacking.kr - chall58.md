# 문제

- 쉘 같은 인터프리터가 주어진다.
- flag ls id 처럼 명령어가 극히 제한된다.
- flag는 권한이 없다고 나온다







# 풀이

- 자바스크립트를 보자

  ```js
  $(function () {
        var username = "guest";
        var socket = io();
        $('form').submit(function(e){
          e.preventDefault();
          socket.emit('cmd',username+":"+$('`#`m').val());
          $('#m').val('');
          return false;
        });
  ```
  
  - socket.io로 통신하고, username(guest) : 입력값의 형태로 전달한다.

- 권한이 없다고 했으므로 전달되는 username을 admin으로 바꿔주자 with burp

# 알게된 것

### form - autocomplete 태그

- 자동완성 기능
- on이면 자동완성 기능이 동작함

### e.preventDefault()

- 이벤트를 중단해주는 함수라고 한다.
- 위 문제에서 왜 쓰였는지 모르겠다