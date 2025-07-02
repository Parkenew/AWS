# AWS CloudFront 정리

## 1. CloudFront란?

**CloudFront**는 AWS에서 제공하는 **CDN (Content Delivery Network)** 서비스입니다.

- 전 세계에 분산된 **Edge Location(캐시 서버)** 을 통해 콘텐츠를 빠르게 제공
- **정적 콘텐츠**(이미지, HTML, JS, CSS 등)를 캐싱하여 성능 향상
- **S3, EC2, ELB**, 외부 서버 등 다양한 소스를 **Origin Server**로 설정 가능
- SSL, 인증, 접속 차단 등 **보안 기능 제공**

---

## 2. Edge Location이란?

- CloudFront의 **전 세계 캐시 서버** 위치
- 사용자 가까운 곳에서 응답하여 **지연(latency)을 최소화**
- 오리진 서버의 부하를 줄이고 성능을 향상시킴
- 사용자가 요청 → 가장 가까운 Edge Location에서 응답

---

## 3. 콘텐츠 제공 흐름

1. 사용자가 웹 또는 앱에서 콘텐츠 요청
2. DNS가 사용자 요청을 **가장 가까운 Edge Location**으로 전달
3. 해당 Edge Location에 **캐시된 콘텐츠가 있으면 바로 전달**
4. 없다면 → **Origin Server**에서 가져와 캐시에 저장 후 응답

---

## 4. CloudFront의 주요 기능

| 기능 | 설명 |
|------|------|
| CDN | 전 세계 엣지 서버에서 정적 콘텐츠 캐싱 |
| 캐싱 | TTL(Time to Live) 설정으로 캐시 유효 시간 조절 |
| 보안 제어 | WAF, HTTPS, IP 차단, 국가 차단 등 |
| 접근 제어 | OAI, OAC, Signed URL 등으로 접근 통제 |
| Custom Origin | S3 외에도 EC2, ELB, 외부 웹 서버도 Origin 가능 |

---

## 5. OAI (Origin Access Identity)

- S3 버킷을 **퍼블릭으로 설정하지 않고** CloudFront를 통해서만 접근 가능하도록 설정
- CloudFront가 **S3에 접근할 수 있는 가상의 IAM 사용자** 역할을 부여
- **S3는 퍼블릭 접근 차단** + OAI에게만 `s3:GetObject` 허용

### S3 버킷 정책 예시
```json
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity EXAMPLE"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```
---

## 6. Presigned URL
- S3 또는 CloudFront 콘텐츠에 일시적으로 접근할 수 있는 URL 생성 방식
- URL에 만료 시간, 서명 정보 포함
- 보안이 필요한 다운로드/스트리밍에 유용

### CloudFront Presigned URL 구성 요소
- CloudFront Key Pair
- 서명 정책 (IP 제한, 만료 시간 등)
- 서명된 URL로만 접근 가능
