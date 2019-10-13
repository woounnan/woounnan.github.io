# 19.09.30

- Preview.vue 기본 컴포넌트 생성
- msg 저장(mongoose update)
- getChat 하다맘(테스트해서 확인 후 조치할 것)



# 19.10.01

- msg 저장 09.30꺼는 에러남(다시 함)

- 채팅 백업(getChat), 파일 저장 구현(db_utils.js::save)

- 내일 할일

  - BasicChat.vue에서 Work.vue 띄워야함

  - import로 선언 안하고 Vuex로 전달하는것 시도해봤으나 실패

    - import Work ... 로 선언하자




# 19.10.02

- modal로 창 띄움
- db/work 구현
  - 단순히 파일저장용도
  - sender인 Work.vue에서 파일과, works(title, contents, date 등)를 설정하여 db/work로 요청
  - db/work는 파일을 저장한 뒤, saveName과 realName을 works에 포함시켜서 응답
  - Work.vue는 받은 works를 bus emit(work)로 BasicChat.vue에 전달 
  - BasicChat.vue는 bus on(work)로 works를 수신하여 일반적인 msg와 동일하게 처리
    - 주의할 점은, receiver가 배열(여러명 가능)이므로, 각 수신자에게 동일한 메시지 처리를 해야한다.
- 큰일남.. multer 오류남. 
  - 다음에 할 때, multer 기본 파일 수신 코드로 바꾼뒤 테스트해야함 하..



# 19.10.04

- multer 오류 해결

  - dictionary 전달이 안됨, 각 요소를 따로 append 해줘야함

    ```js
    fd.append('title', this.v_work.title)
    fd.append('selects', this.v_work.selects)
    fd.append('contents', this.v_work.contents)
    fd.append('startDate', this.v_work.startDate)
    fd.append('endDate', this.v_work.endDate)
    fd.append('notice', this.v_work.notice)
    ```

- WorkCard.vue 구현

  - 기본 틀 구성
  - 

- `state_work`, `state_notice` 추가

  - 알림은 "미확인", "확인", "확인-대기" 으로 구성

    - 받은 사람이 확인했을 경우 "확인"으로 변경가능하며, 이 때 코멘트를 남길 수 있음

    - 여기에 보낸 사람이 응답할시 "확인-대기"로 변경되며, "확인-대기"는 버튼 색깔이

      주황색(미정)으로 표시됨(미확인한게 있다는 의미로)

  - 작업은 "미제출", "승인대기", "승인거절", "승인완료"로 구성

    - "미제출" - 빨강, 응답 대기상태
    - "승인대기" - 주황색, 응답했지만 송신자가 확인안한 상태
    - "승인거절" - 연빨강(?), 응답한 자료가 거절되어 다시 제출해야 함
      - 송신자의 거절사유를 확인할 수 있음
    - "승인완료" - 초록색, 응답한 자료가 승인됨.  
      - 시간이 만료되거나, 수신자가 삭제할 경우 처리할일에서 지나간 일로 등록됨
        - 지나간 일을 구분하기 위한 상태를 추가  - 하자

