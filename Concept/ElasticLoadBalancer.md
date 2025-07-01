# Elastic Load Balancer(ELB)란?

AWS의 L4/L7 로드밸런싱(서버 부하 분산) 서비스로, 외부 또는 내부에서 들어오는 클라이언트의 요청을 가용영역(Availability Zone)에 있는 EC2 인스턴스로 고르게 나누어 전달하며, 대상 인스턴스의 상태를 검사하는 서비스입니다.

## ELB의 주요 특징
- 외부/내부 트래픽은 ELB의 DNS 주소를 목적지로 하여 유입
- 유입된 트래픽은 등록된 EC2 인스턴스로 전달
- 보안 그룹(Security Group) 적용 가능
- ELB는 VPC 내에 존재하며, 공인 IP 또는 사설 IP 사용 가능
- 인터넷 연결 옵션: 공인/사설 IP 생성
- 내부용 ELB 옵션: 사설 IP만 생성
- 기본적으로 지정한 포트(예: HTTP는 80번 포트)로 요청을 수신
- '리스너(Listener)'를 통해 요청 수신 및 처리
  - 리스너당 포트 하나 지정 (예: 80, 443)
- 상태 검사(Health Check) 성공한 인스턴스만 요청 전달 대상이 됨
- SSL 인증서를 등록해 HTTPS 통신 암호화/복호화 지원 (SSL Offload 가능)

## ELB 유형
- **ALB (Application Load Balancer)** : L7 계층
- **NLB (Network Load Balancer)** : L4 계층
- **CLB (Classic Load Balancer)** : 기존 유형


## Target Group(대상 그룹)
- ELB에 연결된 EC2 인스턴스들의 그룹
- 등록된 인스턴스 및 상태 검사 방식(HTTP, HTTPS, TCP 등) 정의
- 상태 검사 주기, Timeout 등도 설정 가능
- **ALB, NLB**는 Target Group 사용  
- **CLB**는 인스턴스를 로드밸런서에 직접 등록


## ALB (Application Load Balancer)
- L7 계층의 로드밸런서
- HTTP/HTTPS Header 기반으로 요청을 라우팅
- 경로 기반, 호스트 기반, Header 기반 라우팅 가능
- 정적 페이지 반환, 리다이렉트 처리 가능
- SSL 인증서를 통한 HTTPS 암호화 처리
- X-Forwarded-For 헤더로 클라이언트 IP 전달 가능
- 교차 영역 로드밸런싱(Cross-zone Load Balancing) 지원
- ALB의 공인 IP는 유동적

### 예시
- HTTP Header의 User-Agent에 'Android' 포함 시, Android 전용 대상 그룹으로 요청 전달


## NLB (Network Load Balancer)
- L4 계층의 로드밸런서
- TCP/UDP 기반 요청 전달
- 리스너: TCP, UDP, TCP/UDP 설정 가능
- 원본 IP/포트, 대상 IP/포트, TCP 시퀀스 번호 등으로 라우팅
- 단순히 TCP의 3-way handshake만 처리 (패킷 조작 없음)
- 클라이언트 IP를 변경하지 않고 EC2에 전달
- 고정 IP 주소 사용
- 교차 영역 로드밸런싱 지원

## CLB (Classic Load Balancer)
- L4/L7 모두 지원하는 과거형 로드밸런서
- EC2-Classic 네트워크에서 사용 가능
- TCP / HTTP / HTTPS(SSL) 프로토콜 지원
- X-Forwarded-For 헤더 지원
- 교차 영역 로드밸런싱 지원

## Sticky Session (세션 고정 기능)
- 사용자의 첫 요청이 특정 EC2 인스턴스에 전달되면, 이후의 요청도 해당 인스턴스로 계속 전달되는 기능
- 상태가 유지되어야 하는 로그인 세션 등에서 유용
- 쿠키 기반 또는 Application 기반 방식으로 설정 가능

## 교차 영역 로드밸런싱 (Cross-zone Load Balancing)
- 기본적으로 ELB는 AZ 단위로 요청을 분배 (예: AZ A 50%, AZ B 50%)
- 각 AZ 내 EC2 개수와 상관없이 동일 비율로 분배되면 특정 AZ에 부하가 집중될 수 있음
- **교차 영역 로드밸런싱을 활성화하면 EC2 개수를 기준으로 부하를 고르게 분배**
  - 예: AZ A에 EC2 2개, AZ B에 EC2 8개 → 비율 20:80으로 트래픽 분산


## X-Forwarded-For Header
- 클라이언트의 실제 IP 주소를 EC2 인스턴스에 전달하기 위한 HTTP Header
- ELB는 기본적으로 클라이언트 요청을 자체 IP로 전달하기 때문에, 원래 IP가 필요한 경우 해당 헤더 사용


## 연결 드레이닝 (Connection Draining)
- Auto Scaling 그룹과 함께 사용할 때 EC2 인스턴스가 제거되기 직전, 해당 인스턴스가 처리 중인 요청이 완료될 때까지 대기
- 무중단 배포 또는 Auto Scaling 종료 시 유용
- 일정 시간 동안 연결을 유지하여 사용자 요청의 손실을 방지

