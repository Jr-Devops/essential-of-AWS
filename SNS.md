# Amazon SNS

---

[Amazon SNS](https://docs.aws.amazon.com/ko_kr/sns/latest/dg/welcome.html)는 Pub/Sub 구조(구독자와 생산자 구조)에서 사용되는 메시지 전송 관리형 서비스입니다.주로 SQS와 비교되는 서비스로 많이 나오는데, 다음과 같은 차이점만 잘 알아두시면 됩니다.

SQS는 메시지 처리 주체가 메시지를 가져와서 소비하는 `폴링` 모델인 반면에, SNS는 다수의 구독자들에게 `푸시` 매커니즘으로 메시지를 보내는 서비스입니다. 구독 주제에 관해서만 업데이트 받습니다.
