## Docker



앞서 도커 스토리지에는 세 가지 종류가 있다. 그중에서 volume 방식을 활용해 PostgreSQL데이터를 관리하는 실습을 하겠습니다.<br/>
volume은 도커 컨테이너에서 생성되는 데이터가 컨테이너를 삭제한 후에도 유지될 수 있도록 도와주는 저장소입니다.<br/>
volume은 도커에 의해 관리됩니다.<br/>

docker volume ls<br/>
docker volume create myvolume01<br/>
docker volume ls<br/>

docker run -e POSTGRES_PASSWORD=mysecretpassword -v myvolume01:/var/lib/postgresql/data -d postgres<br/>
docker volume ls<br/>

docker volume create myvolume03<br/>
docker run -e POSTGRES_PASSWORD=mysecretpassword -v myvolume03:/var/lib/postgresql -d postgres<br/>
<br/>
docker ps<br/>
docker container exec -it (이름) /bin/bash<br/>
psql -U postgres<br/>
CREATE USER user01 PASSWORD '1234' SUPERUSER;<br/>

\du

<img width="582" height="101" alt="image" src="https://github.com/user-attachments/assets/8574c3cc-7dfa-4250-996c-a0ffa5064812" />

\q<br/>

docker volume inspect myvolume03<br/>

<img width="651" height="295" alt="image" src="https://github.com/user-attachments/assets/aeeb5777-5275-4250-a1a2-8f86bdadb8b3" />

컨테이너를 완전히 삭제한 후 새로운 컨테이너를 생성했을 때 데이터가 유지되는지 확인.

docker stop abc(이름)<br/>
docker rm abc(이름)<br/>
docker ps<br/>

<img width="663" height="150" alt="image" src="https://github.com/user-attachments/assets/a90e9bde-9fe7-4c5e-aece-59818f7ea287" />

docker run -e POSTGRES_PASSWORD=mysecretpassword -v myvolume03:/var/lib/postgresql -d postgres<br/>
docker container exec -it (이름) /bin/bash<br/>
psql -U postgres<br/>
\du

<img width="608" height="156" alt="image" src="https://github.com/user-attachments/assets/7849187d-cf9f-46a9-a8d2-dd792c40a465" />

(삭제했지만 user01이 남아있는걸 확인)




## Vagrant

https://developer.hashicorp.com/vagrant/install

C드라이브에 Vagrant 파일 생성

<img width="756" height="587" alt="image" src="https://github.com/user-attachments/assets/96bf8c11-072f-46e9-af8e-c8132d31f3cc" />

#### CMD

vagrant init

vagrant up

https://portal.cloud.hashicorp.com/vagrant/discover/almalinux/8

<img width="668" height="426" alt="image" src="https://github.com/user-attachments/assets/7ae1023d-62a3-49f5-80c8-ec19e026203b" />

vagrant up
















































































