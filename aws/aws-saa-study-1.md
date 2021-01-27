#  AWS-SAA 학습 정리-1



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

aws에서 가장 오래된 서비스이며 글로벌 서비스로 지역을 선택하지 않아도 이용 가능합니다.

- 안전하고 가변적 object 저장공간 제공(간단한 웹 서비스 인터페이스)
- 편리한 UI를 통해 쉽게 데이터를 저장하고 조회 가능
- 파일 크기는 0~5TB까지 지원
- 저장공간은 무제한
- Bucket이라는 이름을 사용하며 보편적 namepace를 사용

### S3의 Object 구성요소

- Key: 객체의 이름
- Value: 파일 자체의 데이터
- Version ID: 버전관리에 필요한 버전 ID (S3의 고유 특징)
- Metadata: 데이터의 데이터
- CORS(Cross Origin Resource Sharing): 한 버킷의 파일을 다른 버킷에서 접근 가능하도록 하는 기능(다른 지역에서도 가능)

### Consistency Model

Read After Write Consistency(PUT)

- S3파일 업로드시 PUT을 사용, S3 버킷에 **업로드한다면 즉시 바로 사용**할 수 있다는 의미

Eventual Consistency(UPDATE, DELETE)

- 버킷의 내용을 **수정하거나 삭제할시** 결과가 바로 나타나지 않는 기능

### S3 생성

1. aws 서비스 -> S3 클릭 -> 버킷 만들기 버튼
2. 일반 구성
   1. 버킷 이름 작성
   2. 리전(지역) 선택(기존 버킷 설정을 복사할 수 있음)
3. 퍼블릭 액세스 차단을 위한 버킷 설정
   1. default 설정만으로도 보안 최적화
4. 버전관리
   1. 활성화 및 비활성화
5. 태그 관리
6. 서버측 암호화
7. 생성 버튼

![image](https://user-images.githubusercontent.com/48043799/106003432-8afca800-60f5-11eb-8acd-c271a1234865.png)

> S3 생성 완료 :)



## S3 상세페이지

![image](https://user-images.githubusercontent.com/48043799/106003565-a9fb3a00-60f5-11eb-8652-cf746fd7ec7d.png)

<br>

### 객체 : 버킷에 넣을 파일

- 업로드 -> 파일 추가 -> 업로드버튼 -> 나가기 -> 객체 리스트 확인

![image-20210127231905625](/Users/lj/Library/Application Support/typora-user-images/image-20210127231905625.png)

이후 위에 업로드된 파일을 누르면 아래와 같이 S3에 업로드된 파일에 대한 정보를 확인할 수 있습니다.

객체 URL을 통해 파일을 확인할 수 있지만 경로로 이동하게 되면 AccessDenied라는 에러를 띄우게 됩니다. 이는 접근 금지에 대한 에러이며 파일에 접근할 권한이 없기때문에 발생하는 현상입니다. 

![image](https://user-images.githubusercontent.com/48043799/106005156-440fb200-60f7-11eb-844c-d7ecf6e70127.png)

해당 파일의 상세페이지에 접근하여 객체 작업 -> 퍼블릭으로 설정으로 제한을 풀면 되지만 이는 버킷의 권한이 모든 부분에서 차단되었기에 해당 권한을 우선적으로 설정을 변경해줘야합니다.

**권한** 탭을 눌러 편집 -> 모든 퍼블릭 액세스 차단을 체크해제 후 -> 저장

이후 다시금 업로드된 파일 상세페이지에 접근하여 객체 작업 -> 퍼블릭으로 설정 -> 이후 객체 URL에 접근한다면 정상적으로 이미지가 보이는 것을 확인할 수 있습니다.

### S3의 버킷 특징

버킷은 동일한 이름을 가질 수 없는 특징을 가지고 있습니다. 따라서 고유한 이름을 지정해야하며 버킷 자체는 글로벌하게 조회가 가능하지만 실제 버킷을 생성시 개별 지역에서 교차를 통해 한쪽 버킷의 파일 및 데이터를 다른 버킷에 자동으로 복제할 수도 있습니다.

예를 들어 서울지역의 S3 버킷을 미국 버지니아 북부의 S3로 복제할 수 있으며 동시에 객체의 스토리지클래스 및 암호화도 수정이 가능합니다. 이를 CORS의 예로 들 수 있습니다.

마지막으로 버킷 액세스를 제한하는 방법으로는 총 3가지가 있습니다.

- 버킷 정책 사용 (전체 버킷에 적용)
- 객체 정책을 사용하여 제어
- 계정내 사용자 그룹의 정책으로 제어



## References

- https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8
- https://www.udemy.com/course/aws-certified-solutions-architect-associate/learn/lecture/13886244#overview
- http://pyrasis.com/book/TheArtOfAmazonWebServices/Chapter16