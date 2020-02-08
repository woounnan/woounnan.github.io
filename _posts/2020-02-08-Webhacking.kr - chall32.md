# 문제

- 클릭하면 투표가 된다.

- 한번만 할 수 있다.

  





# 풀이

- 나에게 100번 투표해야 한다.
- 클릭시 vote_check 쿠키값이 생성된다.(아마)
- vote_check 값을 다른것으로 바꿔보아도 별다른 반응이 없다.
- 아예 쿠키값을 삭제하니 중복투표가 가능하다.
- repeater로 한번에 여러번 투표해서 solve



# 알게된 것

### Fiddler, Burp연동

- fiddler에서 proxy설정을 manual proxy로 burp suite에 잡아주면
- 패킷 -  fiddler - burp suite 순서로 패킷을 옮겨가며 캡쳐할 수 있다.
- 위 상태에서 burp를 끄니, fiddler 단독으로도 동작가능했다.
  - 반대의 경우는  안됨

### Repeater

- 캡쳐했던 패킷을 여러번 전송시켜 주는 기능
- Burp suite로 보냈던 패킷을 편하게 repeater 시킬 수 있었다.
  - 하지만, 한번 수행으로 여러번 패킷을 보내는 기능은 없는것 같다.
  - fiddler로 수행해야 한다.
- Fiddler에서 multiple request 보내기
  - 원하는 패킷을 클릭한 뒤, `Shift + r`