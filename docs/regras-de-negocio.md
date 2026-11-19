# Regras de negócio: Library Management API

- **Sistema:** node-sequelize-library-api (tag `v1-para-teste`)
- **Fonte das regras:** README, models/migrations e decisões minhas
- **Última atualização:** 2026-11-19

## Como ler este documento

- Cada regra tem um ID (RN01, RN02...). Os casos de teste da planilha apontam para esse ID.
- **Origem:** de onde veio a regra.
  - `README`: está documentada
  - `Banco`: vem de constraint ou migration
  - `Decisão`: o sistema não define, eu escolhi o comportamento esperado
- **Situação:**
  - `Confirmada`: não há dúvida do que é esperado
  - `Decidir`: precisa de decisão (registrar também em `lacunas-e-duvidas.md`)
- **Prioridade:** Alta, Média ou Baixa (risco de falhar × impacto)

## Módulo: Autores

### RN01: Nome e nacionalidade obrigatórios

- **Regra:** `name` e `nationality` são obrigatórios. Vazio ou só espaços é rejeitado.
- **Resultado esperado:** 400 com mensagem de erro
- **Endpoint:** POST /authors, PUT /authors/:id
- **Origem:** Banco (`allowNull: false`) + Decisão (vazio/espaços)
- **Situação:** Confirmada
- **Prioridade:** Alta
- **Técnica sugerida:** Partição de equivalência, valor limite

### RN02: Duplicidade de nome

- **Regra:** Não permitir o cadastro de dois autores com o mesmo nome.
- **Resultado esperado:** 400 com mensagem de erro.
- **Endpoint:** POST /authors
- **Origem:** Decisão
- **Situação:** Decidir, até confirmar o comportamento esperado.
- **Prioridade:** Média
- **Técnica sugerida:** Partição de equivalência

### RN03: Consulta de autor por ID inexistente

- **Regra:** Ao consultar autor por ID inexistente, a API deve informar que o recurso não foi encontrado
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** get /authors / :id
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Decidir.
- **Prioridade:** Média
- **Técnica sugerida:** Partição de equivalência

### RN04: Pesquisa de autor sem correspondência

- **Regra:** A API deve tratar pesquisas por autores que não correspondam a nenhum registro.
- **Resultado esperado:** HTTP 200 com uma lista vazia ([]).
- **Endpoint:** GET /authors?name=...
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Decidir.
- **Prioridade:** Média
- **Técnica sugerida:** Partição de equivalência
