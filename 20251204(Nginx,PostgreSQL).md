

## Nginx, django와 연동

docker ps <br/>
(myweb01을 찾기)<br/>
docker start (myweb01의 번호 입력)<br/>


127.0.0.1:8000 <- 윈도우에서 접근 가능한 웹 서버 설치된거 확인

docker network inspect bridge | grep IPv4Address 로 실행중인 컨테이너의 ip주소 확인 가능<br/>

*도커 엔진에 등록된 컨테이너에서 자동으로 방화벽 규칙을 -p 옵션을 통해 입력시 해당 서버의 iptables로 등록<br/>
8000번 포트를 안열었는데 된 이유<br/>


#### Nginx(웹서버) 컨테이너를 실행하고 gunicorn 을 통해 Nginx와 django를 연동하기.

mkdir work<br/>
mkdir work/ch05<br/>
cd work/ch05<br/>
mkdir ex04<br/>
cd ex04<br/>

vi Dockerfile<br/>

>FROM nginx:1.25.3<br/>
>CMD ["ngnix", "-g", "daemon off;"]<br/>
>:wq<br/>

docker build -t mynginix01 .<br/>

cat Dockerfile<br/>
*잘 되면 파란색으로 표시<br/>

docker run -d -p 80:80 mynginx01<br/>

docker container exec -it (아이디) /bin/bash

cat /etc/ngnix/.conf.d/default.conf<br/>
exit<br/>

cd ~/work/ch05<br/>
mkdir ex05<br/>
cd ex05<br/>

cp -r ex03 ex04 ex05<br/>

cd ex05<br/>
mv ex03/ myDjango02<br/>
mv ex04/ myNginx02<br/>

cd myDjango02
vi requirements.txt

>gjango==5.2.8<br/>
>gunicorn==20.1.0<br/>
>:wq<br/>

vi Dockerfile<br/>

>FROM python:3.11.6<br/>
>workdir /usr/src/app<br/>
><br/>
>COPY . .<br/>
><br/>
>RUN python -m pip install --upgrade pip<br/>
>RUN pip install -r requirements.txt<br/>
><br/>
>WORKDIR ./myapp<br/>
CMD gunicorn --bind 0.0.0.0:8000 myapp.wsgi:application<br/>
><br/>
>EXPOSE 8000<br/>
>:wq<br/>

docker image build . -t myweb02<br/>
docker image ls<br/>

cd myNginx02<br/>
vi deault.conf<br/>

>upstream myweb{<br/>
>  server djangotest:8000;<br/>
>}<br/>
><br/>
>server {<br/>
>  listen 80;<br/>
>  server_name localhost;<br/>
>  location /{<br/>
>    proxy_pass http://myweb;<br/>
>  }<br/>
>}<br/>
>:wq<br/>
<br/>
vi Dockerfile<br/>
<br/>
>FROM nginx:1.25.3<br/>
>RUN rm /etc/nginx/conf.d/default.conf<br/>
>COPY default.conf /etc/nginx/conf.d/<br/>
>CMD ["nginx", "-g", "daemon off;"]<br/>
>:wq<br/>

Dockerfile 수정 → 빌드 → 컨테이너 생성<br/>
Docker 컨테이너는 실행 후 수정이 불가능<br/>

docker image build . -t mynginx02<br/>
docker image ls<br/>


docker network ls<br/>
docker network create mynetwork02<br/>
docker network ls<br/>

docker image ls<br/>
docker container run -d --name djangotest --network mynetwork02<br/>
docker container ls<br/>
docker container run -d --name nginxtest --network mynetwork02 -p 80:80 mynginx02<br/>
docker cantainer ls<br/>

### Nginx, django, PostgreSQL 컨테이너 연동

cd work/ch05/<br/>
mkdir ex06<br/>
cd ex06<br/>

vi Dockerfile
>FROM postres:15.4<br/>

docker image build . -t mypostgres03<br/>
docker image ls<br/>

docker volume create myvolume03<br/>
docker volume ls<br/>
docker container run -e POSTGRES_PASSWORD=mysecretpassworld --mount type=volume,source=myvolume03,target=/var/lib/postgresql/data -d mypostgres03<br/>

docker ps<br/>

*docker rm -f (일련번호) stop 안하고 강제 삭제<br/>

cd work/ch05<br/>
ls<br/>
cp -r ex05 ex07<br/>
ls<br/>
cp -r ex06 ex07<br/>
cd ex07<br/>
ls<br/>
mv myDjango02 myDjango03<br/>
mv myNginx02 myNginx03<br/>
mv ex06 myPostgres03<br/>
ls<br/>

























































































































































































































































































