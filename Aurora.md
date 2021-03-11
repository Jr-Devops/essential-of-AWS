# Aurora

[Amazon Aurora](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)는 MySQL 및 PostgreSQL과 호환되는 **완전 관리형** 관계형 데이터베이스 엔진입니다.Aurora는 RDS에 포함되는 데이터베이스지만, 보통의 RDS와 다른 특징들을 가지고 있기에 시험에 별도의 데이터베이스로 언급되고 있습니다.

### Instance Cluster + Endpoint

Amazon Aurora 는 전형적으로 단일 인스턴스가 아닌 인스턴스 군을 포함합니다.각각의 커넥션은 특정한 DB 인스턴스에 의해 관리되고, Aurora Cluster에 접근하면, 호스트 이름과 포트번호로 Endpoint라 불리는 특정 핸들러를 지정받을 수 있습니다.

Aurora에서는 각각의 인스턴스 군이 서로 다른 역할을 가져갈 수 있어서, Endpoint를 사용하면 각각의 커넥션을 역할에 맞는 인스턴스 군으로 매핑할 수 있습니다.

- Cluster Endpoint
    - Writer Endpoint로, Primary DB와 연결됩니다.
- Reader Endpoint
    - Read-Only 커넥션에 대한 Load-Balancing을 제공합니다.

### 암호화

Aurora는 SSL(AES-256)을 사용하여 인스턴스와 애플리케이션 간 연결을 보호합니다.KMS를 통해 관리하는 키를 사용해 데이터베이스를 암호화할 수 있습니다.

암호화되지 않는 기존 데이터베이스는 암호화할 수 없기 때문에 암호화를 활성화한 새로운 DB 인스턴스를 생성하고, 데이터를 마이그레이션 해야합니다.

기본적으로 위에서 소개한 RDB에서의 암호화 제한과 동일한 스냅샷 암호화 제한 조건을 가지고 있습니다.

### Amazon Aurora Parallel Query (병렬 쿼리)

Amazon Aurora 병렬 쿼리는 데이터를 별도의 시스템으로 복사할 필요 없이 현재 데이터에 대한 `분석 쿼리`를 더 빠르게 제공하는 기능입니다.핵심 트랜잭션 워크로드의 처리량을 높게 유지하면서, 쿼리 속도를 최대 100배로 높일 수 있습니다.

일부 데이터베이스는 하나 또는 몇 개 서버의 CPU에 걸쳐 쿼리 처리를 병렬화할 수 있지만, 병렬 쿼리는 Aurora의 고유한 아키텍처를 활용하여 Aurora 스토리지 계층에 있는 수천 개의 CPU 전체로 쿼리 처리를 `푸시 다운`하여 병렬화합니다.병렬 쿼리는 분석 쿼리 처리를 Aurora 스토리지 계층으로 오프로딩하여 트랜잭션 워크로드의 네트워크, CPU 및 버퍼 풀 경합을 줄이게 됩니다.

### Amazon Aurora Serverless

Aurora Serverless는 온디맨드 Auto Scaling 구성입니다.Aurora Serverless DB 클러스터는 요구 사항에 따라 용량을 자동으로 시작, 종료, 확장, 축소합니다.Aurora Serverless는 **사용 빈도가 낮거나 간헐적이거나 예측할 수 없는 워크로드**에 대해 기능을 제공합니다.

- 자주 사용하지 않는 애플리케이션
- 신규 애플리케이션 (필요한 인스턴스 크기를 잘 모를 때)
- 가변성 높은 워크로드
- 예측 불가능한 워크로드
- 개발 및 테스트 데이터베이스 (사용하는 시간대가 정해진 경우)
- 다중 테넌트 애플리케이션

### 장애

- Amazon Aurora 복제본이 동일한 가용 영역 또는 다른 가용 영역에 있는 경우 장애 조치가 진행될 때 Aurora는 DB 인스턴스의 CNAME(정식 이름 레코드)이 정상적인 복제본을 가리키도록 이를 변경하고, 해당 복제본은 승격되어 새로운 기본 복제본이 됩니다.
- Aurora Serverless 및 DB 인스턴스를 실행하거나 AZ를 사용할 수 없으면 Aurora는 다른 AZ에서 DB 인스턴스를 자동으로 다시 생성합니다.
- Amazon Aurora 복제본(즉, 단일 인스턴스)이 없고 Aurora Serverless를 실행하지 않는 경우 Aurora는 원래 인스턴스와 동일한 가용 영역에 새 DB 인스턴스를 생성하려고 시도합니다.

MySQL보다 처리능력이 뛰어난 DB

Amazon RDS에서는 지원하지 않고 Aurora에서 지원하는 기능

- Aurora Serverless
- Aurora Global Database
- AWS 머신러닝 서비스와 통합지원

## 안정성

- 스토리지 자동복구 - 3개 가용영역에 여러개의 복사본이 존재하여 데이터 손실 가능성이 적고 결함을 자동 감지하여 복구함.
- 유지가능한 캐시워밍 - 전원이 꺼진 DB를 다시시작할때 캐시가 있어 빠른 성능을 가짐
- 충돌복구 - 충돌직후에도 빠른 복구로 DB를 안정적으로 사용 가능

## 보안

- IAM 자격증명- AWS에 연결할때 권한을 부여할 수 있는 IAM 정책이 AWS계정에 반드시 필요함
- VPC 서비스 기반 - VPC의 보안 그룹(전송계층보안, SSL, 방화벽규칙) 사용가능
- MySQL, PostgreSQL DB 로그인 및 권한인증 또는 IAM 데이터베이스 인증을 Aurora에서도 사용할 수 있음