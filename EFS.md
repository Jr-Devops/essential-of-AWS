# EFS

[Amazon EFS](https://docs.aws.amazon.com/ko_kr/efs/latest/ug/whatisefs.html)는 **완전 관리형 파일 시스템**으로, 강한 일관성과 file locking이 필요한 경우 사용합니다.파일 추가/삭제에 따라 자동으로 용량이 확장/축소됩니다.

EBS가 단일 AZ에 데이터를 저장하는데 비해, EFS는 다중 AZ에 데이터를 저장하고, **다중 AZ의 EC2에서 동시 연결이 가능**합니다.