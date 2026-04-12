# Last Mile 360

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh.md) | [Nederlands](README.nl.md) | [Русский](README.ru.md) | [한국어](README.ko.md)

> **Statut : Phase 1 — Construction du scanner principal.** Architecture définie, monorepo structuré, agent de sécurité en développement. Voir [Ordre de construction](#ordre-de-construction) pour la feuille de route complète.

La plateforme de préparation à la production pour les applications codées de manière intuitive. Confiance de niveau Norton. Natif Cloudflare. Zéro serveur d'origine.

**« Nous avons une application terminée à 90%. On a juste besoin d'aide pour corriger les dernières erreurs. »** Ces derniers 10% représentent 95% du travail. Cet outil fait ce travail.

---

## Ce que c'est

Last Mile 360 est un outil unique qui prend n'importe quelle application codée intuitivement et la rend sûre pour la production. Il scanne, évalue, corrige et surveille en continu les bases de code selon cinq dimensions : **sécurité, sûreté de la base de données, infrastructure, observabilité et qualité du code**.

Il consolide les meilleures capacités de plus de 15 frameworks d'agents open-source, moteurs d'inférence, systèmes de mémoire et outils d'utilisation d'ordinateur dans une seule plateforme native Cloudflare sécurisée, sans infrastructure auto-hébergée.

**Ce que ce n'est pas :** Il ne corrige pas votre adéquation produit-marché. Il ne redessine pas votre UX. Il n'optimise pas votre logique métier. Il rend votre code sûr pour être présenté à de vrais utilisateurs avec de vraies données.

---

## Démarrage rapide (disponible en Phase 1)

Le CLI est en développement actif. Quand la Phase 1 sera livrée :

```bash
npm install -g @last-mile/cli
last-mile login
cd your-vibe-coded-app
last-mile scan       # Scanne la sécurité, la BDD, l'infra, l'observabilité, la qualité
last-mile score      # Affiche votre score de préparation à la production (0-100)
last-mile fix        # Corrige automatiquement ce qui est corrigeable via PR
last-mile monitor    # Active la surveillance continue
```

### Développement (contribuer maintenant)

```bash
git clone https://github.com/itallstartedwithaidea/last-mile.git
cd last-mile
npm install
npm run dev
```

Voir [CONTRIBUTING.md](CONTRIBUTING.md) pour les instructions de configuration complètes.

---

## Le modèle de confiance Norton 360

Norton 360 a construit la confiance des consommateurs via des principes fondamentaux. Chacun correspond à une lacune de code en production.

| # | Principe Norton | Équivalent Last Mile | Implémentation |
| --- | --- | --- | --- |
| 1 | Protection en temps réel | L'Agent Sécurité scanne à chaque commit, bloque les patterns dangereux avant le merge | Semgrep + Gitleaks en GitHub Action + hook de pré-commit |
| 2 | Pare-feu intelligent | L'Agent Infra valide les routes API, configs réseau, ports exposés, politiques CORS | OWASP ZAP + Checkov + analyseur de routes personnalisé |
| 3 | Protection anti-arnaque IA | L'Agent Supply Chain détecte les attaques de dépendances, le typosquatting, les scripts d'installation malveillants | Socket.dev + OSV-Scanner |
| 4 | LiveUpdate | La synchro de flux CVE récupère les dernières vulnérabilités, patche auto les dépendances, met à jour les règles Semgrep | Cloudflare Cron Triggers + flux OSV/NVD/GitHub Advisory |
| 5 | Sauvegarde cloud | L'Agent BDD impose la sécurité des migrations, snapshot avant migration, chemins de rollback | Prisma introspect + Atlas + snapshot-avant-migration |
| 6 | Surveillance du dark web | Le Scanner d'Exposition détecte les secrets divulgués dans les repos publics, sites de collage, bases de fuites | Trufflehog + scanner personnalisé de collages/fuites |
| 7 | Identité / Secrets | Le Gestionnaire de Secrets centralise tous les identifiants, effectue la rotation des clés, chiffre les fichiers env | Infisical ou Cloudflare Secrets Store |
| 8 | VPN | Le Pipeline de Déploiement assure le déploiement chiffré, CI/CD sécurisé, pas de texte clair en transit | GitHub Actions OIDC + Cloudflare Zero Trust |
| 9 | Performance | L'Agent Qualité supprime le code mort, optimise les bundles, évalue la maintenabilité | SonarQube + knip + Madge + Lighthouse |
| 10 | Contrôle parental | Le Moteur de Politiques applique les standards de codage, bloque les patterns dangereux avant le merge | Règles Semgrep personnalisées + YAML de politique configurable |

---

## Architecture — natif Cloudflare

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

**Pourquoi Cloudflare pour tout :** Zéro serveur à pirater. Zéro serveur à patcher. Zéro serveur à surveiller. Les Workers sont des isolats V8 — sandboxés par défaut, sans système de fichiers, sans réseau sauf ce que vous autorisez. Vous héritez des certifications de conformité de Cloudflare. L'infrastructure qui protège 20% d'Internet protège votre scanner.

---

## Les cinq divisions d'agents

### Division 1 : Agent Sécurité

| Sous-agent | Outil | Ce qu'il fait |
| --- | --- | --- |
| Scanner de secrets | Gitleaks + Trufflehog | Pré-commit + scan complet du repo pour les secrets codés en dur |
| Audit de dépendances | Socket.dev + OSV-Scanner + Snyk | Détection d'attaques de supply chain, scan CVE sur npm/pip/Go/Rust |
| SAST | Semgrep | Moteur de règles personnalisé pour injection SQL, XSS, contournement d'auth |
| Validateur d'auth | OWASP ZAP | Tests de pénétration automatisés contre le staging |
| Sécurité infra | Checkov + Trivy | Mauvaise configuration Terraform/Docker/K8s + scan de vulnérabilités de conteneurs |

### Division 2 : Agent Base de données

**Règle critique :** L'Agent BDD n'exécute JAMAIS de migrations automatiquement. Il génère des fichiers de migration et des PRs. Un humain doit réviser et approuver chaque changement de schéma.

### Division 3 : Agent Infrastructure

Génère des Dockerfiles, workflows CI/CD, configs de déploiement et gestion d'env — tous des fichiers de configuration, jamais d'exécution.

### Division 4 : Agent Observabilité

Remplace `console.log` par du logging structuré, injecte le SDK OpenTelemetry, le suivi d'erreurs Sentry, et génère des endpoints `/health` + `/ready`.

### Division 5 : Agent Qualité

Génère des suites de tests, trouve le code mort (knip), détecte les dépendances circulaires (Madge), et calcule les scores de préparation à la production.

---

## Barème de notation

| Catégorie | Poids | Points de contrôle exemples |
| --- | --- | --- |
| Sécurité | 35% | Secrets codés en dur (CWE-798), injection SQL (CWE-89), auth manquante (CWE-306), XSS (CWE-79) |
| Base de données | 20% | Pas d'historique de migration, pas de config de backup, index manquants, RLS désactivé |
| Infrastructure | 20% | Pas de Dockerfile, pas de CI/CD, secrets dans .env commité, pas de health check |
| Observabilité | 12,5% | Pas de suivi d'erreurs, console.log uniquement, pas d'endpoints de santé, pas de métriques |
| Qualité | 12,5% | Pas de tests, complexité cyclomatique élevée, code mort > 20%, dépendances circulaires |

### Grades

| Score | Grade | Signification |
| --- | --- | --- |
| 0–25 | F — Critique | Pas sûr pour la production. Vulnérabilités de sécurité actives. |
| 26–50 | D — Dangereux | Lacunes majeures. Quelques bases existent mais des problèmes critiques persistent. |
| 51–70 | C — Prudence | En bonne voie. Sécurité de base adressée, besoin de surveillance + tests. |
| 71–85 | B — Prêt pour la production | Sûr pour la production. Tous les problèmes critiques résolus. |
| 86–95 | A — Renforcé | Niveau entreprise. Couverture complète, surveillance continue. |
| 96–100 | A+ — Niveau Norton | Meilleur de sa catégorie. Tous les points de contrôle verts, surveillance complète active. |

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

## Modèle économique

| Niveau | Prix | Ce qui est inclus |
| --- | --- | --- |
| Gratuit | 0$ pour toujours | CLI scan + rapport + score + 20 règles personnalisées |
| Pro | 29$/mois | PRs auto-fix, surveillance continue, tableau de bord, 100 scans/mois |
| Team | 99$/mois | 5 places, SSO, politiques partagées, modèles de conformité, 500 scans/mois |
| Enterprise | Sur devis | Places illimitées, option auto-hébergée, règles personnalisées, SLA, support dédié |

---

## Ordre de construction

| Phase | Semaines | Livrables |
| --- | --- | --- |
| 1 : Scanner principal | 1–4 | CLI, détection de stack, agent sécurité, rapport markdown, score simplifié |
| 2 : Moteur de correction | 5–8 | Génération de PR auto-fix, agent BDD, agent infra, GitHub App |
| 3 : Agents complets | 9–12 | Agent observabilité, agent qualité, dashboard v1, facturation par crédits |
| 4 : Intelligence | 13–16 | Intégration AI Gateway, explications en langage clair, carte d'architecture |
| 5 : Continu | 17–20 | Synchro CVE, détection de dérive, scanning d'exposition, modèles de conformité |
| 6 : Échelle | 21–24 | Fonctionnalités team, SSO, guide d'auto-hébergement, expansion frameworks, validation du scoring |

---

## FAQ

**« Pourquoi ne pas juste utiliser Snyk/SonarQube/Dependabot directement ? »**
Ce sont des outils individuels. Last Mile 360 est la couche d'orchestration qui les exécute tous ensemble, fusionne leurs résultats, élimine les doublons, priorise par risque réel, génère des corrections et surveille en continu.

**« Pourquoi Cloudflare plutôt qu'AWS/Vercel ? »**
Zéro serveur d'origine. AWS nécessite des instances EC2/ECS. Vercel a des cold starts et un calcul limité. Les Workers Cloudflare sont des isolats V8 avec 0ms de cold start et la conformité SOC2/ISO 27001 héritée. Pour un produit de sécurité, la plateforme doit être plus sécurisée que le code qu'elle scanne.

**« Pourquoi pas de modèles auto-hébergés ? »**
Confiance. Un modèle auto-hébergé sur votre infrastructure est un passif, pas un atout. L'API Claude, OpenAI et Workers AI sont des fournisseurs conformes SOC2 avec des équipes de réponse sécurité.

**« Comment mon code est-il protégé ? »**
Chiffrement AES-256-GCM côté client avant upload. URL pré-signée directe vers R2. Suppression automatique après scan. DLP sur toutes les requêtes AI Gateway. Workers AI traite le code sans quitter le réseau de Cloudflare.

---

## Crédits

Créé par [John Williams](https://github.com/itallstartedwithaidea) — Senior Paid Media Specialist chez Seer Interactive, créateur de [googleadsagent.ai](https://googleadsagent.ai), coach au Casteel High School, ancien joueur de football à Washington State (2002–2005).

Norton 360 est une marque déposée de Gen Digital Inc. Last Mile 360 n'est pas affilié à Norton ou Gen Digital. Le modèle de confiance Norton est référencé comme inspiration architecturale et benchmark.

---

*« Ces derniers 10% représentent 95% du travail. Nous faisons ce travail. »*

Licence MIT · Partagez-le. Enseignez-le. Forkez-le.
