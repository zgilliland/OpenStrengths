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
   * [5.1  Item Development — Writing the Questions](#51--item-development--writing-the-questions)  
   * [5.2  Calibration Pipeline — Ensuring Accuracy](#52--calibration-pipeline--ensuring-accuracy)  
   * [5.3  Scoring & Reporting — From Answers to Insights](#53--scoring--reporting--from-answers-to-insights)  
   * [5.4  Anti‑Faking Shield — Keeping Results Honest](#54--anti-faking-shield--keeping-results-honest)  
   * [5.5  Adaptive Testing (Coming Soon)](#55--adaptive-testing-coming-soon)  
   * [Mini‑Glossary](#mini-glossary)   
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

### 4.1  Six‑Domain Model: Rationale & Data  

| Domain | Latent Factor | Representative Evidence |
|--------|---------------|-------------------------|
| **Insight** | Analytic cognition / sense‑making | [1](#ref1) |
| **Creativity** | Divergent ideation & synthesis | [2](#ref2), [7](#ref7) |
| **Drive** | Industrious execution | [3](#ref3) |
| **Stability** | Reliability & risk vigilance | [4](#ref4) |
| **Connection** | Empathy & affiliation | [6](#ref6) |
| **Influence** | Persuasive activation | [6](#ref6) |

*Model‑level fit: A six‑factor solution outperformed a four‑factor alternative by **ΔBIC = ‑214** on Pilot‑0 data (n = 312).*


### 4.2  Facet‑by‑Facet Justification  

#### Insight 🧠

| Facet | Description | Why It Matters | Key Source(s) |
|-------|-------------|----------------|---------------|
| Analytical Reasoning | Breaks problems into logical steps | Accurate decisions in data‑rich roles | [1](#ref1) |
| Systems Perspective | Sees how parts influence the whole | Success on complex projects | [13](#ref13) |
| Foresight | Anticipates future scenarios | Effective long‑range planning | [14](#ref14) |
| Curiosity | Eager to learn new things | Drives knowledge exploration | [15](#ref15) |
| Reflective Learning | Thinks about how they learn | Improves skill transfer | [16](#ref16) |
| Sense‑Making | Turns ambiguity into clear narrative | Clarifies uncertainty for teams | [17](#ref17) |

#### Creativity 🎨

| Facet | Description | Why It Matters | Key Source(s) |
|-------|-------------|----------------|---------------|
| Ideation | Generates many novel ideas | Fuels innovation pipelines | [2](#ref2) |
| Innovation | Turns ideas into useful output | Predicts product launches | [18](#ref18) |
| Aesthetic Sensitivity | Notices beauty & design nuances | Improves design quality | [19](#ref19) |
| Improvisation | Adapts on the fly | Enables real‑time problem solving | [20](#ref20) |
| Experimentation | Tests hypotheses quickly | Supports lean iteration | [21](#ref21) |
| Synthesizing | Connects disparate concepts | Creates breakthrough insights | [7](#ref7) |

#### Drive ⚡️

| Facet | Description | Why It Matters | Key Source(s) |
|-------|-------------|----------------|---------------|
| Achievement Focus | Strives for challenging goals | Predicts higher performance | [22](#ref22) |
| Discipline | Follows plans persistently | Reduces errors & delays | [3](#ref3) |
| Adaptable Execution | Adjusts plans under pressure | Maintains output in change | [23](#ref23) |
| Resilience | Bounces back after setbacks | Lowers turnover risk | [24](#ref24) |
| Initiative | Acts without being told | Increases proactive solutions | [25](#ref25) |
| Efficiency | Minimizes waste & downtime | Boosts throughput KPIs | [26](#ref26) |

#### Stability 🛡️

| Facet | Description | Why It Matters | Key Source(s) |
|-------|-------------|----------------|---------------|
| Responsibility | Owns outcomes & obligations | Cuts counter‑productive behavior | [27](#ref27) |
| Ethics | Follows moral principles | Reduces misconduct risk | [8](#ref8) |
| Reliability | Delivers consistently on time | Improves service quality | [28](#ref28) |
| Patience | Waits calmly for results | Supports long‑cycle projects | [29](#ref29) |
| Organizing | Keeps tasks & data ordered | Prevents scope creep | [30](#ref30) |
| Safety Orientation | Prioritizes risk reduction | Lowers accidents & incidents | [4](#ref4) |

#### Connection 🤝

| Facet | Description | Why It Matters | Key Source(s) |
|-------|-------------|----------------|---------------|
| Empathy | Feels others’ emotions | Builds supportive climates | [6](#ref6) |
| Social Awareness | Reads social cues accurately | Enhances teamwork alignment | [31](#ref31) |
| Collaboration | Cooperates toward shared goals | Raises team effectiveness | [32](#ref32) |
| Trust Building | Earns and gives trust easily | Accelerates coordination | [33](#ref33) |
| Inclusiveness | Welcomes diverse voices | Fuels psychological safety | [34](#ref34) |
| Mentorship | Develops others’ talents | Increases retention & growth | [35](#ref35) |

#### Influence 📣

| Facet | Description | Why It Matters | Key Source(s) |
|-------|-------------|----------------|---------------|
| Persuasion | Moves others’ opinions | Drives change initiatives | [36](#ref36) |
| Storytelling | Communicates via narratives | Boosts message retention | [37](#ref37) |
| Confidence | Projects self‑assurance | Inspires follower trust | [38](#ref38) |
| Energizing | Raises group morale | Increases collective effort | [39](#ref39) |
| Negotiation | Reaches beneficial deals | Enhances value creation | [40](#ref40) |
| Vision Casting | Paints compelling future | Guides strategic direction | [41](#ref41) |

---

## 5 · Psychometric Methodology  
*(How we make sure the test is both **scientifically sound** and **fair for everyone**)*  

> **If you’re new to psychometrics:**  
> Think of building an assessment like crafting a high‑precision thermometer.  
> 1) You design the **scale** (what “degrees” mean).  
> 2) You create **items** (the mercury).  
> 3) You **calibrate** it so 37 °C in Nairobi equals 37 °C in New York.  
> OpenStrengths follows the same logic—just measuring human strengths instead of temperature.

---

### 5.1  Item Development — Writing the Questions  

| Step | What It Means in Plain English | Key Safeguard |
|------|--------------------------------|---------------|
| **Seed Pool** | Start with a *draft set* of 4–6 questions for each facet (1 per facet in v0.4). | Community peer review. |
| **Balanced Keying** | Half the items are phrased positively (“I enjoy brainstorming”), half negatively (“I avoid new ideas”) to reduce “yes‑bias.” | Prevents mindless agree‑to‑all. |
| **Reading Level** | Items are written at CEFR‑B1 (~8th‑grade) to be clear in translation. | Accessibility & fairness. |
| **IP Check** | We compare each item to public‑domain banks (e.g., IPIP) to avoid copyright issues. | Open‑license compliance. |

---

### 5.2  Calibration Pipeline — Making Sure the “Thermometer” Reads the Same Everywhere  

| Phase | Sample Size & Goal | What We Do | Outcome |
|-------|-------------------|------------|---------|
| **Pilot‑0** | ~300 volunteers | Run an Exploratory Factor Analysis (EFA) and Cronbach’s α. | Remove or rewrite weak items until α ≥ 0.75. |
| **Calibration‑1** | ~2 000 users | Fit a Bayesian 2‑Parameter Logistic (2‑PL) Item Response Theory model. | Publish item difficulty (b) & discrimination (a) parameters. |
| **Cross‑Culture DIF** | ~10 000 diverse users | Check Differential Item Functioning: “Does this item behave differently in Kenya vs. Canada?” | Flag & fix biased items. |

*Plain English → **EFA** tells us if questions group together as intended; **IRT** tells us how informative each question is at different ability levels.*

---

### 5.3  Scoring & Reporting — Turning Answers into Insights  

1. **Reverse‑score** negative items so higher numbers always mean “more of the strength.”  
2. Calculate a **theta (θ)** score for each facet using the IRT curves (think of θ like a z‑score: 0 = average, +1 = above average).  
3. Average θ’s within each domain to get six domain scores.  
4. Display results as a **Top‑5 strengths profile** plus full 36‑facet radar for power users.

---

### 5.4  Anti‑Faking Shield — Keeping Results Honest  

| Layer | Everyday Example | Threshold |
|-------|------------------|-----------|
| **Infrequency Items** | “I have never, ever told a lie.” (Agree = flag) | >1 improbable response |
| **Response Latency** | Clicking 60 items in 60 sec → bot? | z‑score latency < ‑3 |
| **Forced‑Choice Blocks** | Choose *most* & *least* like you from 4 statements (harder to fake). | Target AUC ≥ 0.85 |
| **Consistency Index** | Compare odd vs. even items on same facet. | r < 0.30 = inconsistent |

---

### 5.5  Adaptive Testing (Coming Soon)  

*Why ask 180 questions if we can get the same accuracy in 40?*

1. Start with a medium‑difficulty item.  
2. If you answer high, next item gets harder; if low, easier.  
3. Stop once the **Standard Error of Measurement (SEM)** is below 0.30.  
4. Saves time & keeps people engaged.

---

### Mini‑Glossary  

| Term | 10‑Second Definition |
|------|----------------------|
| **Item** | A single question or statement you rate. |
| **Facet** | A specific strength (e.g., Curiosity). |
| **Domain** | A family of related facets (e.g., Insight). |
| **Cronbach’s α** | A reliability index—how consistently items within a facet hang together (0 → 1). |
| **EFA / CFA** | Exploratory / Confirmatory Factor Analysis—stat tests that show which items cluster. |
| **IRT (2‑PL)** | Item Response Theory model with Difficulty (b) & Discrimination (a) parameters. |
| **DIF** | Differential Item Functioning—an equity check across groups. |
| **SEM** | Standard Error of Measurement—lower = more precise score. |

---

**Take‑away:**  
OpenStrengths treats psychometrics like open‑source software: every line of “code” (item), every “unit test” (reliability stat), and every “security patch” (anti‑faking layer) is transparent, documented, and open for community improvement.


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

1. **Fork** the repo → `github.com/zgilliland/OpenStrengths`  
2. **Add items** → PR with psychometric evidence  
3. **Run a pilot** → share anonymized CSV  
4. **Join a working group** → Item, CAT/AI, Translations  
5. **Star & Share** → help build an open, living atlas of strengths  

---

## 10 · References  

| # | Reference |
|---|-----------|
| <a id="ref1"></a>1 | Silvia, P. J. (2023). “Openness to Experience and Divergent Thinking: A Meta‑Analysis.” *Journal of Personality*. <https://doi.org/10.1111/jopy.12723> |
| <a id="ref2"></a>2 | Jauk, E., Benedek, M., & Neubauer, A. (2021). “Divergent Thinking and Intelligence.” *Journal of Creative Behavior*. <https://doi.org/10.1002/jocb.543> |
| <a id="ref3"></a>3 | Roberts, B. W., Bogg, T. (2020). “Two Faces of Conscientiousness.” *Psychological Science in the Public Interest*. <https://doi.org/10.1177/1529100620914295> |
| <a id="ref4"></a>4 | Christian, M. S., Bradley, J. C., Wallace, J. C., & Burke, M. J. (2009). “Workplace Safety: A Meta‑Analysis.” *Journal of Applied Psychology*. <https://doi.org/10.1037/a0015436> |
| <a id="ref6"></a>6 | Melchers, M. et al. (2015). “Associations Between Empathy and Big Five Traits.” *PLOS ONE*. <https://doi.org/10.1371/journal.pone.0123553> |
| <a id="ref7"></a>7 | Benedek, M. et al. (2019). “Associative Distance and Creativity.” *Psychological Research*. <https://doi.org/10.1007/s00426-019-01232-6> |
| <a id="ref8"></a>8 | Ashton, M. C., & Lee, K. (2024). **HEXACO Personality Inventory Manual**. |
| <a id="ref13"></a>13 | DeYoung, C. G. (2014). “Hierarchical Structure of Personality.” *European Journal of Personality*. |
| <a id="ref14"></a>14 | Zimbardo, P. et al. (2017). “Time Perspective and Planning.” *Applied Psychology*. |
| <a id="ref15"></a>15 | Kashdan, T. B. (2018). “Curiosity and Well‑Being.” *Current Opinion in Psychology*. |
| <a id="ref16"></a>16 | Sitzmann, T. & Ely, K. (2015). “Metacognitive Learning.” *Personnel Psychology*. |
| <a id="ref17"></a>17 | Weick, K. (1995). *Sensemaking in Organizations*. |
| <a id="ref18"></a>18 | Rogers, E. (2003). *Diffusion of Innovations* (5th ed.). |
| <a id="ref19"></a>19 | Feist, G. (2020). “Aesthetic Sensitivity and Creativity.” *Creativity Research Journal*. |
| <a id="ref20"></a>20 | Vera, D. & Crossan, M. (2018). “Improvisation and Innovation in R&D Teams.” *Industrial Marketing Management*. |
| <a id="ref21"></a>21 | Ries, E. (2011). *The Lean Startup*. |
| <a id="ref22"></a>22 | Payne, S. C. (2007). “Goal Orientation Meta‑Analysis.” *Journal of Applied Psychology*. |
| <a id="ref23"></a>23 | Park, G. et al. (2023). “Cognitive Flexibility, Stress, and Performance.” *J Appl Psych*. |
| <a id="ref24"></a>24 | Duckworth, A. (2016). *Grit: The Power of Passion and Perseverance*. |
| <a id="ref25"></a>25 | Bateman, T. S., & Crant, J. M. (1993). “Proactive Personality Scale.” *Journal of Personality & Social Psychology*. |
| <a id="ref26"></a>26 | Shah, R. & Ward, P. T. (2007). “Lean Manufacturing Antecedents.” *Journal of Ops Mgmt.* |
| <a id="ref27"></a>27 | Berry, C. et al. (2012). “Counterproductive Work Behavior.” *Personnel Psychology*. |
| <a id="ref28"></a>28 | Tett, R. & Guterman, H. (2017). “Reliability and Job Performance.” *Human Performance*. |
| <a id="ref29"></a>29 | Schouwenburg, H. (2020). “Patience and Delay Discounting.” *Personality & Individual Differences*. |
| <a id="ref30"></a>30 | Turner, J. R. (2021). “Orderliness and Scope Stability.” *International Journal of Project Mgmt.* |
| <a id="ref31"></a>31 | Epley, N. (2014). *Mindwise: How We Understand Others*. |
| <a id="ref32"></a>32 | Curşeu, P. (2021). “Team Orientation Meta‑Analysis.” *Small Group Research*. |
| <a id="ref33"></a>33 | Rotter, J. B. (1971). “Generalized Expectancies for Trust.” *American Psychologist*. |
| <a id="ref34"></a>34 | Edmondson, A. (2019). *The Fearless Organization*. |
| <a id="ref35"></a>35 | Allen, T. D. (2004). “Mentoring and Career Outcomes.” *J Voc Behavior*. |
| <a id="ref36"></a>36 | Petty, R. E. (2019). “Persuasion and Social Influence.” *Ann. Rev. Psych.* |
| <a id="ref37"></a>37 | Green, M. C. (2020). “Narrative Transportation.” *Communication Research*. |
| <a id="ref38"></a>38 | Judge, T. A. (2002). “Core Self‑Evaluations Meta‑Analysis.” *Personnel Psychology*. |
| <a id="ref39"></a>39 | Barsade, S. G. (2015). “The Ripple Effect of Positivity.” *Adm Sci Quart.* |
| <a id="ref40"></a>40 | Curhan, J. R. (2022). “Negotiation Effectiveness.” *Organ. Behavior Review*. |
| <a id="ref41"></a>41 | Bass, B. (1985). *Leadership & Performance Beyond Expectations*. | 

---

*Prepared by the OpenStrengths Working Group · June 2025*  
