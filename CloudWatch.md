# CloudWatch

[Amazon CloudWatch](https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)는 실시간으로 실행 중인 애플리케이션을 여러가지 지표로 모니터링할 수 있는 도구입니다.CloudWatch 그 자체만으로 많은 내용을 다룰 수 있지만, 시험에서는 이 서비스가 어떤 서비스인지, 어떤 지표를 확인할 수 있는지, 어떤 경보를 통해 애플리케이션의 가용성을 관리할 수 있는지 정도로만 알고 계시면 됩니다.

### 지표

5가지 기본 제공 지표를 중심으로 알아두시면 좋습니다.

- 기본 제공 지표
    - CPU Utilization
    - Network Utilization In/Out
    - Disk Reads/Write
- custom 하게 만들 수 있는 지표
    - Memory Utilization
    - Disk Swap Utilization
    - Disk Space Utilization
    - Page File Utilization
    - Log Collection

### CloudWatch Logs

CloudWatch Logs는 애플리케이션 및 사용자 정의 로그 파일을 이용하여 모니터링을 하는 서비스입니다.다음과 같은 경우에 사용합니다.

- 실시간 애플리케이션 및 시스템 모니터링
- 로그 장기 보존