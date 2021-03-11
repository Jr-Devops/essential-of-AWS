# CloudFront

[CloutFront](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)는 HTML, CSS, JS 및 이미지 파일과 같은 정적 및 동적 웹 콘텐츠를 사용자에게 더 빨리 배포하도록 지원하는 CDN(Content delivery network) 웹 서비스입니다.보통 S3와의 조합으로 많이 사용하는데요, S3에 정적 오리진 데이터를 넣어놓고 CloudFront를 연결한 후 글로벌한 CloudFront 엣지 로케이션에 배포해서 각 지역에 맞는 빠른 액세스 전략을 가져갈 수 있습니다.각 지역에서는 오리진 데이터가 있는 S3까지 오지 않아도, 끝단인 CloudFront에 미리 캐싱되어 있는 데이터를 빠르게 받아볼 수 있기 때문에 사용자 지연 시간을 처리하기 위해 많이 사용하는 전략 중 하나입니다.

### Signed URL & Signed Cookies

CloudFront를 통해 특정 경로, 인증를 통해서만 private한 데이터에 액세스할 수 있도록 권한을 제어할 수 있는데요.아래 두 가지 방법이 많이 쓰이므로, 차이를 잘 알아두는 것이 좋습니다.

- Signed URL
    - RTMP(Real Time Message Protocol)을 지원합니다.
    - 개별 파일마다 제한 조건을 둘 수 있습니다.
- Signed Cookies
    - RTMP(Real Time Message Protocol)을 지원하지 않습니다.
    - 여러 파일들에 한꺼번에 제한 조건을 걸 수 있습니다.
    - 현재 제공되고 있는 URL을 변경하지 않아도 됩니다.

### Origin Access Identity (OAI)

Amazon S3 버킷을 CloudFront 배포의 오리진으로 처음 설정하면 모든 사용자에게 버킷의 파일에 대한 권한을 부여하게 됩니다.이렇게 되면 누구나 CloudFront URL 혹은 S3 URL 두 경로 모두로 파일에 접근할 수 있게 됩니다.

이 상황에서, CloudFront의 Signed URL 또는 Signed Cookies를 사용하여 Amazon S3 버킷의 파일에 대한 액세스를 제한하는 경우에는, Amazon S3 URL로 Amazon S3 파일에 직접 액세스하는 사용자도 차단하는 것이 좋습니다.서명된 URL, 서명된 쿠키와 관계 없이 S3 URL을 가지고 있다면 해당 파일에 바로 접근할 수 있기 때문입니다.

따라서 OAI라는 것을 만들어 다음과 같은 작업을 수행해야 합니다.

- 특별한 CloudFront 사용자인 OAI를 만들고 CloudFront 배포와 연결합니다.
- OAI만 읽기 권한을 가지도록 Amazon S3 버킷 권한을 변경합니다.
    - S3 URL로 파일을 읽을 수 없게 됩니다.

### Lambda@Edge

Lambda@Edge를 사용하면 서버를 프로비저닝하거나 관리하지 않고 글로벌 AWS 엣지 로케이션에서 코드를 실행할 수 있으므로 네트워크 지연 시간을 최소화하여 최종 사용자에게 바로 응답할 수 있습니다.Node.js나 Python 코드를 AWS Lambda에 업로드하고 Amazon CloudFront 요청에 대한 응답으로 함수가 트리거되도록 구성하면 됩니다.

![Untitled](https://user-images.githubusercontent.com/37682970/110817466-ac91a780-82cf-11eb-8762-93da2ecf7036.png)

![Untitled 1](https://user-images.githubusercontent.com/37682970/110817482-b0bdc500-82cf-11eb-9445-7ca935e9234c.png)


- 원본을 별도 계층. 즉 중간 도메인으로 관리하는 귀찮음을 겪지 않아도 됩니다
- DNS 레벨의 Health Check에 시달리지 않아도 됩니다. + Health Check하는 계층에게 접근권한을 주지 않아도 됩니다
- 위 Health Check에서 체크하는 경로와 실제 요청하는 객체의 상태-불일치 문제가 발생하지 않습니다
- DNS 레벨에서 Failover가 발생한 이후의 롤백을 고민하지 않아도 됩니다. 객체 단위므로 다른 객체는 다시 Primary부터 요청합니다.

![Untitled 2](https://user-images.githubusercontent.com/37682970/110817499-b3b8b580-82cf-11eb-8578-f95a8564a74e.png)

![Untitled 3](https://user-images.githubusercontent.com/37682970/110817503-b61b0f80-82cf-11eb-85c2-b6185a128ab3.png)


Amazon CloudFront를 사용하면 콘텐츠, API 또는 애플리케이션을 **SSL/TLS**를 통해 전송 가능함.

- **AWS Certificate Manager(ACM)**를 사용하여 사용자 지정 **SSL 인증서**를 손쉽게 생성하여 CloudFront 배포에 무료로 배포 가능함.
