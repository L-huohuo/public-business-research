---
name: public-business-research
description: Research and deliver batches of exactly 50 source-backed public business prospects, with contact prioritization, website-signal pain hypotheses, and deduplication against this skill's earlier workbooks in the current workspace. Use for company lead-list research; do not use for private-person data or sending outreach.
---

# Public Business Research

Create a verified company-research workbook for ethical, targeted business outreach. Research public business information only. Do not send messages, enroll contacts in campaigns, or infer private contact details.

## Intake gate

Before browsing or generating candidates, obtain all three required inputs:

1. What product or service is being sold?
2. Who is the target customer?
3. In which country or region will it be sold?

If any item is missing, ask only for the missing item or items and stop. Once all three are known, restate them with `Batch size: 50` and ask the user to confirm. Begin research only after confirmation. Do not turn optional preferences into additional mandatory questions.

## Batch invariant

Deliver exactly 50 valid, unique companies per run. Fifty candidates are not sufficient: all 50 delivered rows must pass the evidence and quality checks below. If the confirmed market cannot support 50 credible results, report the shortfall and ask before widening the geography or customer definition. Never fill the batch with weak or invented records.

## Stateless cross-batch deduplication

Do not create a central history database and do not import unrelated or legacy lead files.

Before researching, run `scripts/lead_quality.py exclusions --root <current-workspace>`. The helper scans only workbooks bearing this skill's `_meta` marker. Use its normalized company names, root domains, and emails as the exclusion set. This provides deduplication within the current workspace without connecting other projects.

If earlier skill-generated workbooks have been moved or deleted, explain that they cannot be checked automatically. Accept user-supplied marked workbooks as explicit exclusion sources with `--include <workbook>` during exclusion collection and `--prior <workbook>` during final audit.

A company is a duplicate when any of these match an excluded record:

- normalized legal or trading name;
- official website root domain; or
- normalized public email.

Multiple branches or contact addresses under one company/domain count as one company unless they are independently branded businesses with distinct official domains and evidence.

## Research and evidence

Read [references/research-method.md](references/research-method.md) before collecting contacts. Apply these essentials:

- Prefer the official company website and its contact, about, team, and booking pages.
- Every email or phone/WhatsApp value must have a public source URL.
- Prefer a publicly listed owner, founder, general manager, or relevant decision-maker.
- Never guess a person's identity or construct an email from a pattern.
- If no public decision-maker address exists, use a named senior contact or a general business inbox and label it accurately.
- Treat website observations as dated snapshots. Treat pain points as sales hypotheses, not confirmed facts.

Contact tiers:

- `A`: publicly sourced decision-maker work contact;
- `B`: publicly sourced named senior contact or a role-specific route to the decision-maker;
- `C`: general business inbox or switchboard.

## Deliverable

Read [references/workbook-schema.md](references/workbook-schema.md) before building the Excel file. Create these sheets:

- `Overview`
- `Companies`
- `Decision Makers`
- `Methodology`
- `_meta` (hidden)

The workbook itself is the only cross-batch record. The `_meta` sheet identifies files created by this skill; it is not a customer database. Use a filename that includes the market, date, and batch number.

Keep outreach language concise and tailored to the product and visible business signal. Do not claim that a company has a problem; phrase the pain field as a hypothesis and the opener as a respectful conversation starter.

## Validation

Run the workbook through:

```bash
python3 scripts/lead_quality.py audit <workbook.xlsx> --root <current-workspace> --expected 50
```

Add `--prior <earlier-workbook.xlsx>` for each user-supplied marked workbook outside the current workspace.

If the system Python lacks `openpyxl`, use the bundled spreadsheet Python runtime available in Codex or another environment that provides `openpyxl`; do not skip the audit.

The audit must pass before delivery:

- exactly 50 company rows;
- no duplicates within the batch;
- no matches against other marked workbooks in the current workspace;
- valid public email, official website, and contact-source URL on every row;
- required product-fit, website-signal, pain-hypothesis, sales-angle, and outreach fields populated;
- contact tiers use only `A`, `B`, or `C`;
- verification dates and manual-recheck labels are present;
- `_meta` contains the confirmed product, target customer, sales market, and generator marker.

Report the final file path and a compact quality summary. Clearly state how many contacts are A, B, and C tier and how many rows require manual rechecking.
