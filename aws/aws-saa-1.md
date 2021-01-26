# AWS-SAA 학습 정리-1



## IAM

Identiyu and Access Management(식별 및 접근관리)라는 뜻으로 사용자 또는 사용자별 그룹을 생성하여 AWS의 각 리소스(자원)에 대해 제어 및 권한 관리를 제공하는 것을 말합니다. IAM은 실제 서비스를 제공하는 AWS 자체 리소스가 아니라 사용 요금을 발생하지 않습니다.

IAM의 기능을 예로 들자면 A라는 IAM의 설정된 사용자는 EC2만 관리할 수 있거나, 다른 AIM의 사용자는 S3의 내용만 읽을 수 있도록 구성할 수 있습니다. 개인 사용자와 달리 사용자 그룹은 동일한 권한을 여러 IAM 사용자에게 부여할 때 사용합니다.

![te](https://user-images.githubusercontent.com/48043799/105719346-87d2b200-5f65-11eb-8b14-45de1b8fe7b7.png)

> IAM 개념 설명 이미지



## IAM 용어

- Group: 그룹
  - 하나의 그룹은 하나 또는 다수의 유저가 존재
- User: 사용자
- Role: 역할
  - 하나 또는 다수의 정책 지정
- Policy: 정책
  - json으로 이루어진 document이며, 세밀한 접근 권한을 지정
  - 정책은 그룹, 역할에 추가 가능



## Cloud watch

AWS 리소스 사용의 실시간 모니터링을 지원합니다.

모니터링 및 감시하는 방법으로 청구에 따른 경보를 지정할 수 있으며, 다양한 이벤트를 로그파일로 저장할 수 있으며 경보 설정을 통해 SNS, lambda로 전송이 가능합니다.

CloudWatch를 사용 가능한 서비스로는 EC2, RDS, S3, ELB등이 있습니다.

알람은 지역을 버지니아 북부로 설정해야 지정이 가능하며 다음과 같이 설정할 수 있습니다.

- CloudWatch -> Billing -> Create alarm button -> Select metric -> 모든지표 탭 -> 예상 요금 합계 -> 통화선택

### 모니터링 종류

- basic -> default
  - 무료
  - 5분 간격 지표 제공
- detailed
  - 유료
  - 1분 간격 지표 제공

사용하는 예로는 다음과 같습니다.

- use case: 매일 얼마나 많은 사용자들이 서비스를 사용하는지 
- potential issue: 특정한 날에 많은 트래픽이 몰려 병목현상이 생길 수 있는 부분을 예측
- solution: 매일 평균 트래픽과 특정 버튼의 평균 유저의 클릭 횟수를 분석하여 효율적인 서버관리가 가능

### 조건

정적(static) 또는 대역(이상탐지)을 사용할지 유형을 지정할 수 있습니다. 해당 조건에 따른 비교연산자를 통해 추가할 수 있습니다.

<img width="766" alt="스크린샷 2021-01-26 오후 11 26 52" src="https://user-images.githubusercontent.com/48043799/105857810-fda25080-602d-11eb-8b10-296364c60487.png">

> 이후 다음화면에서 알람의 이름과 설명을 입력하면 프리뷰를 확인할 수 있고
>
> 마지막으로 생성버튼을 누르면 알람 생성이 완료됩니다 :)

![image](https://user-images.githubusercontent.com/48043799/105858302-7ef9e300-602e-11eb-8457-7f9b9c1f3083.png)



## S3(Simple Storage Service)

aws에서 가장 오래된 서비스

- 안전하고 가변적 object 저장공간 제공(간단한 웹 서비스 인터페이스)
- 편리한 UI를 통해 쉽게 데이터를 저장하고 조회 가능
- 파일 크기는 0~5TB까지 지원
- 저장공간은 무제한
- Bucket이라는 이름을 사용하며 보편적 namepace를 사용





## References

- https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8
- https://www.udemy.com/course/aws-certified-solutions-architect-associate/learn/lecture/13886244#overview
- http://pyrasis.com/book/TheArtOfAmazonWebServices/Chapter16