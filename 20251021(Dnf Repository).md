## dnf

rpm 명령을 사용하면 패키지를 설치할 수 있다. 하지만 RPM 패키지를 설치할 때 가장 불편한 점은 패키지의 의조넝 문제다.
이 문제를 좀 더 손쉽게 해결할 수 있는 방법이 dnf인데 dnf 를 사용하려면 네트워크가 연결되어 있어야 한다.

#### dnf (Dandified yum)은 8버전 부터 가능하다 yum(~7)



* 패키지의 의존성을 자동으로 해결하여 설치,업그레이드,삭제 할수있다
* 설치 가능한 패키지에 대한 정보가 담긴 저장소를 가지고 있다 
* #### 저장소의 정보에 따라 패키지를 관리한다.

#### dnf upgrade (대상)
하지만 dnf upgrade만 입력하면 현재 설치된 패키지에서 업그레이드가 필요한 전체 패키지에 대해 업그레이드 진행하므로 주의해야한다.

#### dnf install *.rpm
폴더 안에있는 설치파일 (ex.rpm) 을 한번에 설치하는방법

## 심화

dnf - 네트워크 - 리포지터리

로컬 리포지터리
웹 리포지터리 스스로 만들수 있다.


### 로컬 리포지터리

/etc/yum.repos.d<br/>
리포지터리가 관리되는곳<br/>


<img width="907" height="371" alt="Image" src="https://github.com/user-attachments/assets/95917edf-d7bf-457a-b930-f02ed4ab939b" />


[ - ] <- 식별자<br/>
name - 리포지터리 이름<br/>
mirrorlist - 여러 리포지터리를 연결해 줄 주소 -> 지리적으로 가까운 곳으로 연결<br/>
baseurl - 수동 주소 지정 (file:// <- 내 파일 안에서 찾는다는 명령)<br/>
gpgcheck - GPG 검증 활성화 (따로 자신밖에 안쓸거면 할 이유가 없음)<br/>
enabled - 0 , 1 - 해당 리포지터리 활성화<br/>
countme - 통계 수집 활성화<br/>
metadata_expire 메타데이터 유효 캐시 시간 6시간<br/>
gpgkey - 해당 리포지터리에 검증을 시도할 키<br/>
<br/>
BaseOS OS의 기본 구성<br/>
AppStream OS 유틸리티<br/>
<br/>
.repo 파일은 /etc/yum.repos.d 디렉터리에 있어야 유효<br/>
<br/>
<br/>
mkdir /dvd<br/>
mount /dev/sr0 /dvd<br/>

vi custom.repo

<img width="294" height="116" alt="Image" src="https://github.com/user-attachments/assets/17799669-2d7c-4502-957c-ae77b4c3e33c" />

dnf install zsh (BaseOS 에 들어가있는 파일)

<img width="1278" height="345" alt="Image" src="https://github.com/user-attachments/assets/85ddf612-c143-4fe9-a57d-20e748704fe7" />

*꿀팁 esc상태에서 yy 누르고 붙여넣고싶은 곳에서 p 누르면 복사가 된다.


### 웹 리포지터리

A를 클라이언트<br/>
B를 웹 리포지터리<br/>

#### server B

dnf -y install httpd createrepo yum-utils<br/>
systemctl --now enable httpd<br/>
firewall-cmd --add-serivce=http<br/>
<br/>
mkdir /var/www/html/repo<br/>
reposync --repoid=baseos --newest-only --download-metadata -p /var/www/html/repo<br/>

#### server A

cd /etc/yum.repos.d<br/>
vi custom.repo<br/>
gpgcheck=0 <- 필수는 아닌데 좀 더 빠르게 해주기 위해서 사용
*꿀팁 esc상태에서 ggdG하면 전부 삭제<br/>

<img width="412" height="144" alt="Image" src="https://github.com/user-attachments/assets/93042761-eafa-49f6-ba6b-1d2a53fc0db2" />


#### server B

ls /var/www/html/repo/baseos/Packages/<br/>
systemctl restart httpd

<img width="461" height="310" alt="Image" src="https://github.com/user-attachments/assets/947e3eb1-773e-4d75-8324-9b82ae37ca79" />


#### server A

dnf install -y tmux(예시)





















































