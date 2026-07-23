# Recommender — Decision Logic

How the Recommender page's interactive checkpoint tree was decided. This is the portable spec behind the Q&A flow — reusable outside this repo (Claude Projects, other build environments, or a fresh interactive rebuild) since it's just the decision logic, not implementation.

---

## Checkpoint 1 — The Core Objective

**Question:** What's the core objective driving the need for a SIEM?

| Answer | Route |
|---|---|
| Compliance | → **Ending: Log Management & Observability** |
| Proactive threat detection | → Checkpoint 2 |

### Ending: Log Management & Observability
Compliance driven, not hunting driven. Five stop spectrum, cheaper/hands on → pricier/fully managed.

---

## Checkpoint 2 — Organization Size

**Question:** Organization size / profile.

| Answer | Route |
|---|---|
| Lean, SVB, 5–20%+ data growth YR | → Checkpoint 2b (workload tolerance) |
| 100+ devices, EDR+IAM+Cloud, 250+ GB/day, 2+ person TDIR team | → Checkpoint 3 |

### Checkpoint 2b — Workload Tolerance (lean branch only)
Sub question: **Tested against: workload tolerance**

Three tier spectrum, `$` high workload → `$$$` low workload:

| Tier | Vendors | Position |
|---|---|---|
| **Lean SIEM** — high workload, $ cost, DIY run | wazuh · ELK | left |
| **Integrated AI SOC** — medium workload, recognized security focus & expertise | Panther · Artemis · Zaun · Sekoia | middle |
| **MDR** — low workload, $$$ cost, fully managed | AIRMDR · daylight · Exaforce · arcanna | right |

This is terminal — no further checkpoints for the lean path.

---

## Checkpoint 3 — Ecosystem Bundling

**Question:** Already deep in MSFT, Google, Palo Alto, or CrowdStrike? Or a diverse, multi vendor stack?

| Answer | Route |
|---|---|
| Already deep in CrowdStrike | → **Ending: Native Ecosystem (CrowdStrike)** |
| Already deep in Google | → **Ending: Native Ecosystem (Google)** |
| Already deep in Microsoft | → **Ending: Native Ecosystem (Microsoft)** |
| Already deep in Palo Alto | → **Ending: Native Ecosystem (Palo Alto Networks)** |
| A diverse, multi vendor stack | → Checkpoint 4 |

### Ending: Take Advantage of the Ecosystem Bundling
Final recommendation, terminal — no further checkpoints needed once one ecosystem already dominates the stack. The more committed you already are, the harder it is to justify going elsewhere.

---

## Checkpoint 4 — Detection Engineering Readiness (diagnostic, no branch)

**Framing:** *Time for a gut check. Take a real look at where your team actually stands.*

- **Alert volume** — can your team keep up with what's coming in, or are you drowning?
- **Alert quality** — of that volume, how much is signal? Healthy FP rate, precision, recall?
- **Environment** — is your source landscape outgrowing what you started with? More diversity, more non native sources, more custom apps to cover?

No branch here — informational only, feeds directly into Checkpoint 5. Always continues.

---

## Checkpoint 5 — Engineering Commitment

**Framing:** *Weigh what you just answered: volume, quality, environment complexity. Now the real question: are you ready to bring detection engineering home?*

Nobody protects your environment like the team that lives in it. In house detection engineering means institutional knowledge that never leaves, tuning that happens in hours, not one of many ticket queues, and detections built by people who actually understand your business. It's the strongest model there is. It just asks for real commitment to sustain it.

| Answer | Route |
|---|---|
| Fully outsource | → **Ending: Fully Managed SIEM Players** |
| Willing/able to commit to in house DE | → Checkpoint 6 |

### Ending: Fully Managed SIEM Players
Selection Criteria (static checklist, non branching): Company scale · Compliance req · Tech stack support · Provider region · Provider model.

Spectrum, SMB → Enterprise:

| Tier | Vendors | Cost |
|---|---|---|
| SMB | Blumira | $ |
| Heavy On Prem / Fed / Public Sector | RECON InfoSec, Barracuda | $$ |
| Lower Enterprise | Black Hills, Coalition/Wirespeed | $$$ |
| Enterprise | Red Canary | $$$$ |

**Alternative lane:** MDR — AIRMDR, daylight, Exaforce, arcanna — when a lower workload, provider model fit matters more than a straight MSSP relationship.

---

## Checkpoint 6 — Optimize or Rebuild?

**Framing:** *Look at what your SIEM actually is inside your organization.*

Is it deeply embedded, used by other teams, too woven in to touch? Optimize around it.

Or is your organization open to a clean break? Some teams have the appetite and leadership buy in to rip out the SIEM and rebuild on something better suited to where they're headed.

| Answer | Route |
|---|---|
| Stuck with legacy SIEM — optimize | → **Ending: Hybrid Architecture** |
| Willing/able to start over fresh | → Checkpoint 7 |

