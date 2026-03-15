# GrowTechStack

> 국내 IT 기업 기술 블로그의 최신 글을 자동으로 수집·요약하여 한 곳에서 볼 수 있는 기술 콘텐츠 큐레이션 플랫폼

<img width="4564" height="1934" alt="image" src="https://github.com/user-attachments/assets/7d720388-9c67-40f3-8b2a-14bc081359e7" />

🌐 **[growtechstack.com](https://growtechstack.com)**

---

## 서비스 소개

국내 IT 기업 기술 블로그의 RSS 피드를 자동으로 수집하고,
Groq AI (Llama 3.1 8B)를 통해 핵심 내용을 3줄로 요약하여 제공합니다.

---

## 아키텍처

```
[ 사용자 브라우저 ]
        │
        ▼
[ Vercel ] ── React Frontend (growtechstack.com)
        │
        ▼ /api/*
[ Nginx + HTTPS ] ── AWS EC2
        │
        ▼
[ API Gateway :8080 ] ── Spring Cloud Gateway
        │ (Eureka 서비스 디스커버리)
        ├──────────────────────┬──────────────────────┐
        ▼                      ▼                      ▼
[ Collector :29999 ]   [ AI Summary :29998 ]   [ Auth :29997 ]
        │   │                  │
        │   └─── Kafka ────────┘
        │         (Confluent Cloud)
        │
        └── AWS RDS (MySQL 8.0)

[ Eureka Server :8761 ] ── 서비스 등록/디스커버리
```

---

## 레포지토리

| 레포지토리 | 설명 | 기술 |
|------------|------|------|
| [gts-frontend](../gts-frontend) | React 웹 프론트엔드 | React 19, Vite 7, Tailwind CSS 4, TanStack Query |
| [gts-gateway](../gts-gateway) | API 게이트웨이 | Spring Boot 3.3, Spring Cloud Gateway |
| [gts-collector-service](../gts-collector-service) | RSS 수집 및 콘텐츠 API | Spring Boot 3.5, JPA, Kafka, Rome |
| [gts-ai-summary-service](../gts-ai-summary-service) | AI 요약 처리 | Spring Boot 3.3, Spring AI, Groq, Kafka |
| [gts-auth-service](../gts-auth-service) | 인증 및 회원 관리 | Spring Boot 3.3, Spring Security, JWT, JPA |
| [gts-eureka-server](../gts-eureka-server) | 서비스 디스커버리 | Spring Boot 3.3, Netflix Eureka |
| [gts-infra](../gts-infra) | 인프라 및 배포 설정 | Terraform, Docker Compose, Nginx |

---

## 기술 스택

**Backend**
- Java 17 / Spring Boot 3.x
- Spring Cloud Gateway, Netflix Eureka
- Spring Data JPA / MySQL (AWS RDS)
- Apache Kafka (Confluent Cloud)
- Spring AI + Groq API (Llama 3.1 8B Instant)

**Frontend**
- React 19 / TypeScript / Vite 7
- Tailwind CSS v4
- TanStack Query v5 / Zustand v5 / React Router v7

**Infra**
- AWS EC2, RDS, ECR (ap-northeast-2)
- Terraform (IaC)
- Docker / Docker Compose
- Nginx + Let's Encrypt (HTTPS)
- GitHub Actions (CI/CD)
- Vercel (Frontend 배포)

---

## CI/CD

`main` 브랜치 push 시 자동 배포됩니다.

| 서비스 | 배포 방식 |
|--------|-----------|
| Frontend | GitHub Actions → Vercel |
| Gateway | GitHub Actions → ECR → EC2 |
| Collector | GitHub Actions → ECR → EC2 |
| AI Summary | GitHub Actions → ECR → EC2 |
| Auth | GitHub Actions → ECR → EC2 |
| Eureka | GitHub Actions → ECR → EC2 |
