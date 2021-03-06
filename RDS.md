# RDS

Amazon Relational Database Service

[Amazon RDS](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/Welcome.html)는 관계형 데이터베이스를 운영 및 확장할 수 있는 관리형 서비스입니다.주의할 점은 **완전 관리형 서비스가 아닌 관리형 서비스**이기 때문에, Scaling, 읽기전용 복제본 등의 확장 기능을 사용자가 직접 구축해야 한다는 점입니다.

### 자동 백업 및 데이터베이스 스냅샷

RDS는 인스턴스 백업 및 복구를 위해 두 가지 방법을 제공합니다.

- 자동 백업
    - 자동 백업을 활성화하면 매일 자동으로 기본 백업 기간 내에 스냅샷을 만들고, 트랜잭션 로그를 캡처합니다.
    - 특정 시점으로 복구를 시작할 때, 가장 적합한 일일 백업에 트랜잭션 로그가 적용됩니다.
    - 백업 보존 기간은 기본적으로 1일이지만 최대 35일까지 설정할 수 있습니다. (참고 : API, CLI를 사용하면 1일, 콘솔을 사용하면 7일)
- DB 스냅샷
    - 원하는 빈도로 백업한 다음 언제든 해당 상태로 복구할 수 있습니다.
    - 사용자가 명시적으로 삭제할 때까지 유지됩니다.

기본적으로 RDS는 DB 인스턴스를 자동으로 백업하는데요, 이때 보존 기간은 7일입니다.DB 스냅샷과 자동 백업은 S3에 저장됩니다.

### 암호화

각 DB 인스턴스에 대해 SSL/TLS 인증서를 생성하여 애플리케이션과 DB 인스턴스 간 전송되는 데이터를 암호화할 수 있습니다.RDS DB에 저장된 데이터를 암호화하려면 KMS를 통해 관리하는 키를 사용하여 암호화할 수 있습니다.

Amazon RDS 암호화된 DB 인스턴스에는 다음과 같은 제한이 있습니다.

- Amazon RDS DB 인스턴스에 대한 암호화는 암호화를 생성할 때에만 활성화할 수 있으며 DB 인스턴스가 생성된 후에는 불가능합니다.다만 암호화되지 않은 DB 스냅샷의 사본을 암호화할 수 있기 때문에, DB 인스턴스의 스냅샷을 만든 다음 해당 스냅샷의 암호화된 사본을 만들어 복구하면 암호화한 DB 인스턴스를 만들 수 있습니다.
- 암호화된 DB 인스턴스는 암호화를 비활성화하도록 수정할 수 없습니다.
- 암호화되지 않은 DB 인스턴스의 암호화된 읽기 전용 복제본이나 암호화된 DB 인스턴스의 암호화되지 않은 읽기 전용 복제본은 보유할 수 없습니다.
- 암호화된 읽기 전용 복제본이 원본 DB 인스턴스와 동일한 AWS 리전에 있는 경우 해당 인스턴스와 동일한 CMK를 사용하여 암호화해야 합니다.

### 고가용성 다중 AZ 배포

다중 AZ 배포에서 Amazon RDS는 서로 다른 가용 영역에 `동기식` 예비 복제본(Synchronous Standby)을 프로비저닝하고 유지합니다.다만, 고가용성 기능은 읽기 전용 시나리오의 솔루션과 다르기 때문에, 예비 복제본을 사용하여 읽기 전용 복제본처럼 읽기 트래픽을 지원할 수는 없습니다.(**장애 상황이 발생하여 예비 복제본이 승격되기 전까지는 어떠한 경우에도 예비 복제본을 사용할 수 없습니다.**)

### 읽기 전용 복제본(Read Replica)

Amazon RDS 읽기 전용 복제본은 `비동기식`으로 복제되며, 손쉽게 단일 DB 인스턴스를 탄력적으로 확장하여 읽기 중심의 데이터베이스 워크로드를 처리할 수 있습니다.필요한 경우 읽기 전용 복제본은 독립 실행형 DB 인스턴스로 승격될 수 있습니다.

그리고 읽기 전용 복제본을 만들려면 자동 백업을 활성화해야 합니다.

각 내용을 간단히 비교해보자면 다음과 같습니다.

- 다중 AZ 배포
    - 고가용성이 주요 목적
    - 동기식 복제. (Aurora는 비동기식 복제)
- 다중 리전 배포
    - 재해 복구 및 로컬 성능이 주요 목적
    - 비동기식 복제
- 읽기 전용 복제본
    - 확장성이 주요 목적
    - 비동기식 복제

### Enhanced Monitoring

이 옵션을 활성화하고 시간 단위를 설정하면, 정해진 시간 단위에 따라 주요 지표를 수집하고 정보를 처리할 수 있습니다.

CPU, 메모리, 파일 시스템, 디스크 I/O 등의 인스턴스 지표를 수집하고, 해당 정보를 CloudWatch Logs로 전송하여 CloudWatch에서 CloudWatch Logs의 지표 필터를 생성하여 그래프를 표시할 수 있습니다.

CloudWatch는 하이퍼바이저에서 CPU 사용률에 대한 측정치를 수집하는데 반해, 확장 모니터링은 인스턴스 레벨에서 여러 프로세스, 스레드의 CPU 사용률을 수집합니다.

확장 모니터링 프로세스 목록에 표기되는 지표는 다음과 같습니다.

- RDS child processes
- RDS processes
- OS processes