### Ending: Hybrid Architecture
*Augment, layered alongside current SIEM.* Terminal — no further checkpoints.

Pipeline stages: `Collection → Data Engineering Pipeline → Storage → Detection Engineering & Threat Hunting → Triage & Investigation → Remediation`

**+ Pipeline Tech** — Beacon · monad
Route data, save on SIEM ingest — enrich in flight.

**+ Security Data Lake** — matches the Storage pipeline stage
- Cribl — route data, save on SIEM ingest, plus Cribl Lake to store/query.
- databricks · scanner *(S3 only)* — optimize with a SDL: augment visibility, long term investigations, run data science on security data.

**+ Detection Engineering & Threat Hunting Platform** — nebulock · Artemis · spectrum · Anvilogic
Hunt continuously, detect proactively — agentic AI helps author and maintain detection coverage. Unifies detection across SIEM and data lake pairings — plus lite agentic SecOps (check tech stack dependencies).

**+ Agentic SOC Tech** — matches the Remediation pipeline stage — MATE · Cotool · DropzoneAI · Intezer · LEGION
Optimize via continuous detection & response augmentation — takes alerts from current SIEM and orchestrates triage and remediation; modern SIEM <> SOAR mesh.

---

## Checkpoint 7 — Is Real Time Detection a Hard Requirement?

**If yes,** know the tradeoff. Catching threats the instant data streams in is fast, but limited. Only a narrow window of data and a small number of searches can run at that exact moment.

**If not,** let the data land in storage first. You trade a little time for sharper alerts, fewer false positives, and lower cost over the long run.

| Answer | Route |
|---|---|
| Speed is critical | → Checkpoint 7b (Enterprise or Mid Market) |
| Minor delays tolerable | → Checkpoint 8 |

### Checkpoint 7b — Enterprise or Mid Market? (speed critical branch only)

| Answer | Ending |
|---|---|
| Enterprise | **CrowdStrike, Datadog** |
| Mid Market | **abstract.security** |

Terminal — no further checkpoints.

---

## Checkpoint 8 — Cloud Native or Hybrid Legacy?

*(Reached only via: hunting → enterprise → diverse stack → in house DE → rebuild fresh → minor delays tolerable)*

| Answer | Route |
|---|---|
| Born more in the cloud | → **Ending: Modern SIEM Alternatives** |
| Stuck with legacy hybrid environment | → **Ending: Legacy Hybrid — Lima Charlie** |

### Ending: Modern SIEM Alternatives
**Consider: Enterprise vs midmarket maturity**
- **Startups:** hungry, eager to fit your environment, ship fast
- **Established Leaders:** lower risk, staffed research — but slower to learn your environment, one of many in the queue
- **AI:** needs TLC, real tuning period
- **Ask:** partner in and dedicate the time?

Vendors: nebulock · Artemis Security · Databricks LakeWatch

### Ending: Legacy Hybrid — Lima Charlie
Most cost effective path available, but genuinely more DIY.

---

## Full Decision Tree Summary (path notation)

```
1. Objective
├── Compliance → [END: Log Mgmt & Observability]
└── Hunting → 2. Org Size
    ├── Lean/SVB → 2b. Workload
    │   ├── High   → [END: Lean SIEM]
    │   ├── Medium → [END: Integrated AI SOC]
    │   └── Low    → [END: MDR]
    └── Enterprise → 3. Ecosystem
        ├── Native (CS/Google/MSFT/PA) → [END: Native Ecosystem]
        └── Diverse → 4. Detection Eng Readiness (info only) → 5. Commitment
            ├── Outsource → [END: Fully Managed SIEM Players / MDR alt]
            └── In-house → 6. Optimize or Rebuild
                ├── Optimize → [END: Hybrid Architecture]
                └── Rebuild → 7. Real-Time Requirement
                    ├── Critical → 7b. Enterprise/Mid-Market
                    │   ├── Enterprise  → [END: CrowdStrike, Datadog]
                    │   └── Mid-Market  → [END: abstract.security]
                    └── Minor delays OK → 8. Cloud Native or Hybrid Legacy
                        ├── Cloud Native → [END: Modern SIEM Alternatives]
                        └── Hybrid Legacy → [END: Lima Charlie]
```

---

## Excluded Vendors (carried over from earlier framework versions)
Exabeam, LogRhythm, FortiSIEM, QRadar, Reliaquest — explicitly excluded from all endings above.

## Notes for Reuse
- All checkpoint copy above is final, verbatim wording as approved.
- Vendor rosters reflect the most recent Canva poster ("Source of Truth") sync — reverify against the latest poster before reuse if it has since evolved further.
- Detection Quality benchmarking caveat (from earlier research pass): no vendor neutral benchmark yet compares SIEM/MDR/XDR/AI SOC on detection quality directly — MITRE's 2026 evaluation is the first attempt, results expected December 2026. Worth citing wherever this spec discusses vendor detection claims.
