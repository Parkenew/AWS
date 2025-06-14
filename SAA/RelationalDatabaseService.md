# RDS (Relational Database Service)

## 1. 개요
- RDS는 관계형 데이터베이스(Relational Database)를 AWS 상에서 사용할 수 있도록 지원하는 서비스입니다.
- 생성 후 접속만 하면 사용할 수 있으므로 SaaS(Software as a Service)에 해당합니다.
- 지원 엔진: MySQL, MariaDB, PostgreSQL, Oracle, MS SQL, Aurora
- OS에 대한 직접적인 접근이나 Shell 사용은 불가능하며, AWS가 관리합니다.
- 백업, 소프트웨어 패치, 장애 감지 및 복구 등을 AWS가 자동으로 처리합니다.
- Storage 용량 자동 확장(Auto Scaling) 지원

---

## 2. DB Instance
- RDS에서 가장 기본적인 구성 요소이며, 클라우드에서 실행되는 격리된 데이터베이스 환경입니다.
- 하나의 인스턴스에 여러 개의 사용자 데이터베이스를 포함할 수 있습니다.
- 다양한 인스턴스 클래스 제공 (예: db.m5, db.r5 등)
- 단일 가용 영역(AZ)에 배포됨

---

## 3. DB Instance Storage
- EBS(Elastic Block Store)를 사용하여 데이터 저장
- 필요에 따라 자동으로 데이터를 여러 EBS 볼륨에 분산 저장

### 스토리지 유형
- **범용 SSD (gp2, gp3)**: 일반적인 워크로드에 적합
- **Provisioned IOPS (io1, io2)**: 고성능 I/O가 필요한 경우
- **마그네틱**: 낮은 빈도의 접근이 필요한 워크로드에 적합 (권장되지 않음)

---

## 4. Multi-AZ (고가용성 구성)
- 여러 AZ에 인스턴스를 배치하여 고가용성을 제공하는 기능
- Active-Standby 구조:
  - **Active**: 실제 트래픽 처리
  - **Standby**: 동기식 복제, 장애 발생 시 자동 전환
- 읽기 및 쓰기 작업은 Active 인스턴스에서만 가능
- Standby 인스턴스는 읽기 불가능
- 전환 시간: 약 60~120초

### 장애 전환 발생 조건
- 가용 영역(AZ) 중단
- DB 인스턴스 오류
- 인스턴스 유형 변경
- 소프트웨어 패치
- 재부팅(Failover)

### 주의사항
- 단일 AZ 구성보다 쓰기 및 저장 지연 시간이 증가할 수 있음

---

## 5. Read Replica
- 읽기 전용 복제본
- Master DB의 부하를 줄이기 위한 구성
- Snapshot 기반으로 복제본 생성
- **비동기** 복제 방식 사용
- Read Replica는 독립적인 DB 인스턴스로 승격 가능
- 리전 간 복제도 가능
- 최대 5개까지 생성 가능

### 지원 DB 엔진
- MySQL
- MariaDB
- PostgreSQL
- Oracle

