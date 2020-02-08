# 문제

- 두가지 입력값과 captcha를 2초안에 입력해야한다

  

# 풀이

- 자바스크립트를 수정하여 캡차값과 입력값을 한번에 실행하도록 바꿔주면 된다.



# 알게된 것

### Date

- 시간값을 제어할 수 있다.

- 몇가지 샘플

  - new Date().getTime()

    - 현재 시간값 timestamp로 출력

  - ```js
    d = new Date();
    var st = new Date(d.getYear()-100+2000,d.getMonth()+1,d.getDate(),d.getHours(),d.getMinutes(), d.getSeconds()+1, d.getMilliseconds()).getTime()
    
    ```

    - 1초 후의 timestamp값 출력 예시

- 위 문제와는 관련없다.

  - 잘못 착각해서 시간값을 바꿔주려고 시도하다가 검색했다.