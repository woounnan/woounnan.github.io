# Error 처리방식

1. try-catch

   - 선언

     ```js
     setInterval(() => {
       console.log('start');
       try{
         throw new Error('Will break the server!!');
       }catch(error){
         console.error(error);
       }
     }, 1000);
     ```

     ---

---

2. 콜백함수 내장기능

   - 선언

     ```js
     setInterval(() => {
       console.log('start');
       fs.unlink('./asdfasdfsdaf@!@#.js', (err) => {
       	if(err){
       		console.log(err);
       	}
       })
     }, 1000);
     ```

---

3. process.on('uncaughtException', 콜백함수)

   - When declared like this, it works for all exceptions

   - declaration

     ```js
     process.on('uncaughtException', (err) =>{
     	console.error('An error occurred!!');
     })
     
     setInterval(() => {
       console.log('start');
       try{
         throw new Error('Will break the server!!');
       }catch(error){
         console.error(error);
       }
     }, 1000);
     ```

     ---

     ```shell
     root@jkl:/work/nodejs# node error.js
     start
     Error: Will break the server!!
         at Timeout.setInterval [as _onTimeout] (/work/nodejs/error.js:15:11)
         at ontimeout (timers.js:498:11)
         at tryOnTimeout (timers.js:323:5)
         at Timer.listOnTimeout (timers.js:290:5)
     start
     Error: Will break the server!!
         at Timeout.setInterval [as _onTimeout] (/work/nodejs/error.js:15:11)
         at ontimeout (timers.js:498:11)
         at tryOnTimeout (timers.js:323:5)
         at Timer.listOnTimeout (timers.js:290:5)
     start
     Error: Will break the server!!
         at Timeout.setInterval [as _onTimeout] (/work/nodejs/error.js:15:11)
         at ontimeout (timers.js:498:11)
         at tryOnTimeout (timers.js:323:5)
         at Timer.listOnTimeout (timers.js:290:5)
     start
     Error: Will break the server!!
         at Timeout.setInterval [as _onTimeout] (/work/nodejs/error.js:15:11)
         at ontimeout (timers.js:498:11)
         at tryOnTimeout (timers.js:323:5)
         at Timer.listOnTimeout (timers.js:290:5)
     
     ^C
     root@jkl:/work/nodejs#
     ```

     