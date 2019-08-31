---
published : false
---





# 설명



### 흐름

- `my_hash()`에서 구한 hash값을 사용자에게 입력받고
- base64 인코딩된 값을 입력받아 decode한 뒤
- decode한 값에 대해 md5 calculation을 수행한다.



### Secure

- stack canary가 설정되어 있다.





# 분석



### Canary

- `my_hash()`함수에서 canary값이 연산에 사용된다
  - canary값과 랜덤값(seed: time())이 연산되는데
  - 연산내용은 단순 덧셈 뺄셈이므로 랜덤값을 알면 canary값을 구할 수 있다.
  - 랜덤값은 seed가 같아야 하는데 서버에서 사용하는 seed는 `time()` 값이다.
    - `nc` 러닝서버는 `pwnable.kr`의 ssh 접속서버와 동일하다.
    - `time()`을 구하는 프로그램을 만들어, nc 러닝서버와 동시에 실행하면
    - `canary`연산에 사용되는 `time()`값을 구할 수 있다.



### Vulnerability

- `process_hash()`
  - base64 encoding된 입력값을 받아 `g_buf`에 저장한다.
  - `g_buf`의 값(size: 0x400)을 decoding 후 `v3` 배열(size: 0x200)에 저장한다.
    - 0x400의 인코딩된 문자열을 디코딩 한 길이는 0x200을 초과한다
    - 오버플로우 발생
- `my_hash()`에서 사용된 `canary`값과 `process_hash()`에서 사용되는 canary 값이 동일하다.
  - `my_hash()`에서 구한 canary 값을 이용해 오버플로우 시킨다.



### Exploit

- 프로그램 내에 `system()` 함수가 존재하므로

- `return address`를 그곳으로 설정하고

- `g_buf`를 입력할 때 끝에 `/bin/sh`를 입력해준 뒤

- 해당 주소를 `argv`로 설정한다

- 결과

  ![proof](/img/md5/proof.png)

