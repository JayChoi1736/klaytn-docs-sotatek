# 노드 로그

이 페이지는 클레이튼 노드의 중요하거나 자주 묻는 로그에 대해 자세히 설명합니다.
클레이튼 로그를 수정하거나 새로 추가/삭제한 경우, 이 페이지도 수정해 주세요.

로그 유형에 대한 자세한 내용은 [log_modules.go](https://github.com/klaytn/klaytn/blob/dev/log/log_modules.go)를 참조하세요.

비정상적인 상황이 발생하면 [github](https://github.com/klaytn/klaytn/issues), [Klaytn 포럼](https://forum.klaytn.foundation/), [Discord](https://discord.io/KlaytnOfficial)를 통해 클레이튼 팀에 신고해주시기 바랍니다.

## 오류 로그

| 로그 유형 | 노드 유형 | 로그 메시지 | 설명 | 권장 가이드 |
| :--- | :--- | :--- | :--- | :--- |
| Blockchain | CN/PN/EN |########## BAD BLOCK #########Chain config: %v<br /><br />Number: %vHash: 0x%x%v<br /><br />Error: %v############################## | 배드 블록은 수신한 영수증과 실행 결과가 일치하지 않을 때 발생합니다. 노드가 불량 블록 로그와 함께 중지되는 경우 두 가지 이유가 있을 수 있습니다.  <br />- 사례 1. 바이너리 버전과 같이 노드의 구성이 잘못된 경우입니다.  <br />- 사례 2. 코드에 문제가 있습니다. 다른 노드에서도 동일한 문제가 발생할 가능성이 매우 높습니다. | 이 오류는 매우 중요하므로 잘못된 블록을 발견하면 이슈를 만들거나 Klaytn GitHub 리포지토리에 신고해 주세요. |
| ConsensusIstanbulCore | CN/PN/EN | **Drop an empty message from timeout channel** | 라운드 변경 타이머가 만료된다는 의미입니다. 타이머가 실수로 종료되면 이 오류가 출력됩니다. | 다운로더가 시작될 때 이 오류가 발생할 수 있습니다. 다음 로그도 `Block synchronisation started`으로 출력되는지 확인하세요. |
| NetworksP2P | CN/PN/EN | **Protocol istanbul/64 failed** id=04680a827fa1b240 conn=staticdial err="write tcp 10.117.2.105:34396->10.117.2.27:32323: use of closed etwork connection" <br /><br />**Protocol istanbul/64 failed** err="shutting down" | 이 로그는 다른 노드가 연결이 끊어지면 인쇄될 수 있습니다. 보통 `Disconnected a P2P Peer` 로그가 뒤따릅니다. | 연결이 끊긴 피어가 다시 연결되었는지 확인합니다. 재연결되지 않으면 네트워크 상태 또는 피어 연결을 [admin_peers](../../references/json-rpc/admin.md#admin_peers) |
| NodeCN | CN |  **fail to SendNewBlockHashes** err="write tcp 10.117.2.124:24108->10.117.2.108:32323: use of closed network connection"  <br /><br />**fail to SendNewBlockHashes** err="shutting down" | `Protocol istanbul/64 failed`와 같음 | `Protocol istanbul/64 failed`와 같음 |
| NodeCN | CN | **fail to SendNewBlock** peer=d35220eccdb0de7b err="shutting down" | `Protocol istanbul/64 failed`와 동일 | `Protocol istanbul/64 failed`와 동일
| NetworksRPC | EN (대부분) | **FastWebsocketHandler fail to upgrade message** error="websocket: version != 13" | 웹소켓 연결의 버전 문제 | 요청 헤더에 값이 13으로 설정된 `Sec-Websocket-Version` 필드가 포함되어야 합니다. klaytn rpc 클라이언트를 사용하지 않았을 수 있습니다. |


## 경고 로그

| 로그 유형 | 노드 유형 | 로그 메시지 | 설명 | 권장 가이드 |
| :--- | :--- | :--- | :--- | :--- |
| 블록체인 | CN/PN/EN | **Upgrade database version** from=N/A to=3 | 노드 시작 시 기록됨 | 처리할 필요가 없습니다. |
| ConsensusIstanbulCore | CN | **[RC]** round= | 라운드 변경 로그는 [RC] 태그로 시작됩니다. | 라운드가 한두 라운드로 끝나지 않고 계속 올라간다면 네트워크 상태나 피어 연결을 먼저 분석해야 합니다. 피어 연결 확인 API: [admin_peers](../../references/json-rpc/admin.md#admin_peers)|
| ConsensusIstanbulCore | CN | **unexpected request**  address= state="Accept request" seq=312 err="old message" number=311 hash=d960ea…6df6de | 제안자가 블록을 채굴했지만 예기치 않은 것으로 판명되었습니다. 대부분의 경우 새 블록이 되기에는 너무 오래되었습니다. | 처리할 필요가 없습니다. |
| 노드 | CN/PN/EN | **Failed doConnTypeHandshake** addr=10.117.2.252:28516 conn=inbound conntype=-1 err="read tcp 10.117.2.78:32324->10.117.2.252:28516: i/o timeout | 두 P2P 피어는 다이얼링으로 연결을 설정합니다. 설정에 실패하면 이 로그가 인쇄됩니다. | 연결이 끊긴 피어가 다시 연결되는지 확인하세요. 그렇지 않은 경우 네트워크 상태 또는 피어 연결 확인 피어 연결 확인 API: [admin_peers](../../references/json-rpc/admin.md#admin_peers) |
| NodeCN | PN/EN | **Failed to filter bodies** peer=c02e4b4d471c56b9 lenTxs=1 | 노드가 가져올 때 원하지 않는 블록 헤더를 수신했습니다.  - lenTxs: 요청되지 않은 txs 수 | 처리할 필요가 없습니다. |
| Work | CN | **Transaction aborted due to time limit** hash= | 채굴 시 블록 실행 시간이 250ms를 넘지 않아야 하므로 이 시간 제한으로 인해 마지막 트랜잭션이 중단될 수 있습니다. | 트랜잭션이 블록에 진입했는지 확인합니다. |
| Work | CN | **Transaction failed, account skipped** hash=b1b26c...6b220a err="insufficient balance for transfer"<br /><br />Error(v1.6.2 이전)<br />Warn(v1.6.2 이후) | `from` 계정의 잔액이 부족하여 채굴 중 거래가 실행되지 않는 경우 (이론적으로는 트랜잭션을 생성하여 txpool에 입력할 당시에는 잔액이 충분하였으나 실제 실행 시에는 그렇지 않은 경우 발생함). | `from` 계정의 잔액이 실제로 부족하지 않은지 확인합니다. |


## 정보 로그

`info` 로그에는 노드 상태에 대해 자세히 알 수 있는 추가 정보가 포함되어 있으므로 `info` 레벨 로그를 처리할 필요가 없습니다.

| 로그 유형 | 노드 유형 | 로그 메시지 | 설명 |
| :--- | :--- | :--- | :--- |
| Blockchain | CN/PN/EN | **Regenerated local transaction journal** transactions=0 accounts=0 | 노드가 종료되면 로컬 트랜잭션이 파일에 저널링됩니다(기본 파일 이름은 transactions.rlp). 저널링된 파일로 노드를 재시작하면 파일을 기반으로 로컬 트랜잭션을 다시 생성할 수 있습니다.  - transactions: 재생성된 로컬 트랜잭션의 수입니다.  - accounts: 재생성된 계정 수(==from) |
| Blockchain | CN/PN/EN| **Inserted a new block** number=14 hash=13cbfc…f007fc txs=0 gas=0 elapsed=793.458µs  processTxs=167ns finalize=157.708µs validateState=7.542µs totalWrite=443.417µs trieWrite=256.667µs | 노드가 해당 블록의 제안자가 아니고 합의에 성공하면 해당 노드는 블록을 실행(==validates)한 것입니다. 즉, 블록이 삽입된 것입니다.  - gas: 트랜잭션 실행 중 소비된 총 가스입니다.  이 필드는 일반적으로 네트워크를 테스트하여 블록당 사용된 가스를 찾을 때 사용됩니다. |
| NetworksP2P | CN/PN/EN | **[Dial] Add dial candidate from static nodes**  id=62a08a4b9f091c4b NodeType=0 ip=10.117.2.8 mainPort=32323 port=[32323] | 새 P2P 피어가 연결되며, 이 노드는 정적 노드입니다. static-nodes.json 또는 addpeer api를 사용하여 수동으로 추가한 노드를 정적 노드라고 합니다. 멀티채널인 경우, 두 개의 포트를 사용합니다. 예: [32323, 32324].  - id: 정적 피어 아이디 - NodeType: 정적 노드 타입(cn,pn,en,bn) - ip: 정적 IP - mainPort: 정적 TCP 수신 포트 번호 - port: 메인 포트와 서브 포트를 모두 포함한 정적 TCP 수신 포트 번호.
| NetworksP2P | CN/PN/EN | **Added a multichannel P2P Peer** id=28a6760472a078fb conn=staticdial peerID=28a6760472a078fb | 새 피어가 멀티채널 피어로 연결됩니다.  - id/peerID: 내 노드의 피어 ID - conn: 연결 유형 - 인바운드: 누군가 나를 연결함 - staticdial: static-nodes.json 또는 addPeer와 같은 정적 연결 - trusteddial: trust-nodes.json과 같은 신뢰 연결. 최대 연결 수를 초과하더라도 항상 다시 연결하고 설정할 수 있습니다.
| NetworksP2P | CN/PN/EN | **Disconnected a multichannel P2P Peer** id=28a6760472a078fb conn=inbound    peerID=28a6760472a078fb peerName=Klaytn/v1.7.3+acae89350c/darwin-arm64/go1.18.1 err=EOF | 멀티채널 피어의 연결이 끊어졌습니다.  - peerName: 내 노드 정보가 표시됩니다 - err: 연결이 끊어진 이유 |
| NetworksP2P | CN/PN/EN | **ProtocolManager.processConsensusMsg closed** id=28a6760472a078fb conn=inbound    PeerName=Klaytn/v1.7.3+acae89350c/darwin-arm64/go1.18.1 | P2P 노드의 연결이 끊어지면, 노드 간 합의 메시지 채널도 함께 닫힙니다. |
| StorageStateDB | CN/PN/EN | **Persisted trie from memory database** blockNum=23460 updated nodes=4 updated nodes size=499.00B time=539.959µs  gcnodes=68  gcsize=10.55kB gctime=226.499µs  livenodes=245 livesize=37.80kB | 이 로그는 트라이 데이터베이스가 커밋되었음을 알리기 위해 출력합니다. 여기서 커밋은 변경 사항을 실제 DB로 플러시하는 것을 의미합니다.  커밋은 주기적으로 수행됩니다.  - 사례 1. 노드가 풀 노드인 경우, 128블록마다 트라이 커밋이 수행됩니다.  - Case 2. 노드가 아카이브 노드인 경우, 모든 블록에 대해 시도 커밋을 수행합니다.  다음 상황에서도 커밋이 이루어집니다.  - 노드가 종료되면 커밋이 수행됩니다.  - 메모리 크기가 한도를 초과하면 커밋이 수행됩니다.  Tip.  - gc는 가비지 컬렉션을 의미합니다. 여기서 가비지 콜렉션은 트라이 노드 변경에 대한 DB 쓰기를 플러시하는 것을 의미합니다.  - 풀 노드는 128사이클마다의 정보와 최근 128블록의 정보를 저장합니다.  - 아카이브 노드는 모든 블록의 정보를 저장합니다.
| Work | CN | **Commit new mining work** number=14 hash=438ef7…68ca7f txs=0 elapsed=605.375µs commitTime=184.708µs finalizeTime=414.375µs | 모든 CN은 라운드 변경에 대비하여 블록을 채굴합니다 - number: 블록 번호 - hash: 블록 해시(최종 버전이 아님) - txs: 블록의 트랜잭션 수 - elapsed: 총 블록 채굴 시간(commitTime + finalizeTime) - commitTime: 블록의 트랜잭션 실행 시간 - finalizeTime: 블록 최종 완료 시간 |
| Work | CN | **Successfully sealed new block** number=14 hash=13cbfc…f007fc | [제안자만] 새 블록을 봉인하는 데 성공했습니다. 봉인에는 다음 단계가 포함됩니다.  - 블록에 대한 Ibft 합의 프로세스.  - 블록의 타임스탬프와 서명 업데이트 |
| Work | CN | **Successfully wrote mined block** num=14 hash=13cbfc…f007fc txs=0 elapsed=617.709µs | [제안자만] 노드가 제안자이고 합의에 성공한 경우, 제안자는 블록 실행 결과를 DB에 저장해야 합니다. 이 로그는 저장에 성공했음을 의미합니다. |
| Work | CN | **Mining too far in the future** wait=1s | 1초의 블록 생성 기간을 유지하기 위해 노드는 "1초 - 이전 블록 생성/전파/실행 시간" 동안 슬립합니다.  - wait: 노드가 잠자기하는 시간|
| VM | CN/PN/EN | **Returning since the addr is not a program account** addr= | 누군가 존재하지 않는 컨트랙트를 호출하려고 시도합니다.  팁. 클레이튼에서 프로그램 계정은 컨트랙트 계정과 동일합니다. |
