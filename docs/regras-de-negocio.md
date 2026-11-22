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
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /authors, PUT /authors/:id.
- **Origem:** Banco (`allowNull: false`) + Decisão (vazio/espaços).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência, valor limite.

### RN02: Duplicidade de nome

- **Regra:** Não permitir o cadastro de dois autores com o mesmo nome.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /authors.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN03: Consulta de autor por ID inexistente

- **Regra:** Ao consultar autor por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** get /authors /:id.
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
- **Resultado esperado:** HTTP 204 No Content.
- **Endpoint:** DELETE /authors/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN06: Excluir autor inexistente

- **Regra:** Ao tentar excluir um autor por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** HTTP 404 Not Found.
- **Endpoint:** DELETE /authors/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Livros

### RN01: Autor obrigatório

- **Regra:** Todo livro deve estar associado a um autor. O author_id é obrigatório.
- **Resultado esperado:** A API deve rejeitar o cadastro ou a atualização de um livro quando author_id não for informado, retornando erro 400 Bad Request.
- **Endpoint:** POST /books, PUT /books/:id.
- **Origem:** Banco.
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
- **Endpoint:** GET /books?title=...
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN05: Excluir livro existente

- **Regra:** A API deve permitir excluir um livro existente.
- **Resultado esperado:** HTTP 204 No Content.
- **Endpoint:** DELETE /books/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN06: Excluir livro inexistente

- **Regra:** Ao tentar excluir um livro por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** HTTP 404 Not Found.
- **Endpoint:** DELETE /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Bibliotecas

### RN01: Local obrigatório

- **Regra:** 'Local' é obrigatório. Vazio ou só espaço é rejeitado.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /libraries, PUT /libraries/:id.
- **Origem:** Banco ou Decisão, conforme a implementação.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN02: Duplicidade de local

- **Regra:** Não permitir o cadastro de mais de uma biblioteca com o mesmo local.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN03: Consulta de biblioteca por ID inexistente

- **Regra:** Ao consultar uma biblioteca por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** GET /libraries/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN04: Excluir biblioteca existente

- **Regra:** A API deve permitir excluir uma biblioteca existente.
- **Resultado esperado:** 204 No Content.
- **Endpoint:** DELETE /libraries/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN05: Excluir biblioteca inexistente

- **Regra:** Ao tentar excluir uma biblioteca inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** DELETE /libraries/:id.
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

## Módulo: Paginação e listagem

### RN01: Valores padrão da paginação

- **Regra:** Quando os parâmetros page e limit não forem informados, a API deverá utilizar os valores padrão page=1 e limit=20.
- **Resultado esperado:** A listagem deverá retornar os primeiros 20 registros, respeitando a ordenação definida para o recurso.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Código.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN02: Paginação com parâmetros informados

- **Regra:** Quando os parâmetros page e limit forem informados, a API deverá utilizá-los para determinar a página e a quantidade máxima de registros retornados.
- **Resultado esperado:** A API deverá retornar os registros correspondentes à página solicitada, respeitando o limite informado. Caso a última página possua menos registros que o limite, deverá retornar todos os registros disponíveis.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Código.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência e valores-limite.

### RN03: Listagem sem registros

- **Regra:** Quando uma consulta de listagem não retornar registros, a API deverá responder com status HTTP 200 e uma lista vazia.
- **Resultado esperado:** A API deverá retornar 200 OK com o corpo [], tanto quando a página solicitada não possuir registros quanto quando nenhum registro corresponder aos filtros informados.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN04: Filtros de pesquisa textual

- **Regra:** Os campos textuais disponíveis na listagem deverão permitir pesquisas parciais, sem diferenciação entre letras maiúsculas e minúsculas.
- **Resultado esperado:** Ao informar parte de um texto, a API deverá retornar todos os registros que contenham o termo pesquisado no campo correspondente, independentemente da capitalização.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Código.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN05: Filtros de pesquisa numérica

- **Regra:** Os campos numéricos disponíveis na listagem deverão permitir pesquisas por correspondência exata.
- **Resultado esperado:** Ao informar um ano de publicação, a API deverá retornar somente os livros cujo campo publication_year corresponda exatamente ao valor pesquisado.
- **Endpoint:** GET /books.
- **Origem:** Código.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN06: Ordenação dinâmica dos resultados

- **Regra:** A API deverá permitir que o usuário escolha o campo e a direção da ordenação, respeitando os campos previamente autorizados para cada recurso.
- **Resultado esperado:** A listagem deverá permitir ordenação crescente (ASC) ou decrescente (DESC) pelos seguintes campos:
- Authors: name, nationality.
- Books: title, genre, publication_year.
- Libraries: local.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN07: Validação dos parâmetros de ordenação

- **Regra:** A API deverá validar os campos e as direções informados nos parâmetros de ordenação.
- **Resultado esperado:** Quando o usuário informar um campo não autorizado para o recurso ou uma direção diferente de ASC e DESC, a API deverá retornar HTTP 400 Bad Request.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### RN08: Combinação de filtros

- **Regra:** A API deverá permitir a utilização de múltiplos filtros em uma mesma consulta, aplicando todos os critérios simultaneamente.
- **Resultado esperado:** A listagem deverá retornar somente os registros que atendam a todos os filtros informados.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Código.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### RN09: Tratamento de filtros vazios

- **Regra:** API deverá ignorar parâmetros de filtros textuais ou numéricos quando seus valores estiverem vazios.
- **Resultado esperado:** A listagem deverá ser realizada sem aplicar o filtro vazio, mantendo os demais filtros informados, se houver.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Código.
- **Situação:** Confirmada.
- **Prioridade:** Baixa.
- **Técnica sugerida:** Partição de equivalência.

### RN10: Validação dos parâmetros de paginação

- **Regra:** Os parâmetros page e limit deverão aceitar somente números inteiros positivos.
- **Resultado esperado:** Caso o usuário informe valores iguais a zero, negativos, decimais ou não numéricos, a API deverá retornar HTTP 400 Bad Request.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência e valores-limite.

### RN11: Limite máximo de registros por página

- **Regra:** O parâmetro limit deverá aceitar no máximo 100 registros por página.
- **Resultado esperado:** Quando o usuário informar um valor superior a 100, a API deverá retornar HTTP 400 (Bad Request). Valores entre 1 e 100 deverão ser aceitos.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Valores-limite.

## Regras com situação "Decidir"

Lista rápida das regras pendentes, com a decisão tomada:

### RN01: Filtro de livros pela nacionalidade dos autores relacionados

- **Regra:** Permitir pesquisar livros utilizando a nacionalidade dos autores associados a eles.
- **Resultado esperado:** Definir.
- **Endpoint:** GET /books?nationality=...
- **Origem:** Decisão.
- **Situação:** Decidir.
- \*\*Ponto pendente: Definir como o filtro deve se comportar quando um livro possuir mais de um autor.
