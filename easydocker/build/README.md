
## 1. Docker 명령어
| 기능                          | 코드                                                                 | 설명                                                         |
|--------------------------|--------------------------------------------------------------------|------------------------------------------------------------------|
| 이미지 pull                     | `docker pull {레파스트리 계정명}/{이미지명}`                          | Docker Hub 또는 다른 레지스트리에서 이미지를 다운로드합니다.              |
| 이미지 목록 확인                | `docker image ls`                                                  | 현재 로컬에 있는 Docker 이미지 목록을 확인합니다.                      |
| 이미지의 레이어 이력 조회       | `docker image history {이미지명}`                                   | 이미지의 생성 과정을 레이어별로 확인할 수 있습니다.                      |
| 이미지의 세부정보 출력          | `docker image inspect {이미지명}`                                   | 이미지의 세부 정보를 JSON 형식으로 출력합니다.                           |
| 이미지 빌드                     | `docker build -t {이미지명} {Dockerfile 경로}`                   | 현재 디렉토리의 Dockerfile을 사용해 이미지를 빌드합니다.                   |
| 컨테이너 실행                 | `docker run —name {컨테이너명} {이미지명}`              | 특정 이미지를 사용하여 컨테이너를 실행합니다.                    |
| 실행 중인 컨테이너를 이미지로 생성 (커밋) | `docker commit -m {커밋명} {실행 중인 컨테이너명} {생성할 이미지명}`   | 현재 실행 중인 컨테이너의 상태를 저장하여 새로운 이미지를 만듭니다.        |

ex) `docker commit -m "edited index.html by manulneko" -c 'CMD ["nginx", "-g", "daemon off;"]' officialNginx manulneko/commitnginx`  

### 이미지 빌드 옵션
| 코드                                                       | 설명                                                               |
|-----------------------------------------------------------|------------------------------------------------------------------|
| `docker build -f {도커파일명} -t {이미지명} {Dockerfile 경로}`  | 도커파일명이 Dockerfile이 아닐때 사용합니다.        |



### 컨테이너 실행 옵션

| 코드                                                       | 설명                                                               |
|-----------------------------------------------------------|------------------------------------------------------------------|
| `docker run -it —name {컨테이너명} {이미지명} {cmd 명령}`       | 컨테이너 실행과 동시에 cmd 명령을 실행합니다.        |
| `docker run -d --name {컨테이너명} {이미지명}`                 | 컨테이너를 백그라운드에서 실행합니다 (terminal 점유x) |
| `docker run -p 80:80 --name {컨테이너명} {이미지명}`           | 컨테이너의 포트 80을 호스트의 포트 80에 연결합니다. |
 

---

<br>

## 2. Dockerfile 명령어

