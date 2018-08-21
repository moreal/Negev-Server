# 기능 분산
구현 우선도에 따라서 정렬 해놓은 상태입니다.  
본래 AWS에 퍼블리싱하고 싶지만 비용이 걱정되는 관계로 도커 컨테이너로 만들어 성능 좋은 서버 하나에서 모두 돌려보고자 합니다.

## Login, Register (Gate)
로그인과 회원가입 업무를 처리합니다  
**대거 진입**에 대비하여 JobQueue 혹은 SQS로 처리합니다.

- DB: MySQL (for high consistency)
- Language: Python
- Framework: [flask](https://github.com/pallets/flask)
- Library: PyMySQL

## Session (Saver, Artoria)
세션을 발급, 등록하고 해당 키로 접근을 관리합니다.  
**병목 예상 지점**

- DB: Redis
- Language: C++
- Library: [cpp_redis](https://github.com/Cylix/cpp_redis)

## Main (Nuclear)
게임플레이에 필요한 스테이지를 제공하고 스테이지 클리어 여부를 받아 아이템 지급 등을 처리해줍니다.  
플레이어의 인벤토리 등도 관리, 제공해줍니다.

- DB: Couchbase (for high scalability)
- Language: C++
- Library: [libcouchbase](https://github.com/couchbaselabs/libcouchbase-cxx)

## File Provider (Updater)
클라이언트에 업데이트가 필요한지 여부를 체크하고 세션을 체크하여 접근 권한 확인후 리소스를 제공해줍니다.  
만약 플레이어의 악성 접속 요청이 의심 될 경우 유저의 업데이트 정보를 DB에 저장해놓고 제공해 주는 것도 고려해 봐야하나 싶습니다.

- Protocol: HTTP
- Language: Python
- Framework: flask
- Tool(?)
    - Docker (for container)
    - [k8s](https://kubernetes.io/) (for Service LB, Ochestration)

## Mail (Mailer)
플레이어에게 운영 메시지, 아이템 획득등을 처리합니다.  

- DB: TODO
- Server: TODO

## Logging (Clerk)
플레이어의 접속 기록, 클리어 기록 등을 관리하는 데이터입니다.  
후일에 Log 분석을 해보고자 MapReduce를 지원하는 MongoDB를 사용해 보고자 하였습니다.

- DB: MongoDB
- Language: C++
- Library: [mongo-cxx-driver](https://github.com/mongodb/mongo-cxx-driver)

## Manager (Manager)

- DB: 기존의 여러 DB 이용
- Language: Python
- Framework: [flask](https://github.com/pallets/flask)