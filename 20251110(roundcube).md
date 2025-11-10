


## DNS

### Server B

hostnamectl set-hostname mail.4gl.io <br/>
exec bash<br/>
<br/>
dnf install -y sendmail dovecot<br/>
dnf install -y bind-utils <br/>
*SMTP - sendmail | POP3, IMAP - dovecot<br/>
<br/>
vi /etc/resolv.conf <br/>
<br/>

#### DNS가 꼭 바뀌어야 하는 서버

메일 서버 1(Server A)<br/>
메일 서버 2(Server B)<br/>
클라이언트(Server D)<br/>

vi /etc/mail/sendmail.cf

:85 (바로 85행으로 이동)
Cwlocalhost -> Cw4gl.io로 변경

<img width="679" height="342" alt="image" src="https://github.com/user-attachments/assets/8b6f8b77-02f2-4e06-b591-7cb1640fd885" />

vi /etc/mail/local-host-names
mail.4gl.io입력

vi /etc/mail/snedmail.cf
:267 (바로 267행으로 이동)

Addr=127.0.0.1, 삭제

<img width="334" height="62" alt="image" src="https://github.com/user-attachments/assets/857c6cb0-dc18-47ae-b65a-3d085795e186" />

vi /etc/mail/access

<img width="384" height="98" alt="image" src="https://github.com/user-attachments/assets/b90d1f38-aadc-4267-a96c-c8a54a0ae0a9" />


vi /etc/dovecot/dovecot.conf

*24,30,33행 주석 제거

<img width="631" height="191" alt="image" src="https://github.com/user-attachments/assets/76c8f513-df4d-4538-97b5-02843f2745f2" />

vi /etc/dovecot/conf.d/10-ssl.conf

*ssl = yes
<img width="706" height="132" alt="image" src="https://github.com/user-attachments/assets/7df32cf9-9296-4301-aae1-11b36dff58d9" />

vi /etc/dovecot/conf.d/10-mail.conf

25행 주석 제거
121행 mail_access_group = mail

firewall-cmd --add-service={smtp,smtps,imap,imaps,pop3,pop3s}<br/>
firewall-cmd --runtime-to-permanent<br/>



## 웹메일 구축

기존의 A의 웹 서버 + 웹 메일 솔루션

###  Server A

firefox 

https://roundcube.net/download/

tar xfz roundcube<br/>
dnf install -y httpd php*<br/>
<br/>
mv roundcubemail-1.6.11 /var/www/html/<br/>
cd /var/www/html<br/>
ln -s roundcubemail-1.6.11/ rc<br/>
firewall-cmd --add-service=http<br/>
systemctl --now enable httpd<br/>
chwon -R apache.apache roundcubemail-1.6.11<br/>
systemctl --now enable mariadb<br/>

mysql<br/>
CREATE DATABASE rcdb;<br/>
GRANT ALL ON rcdb.* TO rcuser@localhost IDENTIFIED BY '1234';<br/>
EXIT











































































