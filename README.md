<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,50:238636,100:2ea043&height=200&section=header&text=Mutharasu%20Archunan&fontSize=42&fontColor=F0F6FC&fontAlignY=35&desc=Senior%20Software%20Engineer%20%7C%20Backend%20Architect%20%7C%20System%20Designer&descSize=16&descColor=8B949E&descAlignY=55&animation=fadeIn" width="100%"/>

<br/>

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=22&duration=3000&pause=1000&color=2EA043&center=true&vCenter=true&repeat=true&width=700&height=50&lines=Identity+Systems+securing+5+products+%F0%9F%94%90;ETL+Pipelines+processing+200K%2B+records+%F0%9F%93%8A;Debugging+Raft+Consensus+at+protocol+level+%E2%9A%99%EF%B8%8F;Zero+to+Production+Backend+Architecture+%F0%9F%9A%80" />

<br/>

<a href="https://linkedin.com/in/mutharasu-a-90638823b"><img src="https://img.shields.io/badge/-LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/></a>&nbsp;
<a href="https://github.com/MutharasuArchunan13"><img src="https://img.shields.io/badge/-GitHub-171515?style=for-the-badge&logo=github&logoColor=white"/></a>&nbsp;
<a href="https://github.com/typesense/typesense/issues/2638"><img src="https://img.shields.io/badge/OSS-Typesense_Contributor-E5534B?style=for-the-badge&logo=github"/></a>

<br/>

<img src="https://komarev.com/ghpvc/?username=MutharasuArchunan13&color=2EA043&style=flat-square&label=Profile+Views"/>

</div>

<br/>

## 🧑‍💻 About Me

```python
class Mutharasu:
    
    role       = "Senior Software Engineer"
    company    = "10xScale.ai"
    location   = "Hyderabad, India"
    languages  = ["Python", "Java", "JavaScript", "SQL","Typescript","C","Rust"]
    
    architect_of = {
        "Identity Provider": "Centralized OAuth 2.0 + OIDC → 5 products",
        "ETL Pipelines":     "Airflow + PySpark → 200K+ records",
        "Search Infra":      "Typesense + pgvector → semantic search",
        "Web Scrapers":      "Redis/Celery → distributed workers"
    }
    
    open_source = "Typesense #2638 — Raft consensus thread starvation"
    taught      = "Java → 200+ students"
    superpower  = "I build the platform others build on."
```

<br/>

## 🏗️ What I've Built

<table>
<tr>
<td width="50%" valign="top">

**🔐 Centralized Identity Provider**

OAuth 2.0 + OIDC compliant IdP for **5 products**

- JWT with **EdDSA** signing · PKCE for public clients
- Refresh token families with cascade revocation
- Per-product token isolation & subscription-based auth
- **29 endpoints** · Auth, Session, OIDC, Audit routers
- FastAPI + Tortoise ORM + asyncpg + Redis + Taskiq

</td>
<td width="50%" valign="top">

**📊 ETL Pipeline Architecture**

End-to-end data processing from scratch

- **Airflow** orchestration + **PySpark** transforms
- **200K+ recruitment records** processed
- Idempotent design — safe re-execution guaranteed
- **PostgreSQL + pgvector** for semantic search
- Schema-inconsistency handling across sources

</td>
</tr>
<tr>
<td width="50%" valign="top">

**🕷️ Distributed Web Scraping**

High-throughput scraping infrastructure

- Source-specific worker pools (LinkedIn, Naukri)
- **Redis/Celery** task distribution
- Planned **Kubernetes** + KEDA autoscaling
- Rate limiting & anti-detection strategies

</td>
</td>
<td width="50%" valign="top">

**🔀 Open Source — Typesense (26K+ ⭐, C++)**
*Merged contributor ·  3 PRs shipped*

