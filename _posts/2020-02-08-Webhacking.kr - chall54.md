# 문제

- 패스워드가 한글자씩 출력되고 모두출력되면 사라짐







# 풀이

- 자바스크립트에서 패스워드 출력부분을 확인한다

  ```js
  
  function run(){
    if(window.ActiveXObject){
     try {
      return new ActiveXObject('Msxml2.XMLHTTP');
     } catch (e) {
      try {
       return new ActiveXObject('Microsoft.XMLHTTP');
      } catch (e) {
       return null;
      }
     }
    }else if(window.XMLHttpRequest){
     return new XMLHttpRequest();
   
    }else{
     return null;
    }
   }
  x=run();
  function answer(i){
    x.open('GET','?m='+i,false);
    x.send(null);
    aview.innerHTML=x.responseText;
    i++;
    if(x.responseText) setTimeout("answer("+i+")",20);
    if(x.responseText=="") aview.innerHTML="?";
  }
  setTimeout("answer(0)",1000);
  ```

- run()

  - Ajax 오브젝트를 세팅한다

- answer()

  - 생성한 Ajax 오브젝트로 flag값을 받아와서 출력시켜준다.
  - 플래그값을 이어서 출력하도록 =를 +=로 변경하고
  - 마지막에 ""로 플래그값을 초기화시키는 구문을 삭제한다.



# 알게된 것

### Ajax

- window.ActiveXObject 어쩌구 되있으면

  Ajax 사용하는거구나..