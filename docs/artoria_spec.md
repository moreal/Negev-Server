# **Artoria** For Session
## Overview
세션을 발급, 등록하고 해당 키로 접근을 관리합니다.  
**병목 예상 지점**

## Specification
- DB: Redis
- Language: C++
- Library: [cpp_redis](https://github.com/Cylix/cpp_redis)

## Thinking
본 **Artoria** 서버는 단순히 세션을 통해 사용자의 접근 권한을 체크하기 위함입니다.  
사용자가 세션아이디를 가지고 접근할 수 있는 곳인지를 빠르게 판단할 수 있는 인테페이스가 제공될 것입니다.  
Redis를 Master, Slave로 복제를 반복하여 속도를 트래픽을 여러 곳으로 분산 시킵니다.  
k8s Service로 LB 처리합니다.

TTL=1800s

## Structure
### **TCP / Check Session**
sessid를 접근 권한을 검사 후 결과 값을 받습니다

Request Format
```c++
class SessionCheckRequest {
    const PacketType packetType = PacketType::SessionCheckRequestPacket;
    int32_t sessid;
};
```


Response Format
```c++
class SessionCheckResponse {
    const PacketType packetType = PacketType::SessionCheckResponsePacket;
    int8_t status;
};
```

### **TCP / Register Session**
sessid를 접근 권한을 검사 후 결과 값을 받습니다

Request Format
```c++
class SessionRegisterRequest {
    const PacketType packetType = PacketType::SessionRegisterRequestPacket;
    int32_t sessid;
    int32_t ip;
    const char *id;
};
```

Response Format
```c++
class SessionRegisterResponse {
    const PacketType packetType = PacketType::SessionRegisterResponsePacket;
    int8_t status;
};
```

### **TCP / Register Session**
sessid를 접근 권한을 검사 후 결과 값을 받습니다

Request Format
```c++
class SessionRegisterRequest {
    const PacketType packetType = PacketType::SessionRegisterRequestPacket;
    int32_t sessid;
    int32_t ip;
    const char *id;
};
```


### **TCP / Delete Session**
sessid를 접근 권한을 검사 후 결과 값을 받습니다

Request Format
```c++
class SessionRegisterRequest {
    const PacketType packetType = PacketType::SessionRegisterRequestPacket;
    int32_t sessid;
    int32_t ip;
    const char *id;
};
```

Response Format
```c++
class SessionRegisterResponse {
    const PacketType packetType = PacketType::SessionRegisterResponsePacket;
    int8_t status;
};
```

### **TCP / Count Users**
현재 Redis에 등록되어 있는 세션의 수, 유저의 수를 얻습니다.  

Request Format
```c++
class SessionRegisterRequest {
    const PacketType packetType = PacketType::SessionRegisterRequestPacket;
};
```

Response Format
```c++
class SessionRegisterResponse {
    const PacketType packetType = PacketType::SessionRegisterResponsePacket;
    int8_t status;
};
```