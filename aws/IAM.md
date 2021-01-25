# IAM

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





## References

- https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8
- https://www.udemy.com/course/aws-certified-solutions-architect-associate/learn/lecture/13886244#overview
- http://pyrasis.com/book/TheArtOfAmazonWebServices/Chapter16