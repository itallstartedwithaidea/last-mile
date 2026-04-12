# Last Mile 360

[English](README.md) | [Français](README.fr.md) | [Español](README.es.md) | [中文](README.zh.md) | [Nederlands](README.nl.md) | [Русский](README.ru.md) | [한국어](README.ko.md)

> **Estado: Fase 1 — Construyendo el escáner principal.** Arquitectura definida, monorepo estructurado, agente de seguridad en desarrollo. Ver [Orden de construcción](#orden-de-construcción) para la hoja de ruta completa.

La plataforma de preparación para producción de apps codificadas intuitivamente. Confianza nivel Norton. Nativo de Cloudflare. Cero servidores de origen.

**"Tenemos una app terminada al 90%. Solo necesitamos ayuda para corregir los últimos errores."** Ese último 10% es el 95% del trabajo. Esta herramienta hace ese trabajo.

---

## Qué es esto

Last Mile 360 es una herramienta única que toma cualquier aplicación codificada intuitivamente y la hace segura para producción. Escanea, puntúa, corrige y monitorea continuamente las bases de código en cinco dimensiones: **seguridad, seguridad de base de datos, infraestructura, observabilidad y calidad del código**.

Consolida las mejores capacidades de más de 15 frameworks de agentes de código abierto, motores de inferencia, sistemas de memoria y herramientas de uso de computadoras en una sola plataforma nativa de Cloudflare segura, sin infraestructura auto-alojada.

**Lo que no es:** No arregla tu product-market fit. No rediseña tu UX. No optimiza tu lógica de negocio. Hace que tu código sea seguro para ponerlo frente a usuarios reales con datos reales.

---

## Inicio rápido (disponible en Fase 1)

El CLI está en desarrollo activo. Cuando la Fase 1 se lance:

```bash
npm install -g @last-mile/cli
last-mile login
cd your-vibe-coded-app
last-mile scan       # Escanea seguridad, bd, infra, observabilidad, calidad
last-mile score      # Ve tu puntuación de preparación para producción (0-100)
last-mile fix        # Auto-corrige lo corregible vía PR
last-mile monitor    # Activa monitoreo continuo
```

### Desarrollo (contribuye ahora)

```bash
git clone https://github.com/itallstartedwithaidea/last-mile.git
cd last-mile
npm install
npm run dev
```

Ver [CONTRIBUTING.md](CONTRIBUTING.md) para instrucciones completas de configuración.

---

## El modelo de confianza Norton 360

Norton 360 construyó la confianza del consumidor mediante principios fundamentales. Cada uno corresponde a una brecha de código en producción.

| # | Principio Norton | Equivalente Last Mile | Implementación |
| --- | --- | --- | --- |
| 1 | Protección en tiempo real | El Agente de Seguridad escanea en cada commit, bloquea patrones peligrosos antes del merge | Semgrep + Gitleaks como GitHub Action + hook de pre-commit |
| 2 | Firewall inteligente | El Agente de Infra valida rutas API, configs de red, puertos expuestos, políticas CORS | OWASP ZAP + Checkov + analizador de rutas personalizado |
| 3 | Protección contra estafas IA | El Agente de Supply Chain detecta ataques a dependencias, typosquatting, scripts de instalación maliciosos | Socket.dev + OSV-Scanner |
| 4 | LiveUpdate | La sincronización de feeds CVE obtiene últimas vulnerabilidades, parchea deps automáticamente, actualiza reglas Semgrep | Cloudflare Cron Triggers + feeds OSV/NVD/GitHub Advisory |
| 5 | Backup en la nube | El Agente de BD impone seguridad de migraciones, snapshot antes de migrar, rutas de rollback | Prisma introspect + Atlas + snapshot-antes-de-migrar |
| 6 | Monitoreo de dark web | El Escáner de Exposición detecta secretos filtrados en repos públicos, sitios de pegado, bases de datos de brechas | Trufflehog + escáner personalizado de pegados/brechas |
| 7 | Identidad / Secretos | El Gestor de Secretos centraliza todas las credenciales, rota claves, encripta archivos env | Infisical o Cloudflare Secrets Store |
| 8 | VPN | El Pipeline de Deploy asegura despliegue encriptado, CI/CD seguro, sin texto plano en tránsito | GitHub Actions OIDC + Cloudflare Zero Trust |
| 9 | Rendimiento | El Agente de Calidad elimina código muerto, optimiza bundles, evalúa mantenibilidad | SonarQube + knip + Madge + Lighthouse |
| 10 | Control parental | El Motor de Políticas aplica estándares de codificación, bloquea patrones peligrosos antes del merge | Reglas Semgrep personalizadas + YAML de políticas configurable |

---

## Las cinco divisiones de agentes

### División 1: Agente de Seguridad

| Sub-agente | Herramienta | Qué hace |
| --- | --- | --- |
| Escáner de secretos | Gitleaks + Trufflehog | Pre-commit + escaneo completo del repo para secretos codificados |
| Auditoría de dependencias | Socket.dev + OSV-Scanner + Snyk | Detección de ataques de supply chain, escaneo CVE en npm/pip/Go/Rust |
| SAST | Semgrep | Motor de reglas personalizado para inyección SQL, XSS, bypass de auth |
| Validador de auth | OWASP ZAP | Testing de penetración automatizado contra staging |
| Seguridad de infra | Checkov + Trivy | Mala configuración Terraform/Docker/K8s + escaneo de vulnerabilidades de contenedores |

### División 2: Agente de Base de datos

**Regla crítica:** El Agente de BD NUNCA ejecuta migraciones automáticamente. Genera archivos de migración y PRs. Un humano debe revisar y aprobar cada cambio de esquema.

### División 3: Agente de Infraestructura

Genera Dockerfiles, workflows CI/CD, configs de despliegue y gestión de env — todos archivos de configuración, nunca ejecuta.

### División 4: Agente de Observabilidad

Reemplaza `console.log` con logging estructurado, inyecta SDK de OpenTelemetry, tracking de errores Sentry, y genera endpoints `/health` + `/ready`.

### División 5: Agente de Calidad

Genera suites de tests, encuentra código muerto (knip), detecta dependencias circulares (Madge), y calcula puntuaciones de preparación para producción.

---

## Rúbrica de puntuación

| Categoría | Peso | Puntos de control ejemplo |
| --- | --- | --- |
| Seguridad | 35% | Secretos codificados (CWE-798), inyección SQL (CWE-89), auth faltante (CWE-306), XSS (CWE-79) |
| Base de datos | 20% | Sin historial de migraciones, sin config de backup, índices faltantes, RLS deshabilitado |
| Infraestructura | 20% | Sin Dockerfile, sin CI/CD, secretos en .env commiteado, sin health check |
| Observabilidad | 12.5% | Sin tracking de errores, solo console.log, sin endpoints de salud, sin métricas |
| Calidad | 12.5% | Sin tests, alta complejidad ciclomática, código muerto > 20%, deps circulares |

### Grados

| Puntuación | Grado | Significado |
| --- | --- | --- |
| 0–25 | F — Crítico | No es seguro para producción. Vulnerabilidades de seguridad activas. |
| 26–50 | D — Peligroso | Brechas importantes. Existen algunos básicos pero persisten problemas críticos. |
| 51–70 | C — Precaución | En camino. Seguridad básica atendida, necesita monitoreo + tests. |
| 71–85 | B — Listo para producción | Seguro para producción. Todos los hallazgos críticos atendidos. |
| 86–95 | A — Endurecido | Nivel enterprise. Cobertura completa, monitoreo continuo. |
| 96–100 | A+ — Nivel Norton | Lo mejor de su clase. Todos los puntos verdes, monitoreo completo activo. |

---

## Modelo de negocio

| Nivel | Precio | Qué incluye |
| --- | --- | --- |
| Gratis | $0 para siempre | CLI scan + reporte + score + 20 reglas personalizadas |
| Pro | $29/mes | PRs auto-fix, monitoreo continuo, dashboard, 100 scans/mes |
| Team | $99/mes | 5 asientos, SSO, políticas compartidas, plantillas de cumplimiento, 500 scans/mes |
| Enterprise | Personalizado | Asientos ilimitados, opción auto-alojada, reglas personalizadas, SLA, soporte dedicado |

---

## FAQ

**"¿Por qué no usar Snyk/SonarQube/Dependabot directamente?"**
Son herramientas individuales. Last Mile 360 es la capa de orquestación que las ejecuta todas juntas, fusiona sus resultados, elimina duplicados, prioriza por riesgo real, genera correcciones y monitorea continuamente.

**"¿Por qué Cloudflare en vez de AWS/Vercel?"**
Cero servidores de origen. AWS requiere instancias EC2/ECS. Vercel tiene cold starts y computación limitada. Los Workers de Cloudflare son aislados V8 con 0ms de cold start y cumplimiento SOC2/ISO 27001 heredado. Para un producto de seguridad, la plataforma debe ser más segura que el código que escanea.

---

## Créditos

Creado por [John Williams](https://github.com/itallstartedwithaidea) — Senior Paid Media Specialist en Seer Interactive, creador de [googleadsagent.ai](https://googleadsagent.ai), coach en Casteel High School, ex jugador de football en Washington State (2002–2005).

Norton 360 es una marca registrada de Gen Digital Inc. Last Mile 360 no está afiliado con Norton o Gen Digital.

---

*"Ese último 10% es el 95% del trabajo. Nosotros hacemos ese trabajo."*

Licencia MIT · Compártelo. Enséñalo. Forkéalo.
