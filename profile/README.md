# 든든 (DnDn)

> AWS 운영 이벤트를 수집하고, 사람이 바로 판단할 수 있는 보고서와 실행 계획으로 연결하는 클라우드 운영 플랫폼입니다.

든든은 여러 AWS 계정에서 발생하는 변경 이력과 운영 신호를 한곳에 모아,
대시보드, 문서, 보고서, 실행 가능한 IaC 초안까지 이어지도록 설계된 DevOps 프로젝트입니다.

## 우리가 해결하려는 문제

클라우드 운영에서는 변경이 어디서 발생했고, 어떤 영향이 있었는지, 다음 조치를 무엇으로 잡아야 하는지 파악하는 데 시간이 많이 듭니다.

든든은 이 과정을 더 짧고 명확하게 만들기 위해 아래 흐름을 만듭니다.

- AWS 계정의 변경 이력과 이벤트를 수집합니다.
- CloudTrail, AWS Config, Security Hub, AWS Health 기반 신호를 표준 포맷으로 정리합니다.
- 운영자가 바로 읽을 수 있는 대시보드와 문서 화면을 제공합니다.
- AI 기반 보고서와 Terraform 초안을 생성해 후속 조치를 빠르게 이어갈 수 있게 합니다.
- HR 포털로 사용자와 부서 정보를 관리해 운영 권한 체계를 분리합니다.

## 플랫폼 흐름

```text
고객 AWS 계정
  -> CloudTrail / Config / Security Hub / AWS Health
  -> Worker 수집 및 정규화
  -> S3 / MariaDB 저장
  -> API / Report 서비스
  -> Dashboard / Documents / Terraform Draft
```

## Repositories

| Repository | 역할 | 주요 기술 |
| --- | --- | --- |
| [DnDn-App](https://github.com/ACS-DnDn/DnDn-App) | 메인 애플리케이션 모노레포. 웹 대시보드, API, AWS 수집 Worker, 보고서 생성 서비스를 포함합니다. | React, TypeScript, FastAPI, Python, MariaDB |
| [DnDn-Infra](https://github.com/ACS-DnDn/DnDn-Infra) | AWS 인프라 코드, 고객 계정 온보딩 템플릿, Lambda, Terraform, Argo CD 기반 GitOps 구성을 관리합니다. | Terraform, CloudFormation, AWS Lambda, Argo CD |
| [DnDn-HR](https://github.com/ACS-DnDn/DnDn-HR) | 사용자, 부서, 권한 운영을 위한 HR 포털 프론트엔드입니다. | React, TypeScript, Vite |
| [.github](https://github.com/ACS-DnDn/.github) | 조직 프로필과 공통 GitHub 설정을 관리합니다. | Markdown, GitHub |

## DnDn-App 구성

`DnDn-App`은 현재 아래 구성으로 나뉘어 있습니다.

| 경로 | 설명 |
| --- | --- |
| `apps/web` | 운영 대시보드, 문서 뷰어, 워크스페이스 화면 |
| `apps/api` | 인증, 조직, 문서, 워크스페이스, 연동 API |
| `apps/worker` | AWS 변경 이력 수집과 정규화 |
| `apps/report` | 보고서 및 Terraform 생성 서비스 |
| `contracts` | 서비스 간 공유 스키마와 샘플 |

## 기술 스택

| 영역 | 사용 기술 |
| --- | --- |
| Frontend | React 19, TypeScript, Vite |
| Backend | FastAPI, SQLAlchemy, Python |
| Infra | AWS EventBridge, Lambda, S3, SQS, Cognito, EKS |
| IaC / Delivery | Terraform, CloudFormation, Argo CD |
| Data / AI | MariaDB, Amazon Bedrock, OPA |
| Integrations | GitHub, Slack, AWS STS |

## 현재 저장소 기준 핵심 기능

- AWS 변경 이력 수집과 표준 JSON 정규화
- 운영 대시보드와 문서 조회 화면
- 보고서 및 계획서 생성 API
- Terraform 생성, 검증, 수정 보조 흐름
- Cognito 기반 인증 및 사용자 관리
- GitHub, Slack 연동
- Terraform + GitOps 기반 인프라 및 배포 관리

## 팀

ACS-DnDn은 클라우드 운영 자동화와 실무형 DevOps 경험을 연결하는 제품을 만들고 있습니다.

조직 전체를 보려면 위 저장소들부터 확인해 주세요.
