# Docker

> 도커를 사용하며 필요하고 부족했던 부분을 정리한 내용입니다.



<br>

## Docker?

컨테이너 기반의 오픈소스 가상화 플랫폼, 다양한 프로그램, 실행환경을 컨테이너 단위로 추상화하여 동일한 인터페이스로 프로그램의 배포 및 관리를 단순화 시켜주는 기능을 합니다.

백엔드프로그램, DB서버, 메세지 큐 등 어떤 프로그램도 컨테이너로 추상화가 가능하고 aws, azure, google cloud등 실행 가능합니다.



## 컨테이너와 VM의 차이

### 컨테이너

다른 컨테이너들과 호스트 os 자원 커널을 공유해서 사용함, 별개의 프로세스로 독립적인 환경

### VM

하이퍼바이저를 통해 호스트 os에 접속, VM은 application이 필요한 리소스보다 더 많은 것을 제공하여 효율적이지 못함

![exIhw](https://user-images.githubusercontent.com/48043799/62411932-c7049900-b635-11e9-8535-597e57dca388.png)



<br>

## Image

이미지는 실행 가능한 패키지(app이 구동할때 필요한 모든 것 runtime, 라이브러리, 환경변수, 설정파일등)를 말합니다. 격리된 공간에 프로세스가 동작하며 가상화 기술의 하나로 기존의 가상화와는 조금 차이가 있습니다.

#### 기존 가상화

- VMware, VirtualBox 가상머신 호스트 OS위에 게스트 OS 전체를 가상화 방식, 다중으로 os를 가상화 할 수 있고 사용법이 간편했지만 무겁고 느려 운영환경에선 부적합

#### 현재 가상화

- CPU의 가상화기술(HVM), KVM 반가상화(Xen) -> 이들은 게스트 os가 필요하지만 전체 os를 가상화 하는 방식이 아님,  호스트형 가상화 방식에 비해 효율적
- 하지만  반가상화나 전가상화는 성능에 한계가 있어 개선하기 위해 **프로세스 격리하는 방식** 등장
- 리눅스에서 이 방식을 리눅스 컨테이너라 하고 가볍고 빠름 cpu나 메모리는 프로세스가 필요한 만큼만 추가 사용

또한 도커이미지는 컨테이너를 실행하기 위한 모든 정보를 갖고 있기에 용량이 수백메가에 이르는데 이는 기존 이미지에 추가할 때 마다 수백메가를 받을때면 비효율적이기 때문에 레이어라는 개념을 사용합니다.

이는 여러개의 레이어를 하나의 파일시스템으로 사용하는 구조인데 이미지는 여러개 읽기전용으로 구성되고 파일이 추가 및 수정되면 새로운 레이어가 생성되어 기존 레이어를 제외한 변경되거나 추가된 부분만 다운받아 사용할 수 있다.



<br>

## Container

컨테이너는 이미지를 실행한 인스턴스같은 개념입니다. 컨테이너 자체는 이미지를 실행한 상태라고 볼 수 있고 추가되거나 변하는 값들은 컨테이너에 저장이 되는 구조이며, 같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너 상태가 바뀌거나 삭제되더라도 이미지는 변하지 않고 그대로 유지됩니다.

하나의 서버에 여러개 컨테이너 실행시 독립적으로 실행되어 가벼운 VM을 사용하는 느낌이며 실행중인 컨테이너에 접속하여 명령을 할 수 있고 사용자 추가, 여러개 프로세스를 백그라운드로 실행할 수도 있고 호스트의 특정포트와 연결하거나 호스트의 특정 디렉토리를 내부 디렉토리마냥 사용도 가능합니다.



<br>

## Docker Hub

도커는 생성한 이미지를 서버에 배포할때 도커레지스트리란 저장소를 사용합니다. git과 비슷하게 명령어를 사용하여 push, pull을 활용하여 공유할 수 있습니다. 도커 허브는 이러한 이미지를 저장하는 저장소로 이미지를 공유할 수 서비스를 뜻합니다.

아래순서는 터미널 환경에서 진행합니다.

1. docker에 로그인 -> `docker login`
2. 공유할 이미지 태그달기 -> `docker tag image명 repository:tag(버전 혹은 태그)`
3. publish 수행(이미지 게시) -> `docker push 사용자명/repository:tag(버전 혹은 태그)`
   1. dockHub에 업로드
4. 업로드된 이미지를 로컬에 구성하기 -> `docker run -p 4000:80 사용자명/repository:tag`



<br>

## Docker Compose

yaml 파일로 도커컨테이너가 프로덕션환경에서 어떤방식으로 동작할 수 있는지 정의하는 파일



<br>

## Docker Service

분산 응용프로그램에서는 각기 다른 부분을 다 서비스라고 부릅니다. 예를 들어 유투브내에는 데이터베이스에 비디오 데이터를 저장하는 서비스, 사용자가 업로드한 비디오를 인코딩하는 서비스, 프론트엔드영역에 대한 서비스등이 이를 뜻합니다.

서비스는 실제 환경에서 사용하는 컨테이너라고 생각하면 됩니다. 서비스는 오직 하나의 이미지만 실행하며 이를 쳬계화하기 위해선 이미지를 실행하는 방법등을 정의할 수 있는데 이는 도커 플랫폼내에선 `docker-compose.yml` 에 작성할 수 있다.



<br>

## Dockerfile

이미지를 만들때 Dockerfile이라는 파일에 자체 DSL언어를 사용하여 수행할 명령과 설정을 시간 순으로 작성하여 실행합니다. (자동화 sh 파일 생성하여 관리하는 것과 같은 원리)

컨테이너안에 있는 환경들을 어떻게 구성되어야 하는지 정의하는 기능을 하며 port, volume, directory등 한번 등록 후 빌드시 어떤 환경에서든 동일하게 동작하게 됩니다. (도커의 특징 기능)





### Dockerfile 명령어

#### FROM

parent image로 사용하고자 하는 것 정의(기본 베이스 이미지 정의)

```
FROM <image>:<tag>
FROM ubuntu:18.04
```

#### WORKDIR

작업(RUN,ADD,COPY)이 이뤄질 디렉토리 지정, 같은 디렉토리에 계속 작업할 환경에서 사용

```
WORKDIR /path
```

#### COPY

파일, 디렉토리를 이미지로 복사, 디렉토리를 지정해주지 않는다면 자동으로 생성

```
COPY ./usr/local/
```

#### ADD

COPY와 비슷한 개념이지만 현재 디렉토리의 내용을 해당 경로밑에 복사, 경로에 URL입력 가능하며 압축파일이 있다면 자동 압축해제

```
ADD ./usr/local/
```

#### RUN

필요로 하는 패키지를 정의하여 설치(실행 및 설치)

```
RUN ["app","param"]
```

#### EXPOSE

컨테이너 밖에 요청을 기다리는 호스트에서 사용할 포트 지정

```
EXPOSE <port>
```

#### ENV

컨테이너에서 사용할 환경변수 설정 Key:value

```
ENV <key> <value>
```

#### CMD

도커 컨테이너가 실행되었을때 실행하는 명령어 지정, CMD가 여러개일때 가장 마지막 커맨드라인의 명령만 실행. 여러개의 프로그램을 실행할 때는 쉘스크립트 파일을 작성하여 데몬으로 실행.

```
CMD ["executable","param"]
```

#### VOLUME

컨테이너 외부에 파일시스템을 마운트할 때 사용. 지정하지 않아도 마운트가 가능하지만 기본적으로 지정하는 것을 권장

```
VOLUME ["/data"]
```

<br>



## Docker Server

- 도커를 실행하는 데몬(로컬서버, 데스크탑, 가상화, 클라우드등)
- 이미지를 담을 볼륨이 필요하고 해당 이미지는 내부 or 외부의 repository로 사용
- 서버는 운영체제가 동작하고 운영체제엔 컨테이너 서비스를 제공
- docker daemon이 동작해야 가능



<br>

## Docker 명령어

### 도커 정보

```
docker info
```

### 도커 버전

```
docker --version
```

### 컨테이너 목록 확인하기

```
docker ps [options]
docker container ls
options = -a, --all
```

### 도커 실행

```
docker run [image-name]
ex) docker run -rm [이미지명] -> stop시 image 삭제
```

### 컨테이너 중지

```
docker stop [options] container [container..]
docker stop ${TENSORFLOW_CONTAINER_ID}
```

### 컨테이너 삭제

```
docker rm [OPTIONS] CONTAINER [CONTAINER...]
ex) docker container rm [컨테이너_ID] (container 생략 가능)
docker rm ${UBUNTU_CONTAINER_ID} ${TENSORFLOW_CONTAINER_ID}
```

### 도커 이미지 목록

```
docker images
이미지주소, 태그, id, 생성시점, 용량등
```

### 이미지 다운

```
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
docker pull ubuntu
```

<br>

### 그외 옵션

| 옵션          | 설명                                                         |
| :------------ | :----------------------------------------------------------- |
| -d, --detach  | detached mode 흔히 말하는 백그라운드 모드(터미널에 다른 명령어 수행가능) |
| -p, --publish | 호스트와 컨테이너의 포트를 연결 (포워딩)                     |
| -v            | 호스트와 컨테이너의 디렉토리를 연결 (마운트)                 |
| -e            | 컨테이너 내에서 사용할 환경변수 설정                         |
| –name         | 컨테이너 이름 설정                                           |
| –rm           | 프로세스 종료시 컨테이너 자동 제거                           |
| -it           | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션       |
| –link         | 컨테이너 연결 [컨테이너명:별칭]                              |
| /bin/bash     | bash 쉘 접속 가능                                            |

### Example 명령어

`$ docker run --rm nginx`

- nginx 컨테이너가 없을 시 자동 다운
- --rm : 이미지 stop시 컨테이너 제거 옵션

`$ docker run --rm --publish 9000:80 nginx`

- --publish 9000:80 : 해당 서버의 9000 포트와 nginx 80포트와 포워딩

![image](https://user-images.githubusercontent.com/48043799/118389814-b4225380-b666-11eb-8b9a-d247066b7ad9.png)

![image](https://user-images.githubusercontent.com/48043799/118389839-d9af5d00-b666-11eb-91a8-eadf66a53e1d.png)

> Log message 확인

`$ docker run --rm --detach --publish 9000:80 nginx`

- --detach : background mode 실행, docker container ls 로 실행 확인 가능

`$ docker run --rm -it python:3.8 [cmd]`

- -it : docker 이미지 실행시 cmd 명령어 

  