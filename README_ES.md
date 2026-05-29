# vigia-cases

**VIGÃA AnÃ¡lisis de Intencionalidad Forense â Dataset PÃºblico de Benchmark DFIR**

Curado por **Anna Tchijova** | Verificado por el Colectivo VIGÃA (Claude, Kimi, Gemini, DeepSeek, ChatGPT, Qwen)
Preparado para el **SANS FIND EVIL Hackathon 2026**

> ðºð¸ [English version â README.md](./README.md)

---

## QuÃ© hay acÃ¡

10 casos forenses reales de fuentes pÃºblicas verificadas (NIST CFReDS, DFRWS, Digital Corpora, Ali Hadi, Volatility Foundation), convertidos al formato canÃ³nico VIGÃA para benchmarking de agentes de anÃ¡lisis de intencionalidad forense.

Cada caso incluye:

- `case.json` â descriptor del incidente + artifacts (input del agente, **sin ground truth expuesto**)
- `ground_truth.json` â veredicto canÃ³nico, TTPs MITRE, clasificaciÃ³n Peirce, IOCs
- `manifest.json` â hashes SHA-256 por archivo

---

## Estructura del repositorio

```
vigia-cases/
âââ LICENSE                    Apache 2.0
âââ README.md                  VersiÃ³n en inglÃ©s
âââ README_ES.md               Este archivo (espaÃ±ol)
âââ SCORING_GUIDE.md           CÃ³mo puntuar agentes contra este dataset
âââ index.json                 Ãndice global de todos los casos + metadatos
âââ hashes.sha256              Hashes SHA-256 de todos los archivos del repo
âââ cases/
    âââ VIGIA-REAL-001/        NIST Hacking Case (Greg Schardt / Mr. Evil)
    âââ VIGIA-REAL-002/        NIST Data Leakage (insider threat)
    âââ VIGIA-REAL-003/        Ali Hadi Web Server Compromise
    âââ VIGIA-REAL-004/        Ali Hadi SysInternals Malware
    âââ VIGIA-REAL-005/        Ali Hadi Encrypt Them All (SUSPICION, no MALICE)
    âââ VIGIA-REAL-006/        Digital Corpora M57-Jean Spear-Phishing
    âââ VIGIA-REAL-007/        Digital Corpora Nitroba Harassment
    âââ VIGIA-REAL-008/        Volatility Cridex Banking Trojan
    âââ VIGIA-REAL-009/        DFRWS 2008 Linux Exfiltration
    âââ VIGIA-REAL-010/        DFRWS 2011 Android Espionage
```

---

## ClasificaciÃ³n por usabilidad

ClasificaciÃ³n aplicada por **Rob T. Lee** (SANS) en el contexto del hackathon.

### â Score against â scoring confiable

| Caso | Fuente | Incidente |
|------|--------|-----------|
| VIGIA-REAL-007 | Digital Corpora â Nitroba | Forensics de red, atribuciÃ³n por cookies Gmail |
| VIGIA-REAL-002 | NIST CFReDS â Data Leakage | ExfiltraciÃ³n insider + anti-forensics |
| VIGIA-REAL-001 | NIST CFReDS â Hacking Case | War driving, robo de credenciales |

Ground truth verificable contra answer keys o hashes canÃ³nicos confirmados.

### â ï¸ Build and test â score con cuidado

| Caso | Fuente | Incidente | Nota |
|------|--------|-----------|------|
| VIGIA-REAL-005 | Ali Hadi #9 | Ocultamiento con cifrado | **Test de falso positivo intencional: SUSPICION, no MALICE** |
| VIGIA-REAL-003 | Ali Hadi #1 | Compromiso web â persistencia | Disco + memoria, respuestas instructor-gated |
| VIGIA-REAL-009 | DFRWS 2008 | ExfiltraciÃ³n Linux desde admin-share | Ground truth construido por Anna Tchijova |

Las soluciones existen en literatura acadÃ©mica. Reportar si el agente razonÃ³ o recordÃ³.

### ðµ Practice only â no scoring

| Caso | Fuente | RazÃ³n |
|------|--------|-------|
| VIGIA-REAL-006 | Digital Corpora M57-Jean | Soluciones ampliamente publicadas |
| VIGIA-REAL-004 | Ali Hadi #7 SysInternals | E01 instructor-gated |

### ð´ Not ready â no usar

| Caso | RazÃ³n |
|------|-------|
| VIGIA-REAL-010 | Evidencia en Dropbox personal (volÃ¡til); hashes del README etiquetados como MD5 son en realidad SHA1 |
| VIGIA-REAL-008 | Descarga canÃ³nica muerta; repo archivado read-only desde mayo 2025 |

---

## Nota crÃ­tica: VIGIA-REAL-005

Este caso es el **test de falso positivo**. El veredicto esperado es `SUSPICION`, no `MALICE`. Un agente que dispara `MALICE` aquÃ­ falla la puerta de especificidad. El uso de mÃºltiples capas de cifrado es ambiguo â puede ser prÃ¡ctica legÃ­tima de seguridad personal.

---

## CÃ³mo usar este dataset con VIGÃA

```bash
# Clonar el repositorio principal de VIGÃA
git clone git@github.com:annatchijova/vigia-intent-analysis.git
cd vigia-intent-analysis

# Correr un caso contra el engine
python3 run_case.py cases/VIGIA-REAL-007/case.json

# Comparar contra ground truth
python3 run_case.py cases/VIGIA-REAL-007/case.json \
    --ground-truth cases/VIGIA-REAL-007/ground_truth.json
```

---

## VerificaciÃ³n de integridad

```bash
sha256sum --check hashes.sha256
```

---

## Scoring

Ver `SCORING_GUIDE.md` para mÃ©tricas completas, umbrales y protocolo de reporte.

M©tricas principales:

- **Verdict Accuracy** â % de veredictos correctos sobre casos tier `score_against`
- **FPR** â tasa de falso positivo (VIGIA-REAL-005 es el test de especificidad dedicado)
- **FNR-MAL** â casos maliciosos clasificados como BENIGN/NOISE
- **TTP Coverage** â % de TTPs MITRE correctamente identificados

---

## Licencia

Apache License 2.0 â ver `LICENSE`.

Los datasets fuente son de fuentes pÃºblicas verificadas con sus propias licencias:
- NIST CFReDS: dominio pÃºblico
- Digital Corpora: CC BY
- DFRWS Challenges: open access
- Ali Hadi Challenges: uso educativo

---

## CrÃ©ditos

Casos curados por **Anna Tchijova** en dos dÃ­as con conexiÃ³n lenta.
Ground truth construido por el **Colectivo VIGÃA** con auditorÃ­a cruzada de 7 modelos.
VerificaciÃ³n de hashes confirmada contra fuentes canÃ³nicas.

Proyecto principal: [github.com/annatchijova/vigia-intent-analysis](https://github.com/annatchijova/vigia-intent-analysis)
