# 문제

- 

#  풀이

- 


# 알게된 것

### Cookie

- session cookie

  - 웹 브라우저가 페이지에 접속해있는 동안 유지

- persistent cookie

  - 기간을 설정해 지속적으로 유지시키는 쿠키

  - 이용자의 정보수집을 위한 목적으로 활용됨

  - 사이트 접속했을 때 쿠키를 수집하겠다라는 안내문구가 뜨는 경우가 있는데, 이 때 수집하는 쿠키가 persistent cookie

  - 쿠키 헤더에 expires 값이 들어가 있으면 persistent, 아니면 session cookie다

    - ```html
      Set-Cookie: xxxxxxxx=yyyyyy; expires=zzzz
      ```

- 보안 조치

  - 플래그 설정
    - 각 쿠키값 뒤에 플래그 값을 설정할 수 있다.
  - 종류
    - HttpOnly
      - 자바스크립트로는 쿠키값을 확인할 수 없다.
      - 하지만 http로 전송되는 패킷을 스니핑하는 것은 가능한 단점이 있다.
    - Secure
      - HTTPS로 통신되는 경우만 쿠키값이 활성화 된다.
      - HTTP로 접속할 경우 Secure 플래그가 설정된 쿠키값은 보이지 않는다.