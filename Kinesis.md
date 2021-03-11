# Kinesis

### Amazon Kinesis Data Streams

대규모 데이터 기록을 실시간으로 수집하고 처리할 수 있음. (로그, 이벤트 수집기)

### Amazon Kinesis Data Analytic

스트리밍 데이터 분석기, SQL을 통해 실시간 분석, 실시간 대시보등, 실시간 지표등을 생성할 수 있음.

- JSON, CSV파일을 Input으로 받아처리
- Lambda 함수를 통해 데이터를 사전처리
- 입력스트림은 병렬화되어 처리량이 크다

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
