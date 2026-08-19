[Português](README.md)

# ProdOps Portfolio

This repository is a **consumer** of the [ProdOps Framework](https://github.com/produtoreativo/prodops-framework). It contains product artifacts, operational configuration, and local skills for the ProdOps portfolio.

> Installed framework version: see `prodops/exec/framework-lock.yaml`
> Canonical framework documentation: [`prodops-framework`](https://github.com/produtoreativo/prodops-framework)

---

## Repository structure

| Area | Description |
|---|---|
| [prodops/framework/](prodops/framework/) | Canonical framework — do not modify per product |
| [prodops/artifacts/business-intents/](prodops/artifacts/business-intents/) | Registered Business Intents |
| [prodops/framework/journeys/](prodops/framework/journeys/) | The 5 journeys: Discovery, Delivery, Operation, Assessment, Diligence |
| [prodops/artifacts/](prodops/artifacts/) | Produced artifacts: OBCs, BDD Features, plans, trails, evidence |
| [prodops/templates/](prodops/templates/) | Centralized templates by area |
| [prodops/skills/](prodops/skills/) | Executable skills for agents |

---

## Framework documentation

| Document | Description |
|---|---|
| [prodops/framework/principles.en.md](prodops/framework/principles.en.md) | Foundational principles |
| [prodops/framework/glossary.en.md](prodops/framework/glossary.en.md) | Canonical terms |
| [prodops/framework/flow.en.md](prodops/framework/flow.en.md) | Official Framework flow |
| [prodops/framework/origin-streams.en.md](prodops/framework/origin-streams.en.md) | The four Origin Streams |
| [prodops/framework/operating-model.en.md](prodops/framework/operating-model.en.md) | Full operating model |
| [prodops/framework/knowledge-vs-execution.en.md](prodops/framework/knowledge-vs-execution.en.md) | Knowledge × Execution separation |
| [prodops/framework/execution-mapping/README.en.md](prodops/framework/execution-mapping/README.en.md) | Execution Mapping capability |
| [prodops/framework/backlogs.en.md](prodops/framework/backlogs.en.md) | Backlogs and Work Item types |
| [prodops/framework/artifact-governance.en.md](prodops/framework/artifact-governance.en.md) | Artifact governance |

---

## How to keep in sync

```bash
# Update to a new version (opens PR)
bash prodops/scripts/sync-from-framework.sh --version v1.14.0

# Check for drift without changes
bash prodops/scripts/sync-from-framework.sh --check
```
