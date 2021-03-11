# Storage Gateway

현재 이용하고 있는 시스템 파일 or 블록을 AWS에 백업할 수 있도록 지원해주는 서비스  

- 특징  - 업계표준 NFS(네트워크 파일 시스템). SMB(서버 메세지 블록) 을 이용해 저장이 가능하다.

[AWS Storage Gateway](https://docs.aws.amazon.com/ko_kr/storagegateway/latest/userguide/WhatIsStorageGateway.html)는 온프레미스 환경을 클라우드 스토리지와 연결할 수 있는 서비스입니다.보통 온프레미스의 백업 데이터를 받아서 클라우드 환경의 S3로 넘기는 작업을 합니다.즉, 온프레미스 환경에 있던 데이터를 일정 시간이 지나면 AWS로 옮긴다는 뜻이고, AWS DataSync와는 다르게 Hybrid Cloud Storage를 구현할 때 사용합니다.

- 백업을 클라우드로 이동
- 온프레미스 파일 공유
- AWS의 데이터에 낮은 지연 시간으로 액세스 가능

Storage Gateway는 대용량 데이터를 적재하는 데에는 적합하지 않습니다.보통 변경된 데이터나 압축된 데이터에만 적절합니다.대용량 데이터를 적재하고 싶다면 Storage Gateway 대신 AWS DataSync를 사용해야 합니다.

`파일 게이트웨이`, `테이프 게이트웨이`, `볼륨 게이트웨이`의 3가지 게이트웨이를 제공합니다.

### 파일 게이트웨이

**NFS(Network File System) 및 SMB(Server Message Block)** 같은 업계 표준 파일 프로토콜을 사용하여 Amazon S3에서 객체를 저장하고 검색합니다.

### 볼륨 게이트웨이

- **캐시**된 볼륨 아키텍처
    - 캐시 볼륨으로 Amazon S3를 기본 데이터 스토리지로 사용함과 동시에, 자주 액세스하는 데이터를 스토리지 게이트웨이에 로컬 보관합니다.
- **저장**된 볼륨 아키텍처
    - 저장 볼륨을 사용하여 기본 데이터를 로컬에 저장하는 한편, 해당 데이터를 AWS에 비동기식으로 백업합니다.

### 테이프 게이트웨이

백업 데이터를 비용 효율 및 내구성이 좋은 방식으로 Glacier 또는 Glacier Deep Archive에 보관합니다.