### 01. simple build nginx
![image](https://github.com/user-attachments/assets/62ea4eff-9de9-43a8-a521-5e5cabeea9f9)  
Dockerfile을 사용하여 nginx 이미지의 index.html 파일을 나의 index.html로 수정합니다.
> `FROM {이미지명}` 베이스 이미지 지정
>  
> `COPY {파일경로} {복사할 경로}` 파일을 레이어에 복사
> 
> `CMD [”명령어”]` 컨테이너 실행 시 명령어 지정


<br>


### 02. use dockerignore
.dockerignore 파일을 사용하여 빌드 컨텍스트에 전달하지 않을 파일을 지정합니다.

<br>



### 03. build app & set env
![image](https://github.com/user-attachments/assets/32f5cfee-d658-4efb-82e0-13eed6eada38)
Node.js로 개발된 어플리케이션을 빌드합니다. 웹사이트의 글자 색상은 env 값에 따라 달라집니다. (default: green)

#### Dockerfile-basic
Node.js 애플리케이션을 빌드하는 기본 Dockerfile입니다.

#### Dockerfile-meta
Dockerfile-basic에서 시스템 지시어가 추가된 Dockerfile 입니다.
> `WORKDIR {폴더명}` 작업 디렉토리 지정 (새로운 레이어 추가)  
> `USER {유저명}` 명령을 실행할 사용자 변경 (새로운 레이어 추가)  
> - 도커 컨테이너가 실행될때는 기본적으로 명령들이 루트 사용자로 실행되기에 실행된 프로세스가 필요 이상의 권한을 가지게 되는 걸 방지합니다.
> 
> `EXPOSE {포트번호}` 컨테이너가 사용할 포트 명시 
> - 기본적으로 컨테이너는 모든 포트를 사용할수 있지만, 애플리케이션이 사용하는 포트를 문서화하기 위해 사용합니다.

#### Dockerfile-arg
arg을 통해 환경변수를 지정한 Dockerfile입니다. arg로 지정된 환경변수는 이미지 빌드시에만 사용되고 컨테이너 실행에는 사용되지 않기 때문에 컨테이너의 env에는 적용되지 않습니다.
> `ARG {변수명} {변수값}` 이미지 빌드 시점의 환경 변수 설정
> - `docker build —build-arg {변수명}={변수값}`으로 덮어쓰기 가능

#### Dockerfile-env
![image](https://github.com/user-attachments/assets/5984bd29-2461-4464-89e1-24775741a712)
env를 통해 환경변수를 지정한 Dockerfile입니다. env로 지정된 환경변수는 컨테이너 실행에도 유지되기 때문에 웹사이트 글자 색상이 변경된 것을 확인할 수 있습니다. 
> `ENV {변수명} {변수값}` 이미지 빌드 및 컨테이너 실행 시점의 환경 변수 설정 (새로운 레이어 추가)
> - `docker run -e {변수명}={변수값}`으로 덮어쓰기 가능

#### Dockerfile-entrypoint
npm start, npm install... 의 npm과 같이 고정적으로 사용되는 cmd 명령어가 있는 경우 ENTRYPOINT를 사용하여 고정할 수 있습니다. 
> `ENTRYPOINT [”명령어”]` 고정된 명령어를 지정  
> `CMD [”명령어”]` 컨테이너 실행 시 명령어 지정  

`docker run —name buildapp-entrypoint-list buildapp:entrypoint list`
- buildapp:entrypoint 이미지를 CMD를 list로 덮어씌우며 컨테이너로 실행
- ENTRYPOINT에 의해 컨테이너가 실행되며 npm list가 함께 실행됨
  
`docker run —name buildapp-entrypoint-list buildapp:entrypoint /bin/bash`
- buildapp:entrypoint 이미지를 CMD를 /bin/bash로 덮어씌우며 컨테이너로 실행
- ENTRYPOINT에 의해 실제로 실행되는 cmd 명령은 npm /bin/bash
- 쉘 실행 방지


<br>


### 04. multistage
maven, Springboot로 만들어진 백엔드 애플리케이션을 빌드하기 위한 Dockerfile입니다.

- 애플리케이션 빌드시에만 사용되고, 실행시에는 사용되지 않는 파일들은 불필요한 이미지 용량을 차지합니다. 큰 용량의 이미지는 애플리케이션 실행 속도를 늘립니다.
- 소스코드, 빌드에만 사용하는 외부 라이브러리, maven 도구 같은 파일들은 어플리케이션 실행에 불필요합니다.
- 따라서 marven을 사용하여 빌드하는 이미지와, 만들어진 JAR을 통해 실행하는 이미지를 분리하는 것이 좋습니다.

#### Dockerfile.singlestage
![image](https://github.com/user-attachments/assets/f8cb6dd9-2a60-490a-9825-9251396727c0)  
이미지 빌드와 애플리케이션 실행을 분리하지 않은 Dockerfile입니다. 

#### Dockerfile.multistage
![image](https://github.com/user-attachments/assets/3c381cc3-f142-42db-8e4f-55db44ccad46)  
이미지 빌드와 애플리케이션 실행을 각기 다른 컨테이너에서 진행하는 Dockerfile입니다. 최종적으로 만들어지는 이미지에는 OpenJDK 베이스와 애플리케이션 jar 파일만 남아있습니다.


![image](https://github.com/user-attachments/assets/111c9a40-4cbe-47dc-8882-db66581f0ef9)
위 두 Dockerfile을 통해 각각 빌드한 이미지가 size에서 많은 차이를 보임을 알 수 있습니다.
