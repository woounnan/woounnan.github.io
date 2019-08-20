# Path

1. 가져오기

   ` const path = require('path')`

-----

2. parse

   - 경로를 분석해서 딕셔너리로 반환

     ```js
     const path = require('path');
     
     console.log(__filename);
     console.log('----------')
     console.log(path.parse(__filename));
     ```

     ---

     ```js
     PS C:\Users\Administrator\Documents> node .\test.js
     
     C:\Users\Administrator\Documents\test.js
     ----------
     {
       root: 'C:\\',
       dir: 'C:\\Users\\Administrator\\Documents',
       base: 'test.js',
       ext: '.js',
       name: 'test'
     }
     ```

---

3. format

   - parse된 경로를 하나의 문자열로 합쳐줌

     ```js
     const path = require('path');
     
     
     console.log(path.format({
     	root: 'C:\\',
      	dir: 'C:\\Users\\Administrator\\Documents',
      	base: 'test.js',
     	ext: '.js',
      	name: 'test'
     }));
     ```

     ---

     

     ```js
     PS C:\Users\Administrator\Documents> node .\test.js
     
     C:\Users\Administrator\Documents\test.js
     ```

---

4. join

   - 인자값을 모두 상대경로로 인식하여 경로 생성

     ```js
     const path = require('path');
     
     
     console.log(__dirname);
     console.log(path.join(__dirname, '..', '..', '/users', '.', '/woounnan'));
     ```

     ---

     ```js
     PS C:\Users\Administrator\Documents> node .\test.js
     
     C:\Users\Administrator\Documents
     C:\Users\users\woounnan
     ```



---

5. resolve

   - 인자값을 절대경로도 고려하여 경로 생성

     - /users : 현재 경로의 users 폴더가 아닌 루트(/) 아래의 users로 인식

       ```js
       const path = require('path');
       
       
       console.log(__dirname);
       console.log(path.resolve(__dirname, '..', '..', '/users', '.', '/woounnan'));
       ```

       ---

       ```js
       PS C:\Users\Administrator\Documents> node .\test.js
       
       C:\Users\Administrator\Documents
       C:\woounnan
       ```

       

---

