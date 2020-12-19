## [mac] Error: That port is already in use

### 원인

요청한 port로 이미 사용중이라는 메세지입니다.

### 해결 방법

터미널환경에서 명령어를 입력합니다.

**조회**

`lsof -i :포트번호` 

![carbon (3)](https://user-images.githubusercontent.com/48043799/102679088-3b6eb680-41f0-11eb-9fda-00443b1f75a5.png)

명령어를 입력하면 위와 같은 COMMAND가 보입니다. 현재 입력한 포트번호를 할당하고 있는 서비스들의 목록으로 종료하고 싶은 서비스의 PID를 확인합니다.

**종료**

`kill -9 PID`

이후 조회시 사용했던 명령어를 입력하면 해당 서비스가 종료된 것을 확인할 수 있습니다.

