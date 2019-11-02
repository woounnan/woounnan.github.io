# 선행지식

### Off-by-one

길이 검사를 잘못하여 1바이트의 overflow가 생기는 취약점

```c
#include <stdio.h>
 
int main(int argc,char *argv[])
{
    char buf[512];
    
    if(strlen(argv[1]) > 512) {
        printf("prevented oversize\n");
        return -1;
    }
    
    strcpy(buf, argv[1]);
 
    return 0;
}

```



### Poison Null Byte

- 설명 링크
  - [hansoo](https://code1018.tistory.com/220)
  - [bach](https://bachs.tistory.com/entry/how2heap-Poison-NULL-Byte)

