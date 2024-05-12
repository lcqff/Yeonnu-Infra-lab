#가져올 이미지 정의(from dockerhub)
#FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]

#베이스 이미지 지정
#FROM ubuntu:latest
FROM node:18
#FROM python:3

#WORKDIR /path/to/workdir
#작업 디렉토리 변경(경로 설정)
WORKDIR /app

#COPY [--chown=<user>:<group>] <src>... <dest>
#파일 또는 디렉토리 추가
#COPY index.html /var/www/html/
#COPY ./app /usr/src/
COPY package*.json ./
# 호스트 운영체제의 현재 디렉토리 상에 있는 ~파일들을 복사해서 이미지의 현재 디렉토리에 복사해줌

#RUN <command>
#명령어 실행
#RUN apt-get update
RUN npm install
# 현재 디렉토리에 있는 package.json 파일들 읽어서 설치

RUN npm install -g typescript
# typescript를 이용한 node이므로 전역옵션을 이용하여 typescript 모듈을 설치해줍니다.  
COPY ./ ./
# 현재 폴더에 있는 모든 파일들을 복사하여 이미지 디렉토리에 복사합니다. 
# 여기과정이 자신이 작성한 코드등 리소스들이 복사됩니다.  
RUN tsc
# typescript로 작성된 node이므로 build를 하여 javascript 파일로 컴파일 해줍니다.  

EXPOSE 3000
#컨테이너에서 사용할 포트 정보

CMD ["npm", "run", "start"] 





#CMD ["executable","param1","param2"]
#CMD command param1 param2
#컨테이너 생성시 실행할 명령어
#CMD ["node", "app.js"]
#CMD node src/app.tsx

#docker build -t nodejs-server .