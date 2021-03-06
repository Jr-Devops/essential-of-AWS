# Route53

### 프라이빗 호스팅 영역

서비스로 생성한 Route53 내에있는 도메인 및 하위도메인에 대한 VPCs(가상 프라이빗 클라우드)의 DNS 쿼리 응답 정보가 담긴 컨테이너를 말한다.

[Route 53](https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/Welcome.html)은 가용성과 확장성이 우수한 DNS 웹 서비스입니다.다음과 같은 기능을 제공합니다.

- 도메인 이름을 등록할 수 있습니다.
- 인터넷 트래픽을 도메인의 리소스로 라우팅합니다.
    - 도메인 혹은 하위 도메인 요청에 대해 브라우저를 웹 사이트 또는 웹 애플리케이션에 연결하도록 돕습니다.
- 리소스의 상태 확인
    - 웹 서버 같은 리소스로 자동화된 요청을 보내어 접근 및 사용이 가능하고 정상 작동 중인지 확인합니다.
    - 리소스 사용 불가 시 알림을 수신하고 다른 곳으로 트래픽을 라우팅할 수 있습니다.

### 라우팅 정책

Route 53에서 다양한 라우팅 정책을 설정할 수 있는데요, 각 정책의 특징을 잘 구분해서 알아두면 좋습니다.

- 단순 라우팅 정책
    - 도메인에 대해 특정 기능을 수행하는 하나의 리소스만 있는 경우(예: example.com 웹 사이트의 콘텐츠를 제공하는 하나의 웹 서버)에 사용합니다.
- 장애 조치 라우팅 정책 (Failover routing policy)
    - 액티브-패시브 장애 조치를 구성하려는 경우에 사용합니다.
- 지리 위치 라우팅 정책 (Geolocation routing policy)
    - 사용자의 위치에 기반하여 트래픽 라우팅하려는 경우에 사용합니다.
- 지리 근접 라우팅 정책 (Geoproximity routing policy)
    - 리소스의 위치를 기반으로 트래픽을 라우팅하고 필요에 따라 한 위치의 리소스에서 다른 위치의 리소스로 트래픽을 보내려는 경우에 사용합니다.
- 지연 시간 라우팅 정책 (Latency routing policy)
    - 여러 AWS 리전에 리소스가 있고 최상의 지연 시간을 제공하는 리전으로 트래픽을 라우팅하려는 경우에 사용합니다.
- 다중 응답 라우팅 정책 (Multivalue answer routing policy)
    - Route 53이 DNS 쿼리에 무작위로 선택된 최대 8개의 정상 레코드로 응답하게 하려는 경우에 사용합니다.
- 가중치 기반 라우팅 정책 (Weighted routing policy)
    - 사용자가 지정하는 비율에 따라 여러 리소스로 트래픽을 라우팅하려는 경우에 사용합니다.

### Active-Active Failover, Active-Passive Failover

액티브-액티브 장애 조치는 모든 리소스를 대부분의 시간 동안 사용 가능하도록 하는 설정입니다.액티브-패시브 장애 조치는 기본 리소스 그룹이 대부분의 시간 동안 사용 가능하도록 하고 **보조 리소스 그룹은 대기 상태**에 놓은 설정입니다.다음과 같은 구성이 가능합니다.

- 하나의 기본 및 보조 리소스를 사용한 액티브-패시브 장애 조치 구성
- 여러 개의 기본 및 보조 리소스를 사용한 액티브-패시브 장애 조치 구성
- 가중치 레코드를 사용하여 액티브-액티브, 액티브-패시브 장애 조치 구성
    - 레코드 별로 가중치를 설정하여 DNS 쿼리에 응답합니다.
    - 가중치가 0인 레코드는 0이 아닌 가중치의 레코드가 비정상일 경우에만 쿼리에 응답합니다.
