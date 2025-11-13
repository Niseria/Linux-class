



## DHCP



<img width="614" height="528" alt="image" src="https://github.com/user-attachments/assets/49628dec-ec1e-4c59-870f-7bfe2c419ba4" />


#### Server A

dnf install -y dhcp-server
vi /etc/dhcp/dhcpd.conf

<img width="833" height="360" alt="image" src="https://github.com/user-attachments/assets/45900af9-634c-4fd0-8d95-e2ed72215043" />

vi /usr/share/doc/dhcp-server/dhcpd.conf.example

<img width="725" height="355" alt="image" src="https://github.com/user-attachments/assets/34077518-e7ca-4035-9c99-2ac4bf28a15f" />

vi /etc/dhcp/dhcpd.conf

subnet 192.168.111.0 netmask 255.255.255.224 {<br/>
  range 10.5.5.26 10.5.5.30;<br/>
  option domain-name-servers ns1.internal.example.org;<br/>
  option domain-name "internal.example.org";<br/>
  option routers 10.5.5.1;<br/>
  option broadcast-address 10.5.5.31;<br/>
  default-lease-time 600;<br/>
  max-lease-time 7200;<br/>
}<br/>
<br/>

이걸로 변경<br/>
subnet 192.168.111.0 netmask 255.255.255.0 {<br/>
  range 192.168.111.20 192.168.111.30;<br/>
  option domain-name-servers 8.8.8.8;<br/>
  option routers 192.168.111.2;<br/>
  option broadcast-address 192.168.111.255;<br/>
  default-lease-time 28800;<br/>
  max-lease-time 70000;<br/>
}

systecmtl restart dhcpd

#### Server B

nmcli con mod ens33 ipv4.addr "" ipv4.dns "" ipv4.gateway "" ipv4.method auto connection.autoconnect yes<br/>
nmcli con down ens33<br/>
nmcli con up ens33<br/>

<img width="944" height="414" alt="image" src="https://github.com/user-attachments/assets/3583839d-ff80-4917-aecf-0e913179329b" />


*DHCP - IP 배정 가능 , 리눅스 설치를 위한 시동 파일 전송 가능<br/>
*FTP , TCP,대용량의 설치 파일을 전송<br/>
*TFTP , UDP,간단한 파일 전송<br/>
*syslinux , 부트 로더<br/>

#### Server A

dnf -y install syslinux dhcp-server tftp-server vsftpd<br/>
firewall-cmd --add-service=tftp<br/>
firewall-cmd --add-service=ftp<br/>
<br/>

vi /etc/dhcp/dhcpd.conf

>allow booting;<br/>
>allow bootp;<br/>
>next-server 192.168.111.144;<br/>
>filename "pxelinux.0";<br/>

<img width="386" height="232" alt="image" src="https://github.com/user-attachments/assets/56036282-dc2d-4e30-a7a9-6a9e7c5b5661" />

systemctl restart dhcpd

*설치파일의 전달을 위해 FTP 사용 - 익명 계정으로 로그인 없이 - 설치 디스크는 FTP 경로에 마운트하여 복사 없이<br/>
*Rocky 9.6 디스크를 /var/ftp/pub에 마운트<br/>

df<br/>
mount  /dev/sr0 /var/ftp/pub<br/>
<br/>
vi /etc/vsftpd/vsftpd.conf<br/>

-vsftpd 익명 접속 허용<br/>

<img width="617" height="150" alt="image" src="https://github.com/user-attachments/assets/46552816-3c03-436e-8d18-d225d4f78f0a" />


systemctl restart vsftpd<br/>
<br/>
cd /var/libtftpboot<br/>

*TFTP 서버는 별도의 설정 없이 해당 루트 디렉터리에 필요한 파일만 복사<br/>

cp /usr/share/syslinux/pxelinux.0 .<br/>
cp /var/ftp/pub/images/pxeboot/* .<br/>
cp /var/ftp/pub/isolinux/ldlinux.c32 .<br/>
<br/>

mkdir pxelinux.cfg<br/>
vi default<br/>
>DEFAULT         Rocky9_PXE_Install<br/>
>LABEL           Rocky9_PXE_Install<br/>
>        kernel  vmlinuz<br/>
>        APPEND  initrd=initrd.img inst.repo=ftp://192.168.111.100/pub<br/>

*1차 완성 완료

systemctl restart dhcpd vsftpd ftp

#### 새로운 컴퓨터 생성

<img width="299" height="252" alt="image" src="https://github.com/user-attachments/assets/bd79612c-7723-4b5a-9629-b7fa9732f579" />

#### 서버 구축한 컴퓨터와 동일한 스펙으로 만들어주기

<img width="304" height="199" alt="image" src="https://github.com/user-attachments/assets/54a35f36-d8a9-46eb-a44d-3b909cdaeea6" />

<img width="846" height="572" alt="image" src="https://github.com/user-attachments/assets/777816a7-356e-4077-86a6-258470b6b55b" />


#### Server A

ks(킥스타트)기능 -> 설치 정보를 모두 전달
cd

vi ana(tab)

<img width="1172" height="780" alt="image" src="https://github.com/user-attachments/assets/7458c387-7fd3-410c-8083-7a853b65be75" />

<img width="1172" height="788" alt="image" src="https://github.com/user-attachments/assets/6cdfc337-392e-43d1-aee4-952164c758cb" />


*repo , cdrom 수정
ks 파일 작성 -> FTP 서버에 저장 -> 부팅 파일 등을 가져가는 pxelinux.ofg/default 파일에 경로만 명시

vi /var/lib/tftpboot/pxelinux.cfg/default

<img width="920" height="85" alt="image" src="https://github.com/user-attachments/assets/538dfb2a-986b-49c3-921d-3e94e6268cac" />

#### Rocky 9 = inst.repo,inst.ks , Rocky 8 = repo.ks

cp anaconda-ks.cfg /var/ftp/rocky9.ks

chmod 644 /var/ftp/rocky9.ks

*다른 컴퓨터 설치로 돌아가서 테스트

#### 쉘 스크립트
*리눅스 쉘 문법으로 작업, 명령 등을 자동적 혹은 조건에 맞추어 실행

if 문 <br/>
if [조건]<br/>
then<br/>
  명령문<br/>
fi<br/>

vi check.sh

>#!/bin/bash<br/>
>a=qwer<br/>
>b=asdf<br/>
><br/>
>if [ $a != $b ]<br/>
>then<br/>
>  echo "error"<br/>
>fi<br/>

chmod 655 check.sh



































































