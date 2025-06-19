## Route 53이란
AWS의 DNS(Domain Name System) 서비스로, 다음의 세 가지 주요 기능을 제공합니다:
- **도메인 등록**  
  Route 53에서 도메인을 직접 등록할 수 있으며, 등록 비용은 약 12,000원이고 최대 3일 정도 소요됩니다.
- **DNS 라우팅**  
  다양한 방식으로 DNS 쿼리를 처리할 수 있도록 라우팅 정책을 설정할 수 있습니다.
- **상태 체크 (Health Check)**  
  리소스의 상태를 모니터링하여 비정상일 경우 다른 리소스로 트래픽을 전환할 수 있습니다.
도메인을 등록한 후에는 AWS 리소스(EC2, ELB, S3 등)나 외부 서비스와 연결이 가능하며, **레코드 세트(Record Set)**를 생성하여 하위 도메인도 설정할 수 있습니다.

## Route 53의 라우팅 정책
- **Simple Routing**  
  기본 라우팅 방식으로 하나 이상의 IP 주소를 설정할 수 있고, 무작위로 반환됩니다.
- **Weighted Routing**  
  각 IP 주소에 가중치를 설정해 트래픽을 비율로 분산시킬 수 있습니다.
- **Latency-based Routing**  
  사용자와 가장 짧은 지연 시간을 가진 리전으로 쿼리를 전달합니다.
- **Failover Routing**  
  주(Main) 리소스와 보조(Secondary 또는 DR) 리소스를 설정하여, 주 리소스의 상태가 비정상일 경우 보조 리소스로 전환됩니다.
- **Geolocation Routing**  
  사용자 위치(국가, 대륙 등)를 기반으로 라우팅합니다.
- **Geoproximity Routing** (Traffic Flow 사용 시 가능)  
  사용자 위치와 리소스 간 거리 및 설정된 가중치 기반으로 트래픽을 분산합니다.
- **Multi-value Answer Routing**  
  Simple Routing과 유사하지만, 여러 IP 주소를 반환하며 Health Check와 연동되어 비정상 리소스를 제외할 수 있습니다.

## Alias 기능 (Route 53만의 기능)
Alias 레코드는 AWS 리소스에 직접 연결할 수 있도록 하며, 다음과 같은 리소스를 대상 도메인으로 사용할 수 있습니다:
- CloudFront Distribution
- ELB (Elastic Load Balancer)
- S3 Static Website Hosting
- Elastic Beanstalk
- VPC Interface Endpoint
Alias 레코드를 통해 별도의 IP 주소 없이도 AWS 리소스를 DNS에 등록할 수 있습니다.
