# Workbook schema

Use the following sheet names and columns so the deterministic audit and future workspace deduplication can operate reliably.

## `Companies`

The header row must contain these exact labels:

1. `Company Name`
2. `Country`
3. `City`
4. `Official Website`
5. `Root Domain`
6. `Public Email`
7. `Public Phone`
8. `Public WhatsApp`
9. `Contact Name`
10. `Contact Title`
11. `Contact Tier`
12. `Contact Source URL`
13. `Source Type`
14. `Website Signals`
15. `Pain-Point Hypotheses`
16. `Recommended Sales Angle`
17. `English Outreach Opener`
18. `Verified On`
19. `Manual Recheck`
20. `Notes`

One row represents one independently operated company. Use ISO dates (`YYYY-MM-DD`). Use `Yes` or `No` for `Manual Recheck`. Enter at least one format-valid public email per company; when multiple addresses are public, place the best outreach address first and add alternatives to `Notes` with their sources.

`Root Domain` is the registrable-looking company host used for deterministic comparison. Lowercase it, remove `www.`, and omit schemes, paths, ports, and trailing dots. Use the same normalized host consistently rather than attempting an uncertain corporate-family merge.

## `Overview`

Include the confirmed product, target customer, sales market, research date, delivered count, Tier A/B/C counts, manual-recheck count, and a short scope statement. Make clear that pain points are hypotheses based on public website signals.

## `Decision Makers`

Copy Tier A and Tier B rows or provide a filtered priority view containing company, country, contact name, title, email, source URL, sales angle, and opener. Do not invent named contacts for Tier C companies.

## `Methodology`

Summarize source priorities, contact-tier definitions, deduplication keys, freshness limitations, and the public-data boundary. State that human verification is required before outreach and that the workbook does not authorize sending messages.

## `_meta`

Create a two-column key/value sheet and hide it. Include these exact keys:

| Key | Required value |
|---|---|
| `generated_by` | `public-business-research` |
| `schema_version` | `1` |
| `product` | confirmed product or service |
| `target_customer` | confirmed target customer |
| `sales_market` | confirmed country or region |
| `batch_size` | `50` |
| `created_at` | ISO date or ISO date-time |

The marker makes the workbook discoverable for deduplication inside the current workspace. It is not a central database and must not cause unrelated Excel files to be scanned as lead history.
