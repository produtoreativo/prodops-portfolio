---
name: obc-partitioning
description: Execute OBC Partitioning from a Global BI. Use when decomposing a Global OBC into Local OBCs across product repos after Discovery in the BIB. Produces one Local OBC Issue per product repo, linked as sub-issue of the Global BI Issue, added to Project #30, and tracked in the Global OBC rastreabilidade table.
---

# OBC Partitioning

Use this skill to decompose a Global OBC into Local OBC Drafts across the product repositories involved in a Business Intent. This skill is self-sufficient: do not pre-read `AGENTS.md`, `prodops/framework/`, or any other framework documentation before executing it.

## Required input context

Before starting, the agent must have:

- `bi-id` — identifier of the Business Intent (e.g. `BI-01`)
- `global-obc-path` — path to the Global OBC file (e.g. `prodops/artifacts/obcs/global-bi-01-compra-pix-listagem.md`)
- `global-bi-issue` — GitHub Issue number in prodops-portfolio that represents the Global BI (e.g. `5`)
- `repos` — list of product repositories and their expected scope, extracted from the "Produtos Envolvidos" table in the Global OBC
- `release` — Platform Release version (e.g. `1.0.0`)

If any of these are absent, read the Global OBC file to extract `repos` and `release`. If `global-bi-issue` is missing, search open issues in prodops-portfolio with title containing `[{bi-id}]`. Do not proceed without all five inputs resolved.

## Required reading

Read before executing — and only this:

- The Global OBC at `{global-obc-path}` — extract: Produtos Envolvidos table, release, stakeholders, business outcome, events, SLIs, reliability rules.
- `prodops/framework/execution-mapping/work-item-schema.md` — for correct field values when filling Project #30.
- `prodops/exec/manifest.yaml` — for `github.org` and `projects.product_repository.number`.

## Preconditions

1. The Global OBC exists and is in status `Draft` or `Refining`.
2. Discovery in the BIB has produced enough understanding to identify all involved product repos and their responsibilities.
3. The Global BI Issue exists in prodops-portfolio and is added to Project #30.
4. The agent has write access to all target repositories via `gh` CLI.

If any precondition fails, stop and surface the blocker. Do not create partial Issues.

## Steps

Execute the steps below **sequentially for each product repo** in the `repos` list. Complete all four steps for one repo before moving to the next.

---

### Step 1 — Criar Issue de Local OBC no repo de produto

**Input:** repo name, scope description from Global OBC "Produtos Envolvidos" table, release, `global-bi-issue`, `bi-id`.

**Action:**

```bash
gh issue create \
  --repo produtoreativo/{repo} \
  --title "[{bi-id}] Local OBC — {global-obc-title}" \
  --body "## Local OBC — {bi-id}: {global-obc-title}

**Origin:** [{bi-id}](https://github.com/produtoreativo/prodops-portfolio/issues/{global-bi-issue}) — Global OBC: [{global-obc-path}](https://github.com/produtoreativo/prodops-portfolio/blob/prodops-workspace/{global-obc-path})
**Release:** {release}
**Execution Mode:** Downstream

### Escopo deste produto

{scope}

### Próximo passo

Criar \`prodops/artifacts/obcs/local-{bi-id-slug}-{repo}.md\` via PR neste repositório com o Local OBC Draft."
```

**Output:** Issue URL and number. Record both — they are required in Steps 2 and 3.

**Gate:** Issue must be created successfully (HTTP 201). If creation fails, fix the error before proceeding. Do not continue to Step 2 with a failed issue.

---

### Step 2 — Preencher campos do Project #30

**Input:** Issue node_id (from Step 1), Project #30 ID from `manifest.yaml`.

**Action:** Add the Issue to Project #30 and set all canonical fields:

| Field | Value |
|---|---|
| `Artifact Type` | `Local OBC` |
| `Artifact ID` | `local-{bi-id-slug}-{repo}` |
| `Operation` | `Define` |
| `Journey` | `Discovery` |
| `Execution Mode` | `Downstream` |
| `Horizon` | `Agora` |
| `Release` | `{release}` |
| `Owner` | `{repo} Tech Lead` |
| `Evidence Required` | `Não` |

