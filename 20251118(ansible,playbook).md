



## ansible 

#### Server A,B

dnf install -y epel-release <br/>
dnf install -y ansible<br/>

Server A를 Server B의 노드로 등록

#### Server B

vi /etc/ansible/hosts<br/>
>맨 아래에<br/>
>[nodea]<br/>
>192.168.111.144<br/>
:wq<br/>

ssh-keygen<br/>
ssh-copy-id root@192.168.111.144<br/>
>yes<br/>
>[비밀번호]<br/>

ssh root@192.168.111.144 (테스트용)

ansible all -m ping

<img width="486" height="135" alt="image" src="https://github.com/user-attachments/assets/3b8d5d69-1e06-44d8-bb5a-73a42ddb7650" />

#### ansible 명령어

ansible all -m user -a "name=4gl_user state=present"<br/>
ansible all -m shell -a "tail -l /etc/passwd"<br/>
ansible all -m dnf -a "name=nginx state=latest"<br/>
ansible all -m service -a "name=nginx state=started"<br/>
ansible all -m shell -a "firewall-cmd --add-service=http<br/>
ansible all -m firewalld -a "service=http permanent=yes immediate=yes state=enabled"

#### 파일 보내기
echo "web test" > index.html<br/>
ansible all -m copy -a "src=index.html dest=/usr/share/nginx/html"<br/>

#### 역순으로 닫기

ansible all -m firewalld -a "service=http permanent=yes immediate=yes state=disenabled"<br/>
ansible all -m service -a "name=nginx state=stopped"<br/>
ansible all -m dnf -a "name=nginx state=absent"<br/>
ansible all -m user -a "name=4gl_user state=absent"<br/>


### 멱등성 실험

#### 텍스트 입력 모듈
lineinfile

ansible localhost (자기자신 호출)<br/>
touch mytest.txt<br/>
ansible localhost -m lineinfile -a "path=mytest.txt line=251118"<br/>


<img width="762" height="562" alt="image" src="https://github.com/user-attachments/assets/65d916e1-adc6-48c6-8da1-9bb7d9df7ce6" />



### Playbook

vi nginx.yaml

>---<br/>
>.- name: Install Nginx(. 지워야함)<br/>
>  hosts: nodea<br/>
>
>  tasks:<br/>
>    .- name: install nginx package(. 지워야함)<br/>
>      yum: name=nginx state=latest<br/>
>
>    .- name: create user (. 지워야 함)<br/>
>      user: name=webuser state=present<br/>
>    .- name: start nginx service<br/>
>      service:<br/>
>        name: nginx<br/>
>        state: started<br/>
>        enabled: yes
>
>    .- name: create index.html<br/>
>      lineinfile: path=index.html line=my ansible web<br/>
>
>    .- name: copy index.html to web root
>      copy: src=index.html dest=/usr/share/nginx/html
>
>    .- name: open http firewall
>      firewalld:
>        service: http
>        permanent: yes
>        immediate: yes
>        state: enabled
>

ansible-playbook nginx.yaml --syntax-check















































































































































































































































































































