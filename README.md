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
 
#### function network_clean
docker 삭제 및 관련 폴더(파일) 삭제(configtx, docker)
 * network_down

#### function network_down
기존에 생성되어있는 관련 docker 정지 및 삭제
---
#### function network_up
블록체인 네트워크 config 파일 생성 및 구동


## peer
