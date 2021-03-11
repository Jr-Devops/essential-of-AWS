# EBS

Elastic Block Store

## 기능

SSD,HDD 블록수준 저장공간을 제공

[Amazon EBS](https://aws.amazon.com/ko/ebs/features/)

는 대규모의 처리량이 필요하거나 고성능의 트랜잭션 워크로드 지원이 필요한 경우에 많이 사용되는 블록 스토리지 서비스입니다.

필요한 수만큼 EC2 인스턴스에 연결할 수 있으며 물리 서버의 하드 드라이브, 플래시 드라이브, USB 드라이브와 유사하게 사용됩니다.

### EBS 유형

현재 EBS 유형에는 SSD(Solid State Drive) 기술을 사용하는 유형과 기존의 디스크 회전 구동형 HDD(Hard Disk Drive) 기술을 사용하는 유형으로 두 가지가 있습니다.각 유형의 차이를 정확히 아는 것이 중요합니다.

성능은 최고 **IOPS(Input/Output Operations Per Second)**로 측정합니다.

- EBS 프로비저닝된 IOPS SSD (io2)
    - 고성능 I/O 작업이 필요할 때 사용하며, 지연 시간에 민감한 트랜잭션 워크로드를 위해 설계된 볼륨입니다.
    - 최대 64,000 IOPS 성능을 제공합니다.
    - GiB 당 주어지는 IOPS는 500:1 입니다.
- EBS 프로비저닝된 IOPS SSD (io1)
    - 최대 64,000 IOPS 성능을 제공한다.
    - GiB 당 주어지는 IOPS는 50:1 입니다.
    - 참고로 시험에서 io2와 io1의 차이를 묻는 문제는 나오지 않고 있습니다.
- EBS 범용 SSD (gp2)
    - 일반 서버 워크로드에는 이론적으로 짧은 지연 시간 성능을 제공하는 범용 SSD가 적합합니다.
    - 최대 16,000 IOPS 성능을 제공합니다.
    - GiB 당 주어지는 IOPS는 3:1 입니다.
- 처리량에 최적화된 HDD (st1)
    - 로그 처리와 빅데이터 작업 등 처리량이 많은 워크로드에 적합합니다.
    - 최대 500 IOPS 성능을 제공합니다.
- 콜드 HDD (sc1)
    - 빈번하게 엑세스하지 않는 대용량 데이터 작업에 적합합니다.
    - 250 IOPS 성능을 제공합니다.



### SSD VS. HDD

위의 유형을 둘로 나눈 SSD와 HDD의 차이를 아는 것도 중요합니다.

- SSD
    - Small, Random한 작업(workload)
    - Transactional한 작업 (OLTP)
    - IOPS 퍼포먼스가 필요한 비즈니스 애플리케이션, 데이터베이스 작업
    - 비용이 비쌉니다.
    - 주요 속성은 IOPS 입니다.
- HDD
    - Large, Sequential한 작업(workload)
    - Large Streaming 작업
    - 빅데이터, 데이터 웨어하우스, Log processing
    - 비용이 상대적으로 쌉니다.
    - 주요 속성은 처리량(Throughput) 입니다.
    - 
EBS 볼륨은 SSD와 HDD로 나뉘어져 있다. 

숫자가 클수록 더 고성능이다. ex) gp3 > gp2    ,    io2>io1

1. 범용 SSD - 일반적인 경우 사용하는 SSD 
    - gp3
    - gp2
2. 프로비저닝된 IOPS SSD - 더 짧은 지연시간, 더 많은 처리량을 요구할때 사용
    - io2 Block Express - 밀리초 미만,
    - io2 - EBS 다중연결 가능, 내구성 99.999%
    - io1 - EBS 다중연결 가능, 내구성 99.8~99.9%

st1 > sc1

1. 처리량에 최적화된 HDD
    - st1 - 자주 엑세스 하는 경우 적합
2. Cold HDD
    - sc1 - 자주 엑세스 하지 않는 경우 적합  

### 최대 64,000 IOPS를 내기 위해

최대 성능인 64,000 IOPS를 내려면 Provisioned IOPS SSD EBS volume (io1)를 사용하면서 전용 EC2 유형인 `Nitro-based EC2`를 사용해야 합니다.다른 EC2 인스턴스 유형에서는 io1 EBS를 써도 최대 32,000 IOPS 까지밖에 나오지 않습니다.

### 암호화

EBS 볼륨을 암호화해서 데이터를 보호할 수 있고, 내부에서 암호화 키를 관리하거나 AWS KMS에서 제공되는 키를 사용할 수 있습니다.데이터 볼륨, 부팅 볼륨, 스냅샷에 대한 암호화를 제공합니다.

> AWS KMS (Key Management Service) : 암호화 키를 생성 및 관리할 수 있는 서비스

참고로 SSE(Server Side Encryption)는 S3의 옵션이지 EBS의 옵션은 아닙니다.

### 스냅샷

모든 EBS 볼륨은 스냅샷을 통해 복사할 수 있고, 기존 스냅샷을 다른 인스턴스에 공유해서 연결할 수 있으며, AMI에 등록할 수 있는 이미지로 변경할 수 있습니다.

- 루트(Root) 디바이스 역할을 하는 EBS 볼륨의 스냅샷을 생성할 때는 인스턴스를 중지한 후 스냅샷을 생성해야 합니다.
- 최대 절전 모드가 활성화된 인스턴스에서는 스냅샷을 생성할 수 없습니다.
- 볼륨의 이전 스냅샷이 `pending` 상태일 때에도 볼륨의 스냅샷을 생성할 수는 있지만 볼륨의 `pending` 스냅샷을 여러 개 생성하면 스냅샷이 완료될 때까지 볼륨 성능이 저하될 수 있습니다.

![Untitled](https://user-images.githubusercontent.com/37682970/110816590-d1394f80-82ce-11eb-9576-4007f1eb6856.png)
