# Async · Await

- 콜백함수 등록방식

- 선언

  - then / catch를 이용

    ```js
    function goWork(time1, timeStartWork) {
      return wakeUp(time1)
        .then(time2 => tackSubway(time2))
        .then(time3 => takeOffSubway(time3))
        .then(time4 => arriveWork(time4))
        .then(arrivalTime => {
          if (arrivalTime > timeStartWork) {
            fire()
          }
        })
    }
    ```

    ----

  - async / await 이용

    ```js
    async function goWork(time1, timeStartWork) {
      const time2 = await wakeUp(time1)
      const time3 = await takeSubway(time2)
      const time4 = await takeOffSubway(time3)
      const arrivalTime = await arriveWork(time4)
      if (arrivalTime > timeStartWork) {
        fire()
      }
    }
    ```

    *※주의 : 반드시 async가 선언된 함수 안에서만 await 콜백을 등록할 수 있다※*

- 장점

  - 미관상 매우 직관적



# Util 모듈

- promisify

  - 함수를 promise 형태로 변환하여줌

    - 기존의 콜백함수 형식으로 작성

      ```js
      crypto.randomBytes(64, (err, buf) => {
      	console.log(buf);
      	console.log(err);
         const salt = buf.toString('base64');
         console.log('랜덤 salt: ', salt);
         console.time('암호화시간');
         crypto.pbkdf2('cookie', salt, 1055351, 64, 'sha512', (err,key)=>{
             console.log('암호화 완료: ', key.toString('base64'));
             console.timeEnd('암호화시간');
        });
      });
      ```

      ---

    - promisify 사용

      ```js
      const randomBytesPromise = util.promisify(crypto.randomBytes);
      const pbkdf2Promise = util.promisify(crypto.pbkdf2);
      ```

      ---

    - then / catch 형식으로 작성

      ```js
      randomBytesPromise(64)
        .then((buf) =>{
             const salt = buf.toString('base64');
             return pbkdf2Promise('password',salt,1055351, 64, 'sha512');
        })
        .then((key)=>{
             console.log('암호화 완료: ', key.toString('base64'));
        })
        .catch((err)=>{
             console.error(err);
        })
      ```

      ---

    - async / await 형식으로 작성

      ```js
      (async () => {
         const bug = await randomBytesPromise(64);
         const slat = buf.toString('base64');
         const key = pbkdf2Promise('password', salt, 1055351, 64, 'sha512');
         console.log('암호화 완료: ',key.toString('base64'));
      });
      ```

      