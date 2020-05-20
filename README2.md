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
### 1.2 all
블록체인 네트워크 구동 (network_all, peer_all)
``` shell
./fabric.sh all
```
### 1.3 clean
블록체인 네트워크 정지 및 관련 파일 삭제 (network_clean, peer_all)
``` shell
./fabric.sh clean
```

---

## 2. network.sh
블록체인 네트워크 구동
fabric.sh all -> network.sh all
 * 3 orderer
 * 1 CA
 * 1 cli (실해후 삭제)

## network.sh all -> flow
1. network clean (& down)
2. 블록체인 네트워크를 만들기 위해 필요한 configure 파일들 작성
   => confitx.yaml, cryptogen.yaml, orderer.yaml
3. 네트워크에 필요한 key material 생성 (cryptogen.yaml로 정의된 대로 만들어짐)
   => key material (relate MSP)
4. 도커를 빌드하기 위해 필요한 파일 작성
   => docker-compose.yaml
5. genesis.block 및 .tx 채널 생성
6. genesis.block / .tx / key 등을 경로에 맞춰서 복사
7. 도커 빌드

## network.sh 변수 확인
 * net=itt : docker network											
 * vera=1.4.4 : fabric image version										
 * verb=0.4.15 : third party version
 * ccs=etcdraft : consensus type raft
 * rootdir="$( cd "$(dirname "$0")" ; pwd -P )" : root folder
 * ddir=$rootdir/deploy

docker 빌드시 host에서 연결되어있는 폴더 (공유폴더)
 * tdir=$rootdir/template
 * ttdir=$tdir/configtx
 * tgdir=$tdir/cryptogen
 * todir=$tdir/orderer
 * tddir=$tdir/docker

docker 빌드시 docker에 적용되는 폴더 (노 변경)
 * bdir=$rootdir/build
 * btdir=$bdir/configtx
 * bgdir=$bdir/cryptogen
 * bodir=$bdir/orderer
 * bddir=$bdir/docker

 * mc=mc # main channel (For Admin, Do not use this channel)
 * mcp=Mainchain # main channel profile
 * c=dc # sub channel
 * cp=DevChannel # sub channel profile
 * d=itt.co.kr # domain name
 * ip=orderer0.itt.co.kr
 * orgs=( bpa ) # organization names
 * pcs=( 1 ) # number of peers, each at org
 * oc=3   # number of orderer

---

### 2.1 function network_all
기존 도커, 파일, 폴더 삭제 및 블록체인 네트워크 구동
 * network_clean
 * network_up
 
---

#### 2.1.1 function network_clean
docker 삭제 및 관련 폴더(파일) 삭제(configtx, docker)
 * network_down

#### 2.1.1.1 function network_down
기존에 생성되어있는 관련 docker 정지 및 삭제

---

#### 2.1.2 function network_up
블록체인 네트워크 config 파일 생성 및 블록체인 네트워크 구동
 * network_build
 * network_run

---

##### 2.1.2.1 function network_build
패브릭 블록체인 구동을 위한 config 파일 생성
 * network_build_config
 * network_build_cryptogen
 * network_build_docker
 * network_build_tx
 * network_build_deploy

---

#### 2.1.2.1.1 function network_build_config
제공되는 패브릭 블록체인 config template 파일을 복사하고 네트워크 환경에 맞춰서 파일 수정
 * network_copy_configtx
 * network_copy_cryptogen
 * network_copy_orderer

#### 2.1.2.1.1.1 function network_copy_configtx
패브릭 블록체인의 네트워크 config 파일 복사 및 수정
"configtx.yaml"을 최종적으로 완성
 * d (orderer domain name, default : itt.co.kr)
 * ccs (consensus type, default : etcdraft)
 * oc (number of orderer, default : 3)
 * cp (channel profile, default : DevChannel)
 * mcp (main channel profile, default : Mainchain)

#### 2.1.2.1.1.2 function network_copy_cryptogen
패브릭에서 제공하는 crytogen을 사용하여 MSP(Membership Service Provider) 필요한 config 파일 복사 및 수정
"cryptogen.yaml"을 최종적으로 완성
 * pcs (number of peer, default : 1)
 * d (orderer domain name, default : itt.co.kr)
 * oc (number of orderer, default : 3)
 
