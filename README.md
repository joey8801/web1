## fabric.sh
블록체인 네트워크(오더러, 피어, 채널, 체인코드) 구동 및 초기화 스크립트
1. restart
2. all
3. clean
### restart
블록체인 네트워크 정지 및 재구동 (network_restart, peer_restart)
``` shell
./fabric.sh restart
```
### all
블록체인 네트워크 구동 (network_all, peer_all)
``` shell
./fabric.sh all
```
### clean
블록체인 네트워크 정지 및 관련 파일 삭제 (network_clean, peer_all)
``` shell
./fabric.sh clean
```
---
## network
블록체인 네트워크 구동
fabric.sh all -> network.sh all
 * orderer 3개
 * ㅅ
## peer
