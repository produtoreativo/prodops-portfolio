[English](README.en.md)

# ProdOps Portfolio

Este repositório é um **consumer** do [ProdOps Framework](https://github.com/produtoreativo/prodops-framework). Contém artefatos de produto, configuração operacional e skills locais do portfólio ProdOps.

> Framework instalado: veja `prodops/exec/framework-lock.yaml`
> Documentação canônica do Framework: [`prodops-framework`](https://github.com/produtoreativo/prodops-framework)

---

## Estrutura do repositório

| Área | Descrição |
|---|---|
| [prodops/framework/](prodops/framework/) | Framework canônico — não modificar por produto |
| [prodops/artifacts/business-intents/](prodops/artifacts/business-intents/) | Business Intents registradas |
| [prodops/framework/journeys/](prodops/framework/journeys/) | As 5 jornadas: Discovery, Delivery, Operation, Assessment, Diligence |
| [prodops/artifacts/](prodops/artifacts/) | Artefatos produzidos: OBCs, BDD Features, planos, trilhas, evidências |
| [prodops/templates/](prodops/templates/) | Templates centralizados por área |
| [prodops/skills/](prodops/skills/) | Skills executáveis por agentes |

---

## Documentação do Framework

| Documento | Descrição |
|---|---|
| [prodops/framework/principles.md](prodops/framework/principles.md) | Princípios fundacionais |
| [prodops/framework/glossary.md](prodops/framework/glossary.md) | Termos canônicos |
| [prodops/framework/flow.md](prodops/framework/flow.md) | Fluxo oficial do Framework |
| [prodops/framework/origin-streams.md](prodops/framework/origin-streams.md) | Os quatro Origin Streams |
| [prodops/framework/operating-model.md](prodops/framework/operating-model.md) | Modelo operacional completo |
| [prodops/framework/knowledge-vs-execution.md](prodops/framework/knowledge-vs-execution.md) | Separação Knowledge × Execution |
| [prodops/framework/execution-mapping/README.md](prodops/framework/execution-mapping/README.md) | Execution Mapping capability |
| [prodops/framework/backlogs.md](prodops/framework/backlogs.md) | Backlogs e tipos de Work Item |
| [prodops/framework/artifact-governance.md](prodops/framework/artifact-governance.md) | Governança de artefatos |

---

## Como manter em sync

```bash
# Atualizar para uma nova versão (abre PR)
bash prodops/scripts/sync-from-framework.sh --version v1.14.0

# Verificar drift sem alterar
bash prodops/scripts/sync-from-framework.sh --check
```
