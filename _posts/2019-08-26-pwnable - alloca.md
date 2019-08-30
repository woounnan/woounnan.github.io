---
published : false
---



# 문제파일



#### 설명 

- 적용방식을 직접 구현한 프로그램

#### 순서도

1. `size`를 입력받는다.

2. global 변수`canary`를(4byte 정수) 입력받는다.

3. alloca 함수를 이용해 스택에 `size`만큼 `buffer`를 할당한다.

4. `buffer[size]`에 `canary`를 저장한다. 

5. `buffer`에 `fgets`로 문자열을 입력받는다.

6. `check_canary`에서 `buffer[size]`의 `canary`가 저장됬던 4바이트 값을

   가져와서 `canary`값과 XOR연산을 수행한다.

7. 결과가 0이 아니면 messed 됬다는 문자열을 출력한 뒤 프로그램이 종료된다.





# 분석



#### alloca

- `alloca`는 스택에 `buffer`를 할당해주는 함수다.

- `esp`와 `buffer`를 조정할 수 있다.

  - *`buffer`위치를 조정하여   `canary`를 `return address`위치에 덮어쓰도록 만들어볼까?*

    : `ret`위치에 저장하기 위해선 `size`값을 음수로 대입해야 하는데, 

    음수일 경우 `buffer[size]=canary `에서 `&buffer[size] = &ret` 위치를 맞추기가 어렵다.

  - *`esp`를 조정하자*

    *how?*

    ​	코드를 보면 canary를 덮어쓸 수 있는 섹션이 하나 더 존재한다.

    ​	![writeCanary](/img/alloca/writeCanary.png)

    ​	연산 전 canary 값을 알려주기 위해

    ​	다음과 같이 저장하는 부분이다.

    ​	`int canary_before = g_canary;`

    ​	*이를 이용하자*

----------



#### 반환 방식

- 프로그램의 return 방식이 조금 다르다

  ​	![return](/img\alloca\return.png)

  ​	최종적으로 `[ebp-4]`에 저장된 주소 - 4에 위치한 값으로 return하게 된다.

- 수정해야할 값

  1. `ebp-4`

     *너로 정했다!*

  2. direct 주소 수정

     코드를 확인한 결과, 진행되는 `esp` 연산으로 이 위치의 값으로 조작하는 것은 불가능하다.

![return2](C:\Users\Administrator\Downloads\woounnan.github.io\img\alloca\return2.png)

-------

#### size 계산

- 위에서 살펴본대로 `check_canary()` 내의 `ebp-0x14`에 `canary`값을 대입하는 코드를 이용해야 하고,

  따라서, 

  `ebp-0x14` =  `ebp-4`

  가 되도록 스택을 계산해야 한다.

- `check_canary` 까지 `esp`값의 계산과정을 보면

  ![calc](img\alloca\calc.PNG)

  1. alloca
     - size + 34  한 값에 0x10을 나눠준 결과를 esp와 더한다.
  2. More operations... (from alloca to check_canary )
     - esp가 몇번 push, add된다.
     - 이것은 몰라도 된다
  
- *계산할 필요가 없다!*

  `check_canary` 내에 bp를 걸고 `ebp-0x14`의 값을 확인한 뒤

  마지막 `main`함수 끝에서 조작하려는 스택값(`ebp-4`)를 확인하여

  이 둘의 차를 계산하면 되겠다.

- 결과적으로 `-67`~`-82`사이의 값이

