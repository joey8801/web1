##  멀티호스트 TLS CA Certificate 발급
virtualbox - ubuntu 18.04 x 3
NAT network
3 CA : tls-ca / org0-ca / org1-ca

### 초기설정
1. virtual 3 ubuntu 빌드 (램 4GB, 용량 80G)
2. NAT network 설정 변경
  * 파일 -> 환경설정 -> 네트워크 -> 추가 -> 편집 -> 네트워크이름 enitt / 네트워크 CIDR (192.168.100.0/24) / DHCP 지원 체크
  * 개별설정 -> 네트워크 -> 어댑터1탭 -> 다음에연결됨 : NAT 네트워크로 변경 -> enitt 선택 -> 확인
3. 초기설정
  * enitt 사전설치
    + build-essential, curl, git, vim
``` shell
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential
sudo apt-get install curl
sudo apt-get install git
sudo apt-get install vim
```

    + golang
``` shell
wget http://storage.googleapis.com/golang/go1.13.6.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.13.6.linux-amd64.tar.gz
cd $HOME
echo export PATH=$PATH:/usr/local/go/bin >> .profile
mkdir go && mkdir go/src && mkdir go/pkg && mkdir go/bin
echo export GOPATH=$HOME/go >> .profile
source .profile
go version && echo $GOPATH
```
