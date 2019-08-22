# Events 모듈

1. addListener(= on), once

   - 이벤트 등록

     - addListener, on : 그냥 등록
     - once : 등록, 한 번만 호출가능

   - 선언 및 결과

     ```js
     const EventEmitter = require('events');
     
     const myEvent = new EventEmitter();
     
     
     myEvent.addListener('visit', () => {
       console.log('Welcome to visit my home!');
     });
     
     myEvent.on('exit', () => {
       console.log('Bye bye');
     });
     
     myEvent.once('special', () => {
       console.log('Run only once');
     });
     
     myEvent.emit('visit');
     myEvent.emit('exit');
     
     console.log('--------------------');
     
     myEvent.emit('special');
     myEvent.emit('special');
     myEvent.emit('special');
     ```

     ---

     

     ```shell
     root@jkl:/work/nodejs# node test.js
     Welcome to visit my home!
     Bye bye
     --------------------
     Run only once
     ```

----------

2. removeAllListeners, removeListener

   - Listener 제거

     - removeAllListeners : 해당 event의 모든 Listener 콜백함수 제거
     - removeListener : 지정한 하나의 Listener만 제거

   - 선언

     ```js
     const EventEmitter = require('events');
     
     const myEvent = new EventEmitter();
     
     
     myEvent.addListener('visit', () => {
       console.log('Welcome to visit my home!');
     });
     
     myEvent.on('exit', () => {
       console.log('Bye bye');
     });
     
     myEvent.on('exit', () => {
       console.log('Thank you');
     });
     
     
     const callback = () =>{
       console.log('Do you really want to remove this?');
     }
     
     myEvent.on('exit', callback);
     
     
     myEvent.emit('exit');
     console.log('------------------------');
     
     myEvent.removeListener('exit', callback);
     myEvent.emit('exit');
     
     console.log('------------------------');
     myEvent.removeAllListeners('exit');
     myEvent.emit('exit');
     ```

     ---

     ```shell
     root@jkl:/work/nodejs# node test.js
     Bye bye
     Thank you
     Do you really want to remove this?
     ------------------------
     Bye bye
     Thank you
     ------------------------
     root@jkl:/work/nodejs#
     ```

     

