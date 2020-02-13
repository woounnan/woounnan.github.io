# 문제

- XSS 인젝션 우회 문제

- html 코드를 입력할 수 있다.

- 입력된 코드가 저장된 html 문서를 보여준다

  

# 풀이

- 안되는것

  - script
  - on
  - mouse
  - over
  - alert
  - img src
  - ...
  - ...
  - 다 안되네? ㅡㅡ
- XSS 인젝션 우회를 찾아보자.

  - 거의다 안되는걸 보니 문자열 필터기반인듯 한데 이것을 우회하는 방법을 찾으면 될것 같다.
- 연속된 알파벳이 있으면 hack으로 간주되는듯 하다..
    - 띄워쓰기를 중간에 넣어도 된다.
  - 웹서버에서 몇가지를 넣어보면서 테스트 해본 결과
  
- Null 바이트를 인젝션 해야한다.
  - `<script>alert('1');</script>`
    - 위 코드의 연속된 문자 사이에 널바이트를 삽입해주면
    - 연속된 문자를 검사하는 루틴은 통과되지만
    - html 코드로 저장될 때 해당 문자열은 연결되어 저장되고
    - 자바스크립트로써 동작하게 되어 pwn!

# 알게된 것

### HTML의 띄어쓰기

- html에서
  - 띄어쓰기는 적용되지만(1번, 여러번 띄어도 1번)
  - %00는 취급하지 않는다.
    - 따라서 위의 문제처럼
    - <s%00c%00r%00i%00p%00t>와 같은 식으로 입력할 경우`<script>`가 저장된다

### 널바이트 인젝션

- 문자열 종료로 인식해, 검사루틴을 우회할 수 있는 취약점

- PHP 버전에 따라 취약한 버전과 취약하지 않은 버전이 있다(사용함수 다름)

  | **POSIX**(취약) | **PCRE**(안전) |
  | --------------- | -------------- |
  | ereg_replace()  | preg_replace() |
  | ereg()          | preg_match()   |
  | eregi_replace() | preg_replace() |
  | eregi()         | preg_match()   |
  | split()         | preg_split()   |
  | spliti()        | preg_split()   |
  | sql_regcase()   | No equivalent  |

- [참고 - 티스토리]([https://hackability.kr/entry/PHP-%EB%AC%B8%EC%9E%90%EC%97%B4-%ED%95%84%ED%84%B0%EB%A7%81-%ED%95%A8%EC%88%98ereg-eregi-%EC%B7%A8%EC%95%BD%EC%A0%90%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%9A%B0%ED%9A%8C](https://hackability.kr/entry/PHP-문자열-필터링-함수ereg-eregi-취약점을-이용한-우회))

