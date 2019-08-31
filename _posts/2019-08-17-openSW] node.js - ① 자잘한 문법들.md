# 자잘한 문법

### 백틱(``)

- 문자열 정의

- 문자열 안에 변수 삽입

  ```js
  const a = 1
  const b = true
  const c = 'Hi guys'
  
  const d = `${a}는 ${b}는 ${c}` // '', ""는 불가능
  ```

----

### 리터럴

- 대괄호로 <u>객체를 한 번에</u> 정의
- <u>존재하지 않는 객체 멤버값</u>을 정의
- 최신 js에서는 선언하는 방법이 약간 변경됨

----

### 화살표 함수

- 함수를 매크로처럼 간단히 정의(python::lamda와 유사)

  ` const add3 = (x, y) => x + y;`

----

### 비구조화 할당

- 연속된 값을 알아서 변수에 나눠서 저장해줌

  * 배열

    ``` js
    var array = ['nodejs', {}, 10]; 
    ```

    1. ```js
       var node = array[0];
       var obj = array[1];
       var num = array[array.length - 1];
       ```

    2. ` var array = [node, obj, num] = array`

  * 객체

    ```js
    const candyMachine = {
        status: {
            name: 'node',
            count: 5
        },
        getCandy(){
            this.status.count--;
            return this.status.count;
        }
    }
    ```

    1. ```js
       const status = candyMachine.status;
       const getCandy = candyMachine.getCandy;
       ```

    2. `const { status, getCandy, a, b} = candyMachine //a, b란 멤버는 없으므로 undefine 됨`

---

### 가변인자 설정

- 비구조화 할당에서

  ```js
  const array = ['nodejs', {}, 10, true];
  const [node, obj, ...bool] = array;
  
  console$ bool
  (2) [10, true]
  ```

- 함수 인자에서

  ```js
  const m = (x, ...y) => console.log(x, y)
  
  console$ m(1, 2, 3, 4)
  (3) [2, 3, 4]
  ```

---

### 파일 간 데이터 공유

- 공유할 데이터 export

  ```js
  //ex.js
  var greet = function(){
  	console.log('Hello! Im function in ex.js');
  }
  
  module.exports = greet
  ```

- 공유받을 데이터 import

  ```js
  var recvFunc = require('./ex.js');
  recvFunc();
  ```

  ​	*※함수가 아닌 상수, 변수도 가능※*

-----

### console 객체

- console.trace
  - 콜스택 trace 해줌