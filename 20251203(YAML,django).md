

## YAML

YAML은 "YAML Ain't Markup Language"의 줄임말입니다.<br/>
도커 컴포즈를 사용할 때 YAML 파일을 활용하며, 쿠버네티스에서도 YAML 파일을 사용합니다.<br/>
YAML 파일의 확장자는 .yaml과 .yml을 사용합니다<br/>


### pyyaml 설치

pyenv activate py3_11_6<br/>
pip install pyyaml<br/>

vi yaml_practice.yml

>apiVersion: v1<br/>
>kind: Pod<br/>
>metadata:<br/>
>  name: nginx<br/>
>spec:<br/>
>  containers:<br/>
>  - name: nginx<br/>
>    image: nginx:latest<br/>
>  - name: ubuntu<br/>
>    image: ubuntu:latest<br/>


python<br/>
import yaml<br/>
raw = open("/home/ubuntu/work/ch05/ex01/yaml_practice.yml", "r+")<br/>
data = yaml.load(raw, Loader=yaml.SafeLoader)<br/>
data<br/>
<br/>
data['apiVersion']<br/>

### 도커를 활용한 django 실행

django란 파이썬을 통해 웹사이트를 손쉽게 만들 수 있는 웹 프레임워크입니다.<br/>


ls<br/>
mkdir work<br/>
cd work<br/>
mkdir ch05<br/>
cd ch05<br/>
mkdir ex02<br/>
cd ex02<br/>

pyenv activate py3_11_6<br/>
pwd<br/>

django-admin startproject myapp (<- myapp 디렉터리 생성)<br/>
ls<br/>
tree ./<br/>
python manage.py runserver 0.0.0.0:8000<br/>

웹 브라우저를 실행하고 주소창에 127.0.0.1:8000을 입력하면 접속이 잘 되는것을 확인할 수 있습니다.<br/>

#### django 이미지 빌드

디렉터리 정리 -> requirements.txt 파일 작성 -> Dockerfile 파일 작성(중요) -> 이미지 빌드<br/>

cd work/cd05/<br/>
ls<br/>
(ex01 ex02 없으면 생성)<br/>
cp -r ex02 ex03<br/>
ls<br/>
(ex01 ex02 ex03)<br/>
cd ex03<br/>
tree ./ -L 3<br/>

cd ex03
vi requirments.txt

>django==5.2.8<br/>
>:wq<br/>

vi Dockerfile
>FROM python:3.11.6<br/>
>
>WORKDIR /usr/src/app<br/>
>
>COPY . .
>
>RUN pyrhon -m pip install --upgrade pip<br/>
>RUN pip install -r requirements.txt<br/>
>
>WORKDIR ./myapp<br/>
>CMD python manage.py runserver 0.0.0.0:8000<br/>
>EXPOSE 8000<br/>
>:wq<br/>


docker container run -d -p 8000:8000 myweb01


자바 프로젝트를 생성 후 간단한 결과물을 출력

sudo apt install maven

mvn archetype:generate -DgroupId=com.4gl.app -DartifactId=4glapp -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false


cat javatemp/4glapp/src/main/webapp/index.jsp

pom.xml <- maven에서 사용하는 웹 어플리케이션 빌드에 필요한 의존성, 기타 도구를 설치할 명세서<br/>

빌드 명령어<br/>
mvn clean package<br/>

빌드 결과물은 target 디렉터리에 보관<br/>

오류 발생 시 pom.xml 수정

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"<br/>
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd"><br/>
  <modelVersion>4.0.0</modelVersion><br/>
  <groupId>com.4gl.app</groupId><br/>
  <artifactId>4glapp</artifactId><br/>
  <packaging>war</packaging><br/>
  <version>1.0-SNAPSHOT</version><br/>
  <name>4glapp Maven Webapp</name><br/>
  <url>http://maven.apache.org</url><br/>
  <dependencies><br/>
    <dependency><br/>
      <groupId>junit</groupId><br/>
      <artifactId>junit</artifactId><br/>
      <version>3.8.1</version><br/>
      <scope>test</scope><br/>
    </dependency><br/>
  </dependencies><br/>
  <build><br/>
    <finalName>4glapp</finalName><br/>
    <plugins><br/>
        <plugin><br/>
            <groupId>org.apache.maven.plugins</groupId><br/>
            <artifactId>maven-war-plugin</artifactId><br/>
            <version>3.4.0</version><br/>
        </plugin><br/>
    </plugins><br/>
  </build><br/>
</project><br/>
<br/>
mvn clean package<br/>
<br/>
docker run -d -p 8080:8080 tomcat:latest<br/>

FROM tomcat:latest
RUN cp -r /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/weapps
COPY 4glapp.war /usr/lcoal/tomcat/webapps
RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt
WORKDIR ./myapp
CMD python manage.py runserver 0.0.0.0:8000
EXPOSE 8000




















































































































































































































