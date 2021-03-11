# ELB

## ALB vs NLB

### ELB

- ALB가 사용할 서브넷을 discover하기 위해서는 올바른 태그가 달린 subnet이 존재
- NLB, CLB가 사용할 서브넷을 설정하기 위해서는 올바른 태그가 달린 subnet이 존재


![Untitled](https://user-images.githubusercontent.com/37682970/110816057-596b2500-82ce-11eb-8ffb-e12384068693.png)


### ALB(Application Load Balancer)

- Nginx와 HAProxy와 같은 HTTP/HTTPS(L7) 부하분산을 지원

### NLB(Network Load Balancer)

- LVS(Linux Virtual Server)와 같은 TCP(L4) 부하분산을 지원

### GLB(Gateway Load Balancer)

- 타사 어플라이언스 부하분산을 지원

## 관련 문제

## 90번

- VPC 구성 : 2개 퍼블릭 서브넷 + 2개 프라이빗 서브넷
- 응용 프로그램 중 웹 부분만 공개적 액세스
- EC2 인스턴스 SSL 종료 오프 로드

```jsx
1. 사용자는 Public DNS를 통해 ALB에 접근한다.
2. ALB의 트래픽은 IGW를 통해 VPC내로 들어온다.
3. 트래픽은 활성화된 로드밸런서 가용영역 subnet만 참조하게 된다.
3-1. 활성화 된 로드밸런서 가용영역 subnet은 private subnet이다.
3-2. private subnet안에 있는 로드밸런싱 대상의 인스턴스는 public 주소 또는 탄력적 ip가 없다.
3-3. public ip 또는 탄력적 ip없이는 외부와 통신을 할 수 없다.
3-4. 로드밸런서는 계속해서 활성화된 로드밸런서 가용영역 서브넷으로 브로드캐스트 주소로 패킷을 전송하다 유휴제한시간을 초과한다.
3-5. 최종적으로 사용자에게 timeout error을 출력한다
```
![Untitled 1](https://user-images.githubusercontent.com/37682970/110816080-5e2fd900-82ce-11eb-8c4d-3876c30c32ce.png)


![Untitled 2](https://user-images.githubusercontent.com/37682970/110816092-60923300-82ce-11eb-8943-c2a0bf6aef56.png)


- ALB는 Public Subnet에, Web 서버는 Private Subnet에 생성합니다.
- Web 서버에서 인터넷 통신은 퍼블릭 서브넷의 NAT Gateway를 통해서 함

![Untitled 3](https://user-images.githubusercontent.com/37682970/110816107-638d2380-82ce-11eb-9697-289fbc766a9f.png)


![Untitled 4](https://user-images.githubusercontent.com/37682970/110816133-6720aa80-82ce-11eb-8f27-58ae8d2b25bf.png)


현재 로드밸런서는 활성화된 가용 영역(서브넷)이 private subnet이다. private subnet은 igw와 연결되어있지 않으며, 로드밸런싱 대상은 public ip 또는 탄력적 ip가 할당되어 있지 않기 때문에 외부 트래픽을 다룰 수 없게 된다.

이를 해결하기 위해서는 **로드밸런서의 활성화된 가용영역(서브넷)을 public subnet으로 설정**해준다. public 서브넷은 외부와는 물론 내부 서브넷과 통신이 가능하므로 public subnet을 거쳐 로드밸런싱 대상으로 트래픽을 전달하면 쌍방으로 통신이 가능해지기 때문에 위와 같은 이슈를 해결 할 수 있다.

- 오프로드

웹서버가 아니라 ALB에 인증서를 넣는다는 것은  ALB가 SSL 처리 작업을 웹서버 대신 한다는 뜻입니다.

ALB와 EC2 사이에는 HTTP 통신만 하면 되며 웹서버 안에서 SSL 관련한 그 어떤 처리도 필요없습니다. 이걸 SSL Offload 라고 합니다.

- 가용 영역

먼저 로드밸런서 가용영역에 대해 알아보자. 로드밸런서 가용영역이란, 일반적인 우리가 알고 있는 AZ가 아닌 **로드밸런서가 접근할 수 있는 subnet을 의미** 한다. 이에 대해서는 다음 캡쳐본을 참고한다.
