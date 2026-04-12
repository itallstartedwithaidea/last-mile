# Last Mile 360

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh.md) | [Nederlands](README.nl.md) | [Русский](README.ru.md) | [한국어](README.ko.md)

> **상태: 1단계 — 핵심 스캐너 구축 중.** 아키텍처 정의 완료, 모노레포 구성 완료, 보안 에이전트 개발 중. 전체 로드맵은 [구축 순서](#구축-순서)를 참조하세요.

바이브 코딩 앱을 위한 프로덕션 준비 플랫폼. Norton 수준의 신뢰. Cloudflare 네이티브. 오리진 서버 제로.

**"90% 완성된 앱이 있어요. 마지막 오류만 수정하면 됩니다."** 그 마지막 10%가 작업의 95%입니다. 이 도구가 그 작업을 합니다.

---

## 이것은 무엇인가

Last Mile 360은 바이브 코딩으로 만든 애플리케이션을 프로덕션 안전하게 만드는 단일 도구입니다. 다섯 가지 차원에서 코드베이스를 스캔, 점수 평가, 수정, 지속적으로 모니터링합니다: **보안, 데이터베이스 안전, 인프라, 관측성, 코드 품질**.

15개 이상의 오픈소스 에이전트 프레임워크, 추론 엔진, 메모리 시스템, 컴퓨터 사용 도구의 최고 기능을 셀프 호스팅 인프라 없이 하나의 안전한 Cloudflare 네이티브 플랫폼으로 통합합니다.

**이것이 아닌 것:** 프로덕트-마켓 핏을 수정하지 않습니다. UX를 재설계하지 않습니다. 비즈니스 로직을 최적화하지 않습니다. 실제 사용자의 실제 데이터 앞에 코드를 안전하게 놓을 수 있게 합니다.

---

## 빠른 시작 (1단계에서 제공 예정)

CLI가 활발히 개발 중입니다. 1단계가 출시되면:

```bash
npm install -g @last-mile/cli
last-mile login
cd your-vibe-coded-app
last-mile scan       # 보안, DB, 인프라, 관측성, 품질 이슈 스캔
last-mile score      # 프로덕션 준비 점수 확인 (0-100)
last-mile fix        # PR을 통해 수정 가능한 것 자동 수정
last-mile monitor    # 지속적 모니터링 활성화
```

### 개발 (지금 기여하세요)

```bash
git clone https://github.com/itallstartedwithaidea/last-mile.git
cd last-mile
npm install
npm run dev
```

전체 설정 안내는 [CONTRIBUTING.md](CONTRIBUTING.md)를 참조하세요.

---

## Norton 360 신뢰 모델

Norton 360은 핵심 원칙을 통해 소비자 신뢰를 구축했습니다. 각각은 프로덕션 코드 격차에 대응합니다.

| # | Norton 원칙 | Last Mile 대응 | 구현 |
| --- | --- | --- | --- |
| 1 | 실시간 보호 | 보안 에이전트가 매 커밋마다 스캔, 머지 전 위험한 패턴 차단 | Semgrep + Gitleaks를 GitHub Action + pre-commit 훅으로 |
| 2 | 스마트 방화벽 | 인프라 에이전트가 API 라우트, 네트워크 설정, 노출 포트, CORS 정책 검증 | OWASP ZAP + Checkov + 커스텀 라우트 분석기 |
| 3 | AI 스캠 보호 | 공급망 에이전트가 의존성 공격, 타이포스쿼팅, 악성 설치 스크립트 탐지 | Socket.dev + OSV-Scanner |
| 4 | LiveUpdate | CVE 피드 동기화가 최신 취약점을 가져오고, 의존성 자동 패치, Semgrep 규칙 업데이트 | Cloudflare Cron Triggers + OSV/NVD/GitHub Advisory 피드 |
| 5 | 클라우드 백업 | DB 에이전트가 마이그레이션 안전성, 마이그레이션 전 스냅샷, 롤백 경로 강제 | Prisma introspect + Atlas + 마이그레이션 전 스냅샷 |
| 6 | 다크웹 모니터링 | 노출 스캐너가 공개 리포, 페이스트 사이트, 유출 DB에서 누출된 시크릿 탐지 | Trufflehog + 커스텀 페이스트/유출 스캐너 |
| 7 | 신원 / 시크릿 | 시크릿 관리자가 모든 자격 증명을 중앙화, 키 순환, env 파일 암호화 | Infisical 또는 Cloudflare Secrets Store |
| 8 | VPN | 배포 파이프라인이 암호화된 배포, 안전한 CI/CD, 전송 중 평문 없음 보장 | GitHub Actions OIDC + Cloudflare Zero Trust |
| 9 | 성능 | 품질 에이전트가 죽은 코드 제거, 번들 최적화, 유지보수성 평가 | SonarQube + knip + Madge + Lighthouse |
| 10 | 자녀 보호 | 정책 엔진이 코딩 표준 적용, 머지 전 위험 패턴 차단 | 커스텀 Semgrep 규칙 + 설정 가능 정책 YAML |

---

## 5개 에이전트 부문

### 부문 1: 보안 에이전트

| 서브 에이전트 | 도구 | 기능 |
| --- | --- | --- |
| 시크릿 스캐너 | Gitleaks + Trufflehog | Pre-commit + 하드코딩된 시크릿 전체 리포 스캔 |
| 의존성 감사 | Socket.dev + OSV-Scanner + Snyk | 공급망 공격 탐지, npm/pip/Go/Rust CVE 스캔 |
| SAST | Semgrep | SQL 인젝션, XSS, 인증 우회를 위한 커스텀 규칙 엔진 |
| 인증 검증기 | OWASP ZAP | 스테이징에 대한 자동화된 침투 테스트 |
| 인프라 보안 | Checkov + Trivy | Terraform/Docker/K8s 설정 오류 + 컨테이너 취약점 스캔 |

### 부문 2: 데이터베이스 에이전트

**핵심 규칙:** 데이터베이스 에이전트는 절대 자동으로 마이그레이션을 실행하지 않습니다. 마이그레이션 파일과 PR을 생성합니다. 사람이 모든 스키마 변경을 검토하고 승인해야 합니다.

### 부문 3: 인프라 에이전트

Dockerfile, CI/CD 워크플로우, 배포 설정, 환경 관리를 생성합니다 — 모두 설정 파일이며, 절대 실행하지 않습니다.

### 부문 4: 관측성 에이전트

`console.log`를 구조화된 로깅으로 교체, OpenTelemetry SDK 주입, Sentry 에러 트래킹, `/health` + `/ready` 엔드포인트 생성.

### 부문 5: 품질 에이전트

테스트 스위트 생성, 죽은 코드 찾기(knip), 순환 의존성 탐지(Madge), 프로덕션 준비 점수 계산.

---

## 평가 기준

| 카테고리 | 가중치 | 예시 체크포인트 |
| --- | --- | --- |
| 보안 | 35% | 하드코딩된 시크릿(CWE-798), SQL 인젝션(CWE-89), 인증 누락(CWE-306), XSS(CWE-79) |
| 데이터베이스 | 20% | 마이그레이션 이력 없음, 백업 설정 없음, 인덱스 누락, RLS 비활성화 |
| 인프라 | 20% | Dockerfile 없음, CI/CD 없음, 커밋된 .env에 시크릿, 헬스 체크 없음 |
| 관측성 | 12.5% | 에러 트래킹 없음, console.log만 사용, 헬스 엔드포인트 없음, 메트릭 없음 |
| 품질 | 12.5% | 테스트 없음, 높은 순환 복잡도, 죽은 코드 > 20%, 순환 의존성 |

### 등급

| 점수 | 등급 | 의미 |
| --- | --- | --- |
| 0–25 | F — 위험 | 프로덕션에 안전하지 않음. 활성 보안 취약점. |
| 26–50 | D — 위험 | 주요 격차. 일부 기본 사항은 있지만 중요 문제 남아 있음. |
| 51–70 | C — 주의 | 진행 중. 핵심 보안 해결됨, 모니터링 + 테스트 필요. |
| 71–85 | B — 프로덕션 준비 | 프로덕션에 안전. 모든 중요 발견 사항 해결됨. |
| 86–95 | A — 강화 | 엔터프라이즈급. 전체 커버리지, 지속적 모니터링. |
| 96–100 | A+ — Norton급 | 동종 최강. 모든 체크포인트 녹색, 전체 모니터링 활성. |

---

## 비즈니스 모델

| 등급 | 가격 | 포함 내용 |
| --- | --- | --- |
| 무료 | 영원히 $0 | CLI 스캔 + 리포트 + 점수 + 20개 커스텀 규칙 |
| Pro | $29/월 | 자동 수정 PR, 지속적 모니터링, 대시보드, 100 스캔/월 |
| Team | $99/월 | 5석, SSO, 공유 정책, 컴플라이언스 템플릿, 500 스캔/월 |
| Enterprise | 맞춤 | 무제한 석, 셀프 호스팅 옵션, 커스텀 규칙, SLA, 전담 지원 |

---

## 크레딧

[John Williams](https://github.com/itallstartedwithaidea) 제작 — Seer Interactive의 시니어 유료 미디어 전문가, [googleadsagent.ai](https://googleadsagent.ai) 크리에이터, Casteel High School 코치, 전 Washington State 풋볼 선수 (2002–2005).

Norton 360은 Gen Digital Inc.의 상표입니다. Last Mile 360은 Norton 또는 Gen Digital과 무관합니다.

---

*"그 마지막 10%가 작업의 95%입니다. 우리가 그 작업을 합니다."*

MIT 라이선스 · 공유하세요. 가르치세요. Fork 하세요.