Use GraphQL mutations `addProjectV2ItemById` then `updateProjectV2ItemFieldValue` for each field. All fields are mandatory — do not skip any.

**Gate:** All nine fields must be set. Verify by querying the item back if needed. Do not proceed to Step 3 with missing fields.

---

### Step 3 — Adicionar como sub-issue do Global BI Issue

**Input:** numeric database ID of the Issue created in Step 1 (`gh api repos/produtoreativo/{repo}/issues/{number} | jq '.id'`), `global-bi-issue` number.

**Action:**

```bash
gh api repos/produtoreativo/prodops-portfolio/issues/{global-bi-issue}/sub_issues \
  --method POST \
  --field sub_issue_id={issue-db-id}
```

**Output:** confirmation that `sub_issue.number` matches the Issue number from Step 1.

**Gate:** This step is mandatory. A Local OBC Issue that is not a sub-issue of the Global BI Issue is a governance violation — the Partitioning is incomplete. If the API returns an error, diagnose and fix before proceeding to Step 4. Do not mark the Partitioning as complete for this repo if this gate fails.

---

### Step 4 — Atualizar rastreabilidade no Global OBC

**Input:** repo name, Issue URL (Step 1), current date.

**Action:** Update the "Rastreabilidade de Local OBCs" table in `{global-obc-path}`:

```markdown
| {repo} | `prodops/artifacts/obcs/local-{bi-id-slug}-{repo}.md` | [{repo}#{number}]({issue-url}) | Draft (aguardando PR no repo) | {YYYY-MM-DD} |
```

Replace the placeholder row for this repo. Do not modify any other section of the Global OBC.

**Gate:** The file must be saved. Verify the table row was updated before moving to the next repo in the list.

---

## After all repos are processed

1. Commit the updated Global OBC file:

```
feat(prodops): OBC Partitioning — {bi-id} ({release})

Local OBC Issues created and linked as sub-issues of prodops-portfolio#{global-bi-issue}.
Repos: {repo-list}.
```

2. Confirm the sub-issue hierarchy is correct:

```bash
gh api repos/produtoreativo/prodops-portfolio/issues/{global-bi-issue}/sub_issues \
  | jq -r '.[] | "#\(.number) [\(.repository.full_name)]"'
```

Expected output: one entry per repo in the `repos` list.

## Quality Gates (completion criteria)

The Partitioning is complete only when **all** of the following are true:

| Gate | Check |
|---|---|
| Issues created | One open Issue per repo in `repos` list |
| Project #30 filled | All nine canonical fields set on each Issue item |
| Sub-issues linked | Each Issue appears as sub-issue of `prodops-portfolio#{global-bi-issue}` |
| Global OBC updated | Rastreabilidade table has one row per repo, all with Issue URL and date |
| Commit pushed | Updated Global OBC committed to `prodops-workspace` |

Do not declare the Partitioning complete if any gate is unmet. Report the specific blocker.

## Guardrails

- Do not create Local OBC Issues without a corresponding row in the Global OBC "Produtos Envolvidos" table.
- Do not skip Step 3 (sub-issue). A Local OBC Issue without a parent Global BI Issue is a governance violation detectable by Diligence.
- Do not create Issues with `milestone` unless the Platform Release milestone already exists in the target repo. Create the milestone first if missing.
- Do not invent scope for a repo — extract it from the Global OBC. If the scope is unclear, stop and ask.
- Do not modify the Global OBC beyond the "Rastreabilidade de Local OBCs" table in Step 4.
- Do not create more than one Issue per repo per `bi-id`. If an Issue already exists (search by title pattern `[{bi-id}] Local OBC`), use it instead of creating a duplicate.
- All Work Items must follow the canonical title pattern: `[{bi-id}] Local OBC — {description}`. See `prodops/framework/execution-mapping/work-item-schema.md`.

## References

→ [Global OBC template](../../../templates/obcs/global-obc.md)
→ [Local OBC template](../../../templates/obcs/local-obc.md)
→ [Work Item Schema](../../../framework/execution-mapping/work-item-schema.md)
→ [Execution Mapping Matrix](../../../framework/execution-mapping/matrix.md)
→ [Backlogs](../../../framework/backlogs.md)
