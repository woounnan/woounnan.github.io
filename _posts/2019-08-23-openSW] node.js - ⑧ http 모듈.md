

# Http

1. Create a server to send simple html data

   - declaration

     - `createServer` 메소드로 서버 객체를 생성

     - `end`로 html의 `EOF`라는 것을 명시해줘야 한다.

     - `listen`메소드로 서버 객체를 동작시킬 수 있다.

       ```js
       const http = require('http');
       http.createServer((req, res) => {
         console.log('running the server!');
         res.write('<h1>hello!!!!</h1>');
         res.write('<h1>hello!!!!</h1>');
         res.write('<h1>hello!!!!</h1>');
         res.write('<h1>hello!!!!</h1>');
         res.end('<p>The end</p>');
       }).listen(8081, () =>{
         console.log('wating for connection on port 8081');
       });
       ```

       

     ---

     

     ```shell
     root@jkl:/work/mySpace/nodejs# node http.js
     wating for connection on port 8081 //server running
     ```

     ----

     - 클라이언트: attempt to connect to the server

       ![njs-create](/img/njs-create.png)

     ---

     - Server side

       ```shell
       root@jkl:/work/mySpace/nodejs# node http.js
       wating for connection on port 8081
       running the server!
       running the server!
       ```

---

2. Create a service that sends a html document

   - declaration

     - `fs.readFile`로 파일 읽기

     - `server.on()` 이벤트 처리 메소드로 listening, error 리스너 등록하기

       ```js
       const http = require('http');
       const fs = require('fs');
       
       
       const server = http.createServer((req, res) => {
           console.log('running the server!');
           fs.readFile('./test.html', (err, data) => {
           if(err){
             throw err;
           }
           res.end(data);
         })
       }).listen(8081);
       
       server.on('listening', () => {
         console.log('Waiting for connection from the client on port 8081');
       });
       
       server.on('error', (err) => {
         console.error(error);
       });
       ```

       ---

       - Client side

         ![njs-sendHtml](C:\Users\Administrator\Downloads\woounnan.github.io\img\njs-sendHtml.png)

       ---

       - Server side

         ```shell
         root@jkl:/work/mySpace/nodejs# node http.js
         Waiting for connection from the client on port 8081
         running the server!
         running the server!
         ```

---

3. Set cookies

   - declaration

     ```js
     const server = http.createServer((req, res) => {
         console.log('running server!');
         res.writeHead(200, {'Set-Cookie': 'mycookie=test'});
       })
     }).listen(8081);
     ```

     