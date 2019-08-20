# URL

1. 가져오기

   ` const url = require('url')`

-----

2. URL 인스턴스

   - URL을 분석해서 딕셔너리로 반환

   - 최신 버전인 WHATWG URL 형식 버전으로 값을 구할 때 사용

     ```js
     const url = require('url');
     
     const URL = url.URL;
     const myURL = new URL('http://www.pwnable.kr/bin/lfh');
     console.log(myURL)
     ```

     ---

     ```js
     PS C:\Users\Administrator\Documents> node .\test.js
     
     URL {
       href: 'http://www.pwnable.kr/bin/lfh',
       origin: 'http://www.pwnable.kr',
       protocol: 'http:',
       username: '',
       password: '',
       host: 'www.pwnable.kr',
       hostname: 'www.pwnable.kr',
       port: '',
       pathname: '/bin/lfh',
       search: '',
       searchParams: URLSearchParams {},
       hash: ''
     }
     ```

---

3. format

   - parse된 URL을 하나의 URL 문자열로 합쳐줌

     ```js
     const url = require('url');
     
     const URL = url.URL;
     const myURL = new URL('http://www.pwnable.kr/bin/lfh');
     console.log(url.format(myURL));
     ```

     ---

     

     ```js
     PS C:\Users\Administrator\Documents> node .\test.js
     
     http://www.pwnable.kr/bin/lfh
     ```

---

4. parse

   - URL을 분석해서 딕셔너리로 반환

   - 구버전 형식의 URL info를 구할 때 사용

     ```js
     const url = require('url');
     
     console.log(url.parse('http://pwnable.kr/bin/lfh'));
     ```

     ------

     ```js
     PS C:\Users\Administrator\Documents> node .\test.js
     
     Url {
       protocol: 'http:',
       slashes: true,
       auth: null,
       host: 'pwnable.kr',
       port: null,
       hostname: 'pwnable.kr',
       hash: null,
       search: null,
       query: null,
       pathname: '/bin/lfh',
       path: '/bin/lfh',
       href: 'http://pwnable.kr/bin/lfh'
     }
     ```

   - 파일경로만 있는 URL을 파싱할 때 사용한다고 함

     - URL 인스턴스 방식은 해당 URL을 전달할 시에, 에러난다고 함

     ```js
     const url = require('url');
     
     console.log(url.parse('/bin/lfh'));
     ```

     ---

     

     ```js
     PS C:\Users\Administrator\Documents> node .\test.js
     
     Url {
       protocol: null,
       slashes: null,
       auth: null,
       host: null,
       port: null,
       hostname: null,
       hash: null,
       search: null,
       query: null,
       pathname: '/bin/lfh',
       path: '/bin/lfh',
       href: '/bin/lfh'
     }
     ```

     