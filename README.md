## fabric.sh
블록체인 네트워크(오더러, 피어, 채널, 체인코드) 구동 및 초기화 스크립트
1. restart
2. all
3. clean
#### restart
블록체인 네트워크 정지 및 재구동 (network_restart, peer_restart)
``` shell
./fabric.sh restart
```
#### all
블록체인 네트워크 구동 (network_all, peer_all)
``` shell
./fabric.sh all
```
#### clean
블록체인 네트워크 정지 및 관련 파일 삭제 (network_clean, peer_all)
``` shell
./fabric.sh clean
```

---

## network
하이퍼 
블록체인 네트워크 구동
fabric.sh all -> network.sh all
 * 3 orderer
 * 1 peer
 * 1 couchdb
 * 1 cli
 * 1 CA

---

#### function network_all
기존 도커, 파일, 폴더 삭제) 및 블록체인 네트워크 구동
 * network_clean
 * network_up
 
---

#### function network_clean
docker 삭제 및 관련 폴더(파일) 삭제(configtx, docker)
 * network_down

#### function network_down
기존에 생성되어있는 관련 docker 정지 및 삭제

---

#### function network_up
블록체인 네트워크 config 파일 생성 및 블록체인 네트워크 구동
 * network_build
 * network_run
 
#### function network_build
패브릭 블록체인 구동을 위한 config 파일 생성
 * network_build_config
 * network_build_cryptogen
 * network_build_docker
 * network_build_tx
 * network_build_deploy

---

#### function network_build_config
제공되는 패브릭 블록체인 config template 파일을 복사하고 네트워크 환경에 맞춰서 파일 수정
 * network_copy_configtx
	* network_copy_cryptogen
	* network_copy_orderer

#### function network_copy_configtx
패브릭 블록체인의 네트워크 config 파일 복사 및 수정
"configtx.yaml"을 최종적으로 완성
 * d (orderer domain name, default : itt.co.kr)
 * ccs (consensus type, default : etcdraft)
 * oc (number of orderer, default : 3)
 * cp (channel profile, default : DevChannel)
 * mcp (main channel profile, default : Mainchain)

#### function network_copy_cryptogen
패브릭에서 제공하는 crytogen을 사용하여 MSP(Membership Service Provider) 필요한 config 파일 복사 및 수정
"cryptogen.yaml"을 최종적으로 완성
 * pcs (number of peer, default : 1)
 * d (orderer domain name, default : itt.co.kr)
 * oc (number of orderer, default : 3)
 
#### function network_copy_orderer
패브릭 네트워크의 오더러를 셋팅하기 위한 config 파일 복사 및 수정
"orderer.yaml"을 최종적으로 완성
 * orgs (organization name, default : bpa)
 * d (orderer domain name, default : itt.co.kr)
 * mc.block (genesis block, dafult : mc)
 * mcp (main channel profile, default : Mainchain)

---

#### function network_build_crpytogen
fabric-tools 이미지를 통해 cli docker를 띄우고 기존에 만들었던 cryptogen.yaml파일을 이용하여 MSP 생성
생성된 폴더에 755권한 부여

---

#### function network_build_docker

#### function
## peer
