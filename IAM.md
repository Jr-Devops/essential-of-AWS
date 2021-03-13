# IAM
이론 정리 (https://musma.github.io/2019/11/05/about-aws-iam-policy.html)
## UBAC (User-Based Access Control)
```
USER A : Run 가능
USER B : Walk 가능
USER C : Run 가능
USER D : Walk 가능
```
사용자 하나하나 마다 권한을 부여한다. 단순하지만 권한과의 관계나 연관성을 표현할 수 없다.
## GBAC (Group-Based Access Control)
```
Java반 : Java 공부 가능
c반 : c 공부 가능
c++반 : c 공부 가능, c++ 공부가능
영재반 : Java 공부가능, c공부가능 , c++ 공부가능
USER A : Java반
USER B : Java반, c++반
USER C : c반, 영재반
USER D : c반
```
그룹의 권한이 다양해질 경우 그룹은 점점 복잡하게 되고 그룹으로 조건을 거는것이 어려울 수 있다.
-> Role의 등장

## RBAC (Role-Based Access Control)
그룹에 바로 권한을 주는 대신 권한의 논리적 집합을 만들고 역할을 그룹이나 사용자에 적용한다.
Group은 보통 사용자들의 그룹을 묶는데 사용하고 Role은 권한의 Group처럼 사용한다.
```
그룹
관리자그룹
사용자그룹
게스트그룹

역할
a를 할 수 있는 역할
b를 할 수 있는 역할
```
그룹에 사용자를 묶고 역할에 권한을 매칭하면서 효과적으로 권한을 부여할 수 있다.  

## ABAC (Attribute-Based Access Control)
![image](https://user-images.githubusercontent.com/37682970/110927858-81f42d00-8369-11eb-8134-abe98bebb82f.png)

7내무반의 역할은 병기관리, 탄약관리인데 간부인 중사는 탄약고 개방이 허용되고 일병은 병사라서 탄약고 개방이 불가능 한 경우를 생각.
-> 기존 역할 + 추가로 속성에 따라 달리 적용해야한다.

![image](https://user-images.githubusercontent.com/37682970/110928443-39893f00-836a-11eb-8ef2-db4db3280cfd.png)

추가로 권한을 부여하는 형식인 Policy를 통해 기존 구조를 유지하면서 권한을 부여할 수 있다.
## 권한위임

Production계정(나)에서 Development계정(신뢰할수 있는 계정)을 등록 읽기,쓰기등의 엑세스 권한을 줌
