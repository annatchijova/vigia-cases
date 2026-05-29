# vigia-cases

**VIGÍA Análisis de Intencionalidad Forense — Dataset Público de Benchmark DFIR**

Curado por **Anna Tchijova** | Verificado por el Colectivo VIGÍA (Claude, Kimi, Gemini, DeepSeek, ChatGPT, Qwen)
Preparado para el **SANS FIND EVIL Hackathon 2026**

> 🇺🇸 [English version → README.md](./README.md)

---

## Qué hay acá

10 casos forenses reales de fuentes públicas verificadas (NIST CFReDS, DFRWS, Digital Corpora, Ali Hadi, Volatility Foundation), convertidos al formato canónico VIGÍA para benchmarking de agentes de análisis de intencionalidad forense.

Cada caso incluye:

- `case.json` — descriptor del incidente + artifacts (input del agente, **sin ground truth expuesto**)
- `ground_truth.json` — veredicto canónico, TTPs MITRE, clasificación Peirce, IOCs
- `manifest.json` — hashes SHA-256 por archivo

---

## Estructura del repositorio

```
vigia-cases/
├── LICENSE                    Apache 2.0
├── README.md                  Versión en inglés
├── README_ES.md               Este archivo (español)
├── SCORING_GUIDE.md           Cómo puntuar agentes contra este dataset
├── index.json                 Índice global de todos los casos + metadatos
├── hashes.sha256              Hashes SHA-256 de todos los archivos del repo
└── cases/
    ├── VIGIA-REAL-001/        NIST Hacking Case (Greg Schardt / Mr. Evil)
    ├── VIGIA-REAL-002/        NIST Data Leakage (insider threat)
    ├── VIGIA-REAL-003/        Ali Hadi Web Server Compromise
    ├── VIGIA-REAL-004/        Ali Hadi SysInternals Malware
    ├── VIGIA-REAL-005/        Ali Hadi Encrypt Them All (SUSPICION, no MALICE)
    ├── VIGIA-REAL-006/        Digital Corpora M57-Jean Spear-Phishing
    ├── VIGIA-REAL-007/        Digital Corpora Nitroba Harassment
    ├── VIGIA-REAL-008/        Volatility Cridex Banking Trojan
    ├── VIGIA-REAL-009/        DFRWS 2008 Linux Exfiltration
    └── VIGIA-REAL-010/        DFRWS 2011 Android Espionage
```

---

## Clasificación por usabilidad

Clasificación aplicada por **Rob T. Lee** (SANS) en el contexto del hackathon.

### ✅ Score against — scoring confiable

| Caso | Fuente | Incidente |
|------|--------|-----------|
| VIGIA-REAL-007 | Digital Corpora — Nitroba | Forensics de red, atribución por cookies Gmail |
| VIGIA-REAL-002 | NIST CFReDS — Data Leakage | Exfiltración insider + anti-forensics |
| VIGIA-REAL-001 | NIST CFReDS — Hacking Case | War driving, robo de credenciales |

Ground truth verificable contra answer keys o hashes canónicos confirmados.

### ⚠️ Build and test — score con cuidado

| Caso | Fuente | Incidente | Nota |
|------|--------|-----------|------|
| VIGIA-REAL-005 | Ali Hadi #9 | Ocultamiento con cifrado | **Test de falso positivo intencional: SUSPICION, no MALICE** |
| VIGIA-REAL-003 | Ali Hadi #1 | Compromiso web → persistencia | Disco + memoria, respuestas instructor-gated |
| VIGIA-REAL-009 | DFRWS 2008 | Exfiltración Linux desde admin-share | Ground truth construido por Anna Tchijova |

Las soluciones existen en literatura académica. Reportar si el agente razonó o recordó.

### 🔵 Practice only — no scoring

| Caso | Fuente | Razón |
|------|--------|-------|
| VIGIA-REAL-006 | Digital Corpora M57-Jean | Soluciones ampliamente publicadas |
| VIGIA-REAL-004 | Ali Hadi #7 SysInternals | E01 instructor-gated |

### 🔴 Not ready — no usar

| Caso | Razón |
|------|-------|
| VIGIA-REAL-010 | Evidencia en Dropbox personal (volátil); hashes del README etiquetados como MD5 son en realidad SHA1 |
| VIGIA-REAL-008 | Descarga canónica muerta; repo archivado read-only desde mayo 2025 |

---

## Nota crítica: VIGIA-REAL-005

Este caso es el **test de falso positivo**. El veredicto esperado es `SUSPICION`, no `MALICE`. Un agente que dispara `MALICE` aquí falla la puerta de especificidad. El uso de múltiples capas de cifrado es ambiguo — puede ser práctica legítima de seguridad personal.

---

## Cómo usar este dataset con VIGÍA

```bash
# Clonar el repositorio principal de VIGÍA
git clone git@github.com:annatchijova/vigia-intent-analysis.git
cd vigia-intent-analysis

# Correr un caso contra el engine
python3 run_case.py cases/VIGIA-REAL-007/case.json

# Comparar contra ground truth
python3 run_case.py cases/VIGIA-REAL-007/case.json \
    --ground-truth cases/VIGIA-REAL-007/ground_truth.json
```

---

## Verificación de integridad

```bash
sha256sum --check hashes.sha256
```

---

## Scoring

Ver `SCORING_GUIDE.md` para métricas completas, umbrales y protocolo de reporte.

M�tricas principales:

- **Verdict Accuracy** — % de veredictos correctos sobre casos tier `score_against`
- **FPR** — tasa de falso positivo (VIGIA-REAL-005 es el test de especificidad dedicado)
- **FNR-MAL** — casos maliciosos clasificados como BENIGN/NOISE
- **TTP Coverage** — % de TTPs MITRE correctamente identificados

---

## Licencia

Apache License 2.0 — ver `LICENSE`.

Los datasets fuente son de fuentes públicas verificadas con sus propias licencias:
- NIST CFReDS: dominio público
- Digital Corpora: CC BY
- DFRWS Challenges: open access
- Ali Hadi Challenges: uso educativo

---

## Créditos

Casos curados por **Anna Tchijova** en dos días con conexión lenta.
Ground truth construido por el **Colectivo VIGÍA** con auditoría cruzada de 7 modelos.
Verificación de hashes confirmada contra fuentes canónicas.

Proyecto principal: [github.com/annatchijova/vigia-intent-analysis](https://github.com/annatchijova/vigia-intent-analysis)
