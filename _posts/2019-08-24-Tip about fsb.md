#### Check the value in stack on a 64bit system

- 64bit Calling convention

  ![fsb-cv](/img/fsb-cv.png )

  - 7번째 인자부터 스택에 저장되므로 스택값을 확인할 때 `%7$lx`처럼 **7부터 카운트**를 해줘야 한다.
  
  - 7번째 인자부터 `rsp + (DWORD x n)`위치의 스택에 인자값이 저장된다.
  
    ```cpp
    void main()
    {
      printf("%x, %x, %x, %x, %x, 6666: %x, 7777: %x\n", 0x10, 0x20, 0x30, 0x40, 0x50, 0x60, 0x70);
    
    }
    ```
  
    ---
  
    - stack
  
    ```shell
    (gdb) x/20wx $rsp
    0x7fffffffe380: 0x00000060      0x00000000      0x00000070      0x00000000
    ...
    ...
    ```
  
    ---
  
    - output
  
    ```shell
    (gdb) ni
    10, 20, 30, 40, 50, 6666: 60, 7777: 70
    ```
  
    