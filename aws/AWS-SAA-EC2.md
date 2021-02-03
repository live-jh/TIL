# AWS-SAA 학습 정리-2



## EC2

- ### **Elastic Compute Cloud**

- Provides resizable compute allowing you to quickly scale capacity 

  - 확장 가능한 resize 컴퓨팅 용량 제공

- Reduces the time required to obtain boot new server instances to minutes 

  - 새 인스턴스 가져오는데 분단위

- ### **On Demand**

  - pay a fixed rate by the hour with no commitment (인스턴스를 실행한 시간만큼 지불)
  - applications with short term spiky, or unpredictable workloads that can't be interrupted (중단 x, 예측 불가한 워크로드가 존재하는 애플리케이션에 적합)
  - applictions being developed or tested 

- ### **Reserved** 

  -  provides a capacity reservation, offer a significant discount on the hourly charge for an instance (용량 예약 가능 및 해당 인스턴스의 시간당 요금 할인, 기간은 1~3년)
  - steady state or predictable usage (예약은 안정된 상태 및 예측 가능할때 적합)
  - able to make upfront paymnets to reduce (비용 절감을 위한 선불결제)
    - `Standard`
      - offer up to 75%
      - the more upfront the longer the contract the greater the discount
    - `Convertible`
      - offer up to 54%
      - allows you to change between your different instance types
    - `Scheduled`
      - have scheduled reserved instances and these are available to launch within the time windows

- ### **Spot**  

  - enables you to bid whatever price youwant for instance capacity, providing for even greater savings if you applications have flexible start and end times (시작, 종료시간이 유연할시 큰 절감 효과, 효율적 비용)
  - flexible start and end times (유연한 시작, 끝시간)
  - only feasible at very low compute pirces (매우 낮은 컴퓨팅 가격으로 운용시)

- ### **Dedicated Hosts** 

  - physical EC2 server dedicated for your use (사용자 전용 물리 EC2 서버)
  - greate for licensing which doesn't support multi-tenancy or cloud deploy (클라우드를 배포 또는 테넌트 가상화 지원 안하는 라이센스에 적합)  
  - reservation for up to 70% the on-Demand pirce

## EC2 Instance Types

![image](https://user-images.githubusercontent.com/48043799/106768616-6cf7f000-667f-11eb-8853-681f46ce83e0.png)

> if the spot instance is terminated by amazon ec2, you will not be charged for a partial hour of usage. (ec2에 의해 종료되는 경우 요금 부과 x)
>
> however if u terminate the instance yourself, you will be charged for any hour in which the instance ran (유저가 직접 종료시 실행시간에 대한 요금 부과)



## EC2 Launch

- termination protection is **turned off by default**, you must turn it on (종료 방지 기능은 default -> off)
- on an EBS-backed instance, the **default action is for the root EBS** volume to be deleted when the instance is terminated (EBS 지원 인스턴스 default -> 인스턴스 종료시 root EBS 볼륨 삭제)
- EBS Root Volumes of Default AMI's **Can** be encrypted (AMI 루트 볼륨 암호화 가능)
- additional volumes can be encrypted (추가 볼륨 암호화 가능)

<br>

## References

- https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8
- https://www.udemy.com/course/aws-certified-solutions-architect-associate/learn/lecture/13886244#overview
- http://pyrasis.com/book/TheArtOfAmazonWebServices/Chapter1s