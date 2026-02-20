# Compliance Enforcement Framework

**Compliance by Design — Four Domains, Built Into the Architecture**

---

Most compliance systems are bolted on. Rules live in a document somewhere. Someone checks them when they remember. The system runs fine until it doesn't, and then you find out the check wasn't actually running.

I built it the other way. The compliance layer is isolated from the systems it governs — not a step in a workflow, but a separate service that workflows call. You can't publish a product without passing the content check. You can't run an operation outside your authority tier without hitting the enforcement layer. The rules aren't instructions; they're architecture.

There are four distinct compliance domains in this system. They have different sources, different failure modes, and different enforcement strategies. They only look similar because they all run in the same environment.

**Content compliance — automated, external rules, zero discretion**

TGT sells on Amazon. Amazon's content policies and FTC advertising guidelines are external rules I don't control and can't negotiate. A single non-compliant listing can trigger account suspension.

Built prohibited keyword enforcement with four severity tiers: blocked outright, flagged for human review, warned, monitored. Every keyword has a category, a severity, and a reference to the Amazon ToS clause that governs it:

```sql
-- tgt_prohibited_keywords: what gets blocked and why
keyword   | category    | severity        | amazon_tos_reference
----------|-------------|-----------------|---------------------
"gun"     | Firearms    | block           | Associates Policy §4.2
"rifle"   | Firearms    | block           | Associates Policy §4.2
"CBD"     | Supplements | flag_for_review | Health Claims §7.1
```

A daily Spot Audit workflow (v3.1, active) validates that the prevention system is working — not by scanning content, but by verifying the enforcement layer fired correctly. 10 executions, 100% pass rate. Nothing reaches Amazon that hasn't passed the check. The AI doesn't interpret what's compliant — it applies rules that were defined in advance. Interpretation is where violations happen.

**Operational compliance — internal rules, verified behavior**

What the AI is allowed to do. The 18 immutable laws and 5-tier authority system define the boundaries. But written rules aren't the same as enforced ones.

The enforcement layer is the 60-point workflow audit — two phases, 13 categories, static JSON analysis followed by runtime verification. Every workflow has to pass before it's production. That score is the baseline. Not documentation of intent, but verified behavior. If a workflow's score drops below threshold, it's not production anymore.

**Legal compliance — LLC-level obligations, human execution required**

Operating agreements, registered agent requirements, annual report filings. These can't run autonomously — the actions require me. The AI's role is different here: track deadlines, surface them early, make sure nothing arrives unannounced.

The failure mode this prevents isn't a bad filing. It's a missed deadline I didn't know was coming because it was buried in a document I hadn't opened in six months.

**Tax compliance — automated preparation, human submission**

IRS tax category mapping runs automatically on every expense record. Every transaction is categorized before I see it. The categorization is automated; the filing isn't.

Same enforcement pattern as legal: automate everything up to the moment that requires a human decision, then stop and wait. The preparation is never the problem. It's the handoff.

**What holds it together**

Two things: durability and visibility.

Durability means the compliance layer survives changes to the systems it governs. When I updated the product publishing workflow, the prohibited keyword check didn't change — it's a separate service, not a step embedded in that workflow. When skill contracts evolved, authority tiers stayed fixed. Isolated enforcement survives system changes. Embedded enforcement drifts with them.

Visibility means the compliance posture is inspectable without asking. Every content check leaves a record. Every audit produces a score. Every deadline surfaces before it's urgent. The state is always readable — not because someone wrote it down, but because the system produces it as a side effect of running.

**Why it matters:** Compliance programs fail in two ways: the rules don't actually run, or nobody can tell whether they're running. Both problems are architectural. The patterns here — isolated enforcement services, severity tiers, audit scoring, automated preparation with human submission gates — apply to any program with regulatory, legal, or operational compliance requirements. The goal in every domain is the same: make it hard to accidentally be non-compliant, and make the current state visible without having to ask.
