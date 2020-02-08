# 문제





# 알게된 것

### MD5

- 출력은 128bit

### SHA

- sha1 은 40글자이다. //확실하지 않음

### 레인보우 테이블

- 정의

  > 레인보우 테이블은 해시 함수([MD5](https://namu.wiki/w/MD5), [SHA-1](https://namu.wiki/w/SHA-1), [SHA-2](https://namu.wiki/w/SHA-2) 등)을 사용하여 만들어낼 수 있는 값들을 **왕창** 저장한 표이다. 물론 해시 함수는 입력이 무제한이라서 모든 내용을 넣는 게 아니고, 이를테면 영어 소문자와 숫자 조합으로 일정 길이까지의 모든 문자열에 대해서 계산한다거나 하는 것이다. 

### tqdm

- percentage를 나타내줄 수 있는 파이썬 라이브러리

- for문의 in 부분을 tqdm.tqdm() 으로 감싸준다

  `for x in tqdm.tqdm(range(10)):`

  