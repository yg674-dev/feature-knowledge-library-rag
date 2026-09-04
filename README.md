# Feature Knowledge Library (RAG)

Turning a feature inventory into a retrievable knowledge layer — so a business user can ask
"which feature should I use, how does it work, how stable is it?" in natural language instead
of asking a platform owner.

**Prototypes and PRD:** see [What's in this repo](#8-whats-in-this-repo). Every file opens in a browser.

---

## 1. TL;DR

The Feature Platform holds thousands of governance features, but a business user typically knows
only the few dozen they requested themselves. Discovery still runs through asking platform owners
by hand, and the things that decide whether a feature is usable — its meaning, code logic, usage
scope, stability, data source — are not organized as anything a retrieval system can search.

This upgrades the Feature List into a **Feature Knowledge Library**: embedding-ready structured
text per feature, visible index and sync state, an Agent-driven path for creating new features,
and an always-on Ask AI entry that returns feature cards a user can act on.

## 2. Problem

- **Discovery is a person, not a system.** Finding the right feature means asking whoever owns
  the platform, so the answer's quality depends on who is available.
- **The Feature List is an inventory, not a knowledge layer.** It exposes no embedding status,
  no sync status, no failed-re-index handling, and no recall validation — so a PM or RD cannot
  tell whether a given feature is even reachable by Ask AI yet.
- **Creating a feature front-loads the wrong work.** The current flow asks users to hand-fill a
  long RAG template before they have described what they actually need.
- **Retrieval quality is unverifiable.** Without structured text, chunk counts, vector IDs, and
  a recall test surfaced per feature, "the agent found it" is a claim nobody can check.

## 3. Users and jobs to be done

| User | Job |
| --- | --- |
| **Strategy / business owner** | "Tell me which feature fits my rule, what its values mean, and whether I can trust it — without filing a ticket." |
| **Platform PM / RD** | "Show me index coverage, what failed, and why, and let me re-run it in bulk." |
| **Feature requester** | "Let me describe the business need and have the agent ask me the rest, instead of handing me a 19-field form." |

## 4. Goals and non-goals

**Goals**

| # | Goal | Measure |
| --- | --- | --- |
| G1 | Improve searchable coverage | Baseline **7,024 features, 6,683 indexed (95.1%)** → target **≥99% indexed availability**, with failed items batch re-runnable and failure reasons preserved |
| G2 | Reduce discovery effort | From minutes of asking a platform owner → **≤30s** to Ask AI's first response; feature card → detail page in **1 step** |
| G3 | Improve new-feature completeness | **19/19 required fields** and **6/6 Skills Evaluation** checks before Submit; gate coverage 100% |
| G4 | Make recall verifiable | Every detail page shows structured text, embedding model, chunk count, vector ID, and last-synced time |

**Non-goals**
- The detail page is for viewing, confirming, regenerating, and re-indexing — it does not host
  feature creation or backfill.
- MVP does not surface IDSP-side hit / not-hit; feature values are what decide strategy adoption.
- Hover preview of output samples on the list page is P2, not v1.

## 5. Solution

| # | Surface | What it does |
| --- | --- | --- |
| **F1** | **Knowledge Library** | The Feature List upgraded into a management page: KPIs, filters, RAG status, last-synced, batch re-index, and per-row *View RAG* / *Re-run* |
| **F2** | **Feature detail** | Three columns — read-only meta, embedding-ready structured text, RAG status — plus a read-only recall test. Returns vector ID, embedding model, chunk count, sync time, coverage, recall result |
| **F3** | **New Feature Entry · Ask AI** | Agent Q&A is the primary path, the manual template the fallback. The agent asks for feature meaning, entity, data source, owner, usage example, and risk/limitation, then generates a proposal |
| **F4** | **Always-on Ask AI** | Natural-language feature discovery returning cards that link back to F2 for confirmation |
| — | **Sync backend** | Pulls base fields from Feature Platform Meta, maps Knowledge Library and embedding-pipeline state, returns failure reasons, supports batch retry |

**Output Samples** — for categorization features only, optional: the complete enum list with the
business meaning of each value (e.g. `room type` has 10 values; `001 = "Game LIVE room"`). Model
features do not carry them. Agent Q&A can query feature values and their meanings.

## 6. Functional requirements

1. **Proposal gating is two-stage and ordered.** Confirm unlocks only at 19/19 required fields and
   locks the proposal; Skills Evaluation runs *after* Confirm, not after Submit, and Submit unlocks
   only at 6/6. A failed evaluation returns to a fix-and-re-evaluate loop rather than dead-ending.
2. **Ask AI must answer in four explicit states** — `NEED_CLARIFICATION`, `FEATURE_CARDS`,
   `LOW_CONFIDENCE`, `NO_RESULT` — each with its own interaction and permission routing. Silent
   empty results are not an acceptable outcome.
3. **Index failures must be legible and recoverable** — failure reason preserved, batch re-run available.
4. **Dual creation paths persist.** Agent entry and manual template both remain, deliberately, to
   keep token cost bounded.
5. **RAG edit permission gates** who can generate a new feature proposal through Agent Q&A.

## 7. Success metrics

- **North star** — share of feature-selection decisions made without asking a platform owner.
- **Leading** — indexed availability % · Ask AI first-response latency · required-field completion rate
  on agent-created features · re-index success rate after retry.
- **Lagging** — features discovered via Ask AI and subsequently adopted into a strategy.
- **Guardrail** — `LOW_CONFIDENCE` and `NO_RESULT` rates. A retrieval layer that always returns
  something confidently is worse than one that admits a miss.

## 8. What's in this repo

| File | Type | Version | Date |
| --- | --- | --- | --- |
| [`特征RAG-原型-v0.3_2.html`](https://htmlpreview.github.io/?https://github.com/yg674-dev/feature-knowledge-library-rag/blob/main/%E7%89%B9%E5%BE%81RAG-%E5%8E%9F%E5%9E%8B-v0.3_2.html) | Prototype | v0.3_2 | 2025-08-04 |
| [`特征RAG-原型-v0.2.html`](https://htmlpreview.github.io/?https://github.com/yg674-dev/feature-knowledge-library-rag/blob/main/%E7%89%B9%E5%BE%81RAG-%E5%8E%9F%E5%9E%8B-v0.2.html) | Prototype | v0.2 | 2025-07-23 |
| [`Feature Knowledge Library RAG PRD_5.html`](https://htmlpreview.github.io/?https://github.com/yg674-dev/feature-knowledge-library-rag/blob/main/Feature%20Knowledge%20Library%20RAG%20PRD_5.html) | PRD | v5 | 2025-07-30 |

The PRD is bilingual (中文 / English) and carries the full changelog — including why evaluation
timing moved from post-Submit to post-Confirm, and why output samples are scoped to categorization
features only.

## Running locally

```bash
git clone https://github.com/yg674-dev/feature-knowledge-library-rag.git
cd feature-knowledge-library-rag
open "特征RAG-原型-v0.3_2.html"
```

No build step, no server — the prototypes are self-contained HTML.
