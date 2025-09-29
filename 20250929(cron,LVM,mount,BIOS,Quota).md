## Cron / 시스템 자동화

<img width="549" height="266" alt="Image" src="https://github.com/user-attachments/assets/59d1d40a-d7f4-48b0-bf34-1b58d4a7fff3" />

mkdir /root/bin<br/>
<br/>
cd bin<br/>
touch etc_backup.sh<br/>
mkdir /backup<br/>
<br/>

#### vi etc_backup.sh

<br/>
#! /bin/bash<br/>
set $(date)<br/>
name ="etc-backup-$1$2$3.tar.bz2<br/>
tar cfj /backup/$name /etc<br/>

### 깨알 외우기

gz - z<br/>
bz2 j<br/>
xz J<br/>

chmod 755 etc_backup.sh<br/>
ls /etc/cron.weekly<br/>
mv etc_backup.sh /etc/cron.weekly/<br/>

#### vi /etc/crontab

30 04 * * 3 root run-parts /etc/cron.weekly<br/>

<img width="632" height="526" alt="Image" src="https://github.com/user-attachments/assets/10725bae-12c9-4a16-b34c-e7b9ffb8ef49" />

lsblk로 확인<br/>
mdadm --create /dev/md1  --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1<br/>
mdadm --create /dev/md1  --level=1 --raid-devices=2 /dev/sdd1 /dev/sde1<br/>
mdadm --create /dev/md1  --level=1 --raid-devices=2 /dev/sdf1 /dev/sdg1<br/>
<br/>
mdadm --detail --scan 확인<br/>
<br/>
mdadm --create /dev/md15 --level=5 --raid/devices=3 /dev/md1 /dev/md2 /dev/md3<br/>

### LVM

pvcreate /dev/md15<br/>
vgcreate vg1 /dev/md15<br/>

mkdir /srv/web

lvcreate -L(용량 /--size) 1G -n lv_web vg1<br/>
lvcreate -L 2G -n lv_db vg1<br/>
lvcreate -l 100%FREE(남은용량 다 쓰기) -n lv_logs vgl<br/>

### mount

mkfs.ext4 /dev/vg1/lv_logs(포멧)<br/>
moute /dev/vgl1/lv_logs /srv/logs/ (mount)<br/>

#### 자동 마운트

vi /etc/fstab

/dev/vg1/lv_logs        /srv/logs       ext4    defaults 0 0



## 부팅

pc의 전원 스위치를 켜면 제일 먼저 BIOS(Basic Input/Output System)가 작동한다<br/>
BIOS는 보통 ROM에 저장되어 있다.<br/>
BIOS는 PC에 장착된 기본적인 하드웨어의 상태를 확인한 후 부팅장치를 선택하여 부팅 디스크의 첫 섹터에서 512B를 로딩한다<br/>

#### 이 512B를 마스터 부트 레코드 라고 한다

리눅스에서 사용하는 부트로더는 GRUB라고 한다<br/>


## 쿼터

쿼터 설정은 파일 용량으로 제한하는것과 파일 수로 제한하는 것이 있다<br/>

#### 쿼터값을 설정할 때 하드 리미트와 소프트 리미트 두 가지 값이 있다


*defaults 부분이 마운트 옵션이다<br/>
/dev/vg1/lv_logs        /srv/logs       ext4    defaults 0 0<br/>

mkfs -t ext4 /dev/sdb1(포멧 ext4로)<br/>
mount /dev/sdb1 /quota/(마운트 해주기)<br/>
vi /etc/fstab (자동 마운트)<br/>
/dev/sdb1  /quota  ext4  defaults  0  0<br/>

#### 꼭 마운트하고 User 만들기

useradd -d /quota/quser1 quser1 (홈 디렉터리 사용자명,홈 디렉터리를 지정해줄수 있다)<br/>
useradd -d /quota/quser2 quser2<br/>
<br/>
vi /etc/fstab<br/>
/dev/sdb1  /quota  ext4  defaults,usrjquota=aquota.user,jqfmt=vfsv0  0  0으로 수정<br/>
<br/>
systemctl daemon-reload<br/>
mount -o remount /home2<br/>
mount | grep /quota<br/>

#### quotacheck - 파일 시스템을 확인한 후 데이터 베이스 파일이 있으면 디스크 사용량을 수정하고 데이터베이스 파일이 없으면 생성한다.
cd /quota 가서 확인<br/>

quotacheck -avugm

#### quota 켜기 끄기

quotaon -avug / quotaoff -avug

#### edquota로 제한을 걸수있다

edquota -u quser1

<img width="854" height="173" alt="Image" src="https://github.com/user-attachments/assets/2683b1a4-193f-429a-8e0b-ed9ebb5b18e0" />

blocks, soft , hard 는 각각 KB단위<br/>
*20M 30M으로 입력도 가능<br/>
리미트 값이 0이면 사용량에 제한이 없음을 뜻한다<br/>

이제 su - quser1로 확인
<img width="652" height="76" alt="Image" src="https://github.com/user-attachments/assets/f9d411a9-676c-4562-8eb6-ed0f38d0a274" />

cp 등 옮길수 있음






















