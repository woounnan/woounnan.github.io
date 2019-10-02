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