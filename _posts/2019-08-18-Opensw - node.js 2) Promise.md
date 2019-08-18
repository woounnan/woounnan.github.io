# Promise

- 정의

  - 성공/실패로 구분되는 콜백함수를 쉽게 등록할 수 있게 함

- 선언

  ```js
  var _promise = function (param) {
  
  	return new Promise(function (resolve, reject) {
  
  		window.setTimeout(function () {
  			if (param) {
  				resolve("succeeded");
  			}
  			else {
  				reject(Error("failed"));
  			}
  		}, 3000);
  	});
  };
  ```

  - resolve는 then(성공)으로 값을 받을 수 있음
  - reject는 catch(실패)로 값을 받을 수 있음

- then, catch 

  - 다중으로 연결된 예제

    ```js
    async()
    	.then(function() { return asyncThing2();})
    	.then(function() { return asyncThing3();})
    	.catch(function(err) { return asyncRecovery1();})
    
    	.then(function() { return asyncThing4();}, function(err) { return asyncRecovery2(); })
    	.catch(function(err) { console.log("Don't worry about it");})
    
    	.then(function() { console.log("All done!");});
    ```

    - 순서도

      ```flow
      as1=>condition: async
      as2=>condition: asyncThing2
      as3=>condition: asyncThing3
      as4=>condition: asyncThing4
      rec1=>condition: asyncRecovery1
      rec2=>condition: asyncRecovery2
      dwa=>operation: "Don't worry about it"
      e=>end: "All done!"
      
      as1(yes)->as2
      as1(no)->rec1
      as2(yes)->as3
      as2(no)->rec1
      as3(yes)->as4
      as3(no)->rec1
      as4(yes)->e
      as4(no)->dwa
      dwa->e
      
      rec1(yes)->as4
      rec1(no)->rec2
      
      rec2(no)->dwa
      rec2(yes)->e
      ```

      

