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
- **Resultado esperado:** 400 com mensagem de erro.
- **Endpoint:** POST /authors, PUT /authors/:id.
- **Origem:** Banco (`allowNull: false`) + Decisão (vazio/espaços).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência, valor limite.

### RN02: Duplicidade de nome

- **Regra:** Não permitir o cadastro de dois autores com o mesmo nome.
- **Resultado esperado:** 400 com mensagem de erro.
- **Endpoint:** POST /authors.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN03: Consulta de autor por ID inexistente

- **Regra:** Ao consultar autor por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** get /authors / :id.
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN04: Pesquisa de autor sem correspondência

- **Regra:** A API deve tratar pesquisas por autores que não correspondam a nenhum registro.
- **Resultado esperado:** HTTP 200 com uma lista vazia ([]).
- **Endpoint:** GET /authors?name=...
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN05: Excluir autor existente

- **Regra:** A API deve permitir excluir um autor existente.
- **Resultado esperado:** HTTP 204 (No Content).
- **Endpoint:** DELETE /authors/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN06: Excluir autor inexistente

- **Regra:** Ao tentar excluir um autor por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** HTTP 404 (Not Found).
- **Endpoint:** DELETE /authors/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Livros

### RN01: Autor obrigatório

- **Regra:** Todo livro deve estar associado a um autor. O author_id é obrigatório.
- **Resultado esperado:** A definir conforme a validação da API.
- **Endpoint:** POST /books, PUT /books/:id.
- **Origem:** Banco ou Decisão, conforme a implementação.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN02: Campos obrigatórios do livro

- **Regra:** title, genre e publication_year são obrigatórios. Valores vazios devem ser rejeitados.
- **Resultado esperado:** HTTP 400 com mensagem de erro.
- **Endpoint:** POST /books, PUT /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN03: Duplicidade de título de livro

- **Regra:** A definir se a API deve permitir o cadastro de livros com títulos já existentes.
- **Resultado esperado:** A definir.
- **Endpoint:** POST /books.
- **Origem:** Decisão.
- **Situação:** Decidir.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN04: Pesquisa de livro sem correspondência

- **Regra:** A API deve tratar pesquisas por livros que não correspondam a nenhum registro.
- **Resultado esperado:** HTTP 200 com uma lista vazia ([]).
- **Endpoint:** GET /books?name=...
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN05: Excluir livro existente

- **Regra:** A API deve permitir excluir um livro existente.
- **Resultado esperado:** HTTP 204 (No Content).
- **Endpoint:** DELETE /authors/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN06: Excluir livro inexistente

- **Regra:** Ao tentar excluir um livro por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** HTTP 404 (Not Found).
- **Endpoint:** DELETE /authors/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Bibliotecas

### RN01: Local obrigatório

- **Regra:** 'Local' é obrigatório. Vazio ou só espaço é rejeitado.
- **Resultado esperado:** 400 com mensagem de erro.
- **Endpoint:** POST /libraries, PUT /libraries.
- **Origem:** Banco ou Decisão, conforme a implementação.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN02: Duplicidade de local

- **Regra:** Não permitir o cadastro de mais de uma biblioteca com o mesmo local.
- **Resultado esperado:** 400 com mensagem de erro.
- **Endpoint:** POST /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN03: Consulta de livrbibliotecaria por ID inexistente

- **Regra:** Ao consultar uma biblioteca por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN04: Excluir biblioteca existente

- **Regra:** A API deve permitir excluir uma biblioteca existente.
- **Resultado esperado:** 204 (No Content).
- **Endpoint:** DELETE /libraries.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN05: Excluir biblioteca inexistente

- **Regra:** Ao tentar excluir uma biblioteca inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** DELETE /libraries.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Estoque (library-books)

### RN01: Associação entre livro e biblioteca

- **Regra:** Um livro não pode ser associado duas vezes à mesma biblioteca.
- **Resultado esperado:** 409 Conflict.
- **Endpoint:** POST /library-books.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN02: Existência das entidades para a criação

- **Regra:** A associação somente pode ser criada se o livro e a biblioteca informados existirem.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** POST /library-books.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN03: Quantidade positiva no cadastramento

- **Regra:** As quantidades de exemplares totais e disponíveis devem ser maiores que zero.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /library-books.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN04: Quantidade positiva na atualização

- **Regra:** As quantidades de exemplares totais e disponíveis devem ser maiores que zero durante a atualização de uma associação.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** PUT /library-books/:library_id/:book_id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN05: Limite de disponibilidade

- **Regra:** A quantidade de exemplares disponíveis não pode exceder a quantidade total de exemplares cadastrados.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /library-books, PUT /library-books/:library_id/:book_id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN06: Existência da associação para exclusão

- **Regra:** A associação entre livro e biblioteca deve existir para que possa ser excluída.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** DELETE /library-books/:library_id/:book_id
- **Origem:** Decisão
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.
