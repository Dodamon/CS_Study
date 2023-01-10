## 관계형 데이터베이스(RDBMS)

![Untitled](https://user-images.githubusercontent.com/62232531/210686413-1440b678-57e5-49a6-91b3-79f4c449474b.png)

- **행과 열**을 가지는 **테이블** 형식 데이터를 저장하는 형태의 데이터베이스
- 명확하게 **정의 된 스키마**, **데이터 무결성 보장**

>  **스키마**?  
> 데이터베이스의 **구조**와 **제약조건**에 관해 **전반적인 명세**를 기술한 것  
> 속성(Attribute), 개체(Entity), 관계(Relation)에 대한 정의와 제약조건  



- **SQL 언어**를 써서 조작
    - DDL
    - DML
    - DCL
- MySQL, PostgreSQL, 오라클, SQL Server, MSSQL



## MySQL

- 대부분의 운영체제와 호환되며 현재 가장 많이 사용하는 데이터베이스
- B-트리 기반의 인덱스, 스레드 기반의 메모리 할당 시스템, 매우 빠른 조인
- 다양한 스토리지 엔진 지원



### PostgreSQL

- 디스크 조각이 차지하는 영역을 회수할 수 있는 장치인 VACUUM이 특징
- 최대 테이블 크기 32TB

## NoSQL 데이터베이스

- **Not Only SQL**
- **자유롭게 데이터 저장 가능**
- **대용량의 데이터**를 저장하기에 적합
- SQL을 사용하지 않는 데이터베이스
- **유연한 스키마**, 확장성이 특징

## NoSQL 종류

![Untitled](https://user-images.githubusercontent.com/62232531/210686785-d601b295-5e8d-4640-8a4c-2b7f2407677d.png)

**Document 기반**

![Untitled](https://user-images.githubusercontent.com/62232531/210686868-d4444a6a-38ef-43d1-9d19-aaeed1808ac1.png)



### MongoDB

- **도큐먼트** 기반의 데이터베이스
- **JSON**을 통해 데이터 접근 가능
- Binary JSON(BSON)로 데이터 저장
- **확장성**이 뛰어남
- **type변환**이 일어나지 않음!
- 빅데이터를 저장할 때 성능이 좋음(고가용성, 샤딩, 레플리카셋 지원)
- 도큐먼트 **생성시** 마다 중복된 값을 지니기 힘든 유니크한 값 **ObjectID** 생성
    - 타임스탬프(4), 랜덤값(5), 카운터(3)

![image](https://user-images.githubusercontent.com/62232531/210686978-0818d49d-82a0-4361-ada9-4a59e4bd343a.png)

**Key-Value 기반**

![Untitled](https://user-images.githubusercontent.com/62232531/210686907-a7cf3738-151c-46f9-8157-ffe35d5c29d3.png)

- **Key**와 **Value**의 쌍으로 데이터가 저장되는 가장 단순한 형태의 솔루션

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/10776257-5b7e-42c4-a6a4-48382cefc84f/Untitled.png)

### **Redis**

- **인메모리** 데이터베이스
- 데이터타입은 **문자열(String)**
- push/sub 기능을 통해 채팅 시스템, 세션 정보 관리, 실시간 순위표 등에 사용

**Wide-Column 기반**

- Big Table DB라고도 불림
- 대량의 데이터를 신속하게 수집해서 분석가능
- HBase, Cassandra


> 💡 **Cassandra?**  
> JVM기반 DB Engine



**Graph 기반**

![image](https://user-images.githubusercontent.com/62232531/210687084-6e400000-3531-48ed-87c3-e4be7f8de519.png)


- **데이터간 관계**를 탐색하는데 특화
- Neo4j