**[#3010](https://github.com/typesense/typesense/pull/3010) — Non-deterministic sort ordering** `merged`
- 3 identical requests → 3 different orderings, no error
- Cursor vector shared across sort clauses read past its allocation
- Fixed with per-clause cursor keying + regression test

**[#3011](https://github.com/typesense/typesense/pull/3011) — `mode: sum` for `_eval`** `merged`
- Additive ranking needed `2ⁿ - 1` rules → now needs `n`
- 4 signals: 15 rules → 4
- Parser disambiguation vs existing filter syntax; `__int128` overflow accumulation

**[#2638](https://github.com/typesense/typesense/issues/2638) — Raft thread starvation** `reported` and `fixed`
- Write queue blocking `/health` & `/search` during 228K bulk inserts
- Proposed decoupled health endpoints + backpressure

</td>
</tr>
</table>

<br/>

## 🔐 Architecture — Identity Provider

```
┌──────────────────────────────────────────────────────────────────┐
│                   CENTRALIZED IDENTITY PROVIDER                  │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐     │
│  │Product 1│ │Product 2│ │Product 3│ │Product 4│ │Product 5│     │
│  └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘     │
│       └───────────┴─────┬─────┴───────────┴───────────┘          │
│                         │                                        │
│                  ┌──────▼──────┐                                 │
│                  │ API Gateway │                                 │
│                  └──────┬──────┘                                 │
│       ┌─────────────────┼─────────────────┐                      │
│ ┌─────▼─────┐   ┌──────▼──────┐  ┌───────▼──────┐                │
│ │Auth Router│   │OIDC Router  │  │Audit Router  │                │
│ │Login, Reg │   │Authorize,   │  │Logs, Events  │                │
│ │PKCE       │   │Token, JWKS  │  │Sessions      │                │
│ └─────┬─────┘   └──────┬──────┘  └───────┬──────┘                │
│       └─────────────────┼─────────────────┘                      │
│                         │                                        │
│            ┌────────────▼────────────┐                           │
│            │   FastAPI + Async Core  │                           │
│            └────────────┬────────────┘                           │
│     ┌───────────┐ ┌────▼────┐ ┌──────────┐                       │
│     │PostgreSQL │ │  Redis  │ │  Taskiq  │                       │
│     │26 Tables  │ │Sessions │ │Async Jobs│                       │
│     │Snowflake  │ │Cache    │ │Email,    │                       │
│     │IDs+Aerich │ │Rate Lim │ │Webhooks  │                       │
│     └───────────┘ └─────────┘ └──────────┘                       │
│                                                                  │
│ Security: EdDSA JWT · PKCE · HttpOnly+SameSite · No Enumeration  │
│ Per-App: Token Isolation · Unique Audience · Refresh Families    │
└──────────────────────────────────────────────────────────────────┘
```

<br/>

## 🛠️ Tech Stack

<div align="center">

**Languages & Backend**

<a href="https://skillicons.dev">
<img src="https://skillicons.dev/icons?i=python,java,javascript,bash,fastapi,spring,django&theme=dark" />
</a>

**Frontend**

<a href="https://skillicons.dev">
<img src="https://skillicons.dev/icons?i=react,redux,vite,html,css&theme=dark" />
</a>

**Databases, Search & Messaging**

<a href="https://skillicons.dev">
<img src="https://skillicons.dev/icons?i=postgres,mysql,redis,elasticsearch&theme=dark" />
</a>

**Cloud, DevOps & Infra**

<a href="https://skillicons.dev">
<img src="https://skillicons.dev/icons?i=gcp,docker,kubernetes,terraform,nginx,git,githubactions&theme=dark" />
</a>

**Data & ETL**

<img src="https://img.shields.io/badge/Airflow-017CEE?style=for-the-badge&logo=apacheairflow&logoColor=white"/>
<img src="https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white"/>
<img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"/>

**Auth & Security**

<img src="https://img.shields.io/badge/OAuth_2.0-3C3C3D?style=for-the-badge&logo=auth0&logoColor=white"/>
<img src="https://img.shields.io/badge/OIDC-F78C40?style=for-the-badge&logo=openid&logoColor=white"/>
<img src="https://img.shields.io/badge/JWT_(EdDSA)-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white"/>
<img src="https://img.shields.io/badge/PKCE-2EA043?style=for-the-badge"/>

**ORM & Async**

<img src="https://img.shields.io/badge/Tortoise_ORM-2EA043?style=for-the-badge"/>
<img src="https://img.shields.io/badge/SQLAlchemy-D71F00?style=for-the-badge"/>
<img src="https://img.shields.io/badge/asyncpg-316192?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Celery-37814A?style=for-the-badge&logo=celery&logoColor=white"/>
<img src="https://img.shields.io/badge/Taskiq-E92063?style=for-the-badge"/>

</div>

<br/>

## 👨‍🏫 Teaching & Mentoring

<div align="center">

| 🎓 **200+ Students Taught** | 🎥 **Content Creator** | 📊 **Data Engineering** |
|:---:|:---:|:---:|
| Core Java, OOP, Backend | Live → Recorded tutorials | Intermediate DE series |

</div>

<br/>

## 📈 GitHub Stats

<div align="center">

<img src="https://github-readme-streak-stats-eight.vercel.app/?user=MutharasuArchunan13&theme=github-dark-blue&hide_border=true&ring=2EA043&fire=2EA043&currStreakLabel=2EA043&sideNums=F0F6FC&sideLabels=8B949E&dates=6E7681&stroke=21262D&background=0D1117" width="52%"/>

<br/>
<br/>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/MutharasuArchunan13/MutharasuArchunan13/output/github-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/MutharasuArchunan13/MutharasuArchunan13/output/github-snake.svg" />
  <img alt="github-snake" src="https://raw.githubusercontent.com/MutharasuArchunan13/MutharasuArchunan13/output/github-snake-dark.svg" />
</picture>

</div>

<br/>

## 💎 Open Source

<a href="https://github.com/typesense/typesense">
<img src="https://github-readme-stats-ten-olive-80.vercel.app/api/pin/?username=typesense&repo=typesense&theme=github_dark&hide_border=true&bg_color=0D1117&title_color=2EA043&icon_color=2EA043" />
</a>

**Merged contributor — 3 PRs shipped into a 26K+ ⭐ C++ codebase**

> **[PR #3011](https://github.com/typesense/typesense/pull/3011)** `merged` — Added `mode: sum` to `_eval` sort scoring. Expressing an additive ranking model previously required enumerating every combination (`2ⁿ - 1` rules — 15 for 4 signals); it now needs `n`. Included parser disambiguation against existing filter syntax and `__int128` overflow accumulation.

> **[PR #3010](https://github.com/typesense/typesense/pull/3010)** `merged` — Root-caused non-deterministic result ordering: three identical requests returned three different orders with no error. A cursor vector shared across sort clauses was reading past its allocation. Fixed by keying cursors per sort-field index, with a regression test.

> **[Issue #2638](https://github.com/typesense/typesense/issues/2638)** `reported and fixed` — Diagnosed Raft consensus thread starvation during 228K document bulk inserts. Proposed decoupled health endpoints and configurable write backpressure.

<br/>

<div align="center">

*"I don't just write code — I architect systems, debug at the protocol level, and teach others to do the same."*

</div>

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,50:238636,100:2ea043&height=120&section=footer" width="100%"/>
