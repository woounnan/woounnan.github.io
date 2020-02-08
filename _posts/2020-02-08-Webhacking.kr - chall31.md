# 문제

- 10000~10100 포트로 소켓 데이터를 전송한다.





# 풀이

- 해당 데이터를 캡쳐해야 한다.

-  포트포워딩으로 10000~10100 에 해당하는 패킷이 들어오면

  nc로 열어둔 포트(10000)으로 포워딩하도록 설정한다.

- nc로 데이터 캡쳐하면 끝



# 알게된 것

### 아파치 포트 변경

- 변경

  ```shell
  sudo vi /etc/apache2/ports.conf
  Listen 8090
  sudo vi /etc/apache2/sites-enabled/000-default.conf
  <VirtualHost *:8090>
  ```

- [참조](https://www.ostechnix.com/how-to-change-apache-ftp-and-ssh-default-port-to-a-custom-port-part-1/)

### Virtual Box 포트포워딩 삽질

- host-only와 NAT 어댑터를 동시에 활성화 시킬 순 없다.(바꾸는 방법 있을텐데 아직 모르겠음)

- 호스트pc와 동일하게 공유기에서 직접 IP받도록 설정하려면

  어댑터를 브릿지 모드로 설정한다.

  - 호스토 온리니 nat니 개삽질 하지말고, 그냥 처음부터 브릿지 모드로 사용하자.