- multiple receiver 처리

  - receiver 배열요소 각각에게 모두 메시지 전송

    - 발생문제

      - file 전송시 배열은 문자열로 치환된다... (['계획과장',''군수과장'] -> '계획과장,군수과장')
        - split해서 배열로 만들어줘야함..
      - 배열은 forEach시 value가 인자로 들어가네??
        - 지난번엔 idx가 들어가더니 하
        - 별로 중요한거 아님
      - 이상한 에러가 발생한다. 백엔드 서버에서.. 해결해야 됨




# 10.05

- multiple receiver 에러 해결

  - `notice` 자료형이 boolean인데 work등록시 true/false 전달하는게 문자열로 변환되서

    에러가 난것이었다.

    - 다시 boolean으로 변경해주어 해결

      ```js
      notice: data.notice === 'true',
      ```

- 여러명에게 보낼 때, 보낸 메시지가 모두 지금 채팅방에 뜨고, 채팅방 다시열어야 사라지던거 해결

  - setInput처리에서 보낸 메시지 모두 `feed`에 push하도록 만들어서 채팅방에 모든 메시지가

    다 입력된 것이었다.

    - 지금 열린 채팅방 대상일 때만 채팅방에 msg가 push되도록 변경

      ```js
            if(to === this.title)
              this.onNewOwnMessage(msg)
          },
      ```

- 상태 추가
  - state_notice : '미확인', '확인', '확인-대기'
  - state_work : '미제출', '승인대기', '승인거절', '승인완료'
  
- 채팅/등록창에서 작업/알림 구분 표시

- 부서에게 전송

  - 확인해봐야함

- 부서, 사람 따로 검색하던거 합쳐서 검색하게 만듬

- 파일 다운로드 링크 해야함!

- 일반 메시지 보내는데 works로 인식되는거 해결해야함.. ㅡㅡ



# 10.06

- 부서에게 전송 구현

- 일반메시지 works로 인식되는거 해결

  - `flag_expired`가 default 설정되있어서, 일반메시지에 works를 설정안했는데도 DB에 저장시

    works가 생긴 상태로 저장되서 그랫던 것.

    - default를 삭제시키고, work 등록시 `flag_expired` 설정해주도록 만들어서 해결

- tabs 기본 틀 구현

  - 남은 것
    - UI 다듬기(즐겨찾기, contents, 남은 시간, 상태 표시 등)
    - `getWorks` 에서 받아온거 출력시키기
    - 동적인 기능 만들기(클릭시 나오는 창, 즐겨찾기 등)

- `flag_expired` 삭제
  
  - 시간 지났다는건 front에서 시간값 비교해서 즉시 확인하는게 맞네.
- 작업 구분해서 리스트화 시키기 in `store.js`
  - 내일 해야될거임.
  - `date`로 작업을 구분해서 저장
  - `convs.id`와 내 id가 다르면 받은 알림/작업임
  - `date`로 구분해서 작업을 계속 등록하는데, `date`가 같으면 같은 작업을 다른 대상에게 보낸거임
    - to에 대상만 추가
  - 문제...
    - works.notice 가 undefined인 것 제거하려고 햇는데 잘 안됨.. 이거 해결해야함



# 10.08

- works 아닌 메시지 제거해야함
  - list_user 초기화하고 다시 메시지 생성해서 확인해보기.
  - 해결, splice를 잘못 활용했었음
- 메시지가 2번 보내짐
  - 원인 찾아야 함. 이따가 이거부터 할것
  - 다시 잘됨.. 일단 넘어가고 나중에 안되면 다시 확인하기
- works 등록할 때 처음부터 이미 등록된 works라고 나오는거 해결해야됨



# 10.09

- 이미 등록된 works 해결
  - list_keys의 push를 잘못했었음
  - works 배열 루프돌 때 forEach로 사용해야함
- ViewWork.vue 구현해야함
  - reply 구현
  - file link 구현
  - ui 정리
  - 스크롤 이상함 + 화면 이상하게 뜨는거 질문올려서 해결해야함



# 10.10

- 화면 픽스해야됨

  - 두번째께 잘 되는것 같음
  - 속도가 느리고 까만건 다이얼로그가 여러번 뜨는것 같음 
    - 확인해야됨
  - 해야될건 다이얼로그 한번만 뜨게 하고, 가운데 뜨게하기

- 댓글 사용자 값으로 제대로 출력하기

- 상태 제대로 출력하기

  - 기능도 만들기
- 완료
  
- 최종

  - 사용자 실제로 받아와서 출력

  - 기능 동작하도록 구현

    

# 10.12

- time_expired 구현 50%
  - `flag_expired`를 이용하여 만료되게 하는것은 구현햇음
  - 이것을 View에서 표시할 때 구분되서 표시되고 지나감은 회색처리되도록 만들기만 하면 됨
- works.favor 속성 추가(즐겨찾기)
- 연등때 해야할 것
  - 사용자 실제 리스트 받아와서 출력
  - 단추를 상태에 따라 다르게 표시
  - 단추 기능 구현
    - 미제출 
      - 자료제출 버튼
      - 제출했을 때 승인대기로 변경
      - 상태가 변경될 때 관련자 db 업데이트, view 업데이트
    - 승인대기
      - 승인, 거절 버튼
        - 승인했을 때 
          - 삭제가능하도록 만드는 것(이건 부가적인거라 나중에...)
          - 승인완료로 업데이트 시키기
        - 거절햇을 때
          - 승인거절로 업데이트 시키기
          - 거절할 때 사유 입력하는 폼 만들기
          - C에게 다시 제출버튼 생성 및 거절사유 표시
- works 전송 메커니즘 구현
  - works 상태관리
    - 받은 사람이 작업을 제출하면 받은 사람의 works 상태를 변경한다.
    - 요청자의 상태는 store.js에서 판단하여 변경시킨다.
      - 모든 받은 사람이 작업을 제출했다(모든 상태가 승인대기다)
      - 그러면 그 때 store.js에서 요청자의 해당 works 상태를 승인대기로 변경시킨다.
    - 첫 works 요청 메시지 외에 상태변경시마다 msg를 추가해야 하는 문제 해결
      - 단순히 msg만 추가시키면 되므로
      - work.state를 승인대기로 바꾸고 일반적인 works 메시지 추가하듯이
      - bus.$emit('works')를 보낸다
        - 그러면 chating msg에 추가되겟지
      - View에서 관리하는 works는 
        - 추가할 때 조건을 달아놔서
        - 위에서 msg에 새로 추가한 works와 관련없다.
        - 정상 동작

# 10.13

- 현재 문제
  - 제출을 해도 상태가 안바뀐다
    - 기존의 work를 못찾아서 상태가 안바뀌는것 같다.
    
  - signal work는 BasicChat을 한번 열어야지 on이 된다.
    - BasicChat을 mount 안하고 전송가능하게 만들어야 한다
      
      - Chat.vue는 홈 화면이라 항상 mount 된다.
      
      - Chat.vue에다가 work를 send해주는 on을 만들고
      
        제출할 때 이 on에 emit를 보내야한다.
      
    
  - only_view work 메시지가 전송될 때 상태가 안바뀐다
  
    - flag_date를 설정햇다
      - 해당 works를 검색하기 위함
    - 테스트 해보고 안되는거 찾아서 하기 *필수
  
  - '제출하기' 기능 구현 *필수
  
  - 제출하기 외 나머지 버튼 만들기 *필수
  
  - Preview 구현  *필수
  
  - 남은 시간 계산해서 ViewParts에 표시 *필수
  
  - 정렬 구현
  
  - 새 메시지 구현 
  
  - 알림 구현
  
  - 당직관리 구현