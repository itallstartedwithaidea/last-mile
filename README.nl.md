# Last Mile 360

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh.md) | [Nederlands](README.nl.md) | [Русский](README.ru.md) | [한국어](README.ko.md)

> **Status: Fase 1 — De kernscanner wordt gebouwd.** Architectuur gedefinieerd, monorepo opgezet, security agent in ontwikkeling. Zie [Bouwvolgorde](#bouwvolgorde) voor de volledige roadmap.

Het productiegereedheidsplatform voor vibe-gecodeerde apps. Norton-grade vertrouwen. Cloudflare-native. Nul origin servers.

**"We hebben een app die voor 90% klaar is. We hebben alleen hulp nodig met de laatste fouten."** Die laatste 10% is 95% van het werk. Deze tool doet dat werk.

---

## Wat dit is

Last Mile 360 is een enkele tool die elke vibe-gecodeerde applicatie productieveilig maakt. Het scant, scoort, fixt en monitort continu codebases over vijf dimensies: **security, databaseveiligheid, infrastructuur, observability en codekwaliteit**.

Het consolideert de beste mogelijkheden van 15+ open-source agentframeworks, inferentie-engines, geheugensystemen en computer-use tools in één veilig, Cloudflare-native platform zonder zelf-gehoste infrastructuur.

**Wat het niet is:** Het fixt je product-market fit niet. Het herontwerpt je UX niet. Het optimaliseert je bedrijfslogica niet. Het maakt je code veilig om aan echte gebruikers met echte data te presenteren.

---

## Snel starten (beschikbaar in Fase 1)

De CLI is in actieve ontwikkeling. Wanneer Fase 1 wordt uitgebracht:

```bash
npm install -g @last-mile/cli
last-mile login
cd your-vibe-coded-app
last-mile scan       # Scan op security, db, infra, observability, kwaliteitsproblemen
last-mile score      # Bekijk je productiegereedheidsscore (0-100)
last-mile fix        # Auto-fix wat fixbaar is via PR
last-mile monitor    # Schakel continue monitoring in
```

### Ontwikkeling (draag nu bij)

```bash
git clone https://github.com/itallstartedwithaidea/last-mile.git
cd last-mile
npm install
npm run dev
```

Zie [CONTRIBUTING.md](CONTRIBUTING.md) voor volledige setup-instructies.

---

## Het Norton 360 vertrouwensmodel

Norton 360 bouwde consumentenvertrouwen op via kernprincipes. Elk principe komt overeen met een productiecode-gat.

| # | Norton-principe | Last Mile-equivalent | Implementatie |
| --- | --- | --- | --- |
| 1 | Realtimeprotectie | Security Agent scant bij elke commit, blokkeert gevaarlijke patronen vóór merge | Semgrep + Gitleaks als GitHub Action + pre-commit hook |
| 2 | Slimme firewall | Infra Agent valideert API-routes, netwerkconfiguraties, open poorten, CORS-beleid | OWASP ZAP + Checkov + aangepaste route-analyser |
| 3 | AI-scambescherming | Supply Chain Agent detecteert dependency-aanvallen, typosquatting, kwaadaardige installatiescripts | Socket.dev + OSV-Scanner |
| 4 | LiveUpdate | CVE Feed Sync haalt nieuwste kwetsbaarheden op, patcht automatisch deps, updatet Semgrep-regels | Cloudflare Cron Triggers + OSV/NVD/GitHub Advisory feeds |
| 5 | Cloudback-up | Database Agent dwingt migratileveiligheid af, backup-voor-migratie, rollback-paden | Prisma introspect + Atlas + snapshot-voor-migratie |
| 6 | Dark web-monitoring | Exposure Scanner detecteert gelekte secrets in publieke repo's, paste-sites, breach-databases | Trufflehog + aangepaste paste/breach-scanner |
| 7 | Identiteit / Secrets | Secrets Manager centraliseert alle credentials, roteert sleutels, versleutelt env-bestanden | Infisical of Cloudflare Secrets Store |
| 8 | VPN | Deploy Pipeline zorgt voor versleutelde deployment, veilige CI/CD, geen plaintext in transit | GitHub Actions OIDC + Cloudflare Zero Trust |
| 9 | Prestatie | Quality Agent verwijdert dode code, optimaliseert bundles, scoort onderhoud | SonarQube + knip + Madge + Lighthouse |
| 10 | Ouderlijk toezicht | Policy Engine dwingt codeerstandaarden af, blokkeert gevaarlijke patronen vóór merge | Aangepaste Semgrep-regels + configureerbaar beleid-YAML |

---

## De vijf agentdivisies

### Divisie 1: Security Agent

| Sub-agent | Tool | Wat het doet |
| --- | --- | --- |
| Secret scanner | Gitleaks + Trufflehog | Pre-commit + volledige repo-scan op hardgecodeerde secrets |
| Dependency audit | Socket.dev + OSV-Scanner + Snyk | Supply chain-aanvaldetectie, CVE-scanning over npm/pip/Go/Rust |
| SAST | Semgrep | Aangepaste regelengine voor SQL-injectie, XSS, auth-bypass |
| Auth validator | OWASP ZAP | Geautomatiseerde penetratietests tegen staging |
| Infra security | Checkov + Trivy | Terraform/Docker/K8s-misconfiguratie + container-kwetsbaarheidsscanning |

### Divisie 2: Database Agent

**Kritieke regel:** De Database Agent voert NOOIT automatisch migraties uit. Hij genereert migratiebestanden en PR's. Een mens moet elke schemawijziging beoordelen en goedkeuren.

### Divisie 3: Infrastructuur Agent

Genereert Dockerfiles, CI/CD-workflows, deploymentconfiguraties en env-beheer — allemaal configuratiebestanden, voert nooit uit.

### Divisie 4: Observability Agent

Vervangt `console.log` door gestructureerde logging, injecteert OpenTelemetry SDK, Sentry-fouttracking, en genereert `/health` + `/ready` endpoints.

### Divisie 5: Quality Agent

Genereert testsuites, vindt dode code (knip), detecteert circulaire dependencies (Madge), en berekent productiegereedheidsscores.

---

## Scoringsrubric

| Categorie | Gewicht | Voorbeeldcheckpoints |
| --- | --- | --- |
| Security | 35% | Hardgecodeerde secrets (CWE-798), SQL-injectie (CWE-89), ontbrekende auth (CWE-306), XSS (CWE-79) |
| Database | 20% | Geen migratiegeschiedenis, geen backupconfiguratie, ontbrekende indexes, RLS uitgeschakeld |
| Infrastructuur | 20% | Geen Dockerfile, geen CI/CD, secrets in gecommitte .env, geen health check |
| Observability | 12,5% | Geen fouttracking, alleen console.log, geen health endpoints, geen metrics |
| Kwaliteit | 12,5% | Geen tests, hoge cyclomatische complexiteit, dode code > 20%, circulaire deps |

### Cijfers

| Score | Cijfer | Betekenis |
| --- | --- | --- |
| 0–25 | F — Kritiek | Niet veilig voor productie. Actieve beveiligingskwetsbaarheden. |
| 26–50 | D — Gevaarlijk | Grote gaten. Sommige basis aanwezig maar kritieke problemen blijven. |
| 51–70 | C — Voorzichtig | Op de goede weg. Kernsecurity aangepakt, monitoring + tests nodig. |
| 71–85 | B — Productiegereed | Veilig voor productie. Alle kritieke bevindingen aangepakt. |
| 86–95 | A — Versterkt | Enterprise-grade. Volledige dekking, continue monitoring. |
| 96–100 | A+ — Norton-grade | Beste in klasse. Elk checkpoint groen, volledige monitoring actief. |

---

## Bedrijfsmodel

| Niveau | Prijs | Wat is inbegrepen |
| --- | --- | --- |
| Gratis | €0 voor altijd | CLI scan + rapport + score + 20 aangepaste regels |
| Pro | €29/mnd | Auto-fix PR's, continue monitoring, dashboard, 100 scans/mnd |
| Team | €99/mnd | 5 stoelen, SSO, gedeeld beleid, compliance-templates, 500 scans/mnd |
| Enterprise | Op maat | Onbeperkt stoelen, zelf-hosting optie, aangepaste regels, SLA, dedicated support |

---

## Credits

Gebouwd door [John Williams](https://github.com/itallstartedwithaidea) — Senior Paid Media Specialist bij Seer Interactive, maker van [googleadsagent.ai](https://googleadsagent.ai), coach bij Casteel High School, voormalig Washington State football (2002–2005).

Norton 360 is een handelsmerk van Gen Digital Inc. Last Mile 360 is niet gelieerd aan Norton of Gen Digital.

---

*"Die laatste 10% is 95% van het werk. Wij doen dat werk."*

MIT-licentie · Deel het. Onderricht het. Fork het.
