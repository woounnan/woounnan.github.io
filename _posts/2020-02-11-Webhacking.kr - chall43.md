# 문제

- 파일 업로드 폼이 있고

- cat flag를 실행시키라는 안내문구가 나온다.

  

# 풀이

- 파일 업로드 취약점을 이용하여 cat flag를 실행시키는 php 파일을 업로드시켜야할 것 같다.
- 시도
  - 확장자명 바꾸기
    - `myShell.txt.php`
    - 땡
  - 널 삽입
    - `myShell%00.php`
    - 땡
  - 대소문자 바꿔보기
    - `myShell.PHP`
    - 우회도 안되고
    - 원래 리눅스 웹서버에서 php로 인식을 안함..
    - 땡
  - . htaccess 파일 전송해보기
    - .htaccess 파일로 설정값을 조작하면 php파일을 txt로 바꿔놔도 웹서버에 업로드시 php파일로써 실행되게 만들 수 있음.
    - 땡
  - 아무 파일 올려보기
    - `.txt`, `none extension`
      - `.txt`일 때 wrong type
      - `none extension`일 때 not detected
      - `txt`도 우롱타입이라는 것은
      - `none extension`을 type이 not detected라고 표현한다는 것은
      - 확장자로 접근하는 문제가 아니구나!
  - 아무 파일 업로드 하고 ` Content-Type: application/octet-stream` 부분을 삭제해보기
    - not detected!
    - 이걸 바꾸면 되겠다.
- content-type을 이미지로 변경하니 업로드가 된다.
- `system('cat /flag')` 실행하면 끝

# 알게된 것

### MIME

- Multipurpose Internet Mail Extension

- HTTP에서(원래는 메일전송시)단순 문자데이터 이외의 타입(미디어 파일)을 전송하기 위한 인코딩 방식(?), 그리고 그것을 표현하는 방법

- 기능

  - 미디어 파일을 전송 가능한 아스키 형식으로 변환
  - 읽을 때 미디어 타입으로 다시 변환

- 타입

  - 개별타입

    | 타입          | 설명                                                         | 일반적인 서브타입 예시                                       |
    | :------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
    | `text`        | 텍스트를 포함하는 모든 문서를 나타내며 이론상으로는 인간이 읽을 수 있어야 합니다 | `text/plain`, `text/html`, `text/css, text/javascript`       |
    | `image`       | 모든 종류의 이미지를 나타냅니다. (animated gif처럼) 애니메이션되는 이미지가 이미지 타입에 포함되긴 하지만, 비디오는 포함되지 않습니다. | `image/gif`, `image/png`, `image/jpeg`, `image/bmp`, `image/webp` |
    | `audio`       | 모든 종류의 오디오 파일들을 나타냅니다.                      | `audio/midi`, `audio/mpeg, audio/webm, audio/ogg, audio/wav` |
    | `video`       | 모든 종류의 비디오 파일들을 나타냅니다.                      | `video/webm`, `video/ogg`                                    |
    | `application` | 모든 종류의 이진 데이터를 나타냅니다.                        | `application/octet-stream` (잘 알려지지 않은 이진파일), `application/xml`,  `application/pdf` |

  - 멀티 타입

    ```html
    multipart/form-data
    multipart/byteranges
    ```

    - 다양한 타입을 전송할 때 사용
    - 각 파트는 자신만의 헤더를 갖음

  