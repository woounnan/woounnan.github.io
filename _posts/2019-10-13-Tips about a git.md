### 인증오류

- 다음과 같은 에러 발생시

  ```shell
  $ git clone http://github.com/woounnan/woounnan.github.io
  Cloning into 'woounnan.github.io'...
  fatal: unable to access 'http://github.com/woounnan/woounnan.github.io/': error setting certificate verify locations:
    CAfile: D:\Program
    CApath: none
  
  ```

  - ca-bundle.crt 경로를 지정해준다.

    ```powershell
    D:\Program Files\Git>git config --global http.sslCAinfo "d:\Program Files\Git\usr\ssl\certs\ca-bundle.crt"
    ```

    

