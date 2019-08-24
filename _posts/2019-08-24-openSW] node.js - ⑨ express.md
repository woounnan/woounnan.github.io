# 익스프레스

### 설치

- `npm i -g express-generator` on ubuntu 18.10

-------

### 프로젝트 생성

- `express [프로젝트 이름] --view=[format]`

  ```shell
  root@jkl:/work/mySpace/nodejs# express basic --view=pug
  
     create : basic/
     create : basic/public/
     create : basic/public/javascripts/
     create : basic/public/images/
     create : basic/public/stylesheets/
     create : basic/public/stylesheets/style.css
     create : basic/routes/
     create : basic/routes/index.js
     create : basic/routes/users.js
     create : basic/views/
     create : basic/views/error.pug
     create : basic/views/index.pug
     create : basic/views/layout.pug
     create : basic/app.js
     create : basic/package.json
     create : basic/bin/
     create : basic/bin/www
  
     change directory:
       $ cd basic
  
     install dependencies:
       $ npm install
  
     run the app:
       $ DEBUG=basic:* npm start
       
  root@jkl:/work/mySpace/nodejs#
  ```

  ---

- 패키지 설치

  - `npm i`

    ```shell
    root@jkl:/work/mySpace/nodejs/basic# npm i
    
    > core-js@2.6.9 postinstall /work/mySpace/nodejs/basic/node_modules/core-js
    > node scripts/postinstall || echo "ignore"
    
    Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!
    
    The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:
    > https://opencollective.com/core-js
    > https://www.patreon.com/zloirock
    
    Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)
    
    npm notice created a lockfile as package-lock.json. You should commit this file.
    added 118 packages from 174 contributors in 4.666s
    ```

    - `basic/package.json` 에 저장된 설정대로 설치된다.

      ```json
      {
        "name": "basic",
        "version": "0.0.0",
        "private": true,
        "scripts": {
          "start": "node ./bin/www"
        },
        "dependencies": {
          "cookie-parser": "~1.4.4",
          "debug": "~2.6.9",
          "express": "~4.16.1",
          "http-errors": "~1.6.3",
          "morgan": "~1.9.1",
          "pug": "2.0.0-beta11"
        }
      }
      ```

----

### 서버 구동

- `npm [run] start`

  - `package.json::script`에 `start` 로 저장된 `node ./bin/www`가 실행됨

  ```shell
  root@jkl:/work/mySpace/nodejs/basic# npm start
  
  > basic@0.0.0 start /work/mySpace/nodejs/basic
  > node ./bin/www
  
  ^Croot@jkl:/work/mySpace/nodejs/basic# npm start
  
  > basic@0.0.0 start /work/mySpace/nodejs/basic
  > node ./bin/www
  
  GET / 200 239.864 ms - 170
  GET /stylesheets/style.css 200 2.841 ms - 111
  GET /favicon.ico 404 11.897 ms - 1082
  ```

  ----

  **Client side**

  ​	![runServer](/img/express/runServer.png)

