# Global OBC — BI-01: Compra de 1 item por Pix via Listagem

## Status

Draft. Discovery em andamento no Business Intent Backlog.

---

## Objetivo de Negócio

Permitir que o cliente realize a compra de 1 item via Pix a partir da listagem de produtos da plataforma, com fluxo completo de pedido e pagamento rastreável, viabilizando a operação da plataforma antes da Black Friday.

---

## Valor de Negócio

A plataforma passa a ter um canal de venda digital funcional com o meio de pagamento de maior adoção no Brasil. As Releases 1.0 e 1.1 estabelecem o Value Stream central sobre o qual todas as demais releases do roadmap serão construídas.

| Métrica | Baseline atual | Meta | Prazo |
|---|---|---|---|
| Conversão de compra via Pix | 0% (canal não existe) | Fluxo funcional e rastreável | Release 1.1.0 |
| Pedidos criados com sucesso | 0 | Fluxo estável em produção | Release 1.0.0 |

---

## Stakeholders

| Stakeholder | Papel | Responsabilidade |
|---|---|---|
| Áreas de negócio | Patrocinador | Validaram o roadmap até BF |
| Portfolio PM | Aprovador | OBC Partitioning e tracking |
| Tech Leads (webshop-api, order-mngt-api, payments-api) | Impactados | Local OBCs e entrega |

---

## Regras de Negócio

- Um pedido só pode ser criado para itens disponíveis na listagem
- O pagamento Pix deve ter prazo de expiração definido
- O status do pedido deve refletir o estado do pagamento em tempo real
- O fluxo de Release 1.0 deve funcionar sem pagamento (pedido criado, sem invoice)
- O fluxo de Release 1.1 adiciona a invoice Pix ao fluxo de pedido existente — sem reescrever o fluxo anterior

---

## Eventos de Negócio

| Evento | Significado | Quando ocorre |
|---|---|---|
| `pedido.criado` | Cliente formalizou intenção de compra | Ao confirmar item na listagem |
| `pedido.recebido` | Order management processou o pedido | Após criação pelo webshop-api |
| `invoice.pix.criada` | Invoice Pix gerada para o pedido | Após recebimento do pedido (R1.1) |
| `pagamento.confirmado` | Pagamento Pix confirmado pelo provider | Após confirmação bancária (R1.2+) |

---

## KPIs / Resultados Esperados

| KPI | Baseline | Meta | Prazo |
|---|---|---|---|
| Fluxo pedido → recebimento funcional | Não existe | 100% dos pedidos criados chegam ao order-mngt-api | Release 1.0.0 |
| Fluxo pedido → invoice Pix funcional | Não existe | 100% dos pedidos com invoice gerada | Release 1.1.0 |
| Listagem de produtos disponível | Não existe | Listagem simples retornando produtos do catálogo | Release 1.0.0 |

---

## Value Stream

```
Cliente → webshop (listagem) → webshop-api (criar pedido) → order-mngt-api (receber pedido)
                                                                      ↓ (Release 1.1)
                                                            payments-api (invoice Pix)
```

---

## Produtos Envolvidos

| Produto / Repositório | Release | Responsabilidade esperada |
|---|---|---|
| webshop | 1.0 | Front-end do Value Stream: tela de listagem e fluxo de compra |
| webshop-api | 1.0 | Listagem de Produtos, Criar Pedido, Detalhe do Pedido |
| search-api | 1.0 | Listagem simples de produtos do catálogo |
| order-mngt-api | 1.0 | Criar Pedido, Receber Pedido |
| payments-api | 1.1 | Criar Invoice Pix, Listar meios de pagamento |

---

## Rastreabilidade de Local OBCs

| Produto | Local OBC | Issue de Partitioning | Estado | Última atualização |
|---|---|---|---|---|
| webshop | `prodops/artifacts/obcs/local-bi-01-webshop.md` | [webshop#3](https://github.com/produtoreativo/webshop/issues/3) | Draft (aguardando PR no repo) | 2026-08-18 |
| webshop-api | `prodops/artifacts/obcs/local-bi-01-webshop-api.md` | [webshop-api#18](https://github.com/produtoreativo/webshop-api/issues/18) | Draft (aguardando PR no repo) | 2026-08-18 |
| search-api | `prodops/artifacts/obcs/local-bi-01-search-api.md` | [search-api#11](https://github.com/produtoreativo/search-api/issues/11) | Draft (aguardando PR no repo) | 2026-08-18 |
| order-mngt-api | `prodops/artifacts/obcs/local-bi-01-order-mngt-api.md` | [order-mngt-api#11](https://github.com/produtoreativo/order-mngt-api/issues/11) | Draft (aguardando PR no repo) | 2026-08-18 |
| payments-api | `prodops/artifacts/obcs/local-bi-01-payments-api.md` | [payments-api#181](https://github.com/produtoreativo/payments-api/issues/181) | Draft (aguardando PR no repo — Release 1.1) | 2026-08-18 |

---

## Notas de Discovery

### Hipóteses

- [ ] Listagem simples via search-api é suficiente para o MVP sem busca avançada
- [ ] webshop-api atua como BFF — orquestra search-api e order-mngt-api sem lógica de negócio própria
- [ ] payments-api pode ser integrado na Release 1.1 sem alterar o contrato do fluxo de pedido da Release 1.0
- [ ] webshop (front-end) tem capacidade de entregar as telas para a Release 1.0

### Experimentos

— (nenhum experimento necessário; modo Downstream)

### Decisões

- Adoção de Downstream para todas as releases: escopo validado com negócio, sem incerteza estratégica que justifique Upstream
- Release 1.0 entrega o fluxo sem pagamento intencionalmente — permite validar o pipeline de pedidos independente de integração financeira
