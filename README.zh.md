# Last Mile 360

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh.md) | [Nederlands](README.nl.md) | [Русский](README.ru.md) | [한국어](README.ko.md)

> **状态：第 1 阶段 — 构建核心扫描器。** 架构已定义，monorepo 已搭建，安全代理正在开发中。查看[构建顺序](#构建顺序)了解完整路线图。

为直觉编码应用打造的生产就绪平台。Norton 级信任。Cloudflare 原生。零源服务器。

**"我们有一个完成了 90% 的应用。只需要帮忙修复最后的错误。"** 最后的 10% 占了 95% 的工作量。这个工具就是做这些工作的。

---

## 这是什么

Last Mile 360 是一个单一工具，它能将任何直觉编码的应用变得可以安全投入生产。它在五个维度上扫描、评分、修复和持续监控代码库：**安全性、数据库安全、基础设施、可观测性和代码质量**。

它将 15 个以上开源代理框架、推理引擎、记忆系统和计算机使用工具的最佳能力整合到一个安全的 Cloudflare 原生平台中，无需自托管基础设施。

**它不做什么：** 它不会修复你的产品市场契合度。它不会重新设计你的 UX。它不会优化你的业务逻辑。它让你的代码安全地面对真实用户的真实数据。

---

## 快速开始（第 1 阶段推出时可用）

CLI 正在积极开发中。当第 1 阶段发布时：

```bash
npm install -g @last-mile/cli
last-mile login
cd your-vibe-coded-app
last-mile scan       # 扫描安全、数据库、基础设施、可观测性、质量问题
last-mile score      # 查看你的生产就绪评分 (0-100)
last-mile fix        # 通过 PR 自动修复可修复的问题
last-mile monitor    # 启用持续监控
```

### 开发（立即参与贡献）

```bash
git clone https://github.com/itallstartedwithaidea/last-mile.git
cd last-mile
npm install
npm run dev
```

完整设置说明请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## Norton 360 信任模型

Norton 360 通过核心原则建立了消费者信任。每一个都对应一个生产代码的缺口。

| # | Norton 原则 | Last Mile 对应 | 实现方式 |
| --- | --- | --- | --- |
| 1 | 实时保护 | 安全代理在每次提交时扫描，在合并前阻止危险模式 | Semgrep + Gitleaks 作为 GitHub Action + pre-commit 钩子 |
| 2 | 智能防火墙 | 基础设施代理验证 API 路由、网络配置、暴露端口、CORS 策略 | OWASP ZAP + Checkov + 自定义路由分析器 |
| 3 | AI 反诈骗保护 | 供应链代理检测依赖攻击、域名仿冒、恶意安装脚本 | Socket.dev + OSV-Scanner |
| 4 | LiveUpdate | CVE 馈送同步获取最新漏洞，自动修补依赖，更新 Semgrep 规则 | Cloudflare Cron Triggers + OSV/NVD/GitHub Advisory 馈送 |
| 5 | 云备份 | 数据库代理强制执行迁移安全、迁移前快照、回滚路径 | Prisma introspect + Atlas + 迁移前快照 |
| 6 | 暗网监控 | 暴露扫描器检测公共仓库、粘贴网站、泄露数据库中的泄露密钥 | Trufflehog + 自定义粘贴/泄露扫描器 |
| 7 | 身份/密钥 | 密钥管理器集中所有凭证，轮换密钥，加密 env 文件 | Infisical 或 Cloudflare Secrets Store |
| 8 | VPN | 部署流水线确保加密部署、安全 CI/CD、传输中无明文 | GitHub Actions OIDC + Cloudflare Zero Trust |
| 9 | 性能 | 质量代理删除死代码，优化打包，评估可维护性 | SonarQube + knip + Madge + Lighthouse |
| 10 | 家长控制 | 策略引擎强制执行编码标准，在合并前阻止危险模式 | 自定义 Semgrep 规则 + 可配置策略 YAML |

---

## 五个代理部门

### 部门 1：安全代理

| 子代理 | 工具 | 功能 |
| --- | --- | --- |
| 密钥扫描器 | Gitleaks + Trufflehog | Pre-commit + 全仓库扫描硬编码密钥 |
| 依赖审计 | Socket.dev + OSV-Scanner + Snyk | 供应链攻击检测，跨 npm/pip/Go/Rust 的 CVE 扫描 |
| SAST | Semgrep | 针对 SQL 注入、XSS、认证绕过的自定义规则引擎 |
| 认证验证器 | OWASP ZAP | 对预发环境的自动化渗透测试 |
| 基础设施安全 | Checkov + Trivy | Terraform/Docker/K8s 配置错误 + 容器漏洞扫描 |

### 部门 2：数据库代理

**关键规则：** 数据库代理永远不会自动执行迁移。它生成迁移文件和 PR。人工必须审查和批准每个架构变更。

### 部门 3：基础设施代理

生成 Dockerfile、CI/CD 工作流、部署配置和环境管理——全部是配置文件，从不执行。

### 部门 4：可观测性代理

用结构化日志替换 `console.log`，注入 OpenTelemetry SDK、Sentry 错误追踪，生成 `/health` + `/ready` 端点。

### 部门 5：质量代理

生成测试套件，查找死代码（knip），检测循环依赖（Madge），计算生产就绪评分。

---

## 评分标准

| 类别 | 权重 | 示例检查点 |
| --- | --- | --- |
| 安全 | 35% | 硬编码密钥 (CWE-798)、SQL 注入 (CWE-89)、缺失认证 (CWE-306)、XSS (CWE-79) |
| 数据库 | 20% | 无迁移历史、无备份配置、缺失索引、RLS 已禁用 |
| 基础设施 | 20% | 无 Dockerfile、无 CI/CD、密钥在已提交的 .env 中、无健康检查 |
| 可观测性 | 12.5% | 无错误追踪、仅 console.log、无健康端点、无指标 |
| 质量 | 12.5% | 无测试、高圈复杂度、死代码 > 20%、循环依赖 |

### 等级

| 分数 | 等级 | 含义 |
| --- | --- | --- |
| 0–25 | F — 危急 | 不适合生产。存在活跃的安全漏洞。 |
| 26–50 | D — 危险 | 重大缺口。一些基础存在但关键问题仍在。 |
| 51–70 | C — 谨慎 | 在路上了。核心安全已解决，需要监控 + 测试。 |
| 71–85 | B — 生产就绪 | 适合生产。所有关键发现已解决。 |
| 86–95 | A — 加固 | 企业级。全面覆盖，持续监控。 |
| 96–100 | A+ — Norton 级 | 同类最佳。所有检查点绿色，完全监控激活。 |

---

## 商业模式

| 层级 | 价格 | 包含内容 |
| --- | --- | --- |
| 免费 | 永远 $0 | CLI 扫描 + 报告 + 评分 + 20 条自定义规则 |
| Pro | $29/月 | 自动修复 PR、持续监控、仪表盘、100 次扫描/月 |
| Team | $99/月 | 5 个席位、SSO、共享策略、合规模板、500 次扫描/月 |
| Enterprise | 定制 | 无限席位、自托管选项、自定义规则、SLA、专属支持 |

---

## 常见问题

**"为什么不直接使用 Snyk/SonarQube/Dependabot？"**
那些是单独的工具。Last Mile 360 是将它们全部一起运行的编排层，合并输出，消除重复，按实际风险优先排序，生成修复，并持续监控。

**"为什么选 Cloudflare 而不是 AWS/Vercel？"**
零源服务器。AWS 需要 EC2/ECS 实例。Vercel 有冷启动和有限的计算能力。Cloudflare Workers 是 V8 隔离体，0ms 冷启动，继承 SOC2/ISO 27001 合规。对于安全产品，平台必须比它扫描的代码更安全。

---

## 致谢

由 [John Williams](https://github.com/itallstartedwithaidea) 构建 — Seer Interactive 高级付费媒体专家，[googleadsagent.ai](https://googleadsagent.ai) 创建者，Casteel High School 教练，前 Washington State 橄榄球队成员（2002–2005）。

Norton 360 是 Gen Digital Inc. 的商标。Last Mile 360 与 Norton 或 Gen Digital 无关。

---

*"最后的 10% 占了 95% 的工作量。我们做这些工作。"*

MIT 许可证 · 分享它。教授它。Fork 它。
