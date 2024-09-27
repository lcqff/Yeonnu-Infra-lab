#베이스 이미지 지정
FROM nginx:1.23

# index.html을 nginx 이미지의 index.html에 덯어쓰기한다.
COPY index.html /usr/share/nginx/html/index.html

# Nginx 컨테이너 실행시 Nginx 웹서버를 실행하도록 한다.
CMD ["nginx", "-g", "daemon off;"]

# 도커파일 빌드(Terminal)
# [option] t: 빌드할 이미지 이름 지정
#  .:도커 파일이 위치한 경로
# docker build -t {레지스트리 계정명}/buildnginx .

# 컨테이너 실행 (terminal)
# docker run -d -p 80:80 --name build-nginx manulneko/buildnginx
