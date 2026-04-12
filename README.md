# Last Mile 360

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh.md) | [Nederlands](README.nl.md) | [Русский](README.ru.md) | [한국어](README.ko.md)

> **Status: Phase 1 — Building the core scanner.** Architecture defined, monorepo scaffolded, security agent in development. See [Build Order](#build-order) for the full roadmap.

The production-readiness platform for vibe-coded apps. Norton-grade trust. Cloudflare-native. Zero origin servers.

**"We have a 90% finished app. We just need help fixing the last errors."** That last 10% is 95% of the work. This tool does that work.

---


## What this is

Last Mile 360 is a single tool that takes any vibe-coded application and makes it production-safe. It scans, scores, fixes, and continuously monitors codebases across five dimensions: **security, database safety, infrastructure, observability, and code quality**.

It consolidates the best capabilities from 15+ open-source agent frameworks, inference engines, memory systems, and computer-use tools into one secure, Cloudflare-native platform with zero self-hosted infrastructure.

**What it is not:** It does not fix your product-market fit. It does not redesign your UX. It does not optimize your business logic. It makes your code safe to put in front of real users with real data.

---

## Quick start (coming in Phase 1)

The CLI is under active development. When Phase 1 ships:

```bash
npm install -g @last-mile/cli
last-mile login
cd your-vibe-coded-app
last-mile scan       # Scan for security, db, infra, observability, quality issues
last-mile score      # See your production readiness score (0-100)
last-mile fix        # Auto-fix what's fixable via PR
last-mile monitor    # Enable continuous monitoring
```

### Development (contribute now)

```bash
git clone https://github.com/itallstartedwithaidea/last-mile.git
cd last-mile
npm install
npm run dev
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for full setup instructions.

---

## The Norton 360 trust model

Norton 360 built consumer trust through core principles. Every one maps to a production code gap.

| # | Norton principle | Last Mile equivalent | Implementation |
| --- | --- | --- | --- |
| 1 | Real-time protection | Security Agent scans on every commit, blocks dangerous patterns before merge | Semgrep + Gitleaks as GitHub Action + pre-commit hook |
| 2 | Smart Firewall | Infra Agent validates API routes, network configs, exposed ports, CORS policies | OWASP ZAP + Checkov + custom route analyzer |
| 3 | AI scam protection | Supply Chain Agent detects dependency attacks, typosquatting, malicious install scripts | Socket.dev + OSV-Scanner |
| 4 | LiveUpdate | CVE Feed Sync pulls latest vulnerabilities, auto-patches deps, updates Semgrep rules | Cloudflare Cron Triggers + OSV/NVD/GitHub Advisory feeds |
| 5 | Cloud Backup | Database Agent enforces migration safety, backup-before-migrate, rollback paths | Prisma introspect + Atlas + snapshot-before-migrate |
| 6 | Dark Web Monitoring | Exposure Scanner detects leaked secrets in public repos, paste sites, breach databases | Trufflehog + custom paste/breach scanner |
| 7 | Identity / Secrets | Secrets Manager centralizes all credentials, rotates keys, encrypts env files | Infisical or Cloudflare Secrets Store |
| 8 | VPN | Deploy Pipeline ensures encrypted deployment, secure CI/CD, no plaintext in transit | GitHub Actions OIDC + Cloudflare Zero Trust |
| 9 | Performance | Quality Agent removes dead code, optimizes bundles, scores maintainability | SonarQube + knip + Madge + Lighthouse |
| 10 | Parental Controls | Policy Engine enforces coding standards, blocks dangerous patterns pre-merge | Custom Semgrep rules + configurable policy YAML |

---

## Architecture — Cloudflare-native

```
┌─────────────────────────────────────────────────────────────────┐
│  USER: npx last-mile scan                                       │
└──────────────────────────┬──────────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────────┐
│              CLOUDFLARE TRUST PERIMETER                          │
│  DDoS protection · WAF · Zero Trust · Edge encryption           │
│  SOC2 Type II · ISO 27001 · PCI DSS inherited                   │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │  PANOPTES ORCHESTRATOR                                     │  │
│  │  Cloudflare Workers + Durable Objects                      │  │
│  │  Detects stack → dispatches agents → merges reports        │  │
│  └────┬────────┬────────┬────────┬────────┬───────────────────┘  │
│       │        │        │        │        │                      │
│  ┌────▼──┐ ┌───▼───┐ ┌──▼───┐ ┌──▼────┐ ┌▼───────┐             │
│  │SECURITY│ │  DB   │ │INFRA │ │OBSERVE│ │QUALITY │             │
│  │ Agent  │ │ Agent │ │Agent │ │ Agent │ │ Agent  │             │
│  │Worker  │ │Worker │ │Worker│ │Worker │ │Worker  │             │
│  └────┬───┘ └───┬───┘ └──┬───┘ └──┬────┘ └┬───────┘             │
│       └─────────┴────────┴────────┴───────┘                      │
│                          │                                       │
│  ┌───────────────────────▼───────────────────────────────────┐   │
│  │  TRUSTED INFERENCE (no self-hosted models)                 │   │
│  │  Claude API · Workers AI · OpenAI / Gemini                 │   │
│  │  ALL routed through Cloudflare AI Gateway                  │   │
│  └────────────────────────────────────────────────────────────┘   │
│                                                                   │
│  ┌────────────────────────────────────────────────────────────┐   │
│  │  CLOUDFLARE-NATIVE PERSISTENCE                             │   │
│  │  D1 (SQL) · R2 (Objects) · KV (Cache) · Vectorize          │   │
│  │  Queues · Durable Objects                                   │   │
│  └────────────────────────────────────────────────────────────┘   │
│                                                                   │
│  Zero origin servers · Every byte encrypted · SOC2 inherited     │
└───────────────────────────────────────────────────────────────────┘
```

**Why Cloudflare for everything:** Zero servers to hack. Zero servers to patch. Zero servers to monitor. Workers are V8 isolates — sandboxed by default, no filesystem, no network except what you grant. You inherit Cloudflare's compliance certifications. The infrastructure that protects 20% of the internet protects your scanner.

---

## The five agent divisions

### Division 1: Security Agent

| Sub-agent | Tool | What it does |
| --- | --- | --- |
| Secret scanner | Gitleaks + Trufflehog | Pre-commit + full-repo scan for hardcoded secrets, entropy analysis + verified credential detection |
| Dependency audit | Socket.dev + OSV-Scanner + Snyk | Supply chain attack detection, CVE scanning across npm/pip/Go/Rust |
| SAST | Semgrep | Custom rule engine for SQL injection, XSS, auth bypass, vibe-code anti-patterns |
| Auth validator | OWASP ZAP | Automated penetration testing against staging |
| Infra security | Checkov + Trivy | Terraform/Docker/K8s misconfiguration + container vulnerability scanning |

### Division 2: Database Agent

| Sub-agent | Tool | What it does |
| --- | --- | --- |
| Schema introspect | Prisma | Generates schema from existing database |
| Schema management | Atlas | Declarative schema management — define desired state, compute migration |
| Migration safety | Custom | Pre-migration snapshot, dry-run validation, rollback path generation |
| Schema validation | Custom + Claude API | Detects orphaned tables, missing indexes, ORM-vs-DB drift |

**Critical rule:** The Database Agent NEVER executes migrations automatically. It generates migration files and PRs. A human must review and approve every schema change.

### Division 3: Infrastructure Agent

Generates Dockerfiles, CI/CD workflows, deployment configs, and env management — all config files, never executes.

### Division 4: Observability Agent

Replaces `console.log` with structured logging, injects OpenTelemetry SDK, Sentry error tracking, and generates `/health` + `/ready` endpoints.

### Division 5: Quality Agent

Generates test suites, finds dead code (knip), detects circular dependencies (Madge), and calculates production readiness scores.

---

## Trusted inference — no self-hosted models

**Rule:** No self-hosted models. No unaudited inference. Every LLM call goes through Cloudflare AI Gateway.

| Tier | Provider | Use case | Why trusted |
| --- | --- | --- | --- |
| 1 | Claude API (Anthropic) | Complex security analysis, code review, user-facing explanations | SOC2 Type II, established security response process |
| 2 | Cloudflare Workers AI | Edge inference — code analyzed without leaving Cloudflare's network | Runs inside Cloudflare's trust perimeter, SOC2 inherited |
| 3 | OpenAI / Gemini | Fallback + model diversity for cross-validation | SOC2 compliant, enterprise data agreements |

Automatic fallback chain: Claude → Workers AI → OpenAI → Gemini.

---

## Scoring rubric

Every checkpoint maps to a CWE (Common Weakness Enumeration) identifier. Weights come from CVSS severity scores.

| Category | Weight | Example checkpoints |
| --- | --- | --- |
| Security | 35% | Hardcoded secrets (CWE-798), SQL injection (CWE-89), missing auth (CWE-306), XSS (CWE-79) |
| Database | 20% | No migration history, no backup config, missing indexes, RLS disabled |
| Infrastructure | 20% | No Dockerfile, no CI/CD, secrets in .env committed, no health check |
| Observability | 12.5% | No error tracking, console.log only, no health endpoints, no metrics |
| Quality | 12.5% | No tests, high cyclomatic complexity, dead code > 20%, circular deps |

### Score grades

| Score | Grade | Meaning |
| --- | --- | --- |
| 0–25 | F — Critical | Not safe for production. Active security vulnerabilities. |
| 26–50 | D — Dangerous | Major gaps. Some basics exist but critical issues remain. |
| 51–70 | C — Caution | Getting there. Core security addressed, needs monitoring + tests. |
| 71–85 | B — Production-ready | Safe for production. All critical findings addressed. |
| 86–95 | A — Hardened | Enterprise-grade. Full coverage, continuous monitoring. |
| 96–100 | A+ — Norton-grade | Best-in-class. Every checkpoint green, full monitoring active. |

---

## Source repo consolidation

This project evaluated 15+ open-source repos and took **patterns, not dependencies**. No code was imported. All implementation is original TypeScript on Cloudflare Workers.

| Capability | Source inspiration | Our implementation |
| --- | --- | --- |
| Right-sized inference | Claw family (OpenClaw → MimiClaw) | Claude API + Workers AI + OpenAI — audited, SOC2 compliant |
| Agent isolation | OpenFang Agent OS | Each agent = separate Cloudflare Worker (V8 isolate sandbox) |
| Role-based crews | CrewAI | Scanner/Validator/Remediator roles per division — pure TypeScript |
| Multi-agent conversation | AutoGen | Claude API with structured prompts — no Microsoft dependency |
| Autonomous tool use | SuperAGI | Workers calling external tools with human-in-the-loop for destructive ops |
| Pipeline composition | LangChain | Workers → Queues → Workers chain — no abstraction leak |
| Three-tier memory | memU | KV (session) + D1 (project history) + Vectorize (semantic search) |
| Visual testing | Agent S3 | Playwright in CI now; CF Browser Rendering later |
| Agent loop pattern | Nanobot | Reason → Act → Observe → Repeat — ~200 lines of TS per agent |

---

## Configuration

```yaml
# .last-mile.yml
version: 1

merge-threshold: 70

agents:
  security: true
  database: true
  infrastructure: true
  observability: true
  quality: true

policy:
  disable:
    - console-log-in-production
  override:
    cors-wildcard: warning

framework: nextjs
database: supabase

compliance:
  - hipaa
  - gdpr
```

---

## Custom Semgrep rules (the competitive moat)

Framework-agnostic vibe-code anti-patterns plus framework-specific rules:

**Universal patterns:** `hardcoded-secrets`, `console-log-in-production`, `no-error-handling`, `raw-sql-no-params`, `no-input-validation`, `cors-wildcard`, `no-rate-limiting`, `jwt-decode-not-verify`, `env-in-client-bundle`, `no-csrf-protection`, `missing-auth-middleware`, `eval-usage`, `dangerouslySetInnerHTML`, `weak-crypto`, `admin-route-no-auth`

**Next.js:** `api-route-no-auth`, `getServerSideProps-leak`, `middleware-bypass`, `server-action-unvalidated`

**Express:** `no-helmet`, `body-parser-limit`, `trust-proxy-misconfigured`

**FastAPI:** `no-depends-auth`, `pydantic-no-validation`, `cors-allow-all`

**Supabase:** `rls-disabled`, `service-key-client-side`, `anon-key-write-access`

---

## Security posture

How we secure the scanner itself:

- **CLI:** No eval, auto-update, integrity checks, npm provenance
- **Code upload:** Client-side AES-256-GCM encryption, presigned URL direct to R2, auto-deleted after scan
- **Workers:** All secrets in Secrets Store, whitelisted outbound only, V8 isolate sandbox
- **LLM:** Structured XML prompts (prompt injection defense), DLP on all AI Gateway requests, no training on customer data
- **Infrastructure:** Zero Trust dashboard access, Cloudflare WAF, immutable audit logging
- **Self-dogfooding:** We scan ourselves on every PR, bug bounty program, public score badge

---

## Business model

| Tier | Price | What's included |
| --- | --- | --- |
| Free | $0 forever | CLI scan + report + score + 20 custom rules |
| Pro | $29/mo | Auto-fix PRs, continuous monitoring, dashboard, 100 scans/mo |
| Team | $99/mo | 5 seats, SSO, shared policies, compliance templates, 500 scans/mo |
| Enterprise | Custom | Unlimited seats, self-hosted option, custom rules, SLA, dedicated support |

---

## Build order

| Phase | Weeks | Deliverables |
| --- | --- | --- |
| 1: Core scanner | 1–4 | CLI, stack detection, security agent, markdown report, simplified score |
| 2: Fix engine | 5–8 | Auto-fix PR generation, database agent, infra agent, GitHub App |
| 3: Full agents | 9–12 | Observability agent, quality agent, dashboard v1, credit billing |
| 4: Intelligence | 13–16 | AI Gateway integration, plain-English explanations, architecture map |
| 5: Continuous | 17–20 | CVE sync, drift detection, exposure scanning, compliance templates |
| 6: Scale | 21–24 | Team features, SSO, self-hosting guide, framework expansion, scoring validation |

---

## What's still missing

| Gap | Why it matters | When |
| --- | --- | --- |
| Scoring validation data | Score means nothing without empirical correlation to real incidents | Phase 6 |
| Third-party security audit | Need independent validation equivalent to AV-TEST | Phase 6 |
| False positive management | Need feedback loop to tune rules | Phase 4 |
| Incident auto-response | Critical vuln at 2 AM — what happens automatically? | Phase 5 |
| IDE extension | Findings should show inline in VS Code/Cursor | Phase 5 |
| Offline mode | Some teams can't upload code to cloud | Phase 6 |

---

## FAQ

**"Why not just use Snyk/SonarQube/Dependabot directly?"**
Those are individual tools. Last Mile 360 is the orchestration layer that runs all of them together, merges their output, eliminates duplicates, prioritizes by actual risk, generates fixes, and monitors continuously.

**"Why Cloudflare over AWS/Vercel?"**
Zero origin servers. AWS requires EC2/ECS instances. Vercel has cold starts and limited compute. Cloudflare Workers are V8 isolates with 0ms cold start and inherited SOC2/ISO 27001 compliance. For a security product, the platform must be more secure than the code it scans.

**"Why no self-hosted models?"**
Trust. A self-hosted model on your infrastructure is a liability, not an asset. Claude API, OpenAI, and Workers AI are SOC2-compliant providers with security response teams. The Claw family repos are technically interesting but fundamentally incompatible with Norton-grade trust.

**"What if Claude/OpenAI goes down?"**
AI Gateway automatic fallback: Claude → Workers AI → OpenAI → Gemini. The security scanning tools (Semgrep, Gitleaks, Snyk) don't use LLMs at all — they work even if every AI provider is down.

**"How is my code protected?"**
Client-side AES-256-GCM encryption before upload. Presigned URL direct to R2. Auto-deleted after scan. DLP on all AI Gateway requests. Workers AI processes code without leaving Cloudflare's network.

---

## Credits

Built by [John Williams](https://github.com/itallstartedwithaidea) — Senior Paid Media Specialist at Seer Interactive, creator of [googleadsagent.ai](https://googleadsagent.ai), coach at Casteel High School, former Washington State football (2002–2005).

Architectural concepts informed by: OpenFang (agent isolation), CrewAI (role-based agents), AutoGen (conversational multi-agent), memU (three-tier memory), Nanobot (minimal agent loop), OpenClaw Handbook (engineering documentation patterns).

No code was imported from any source repo. All implementation is original TypeScript on Cloudflare Workers.

Norton 360 is a trademark of Gen Digital Inc. Last Mile 360 is not affiliated with Norton or Gen Digital. The Norton trust model is referenced as an architectural inspiration and benchmark.

---

*"That last 10% is 95% of the work. We do that work."*

MIT License · Share it. Teach it. Fork it.
