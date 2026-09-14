# Platform Governance & Regionalization Agent Suite

Five small, working prototypes built to demonstrate the kind of tooling a
Technical Program Manager on a platform/regionalization team would actually
want — each one models a piece of work I did by hand at Autodesk (API
governance, cross-region risk reporting) or maps directly onto this role's
mandate (regionalization readiness, developer enablement).

Each agent is a standalone Python CLI tool, lives in its own repo with
sample data included, and runs out of the box. Set `ANTHROPIC_API_KEY` in
your environment to also get a Claude-written narrative/summary layer on
top of the deterministic logic — without a key, every tool still produces
a full report using local logic only, so nothing is a black box.

## 1. [API Governance Compliance Checker](https://github.com/digitalnomadyh/01-api-governance-checker)

Reads an OpenAPI spec and a governance ruleset (auth required, versioned
paths, no PII in query params, rate-limit docs, deprecation notices), checks
every endpoint against every rule, and produces a pass/fail report. This is
the automated version of the manual Forge/APS API governance review process.

```bash
git clone https://github.com/digitalnomadyh/01-api-governance-checker
cd 01-api-governance-checker
python checker.py --spec data/sample_api_spec.json --rules rules.yaml --out output/report.md
```

## 2. [Cross-Team Dependency & Risk Radar](https://github.com/digitalnomadyh/02-dependency-risk-radar)

Reads a multi-team task list (CSV), builds a dependency graph, flags blocked
and overdue tasks, and identifies the critical path — then drafts an
exec-ready status update. Models the quarterly KPI/risk reporting done for
a multi-region program like Global Transaction Management (APAC/EMEA/US).

```bash
git clone https://github.com/digitalnomadyh/02-dependency-risk-radar
cd 02-dependency-risk-radar
python radar.py --tasks data/tasks.csv --out output/risk_report.md
```

## 3. [Regionalization Readiness Tracker](https://github.com/digitalnomadyh/03-regionalization-readiness-tracker)

Reads a per-region compliance/infra checklist (data residency, ISO 27001,
SOC 2, FedRAMP, DR, latency SLA), scores readiness per region, flags gaps,
and renders a status report with a bar chart. Maps directly onto "standing
up new compliance regions."

```bash
git clone https://github.com/digitalnomadyh/03-regionalization-readiness-tracker
cd 03-regionalization-readiness-tracker
python tracker.py --regions data/regions.yaml --out output/readiness_report.md
```

## 4. [Developer Access Request Assistant](https://github.com/digitalnomadyh/04-dev-access-assistant)

Screens incoming API access requests against a risk policy (high-risk
scopes, weak justification, prod-vs-sandbox mismatches), auto-approves
low-risk requests, and drafts a reviewer note for anything flagged. This is
the self-serve, guardrails-included side of developer enablement.

```bash
git clone https://github.com/digitalnomadyh/04-dev-access-assistant
cd 04-dev-access-assistant
python assistant.py --requests data/requests.json --out output/decisions.md
```

## 5. [Data Residency Compliance Checker](https://github.com/digitalnomadyh/05-data-residency-compliance-checker)

"API compliance" isn't one checklist — EU, APAC, and Amer each impose
different, non-overlapping data residency rules (GDPR, PIPL, PDPA, DPDP
Act, FedRAMP, US state law). This checks *where data actually lives*
against *what its jurisdiction requires*, flagging data stored outside its
approved region(s) without a documented cross-border transfer mechanism.
A different question from whether an endpoint has the right auth scope
(tool #1) or whether a region is launch-ready (tool #3).

```bash
git clone https://github.com/digitalnomadyh/05-data-residency-compliance-checker
cd 05-data-residency-compliance-checker
python residency_checker.py --rules data/residency_rules.yaml --flows data/data_flows.json --out output/residency_report.md
```

## Setup

Each repo ships its own `requirements.txt`, so `pip install -r
requirements.txt` inside a given tool's directory is all you need. Across
all four, the combined dependency set is:

```bash
pip install anthropic pyyaml networkx matplotlib
export ANTHROPIC_API_KEY=sk-...   # optional — enables the Claude narrative layer
```

## Why these four

Each one takes a piece of language directly from the Autodesk PSET TPM job
description — "regulated cloud environments," "developer enablement
infrastructure," "delivery forecasting, execution tracking, transparent
reporting," "single source of truth" — and turns it into something small
but real, rather than just a bullet point on a resume.
