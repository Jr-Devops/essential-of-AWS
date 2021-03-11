# Kinesis

[Amazon Kinesis Data Streams](Kinesis%20515a30e60a004f54bd2d866530183399/Amazon%20Kinesis%20Data%20Streams%20a9fa4c21d679435cbe8264c42f603a31.md)

[Amazon Kinesis Data Analytic](Kinesis%20515a30e60a004f54bd2d866530183399/Amazon%20Kinesis%20Data%20Analytic%20d4256e9e198e44b795491a8422ddf9c3.md)

[Amazon Kinesis](https://aws.amazon.com/ko/kinesis/)는 실시간 비디오, 실시간 스트리밍 데이터를 수집, 처리, 분석할 수 있는 서비스입니다.하위 기능으로 다음 4가지의 서비스가 제공되는데, 시험에는 주로 Kinesis Data Streams가 많이 나옵니다.

- Kinesis Video Streams
    - 머신러닝, 분석, 재생 등을 위해 비디오를 스트리밍하는 서비스입니다.
- Kinesis Data Streams
    - 고도로 확장 가능하고 내구력 있는 실시간 데이터 스트리밍 서비스입니다.
    - 웹 사이트 클릭스트림, 데이터베이스 이벤트 스트림, 금융 트랜잭션, 소셜 미디어 피드, IT 로그 및 위치 추적 이벤트 등
    - Amazon S3, AWS Lambda에 제공할 수 있습니다.
    - 기본적으로는 데이터를 24시간 저장하고, 최대 168시간까지 저장할 수 있습니다.
    - 파티션 방식이 아닌 샤딩 방식입니다.
- Kinesis Data Firehose
    - 스트리밍 데이터를 데이터 레이크, 데이터 스토어 및 분석 서비스에 안정적으로 로드할 수 있는 서비스입니다.
    - Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, 일반 HTTP 엔드포인트 및 Datadog, New Relic, MongoDB, Splunk와 같은 서비스 공급자로 전송할 수 있습니다.
- Kinesis Data Analytics
    - 데이터 스트림 처리를 위한 오픈 소스 프레임워크 서버리스 엔진인 Apache Flink를 통해 실시간으로 스트리밍 데이터를 변환 및 분석하는 서비스입니다.