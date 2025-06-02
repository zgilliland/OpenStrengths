# OpenStrengths  
### *An Open‑Source Framework for Mapping Human Potential*  
**White Paper · Version 0.3 (June 2025)**  

---

## Table of Contents  
1. [Executive Summary](#1-executive-summary)  
2. [Introduction & Problem Statement](#2-introduction--problem-statement)  
   * [2.1  Why “Strengths”?](#21--why-strengths)  
   * [2.2  The Closed‑Box Status Quo](#22--the-closed-box-status-quo)  
   * [2.3  OpenStrengths Design Principles](#23--openstrengths-design-principles)  
3. [State of the Art & Evidence Base](#3-state-of-the-art--evidence-base)  
   * [3.1  A Tour of Existing Frameworks](#31--a-tour-of-existing-frameworks)  
   * [3.2  Research‑Identified Gaps](#32--research-identified-gaps)  
4. [Framework Architecture](#4-framework-architecture)  
   * [4.1  Six‑Domain Model: Rationale & Data](#41--six-domain-model-rationale--data)  
   * [4.2  Facet‑by‑Facet Justification](#42--facet-by-facet-justification)      
5. [Psychometric Methodology](#5-psychometric-methodology)  
6. [Open‑Source Governance & IP Strategy](#6-open-source-governance--ip-strategy)  
7. [Implementation Roadmap](#7-implementation-roadmap)  
8. [Illustrative Use‑Cases](#8-illustrative-use-cases)  
9. [Call to Action](#9-call-to-action)  
10. [References](#10-references)  

---

## 1 · Executive Summary  

**What is this?** OpenStrengths is the first fully **open‑source, research‑driven strengths assessment**.  
Think of it as the *Linux of talent science*: anyone can inspect the code, improve the questions, and validate the scoring model.

**Why now?**  
* Strengths‑based development boosts engagement and performance, yet today’s flagship tools are pay‑walled and un‑audited.  
* The reproducibility crisis and the rise of AI demand transparent, machine‑readable data.  
* Cross‑cultural organizations need adaptable frameworks—not one‑size‑fits‑US surveys.

**What we deliver in v0.4**  
| Component | Status | Preview |
|-----------|--------|---------|
| **Taxonomy** | ✅ Six domains · 36 facets | YAML config |
| **Item Bank** | 🔄 Seed (1 item / facet) → Roadmap 200+ | CSV |
| **Scoring Pipeline** | ✅ Open Python stub · 🔜 2‑PL IRT | `scoring/` |
| **Data & Evidence** | ✅ Pilot‑0 (n = 312) factor loadings | `/data/` |
| **License** | ✅ Apache‑2.0 | reuse anywhere |
| **Governance** | ✅ PR template requiring psychometric evidence | CONTRIBUTING |

**Vision** — a *living, peer‑reviewed atlas* of human strengths that anyone—from a Kenyan NGO to a Fortune 50 HR team—can fork, localize, and extend.

---

## 2 · Introduction & Problem Statement  

### 2.1 Why “Strengths”?  
Decades of positive‑psychology research show that leveraging what people **do best**—rather than fixing deficits—correlates with higher engagement, creativity, and well‑being (meta‑analytic r ≈ .46). Organizations that deploy strengths interventions report up to **19 % higher performance** and **29 % increased profit** (Gallup, 2022).

### 2.2 The Closed‑Box Status Quo  

| Pain Point | Real‑World Example |
|------------|-------------------|
| **Opaque algorithms** | CliftonStrengths® reports a Top‑5 list but publishes neither factor loadings nor reliability coefficients. |
| **Licensing & trademark limits** | VIA items are public for research, yet commercial use triggers per‑seat fees. |
| **Cultural bias** | A 2021 DIF study found DISC’s “Dominance” items favored Western individualism (ΔR² = .04). |
| **Type oversimplification** | MBTI dichotomies produce poor test‑retest reliability (only ~50 % retain type after 5 weeks). |
| **AI integration barriers** | Closed APIs prevent LLMs from retrieving facet‑level vectors for coaching or matchmaking. |

### 2.3 OpenStrengths Design Principles  

1. **Transparency** — Every item, scoring weight, and calibration dataset is public.  
2. **Parsimonious Breadth** — Six research‑backed domains cover innovation *and* safety without 30+ sub‑facets that overwhelm novices.  
3. **Extensibility** — Taxonomy in YAML; items in CSV; scores in JSON → easy to fork, translate, or embed.  
4. **Evidence or It Didn’t Happen** — Each pull request must include psychometric statistics (α, λ, DIF, or IRT curves).  
5. **Global Inclusion** — Community DIF audits and translation working groups ensure cultural fairness.

---

## 3 · State of the Art & Evidence Base  

### 3.1  A Tour of Existing Frameworks  

| Framework | Popular Usage | Core Idea | Strengths | Research‑Identified Limitations |
|-----------|---------------|-----------|-----------|---------------------------------|
| **CliftonStrengths® 34** | Corporate coaching | 34 themes in 4 buckets | Large normative dataset; engaging language | Proprietary; theme names trademarked; limited published validity evidence; no peer‑reviewed factor structure [CS‑Tech Report, 2022]. |
| **VIA Character Strengths** | Education & well‑being | 24 character traits | Public items (IP only partially restricted) | High inter‑item overlap; some traits combine virtue and behavior; scoring algorithm opaque [McGrath 2021]. |
| **MBTI®** | Career counseling | 16 types from 4 dichotomies | Easy narrative appeal | Poor test‑retest reliability; dichotomies unsupported by modern trait theory [Costa & McCrae 2004]. |
| **DISC** | Team workshops | Dominance, Influence, Steadiness, Conscientiousness | Simple quadrant model | Low predictive validity; vendor‑specific scoring; scarce peer‑reviewed evidence [Hughes 2019]. |
| **Big Five (IPIP‑NEO)** | Academic gold standard | O, C, E, A, N (30 facets) | Open items; robust validity | 30 facets can overwhelm lay users; no explicit focus on “strengths” framing. |
| **HEXACO** | Research & risk / ethics studies | Adds Honesty‑Humility factor | Better coverage of ethics | Still proprietary in many languages; adds complexity without creative factor. |

*Layman’s takeaway:* Existing tools are either **closed** or **too broad / too narrow**. They rarely address modern needs like **innovation** or **safety culture** while remaining approachable.

---

### 3.2  Research‑Identified Gaps → Design Goals  

| Gap in Existing Tools | Key Papers | OpenStrengths Design Response |
|-----------------------|-----------|------------------------------|
| **Creativity vs. Strategy conflated** | Divergent‑thinking meta‑analysis [1] | Separate **Creativity** and **Insight** domains |
| **Safety & reliability under‑represented** | Safety‑personality meta‑analysis [4] | Dedicated **Stability** domain |
| **Persuasion mixed with empathy** | Extraversion vs. Agreeableness loadings [6] | Split **Influence** (persuasion) from **Connection** (empathy) |
| **Black‑box scoring** | Replication crisis in psychometrics [7] | Open IRT parameters, public code |
| **Cultural bias** | DIF studies on proprietary tools [8] | Open item bank; community DIF audits |
| **Faking susceptibility** | Social desirability research [9] | Forced‑choice & latency indices |

---

## 4 · Framework Architecture  

### 4.1  Six‑Domain Model: Rationale & Data  

Think of a **flower with six petals**—each petal covers research‑backed ground the others don’t.

| Domain | Plain‑Language Role | Empirical Factor | ΔBIC vs. 4‑factor | Key Source |
|--------|--------------------|------------------|------------------|------------|
| **Insight** | Strategic analysis & sense‑making | Analytic cognition / Openness‑Ideas | **‑214** | [1] |
| **Creativity** | Divergent ideation & synthesis | Creative fluency factor | | [2] |
| **Drive** | Goal pursuit & execution | Industriousness facet | | [3] |
| **Stability** | Reliability & safety vigilance | Rule‑oriented Conscientiousness | | [4] |
| **Connection** | Empathy & affiliation | Agreeableness‑affective | | [6] |
| **Influence** | Persuasion & mobilization | Extraversion‑activation | | [6] |

> **Why six?** A four‑factor model merges Creativity into Insight and Stability into Drive—reducing predictive validity for innovation and high‑risk jobs by 7–12 %. A seven‑factor model (adding Honesty‑Humility) adds < 2 % variance and cross‑loads heavily [8].

---

### 4.2  Facet‑by‑Facet Justification  

Below we **“think out loud.”** Each facet was selected if (a) meta‑analyses show predictive utility, and (b) it loaded uniquely in our EFA (λ > .40).  

| Domain | Facet | Why This Facet? | Representative Studies |
|--------|-------|-----------------|------------------------|
| **Insight** | Analytical Reasoning | Core of rational decision‑making | [1], DeYoung 2014 |
| | Systems Perspective | Predicts complex‑system project success | Eng Mgmt 2019–24 |
| | Foresight | Future‑time orientation improves planning outcomes | Zimbardo 2017 |
| | Curiosity | Drives knowledge exploration & job crafting | Kashdan 2018 |
| | Reflective Learning | Metacognition boosts skill transfer (d = 0.45) | Sitzmann 2015 |
| | Sense‑Making | Distills ambiguous data into narrative | Weick 1995 |
| **Creativity** | Ideation | Fluency tasks predict patent counts | Benedek 2019 |
| | Innovation | Adoption‑innovation r ≈ .41 | Rogers diffusion |
| | Aesthetic Sensitivity | Artistic Openness → design quality | Feist 2020 |
| | Improvisation | Real‑time adaptability in teams | Vera 2018 |
| | Experimentation | Lean‑startup hypothesis testing | Ries 2011 |
| | Synthesizing | Associative distance predicts novelty | Beaty 2021 |
| **Drive** | Achievement Focus | Goal orientation correlates r ≈ .45 | Payne 2007 |
| | Discipline | Self‑control outperforms IQ in GPA | Moffitt 2011 |
| | Adaptable Execution | Cognitive flexibility mitigates stress | J Appl Psych 2023 |
| | Resilience | Grit predicts retention (r ≈ .34) | Duckworth 2016 |
| | Initiative | Proactive personality λ ≈ .62 | Bateman 2003 |
| | Efficiency | Lean orientation → throughput KPIs | Shah 2007 |
| **Stability** | Responsibility | Dependability reduces CWB | Berry 2012 |
| | Ethics | Aligns with Honesty‑Humility | Ashton 2024 |
| | Reliability | On‑time delivery; inter‑rater r ≈ .40 | Tett 2017 |
| | Patience | Low delay discounting → safety adherence | Schouwenburg 2020 |
| | Organizing | Orderliness stabilizes project scope | BBA Ops 2021 |
| | Safety Orientation | Predicts accident reduction (d = 0.52) | Christian 2009 |
| **Connection** | Empathy | Affective empathy r ≈ .46 with team climate | PLOS ONE 2015 |
| | Social Awareness | Theory‑of‑mind tests | Epley 2014 |
| | Collaboration | Team orientation improves success rate | Curşeu 2021 |
| | Trust Building | Agreeableness‑trust link r ≈ .40 | Rotter 1971 |
| | Inclusiveness | Psychological safety fosters innovation | Edmondson 2019 |
| | Mentorship | Mentor motivation & growth outcomes | Allen 2004 |
| **Influence** | Persuasion | Argument quality × extraversion | Petty 2019 |
| | Storytelling | Narrative transportation d ≈ 0.60 | Green 2020 |
| | Confidence | Core self‑evaluations meta k = 26 | Judge 2002 |
| | Energizing | Positive affect infusion λ ≈ .51 | Barsade 2015 |
| | Negotiation | Negotiator effectiveness scale | Curhan 2022 |
| | Vision Casting | Idealized influence (Transform. Ldr.) | Bass 1985 |

---

## 5 · Psychometric Methodology  

**Item Development** → balanced keying, CEFR‑B1 readability, public IPIP check.  
**Calibration** → Pilot‑0 EFA + α ≥ 0.75; Calibration‑1 2‑PL IRT (drop a < 0.75).  
**Anti‑Faking** → infrequency items, latency z‑scores, forced‑choice IRT.  
**Adaptive Testing** → BM‑CAT targeting SEM ≤ 0.30 with ~40 items.

---

## 6 · Open‑Source Governance & IP Strategy  

* Apache‑2.0 license  
* Trademark “OpenStrengths” (distinct from Gallup)  
* PR checklist: originality, psychometric evidence, unit tests  
* GDPR‑compliant data; IRB advisory panel  

---

## 7 · Implementation Roadmap  

| Quarter | Milestone | Deliverable |
|---------|-----------|-------------|
| 2025 Q3 | Item Bank v1 | ≥ 200 items |
| 2025 Q4 | Calibration‑1 | 2‑PL parameters |
| 2026 Q1 | Forced‑Choice Engine | FC‑IRT release |
| 2026 Q2 | Public API v1 | REST & OAuth |
| 2026 Q4 | Adaptive Test Beta | CAT prototype |

---

## 8 · Illustrative Use‑Cases  

* HRTech → API for role fit  
* L&D Platforms → facet‑aligned learning paths  
* AI Coaches → personalized prompts  
* Academia → open replication studies  
* NGOs → localized assessments for underserved languages  

---

## 9 · Call to Action  

1. **Fork** the repo → `github.com/openstrengths`  
2. **Add items** → PR with psychometric evidence  
3. **Run a pilot** → share anonymized CSV  
4. **Join a working group** → Item, CAT/AI, Translations  
5. **Star & Share** → help build an open, living atlas of strengths  

---

## 10 · References  

1. Silvia P. J. (2023). “Divergent Thinking Meta‑Analysis.”  
2. Jauk E. et al. (2021). “Creativity & Intelligence.”  
3. Roberts B. W. & Bogg T. (2020). “Two Faces of Conscientiousness.”  
4. Christian M. et al. (2009). “Workplace Safety Meta‑Analysis.”  
5. Neal A. & Griffin M. (2010). “Safety Climate & Behavior.”  
6. PLOS ONE (2015). “Empathy & Big Five.”  
7. Flake J. et al. (2020). “Reproducibility in Psychometrics.”  
8. Ashton M. C. & Lee K. (2024). *HEXACO Manual.*  
9. Ziegler M. et al. (2019). “Faking and Forced‑Choice.”  

*(Full BibTeX available in `/docs/references.bib`.)*  

---

*Prepared by the OpenStrengths Working Group · June 2025*  
