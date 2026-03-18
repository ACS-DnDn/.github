# 든든 (DnDn) 🛡️

> **CloudTrail · AWS SDK · IaC 자동화 기반의 실무형 DevOps 플랫폼**
>
> 고객 AWS 계정의 변경 흐름을 실시간으로 추적하고, 보안·비용 검증이 완료된 Terraform Revision을 자동 생성하는 통합 클라우드 관리 대시보드입니다.

---

## 📌 프로젝트 개요

**든든(DnDn)** 은 여러 고객의 AWS 계정을 단일 플랫폼에서 통합 관리하는 DevOps 솔루션입니다.

- **CloudTrail** 로 인프라 변경 이력을 추적합니다.
- **AWS Security Hub · Config · Health** 이벤트를 수집·정규화하여 보고서를 자동 생성합니다.
- **IaC(Terraform)** Revision을 보안·비용 검증 후 자동으로 제안합니다.
- HR 포털을 통해 멤버(member) · 리더(leader) · HR 담당자 권한을 분리하여 운영합니다.

---

## 🏗️ 전체 아키텍처

```
┌─────────────────────────────────────────────────────────────────┐
│                        고객 AWS 계정                              │
│  CloudTrail · Config · Security Hub · Health  →  EventBridge    │
└───────────────────────────┬─────────────────────────────────────┘
                            │  크로스 계정 이벤트 전달
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      DnDn 플랫폼 계정                              │
│                                                                 │
│  EventBridge Bus                                                │
│       ├─ Worker Lambda   → SQS → 보고서 생성 트리거                 │
│       ├─ Finding Lambda  → S3  → Security Hub 이벤트 정규화        │
│       └─ Health Lambda   → S3  → AWS Health 이벤트 정규화          │
│                                                                 │
│  API Server (NestJS)  ←─────────────────  Web Dashboard (React) │
│       └─ MariaDB · S3 · SQS                                     │
│                                                                 │
│  Report Service ─ AI 기반 보고서 자동 생성                           │
│  HR Portal      ─ 멤버·리더·HR 권한 관리                            │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📂 레포지토리 구성

| 레포지토리 | 설명 | 주요 기술 |
|---|---|---|
| [**DnDn-App**](https://github.com/ACS-DnDn/DnDn-App) | 플랫폼 메인 애플리케이션 모노레포 (`web` · `api` · `worker` · `report`) | TypeScript · React · NestJS |
| [**DnDn-Infra**](https://github.com/ACS-DnDn/DnDn-Infra) | AWS 연동 인프라 자산 (CloudFormation · Lambda 이벤트 파이프라인) | HCL · Python · CloudFormation |
| [**DnDn-HR**](https://github.com/ACS-DnDn/DnDn-HR) | HR 포털 (멤버·리더·HR 권한 관리 웹 앱) | TypeScript · React · Vite |
| [**.github**](https://github.com/ACS-DnDn/.github) | 조직 프로필 및 공통 설정 | — |

### DnDn-App 내부 구조

```
DnDn-App/
├─ apps/
│  ├─ web/       # React 기반 통합 대시보드 프론트엔드
│  ├─ api/       # NestJS 기반 백엔드 API 서버
│  ├─ worker/    # SQS 소비 · 보고서 생성 트리거 워커
│  └─ report/    # AI 기반 보고서 자동 생성 서비스
└─ contracts/    # 공유 타입 및 DTO 정의
```

### DnDn-Infra 내부 구조

```
DnDn-Infra/
├─ cloudformation/
│  ├─ dndn-ops-agent-role.yaml      # 고객 계정 크로스 계정 롤
│  ├─ dndn-platform-eventbus.yaml   # 플랫폼 EventBridge Bus
│  └─ cognito-userpool.yaml         # Cognito 인증 (hr · leader · member)
└─ lambda/
   └─ event-enricher/               # 이벤트 정규화 Lambda
      ├─ event_router.py
      ├─ finding_enricher.py
      └─ health_enricher.py
```

---

## 🛠️ 기술 스택

| 영역 | 기술 |
|---|---|
| **프론트엔드** | React · TypeScript · Vite |
| **백엔드** | NestJS · TypeScript |
| **인프라** | AWS CloudFormation · EventBridge · Lambda · S3 · SQS · Cognito |
| **데이터베이스** | MariaDB |
| **IaC** | Terraform · HCL |
| **인증** | AWS Cognito (hr · leader · member 그룹) |

---

## 👥 팀

**ACS-DnDn** 팀이 개발하고 있습니다.