#### 2.1.2.1.1.3 function network_copy_orderer
패브릭 네트워크의 오더러를 셋팅하기 위한 config 파일 복사 및 수정
"orderer.yaml"을 최종적으로 완성
 * orgs (organization name, default : bpa)
 * d (orderer domain name, default : itt.co.kr)
 * mc.block (genesis block, dafult : mc)
 * mcp (main channel profile, default : Mainchain)

---

#### 2.1.2.1.2 function network_build_crpytogen
fabric-tools 이미지를 통해 cli docker를 띄우고 기존에 만들었던 cryptogen.yaml파일을 이용하여 MSP 생성
생성된 폴더에 755권한 부여

---

#### 2.1.2.1.3 function network_build_docker
패브릭 블록체인에 필요한 물리적 요소들의 docker를 생성하기 위한 docker-compose.yaml 파일 생성

---

#### 2.1.2.1.4 function network_build_tx
패브릭 블록체인의 genesis block을 생성함 (mc.block)
패브릭 블록체인의 channel 생성함 (c.tx)

---

#### 2.1.2.1.5 function network_build_deploy
패브릭 블록체인의 네트워크의 폴더 구성에 맞게 폴더를 생성하고 이전에 만들었던 제네시스블록, 채널파일, MSP 파일 복사

---

#### 2.1.2.2 function network_run
미리 만들어져있던 docker-compose.yaml 파일을 도커 빌드함

---

### 2.1.3 function network_restart
 * network_down
 * network_run

---

## 3. peer.sh
블록체인 네트워크에 피어를 생성/채널 조인/체인코드 설치/채널에 체인코드 initiate/
fabric.sh all -> network.sh all
 * 1 peer
 * 1 peer.cli
 * 1 couchdb
 
### 3.1 function peer_all
기존 도커, 폴더, 파일 삭제 및 피어 생성/채널 조인/체인코드 설치/체인코드 initiate
(peer / peer_channel / peer_chaincode)
 * peer_clean
 * peer_up
 * peer_channel_create
 * peer_channel_join
 * peer_chaincode_install
 * peer_chaincode_instantiate

---
### peer
 * all
 * build
 * rebuild
 * run
 * up
 * down 
 * restart
 * channel
  - create
  - join
 * chaincode
  - install
  - initiate
  - upgrade
  - invoke
  - query
 * clean
---

#### 3.1.1 function peer_clean
기존 도커 정지/관련 파일 삭제 및 기존 폴더, 파일 삭제
 * peer_down
 
#### 3.1.1.1 function peer_down
기존 도커 삭제 및 관련 파일 삭제

---

#### 3.1.2 function peer_up
피어를 생성하기 위한 config 파일을 생성하고 피어 도커를 구동
 * config_set
 * peer_build
 * peer_run
 
#### 3.1.2.1 function config_set
피어 도커 config파일에 필요한 변수들의 set

#### 3.1.2.2 function peer_build
피어 도커 config 
 * peer_build_config
 * peer_build_docker

---

#### 3.1.2.2.1 function peer_build_config
생성되었던 피어 MSP 복사, 참여하고자 하는 채널 파일 복사, 코어 config 파일 복사 및 수정
 * peer_copy_crypto_config
 * peer_copy_channel_config
 * peer_copy_core_config

#### 3.1.2.2.1.1 function peer_copy_crypto_config
기존 network.sh로 만들어진 피어 MSP 복사

#### 3.1.2.2.1.2 function peer_copy_channel_config
기존 network.sh로 만들어진 채널 .tx 파일 복사

#### 3.1.2.2.1.3 function peer_copy_core_config
피어 core config template 복사 및 수정

---

#### 3.1.2.2.2 function peer_build_docker
피어 도커 생성을 위한 docker-compose.yaml 파일 작성

---

#### 3.1.2.3 function peer_run
작성된 docker-compose.yaml 파일을 기반으로 피어 도커 실행

---

#### 3.1.3 function peer_channel_create
채널생성
c.block 복사

#### 3.1.4 function peer_channel_join
채널조인

---

#### 3.1.5 function peer_chaincode_install
피어에 example02.go 체인 코드 설치

#### 3.1.6 function peer_chanincode_instantiate
채널에 체인코드 initantiate (a:100, b:200)

#### 3.1.7 function peer_chaincode_upgrade
#### 3.1.8 function peer_chaincode_invoke
#### 3.1.9 function peer_chaincode_query
