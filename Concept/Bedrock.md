# AWS Bedrock

## 개요
- **Amazon Bedrock**은 다양한 **생성형 AI 모델(Foundation Models, FMs)**을 **API 형태로 제공**하는 완전관리형 서비스입니다.
- 자체 모델을 만들 필요 없이, **다양한 기업의 대형 언어 모델(LLM)**을 애플리케이션에 쉽게 통합할 수 있습니다.
- AWS 인프라를 기반으로, **확장성**, **보안성**, **비용 효율성**을 갖춘 AI 서비스를 구축할 수 있습니다.

---

## 주요 특징

### 1. 다양한 모델 제공
- Anthropic의 **Claude**
- AI21 Labs의 **Jurassic-2**
- Stability AI의 **Stable Diffusion**
- Meta의 **Llama 3**
- Amazon 자체 모델인 **Titan 시리즈**  
→ 필요에 따라 가장 적합한 모델을 선택 가능

### 2. 서버 관리 불필요
- 인프라 구축 없이 **API 호출만으로** 모델을 사용
- 모델 훈련이나 파인튜닝 없이 바로 애플리케이션에 적용 가능

### 3. 사용자 정의 (Customization)
- 사용자의 자체 데이터를 기반으로 **RAG(Retrieval Augmented Generation)**이나 **파인 튜닝(fine-tuning)**을 통해 맞춤형 응답 생성 가능

### 4. 통합 및 보안
- 다른 AWS 서비스 (S3, Lambda, SageMaker 등)와 자연스럽게 통합
- IAM 기반 **접근 제어**, **로깅**, **모니터링** 등 AWS의 보안/운영 체계 활용 가능

---

## 사용 사례
- 챗봇, 문서 요약, 코드 생성, 이미지 생성, 고객 응대 자동화, 검색 향상 등  
→ 생성형 AI 기능을 **빠르고 안정적으로 제품에 통합**하고 싶은 경우 적합
