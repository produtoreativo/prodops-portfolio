# Business Intent — BI-01: Compra de 1 item por Pix via Listagem

Localização canônica: `prodops/artifacts/business-intents/bi-01-compra-pix-listagem.md`

---

## Identificação

| Campo | Conteúdo |
|---|---|
| ID | BI-01 |
| Título | Compra de 1 item por Pix via Listagem |
| Origin Stream | Business |
| Data de registro | 2026-08-18 |
| Solicitante | Áreas de negócio (validado até Black Friday) |
| Dono de produto | Portfolio PM |
| Business Signal | [BS-01](../product/backlogs/tracking-list.md) |

---

## Intenção

Queremos que o cliente consiga comprar 1 item via Pix a partir da listagem de produtos da plataforma, para que a plataforma tenha um fluxo de compra funcional e rastreável com pagamento digital antes da Black Friday.

---

## Contexto

O roadmap validado com as áreas de negócio (Discussion #2) compromete a entrega de um fluxo de compra completo até a Black Friday. A Release 1.0.0 estabelece o MVP de compra sem pagamento (fluxo de pedido) e a Release 1.1.0 integra o pagamento por Pix. Este é o Value Stream central da plataforma — sem ele, as demais releases não têm base de operação.

---

## Hipóteses

- [ ] A listagem de produtos com busca simples é suficiente para o fluxo de compra do MVP (Release 1.0)
- [ ] O Pix é o meio de pagamento prioritário para o público-alvo no lançamento
- [ ] A separação webshop-api / order-mngt-api / payments-api permite entregar as releases incrementalmente sem retrabalho
- [ ] O fluxo Criar Pedido → Receber Pedido → Criar Invoice Pix cobre os casos de uso essenciais da Release 1.1

---

## Perguntas em aberto

- [ ] Qual o comportamento esperado quando o pagamento Pix expira sem confirmação?
- [ ] O webshop (front-end) tem capacidade de entregar as telas de listagem e detalhe de pedido para a Release 1.0?
- [ ] Existe dependência de autenticação/sessão do usuário não mapeada neste BI?
- [ ] O search-api com listagem simples já suporta o volume esperado de Black Friday?

---

## Modo de execução sugerido

- [x] **Downstream** — há clareza suficiente sobre o escopo das Releases 1.0 e 1.1; OBC e BDD podem ser escritos por produto

Justificativa: o Roadmap Black Friday já foi validado com as áreas de negócio e tem releases com escopo definido. As perguntas em aberto são de refinamento, não de viabilidade estratégica.

---

## Platform Releases

| Release | Escopo desta BI | Produtos envolvidos |
|---|---|---|
| 1.0.0 | Fluxo de pedido sem pagamento (MVP de listagem + pedido) | webshop, webshop-api, search-api, order-mngt-api |
| 1.1.0 | Integração de pagamento Pix ao fluxo de pedido | + payments-api |

---

## Artefatos gerados

| Artefato | Localização |
|---|---|
| Global OBC | [global-bi-01-compra-pix-listagem.md](../obcs/global-bi-01-compra-pix-listagem.md) |
