# 문제

- 조잡한 경주형태 게임
- 클릭하면 오른쪽으로 1칸 옮겨지며 Goal을 통과해야 함







# 풀이

- 자바스크립트를 보자

  ```js
  <a id="hackme" style="position:relative;left:0;top:0" onclick="this.style.left=parseInt(this.style.left,10)+1+'px';if(this.style.left=='1600px')this.href='?go='+this.style.left" onmouseover="this.innerHTML='yOu'" onmouseout="this.innerHTML='O'">O</a>
  ```
  
  - 클릭할 때 1px가 늘어나고 1600px일 때 solve된다.

- 증가되는 코드를 1600px로 초기화하는 코드로 바꾸고 클릭

# 알게된 것

### 없다...

- 푼것같지 않은 문제였다..
- 내가 모르는게 있었겠지?ㄴ