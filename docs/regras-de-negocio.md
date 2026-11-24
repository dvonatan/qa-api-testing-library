# Regras de negócio: Library Management API

- **Sistema:** node-sequelize-library-api (tag `v1-para-teste`)
- **Fonte das regras:** README, models/migrations e decisões minhas
- **Última atualização:** 2026-11-24

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

### AUT-RN01: Nome e nacionalidade obrigatórios

- **Regra:** `name` e `nationality` são obrigatórios. Vazio ou só espaços é rejeitado.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /authors, PUT /authors/:id.
- **Origem:** Banco (`allowNull: false`) + Decisão (vazio/espaços).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência, valor limite.

### AUT-RN02: Duplicidade de nome

- **Regra:** Não permitir o cadastro de autores com nomes duplicados. Para verificar a duplicidade, a comparação deverá desconsiderar diferenças entre letras maiúsculas e minúsculas, remover espaços no início e no final e reduzir sequências de espaços internos a um único espaço. A comparação deverá preservar os acentos, de modo que nomes com grafias diferentes pela presença ou ausência de acentos sejam considerados distintos.
- **Resultado esperado:** A API deve rejeitar o cadastro de um autor cujo nome, após a normalização para comparação, já esteja cadastrado, retornando HTTP 409 Conflict.
- **Endpoint:** POST /authors.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### AUT-RN03: Consulta de autor por ID inexistente

- **Regra:** Ao consultar autor por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** GET /authors/:id.
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### AUT-RN04: Pesquisa de autor sem correspondência

- **Regra:** A API deve tratar pesquisas por autores que não correspondam a nenhum registro.
- **Resultado esperado:** HTTP 200 com uma lista vazia ([]).
- **Endpoint:** GET /authors?name=...
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### AUT-RN05: Excluir autor existente

- **Regra:** A API deve permitir excluir um autor existente.
- **Resultado esperado:** HTTP 204 No Content.
- **Endpoint:** DELETE /authors/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### AUT-RN06: Excluir autor inexistente

- **Regra:** Ao tentar excluir um autor por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** HTTP 404 Not Found.
- **Endpoint:** DELETE /authors/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### AUT-RN07: Exclusão de autor com livros associados

- **Regra:** Não permitir a exclusão de um autor que possua livros associados.
- **Resultado esperado:** A API deve rejeitar a exclusão do autor enquanto existirem livros associados a ele, retornando HTTP 409 Conflict.
- **Endpoint:** DELETE /authors/:id
- **Origem:** Banco + Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### AUT-RN08: Atualização de autor por ID inexistente

- **Regra:** Ao atualizar um autor por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** PUT /authors/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### AUT-RN09: Validação do formato de ID

- **Regra:** Os endpoints que recebem o ID de um autor pela URL devem validar seu formato. Caso o identificador informado não corresponda ao formato numérico esperado, a API deve rejeitar a requisição, sem realizar a consulta, atualização ou exclusão do recurso.
- **Resultado esperado:** A API deve retornar HTTP 400 Bad Request quando o ID informado apresentar formato inválido. IDs em formato válido, mas sem registro correspondente, devem continuar retornando HTTP 404 Not Found.
- **Endpoint:** GET /authors/:id, PUT /authors/:id e DELETE /authors/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Livros

### LIV-RN01: Autor obrigatório

- **Regra:** Todo livro deve estar associado a um autor. O author_id é obrigatório.
- **Resultado esperado:** A API deve rejeitar o cadastro ou a atualização de um livro quando author_id não for informado, retornando erro 400 Bad Request.
- **Endpoint:** POST /books, PUT /books/:id.
- **Origem:** Banco.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN02: Campos obrigatórios do livro

- **Regra:** Os campos title, genre e publication_year são obrigatórios. Os campos textuais não podem estar ausentes, ser null, conter strings vazias ou apenas espaços. O campo publication_year não pode estar ausente, ser null, vazio, conter apenas espaços, apresentar valor não numérico, decimal ou menor ou igual a zero. Deve aceitar somente números inteiros positivos.
- **Resultado esperado:** A API deve rejeitar o cadastro ou a atualização de um livro quando qualquer um dos campos obrigatórios não atender às condições estabelecidas, retornando HTTP 400 Bad Request.
- **Endpoint:** POST /books, PUT /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN03: Consulta de livro inexistente

- **Regra:** Ao consultar um livro pelo ID, caso não exista um registro correspondente na base de dados, a API deverá informar que o livro não foi encontrado.
- **Resultado esperado:** A API deve retornar HTTP 404 Not Found.
- **Endpoint:** GET /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN04: Permissão de títulos duplicados

- **Regra:** A API deve permitir que livros com títulos iguais sejam cadastrados ou atualizados, independentemente dos autores associados.
- **Resultado esperado:** Livros com títulos duplicados devem ser cadastrados ou atualizados normalmente, desde que atendam às demais regras de validação.
- **Endpoint:** POST /books, PUT /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN05: Atualização de livro por ID inexistente

- **Regra:** Ao atualizar um livro por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** PUT /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN06: Pesquisa de livro sem correspondência

- **Regra:** A API deve tratar pesquisas por livros que não correspondam a nenhum registro.
- **Resultado esperado:** HTTP 200 com uma lista vazia ([]).
- **Endpoint:** GET /books?title=...
- **Origem:** Decisão (o README não especifica o comportamento).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN07: Excluir livro existente

- **Regra:** A API deve permitir excluir um livro existente.
- **Resultado esperado:** HTTP 204 No Content.
- **Endpoint:** DELETE /books/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN08: Excluir livro inexistente

- **Regra:** Ao tentar excluir um livro por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** HTTP 404 Not Found.
- **Endpoint:** DELETE /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN09: Exclusão de livro associado a bibliotecas

- **Regra:** Não permitir a exclusão de um livro que esteja associado a uma ou mais bibliotecas.
- **Resultado esperado:** A API deve rejeitar a exclusão do livro enquanto existirem associações com bibliotecas, retornando HTTP 409 Conflict.
- **Endpoint:** DELETE /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN10: Validação do formato de ID

- **Regra:** Os endpoints que recebem o ID de um livro pela URL devem validar seu formato. Caso o identificador informado não corresponda ao formato numérico esperado, a API deve rejeitar a requisição, sem realizar a consulta, atualização ou exclusão do recurso.
- **Resultado esperado:** A API deve retornar HTTP 400 Bad Request quando o ID informado apresentar formato inválido. IDs em formato válido, mas sem registro correspondente, devem continuar retornando HTTP 404 Not Found.
- **Endpoint:** GET /books/:id, PUT /books/:id e DELETE /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN11: Referência a autor inexistente

- **Regra:** Não permitir o cadastro ou a atualização de livros com um author_id que não corresponda a nenhum autor cadastrado na API.
- **Resultado esperado:** A API deve rejeitar a requisição e retornar HTTP 404 Not Found quando o author_id informado não corresponder a um autor existente.
- **Endpoint:** POST /books e PUT /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### LIV-RN12: Validação do limite superior do ano de publicação

- **Regra:** O campo publication_year deve aceitar somente anos menores ou iguais ao ano corrente. Não permitir o cadastro ou a atualização de livros com ano de publicação futuro.
- **Resultado esperado:** A API deve retornar HTTP 400 Bad Request quando publication_year for maior que o ano corrente.
- **Endpoint:** POST /books e PUT /books/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Bibliotecas

### BIB-RN01: Local obrigatório

- **Regra:** local é obrigatório. Vazio ou só espaço é rejeitado.
- **Resultado esperado:** 400 Bad Request.
- **Endpoint:** POST /libraries, PUT /libraries/:id.
- **Origem:** Banco + Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### BIB-RN02: Duplicidade de local

- **Regra:** Não permitir o cadastro de bibliotecas com locais duplicados. Para verificar a duplicidade, a comparação deverá desconsiderar diferenças entre letras maiúsculas e minúsculas, remover espaços no início e no final e reduzir sequências de espaços internos a um único espaço. A comparação deverá preservar os acentos, de modo que locais com grafias diferentes pela presença ou ausência de acentos sejam considerados distintos.
- **Resultado esperado:** a API deve rejeitar o cadastro de uma biblioteca cujo local já esteja cadastrado, retornando HTTP 409 Conflict.
- **Endpoint:** POST /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### BIB-RN03: Consulta de biblioteca por ID inexistente

- **Regra:** Ao consultar uma biblioteca por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** GET /libraries/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### BIB-RN04: Excluir biblioteca existente

- **Regra:** A API deve permitir excluir uma biblioteca existente.
- **Resultado esperado:** 204 No Content.
- **Endpoint:** DELETE /libraries/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### BIB-RN05: Excluir biblioteca inexistente

- **Regra:** Ao tentar excluir uma biblioteca inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** DELETE /libraries/:id.
- **Origem:** README (endpoint documentado) + Decisão (resultado esperado).
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### BIB-RN06: Atualização de biblioteca por ID inexistente

- **Regra:** Ao atualizar uma biblioteca por ID inexistente, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** PUT /libraries/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### BIB-RN07: Validação do formato de ID

- **Regra:** Os endpoints que recebem o ID de uma biblioteca pela URL devem validar seu formato. Caso o identificador informado não corresponda ao formato numérico esperado, a API deve rejeitar a requisição, sem realizar a consulta, atualização ou exclusão do recurso.
- **Resultado esperado:** A API deve retornar HTTP 400 Bad Request quando o ID informado apresentar formato inválido. IDs em formato válido, mas sem registro correspondente, devem continuar retornando HTTP 404 Not Found.
- **Endpoint:** GET /libraries/:id, PUT /libraries/:id e DELETE /libraries/:id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### BIB-RN08: Exclusão de biblioteca com associações de estoque

- **Regra:** Permitir a exclusão de uma biblioteca mesmo que possua associações de estoque cadastradas. Ao excluir a biblioteca, todas as associações vinculadas a ela deverão ser removidas automaticamente.
- **Resultado esperado:** A API deve excluir a biblioteca e suas respectivas associações de estoque, retornando HTTP 204 No Content.
- **Endpoint:** DELETE /libraries/:id.
- **Origem:** Banco + Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Estoque (library-books)

### EST-RN01: Associação entre livro e biblioteca

- **Regra:** Um livro não pode ser associado duas vezes à mesma biblioteca.
- **Resultado esperado:** 409 Conflict.
- **Endpoint:** POST /library-books.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### EST-RN02: Existência das entidades para a criação

- **Regra:** A associação somente pode ser criada se o livro e a biblioteca informados existirem.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** POST /library-books.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### EST-RN03: Validação das quantidades no cadastramento.

- **Regra:** total_copies e available_copies devem aceitar somente valores numéricos inteiros. total_copies deve ser maior que zero, e available_copies deve ser maior ou igual a zero.
- **Resultado esperado:** A API deve rejeitar o cadastro quando total_copies ou available_copies receberem valores decimais ou textuais, quando total_copies for menor ou igual a zero ou quando available_copies for negativo, retornando HTTP 400 Bad Request.
- **Endpoint:** POST /library-books.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### EST-RN04: Validação das quantidades na atualização.

- **Regra:** total_copies e available_copies devem aceitar somente valores numéricos inteiros. total_copies deve ser maior que zero, e available_copies deve ser maior ou igual a zero.
- **Resultado esperado:** A API deve rejeitar a atualização quando total_copies ou available_copies receberem valores decimais ou textuais, quando total_copies for menor ou igual a zero ou quando available_copies for negativo, retornando HTTP 400 Bad Request.
- **Endpoint:** PUT /library-books/:library_id/:book_id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### EST-RN05: Limite de disponibilidade

- **Regra:** available_copies não pode exceder total_copies cadastrados.
- **Resultado esperado:** A API deve rejeitar o cadastro ou a atualização quando available_copies for maior que total_copies, retornando HTTP 400 Bad Request.
- **Endpoint:** POST /library-books, PUT /library-books/:library_id/:book_id.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### EST-RN06: Existência da associação para exclusão

- **Regra:** A associação entre livro e biblioteca deve existir para que possa ser excluída.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** DELETE /library-books/:library_id/:book_id.
- **Origem:** Decisão
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### EST-RN07: Atualização de associação de estoque inexistente

- **Regra:** Ao atualizar uma associação entre livro e biblioteca que não exista, a API deve informar que o recurso não foi encontrado.
- **Resultado esperado:** 404 Not Found.
- **Endpoint:** PUT /library-books/:library_id/:book_id.
- **Origem:** Decisão
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### EST-RN08: Validação do formato dos IDs

- **Regra:** Os endpoints que recebem os identificadores de uma biblioteca e de um livro pela URL devem validar seus formatos. Caso qualquer um dos identificadores não corresponda ao formato numérico esperado, a API deve rejeitar a requisição, sem realizar a consulta, atualização ou exclusão da associação.
- **Resultado esperado:** A API deve retornar HTTP 400 Bad Request quando library_id ou book_id apresentar formato inválido. Identificadores em formato válido, mas sem registro ou associação correspondente, devem continuar retornando HTTP 404 Not Found.
- **Endpoint:** PUT /library-books/:library_id/:book_id e DELETE /library-books/:library_id/:book_id.
- **Origem:** Decisão
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

## Módulo: Paginação e listagem

### PAG-RN01: Valores padrão da paginação

- **Regra:** Os parâmetros page e limit são opcionais. Quando não forem informados, seus valores padrão serão aplicados individualmente: page = 1 e limit = 20.
- **Resultado esperado:**
- Sem os dois parâmetros: retornar a primeira página com até 20 registros.
- Somente page informado: utilizar o valor recebido e limit = 20.
- Somente limit informado: utilizar page = 1 e o valor recebido.
- Ambos informados: utilizar os valores recebidos.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN02: Paginação com parâmetros informados

- **Regra:** Quando os parâmetros page e limit forem informados, a API deverá utilizá-los para determinar a página e a quantidade máxima de registros retornados.
- **Resultado esperado:** A API deverá retornar os registros correspondentes à página solicitada, respeitando o limite informado. Caso a última página possua menos registros que o limite, deverá retornar todos os registros disponíveis.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência e valores-limite.

### PAG-RN03: Listagem sem registros

- **Regra:** Quando uma consulta de listagem não retornar registros, a API deverá responder com status HTTP 200 e uma lista vazia.
- **Resultado esperado:** A API deverá retornar 200 OK com o corpo [], tanto quando a página solicitada não possuir registros quanto quando nenhum registro corresponder aos filtros informados.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN04: Filtros de pesquisa textual

- **Regra:** Os filtros textuais devem realizar buscas parciais, desconsiderando diferenças entre letras maiúsculas e minúsculas e entre letras acentuadas e não acentuadas. Os campos que aceitam filtros textuais são:
- Autores: name e nationality.
- Livros: title e genre.
- Bibliotecas: local.
- **Resultado esperado:** A API deve retornar os registros que contenham o valor informado no respectivo campo, independentemente de diferenças de capitalização ou acentuação.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN05: Filtros de pesquisa numérica

- **Regra:** O campo publication_year, na listagem de livros, deverá permitir pesquisas por correspondência exata.
- **Resultado esperado:** Ao informar um ano de publicação, a API deverá retornar somente os livros cujo campo publication_year corresponda exatamente ao valor pesquisado.
- **Endpoint:** GET /books.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN06: Ordenação dinâmica dos resultados

- **Regra:** A API deve permitir a ordenação dinâmica dos resultados por meio dos parâmetros sortBy e order. Os campos permitidos para ordenação são:
- Authors: name, nationality.
- Books: title, genre, publication_year.
- Libraries: local.
  O parâmetro sortBy define o campo utilizado na ordenação, enquanto order define a direção, aceitando ASC (crescente) ou DESC (decrescente).
- **Resultado esperado:** A API deve retornar os registros ordenados conforme os parâmetros informados.
- **Critério de desempate:** Quando dois ou mais registros apresentarem o mesmo valor no campo utilizado para ordenação, a API deverá utilizar id ASC como critério secundário.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN07: Validação dos parâmetros de ordenação

- **Regra:** A API deve validar os parâmetros de ordenação sortBy e order.
- sortBy: deve aceitar somente os campos permitidos para ordenação em cada recurso.
- order: deve aceitar somente ASC ou DESC, desconsiderando diferenças entre letras maiúsculas e minúsculas.
- **Resultado esperado:** A API deve rejeitar parâmetros de ordenação inválidos, retornando HTTP 400 Bad Request.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN08: Combinação de filtros

- **Regra:** A API deverá permitir a utilização de múltiplos filtros em uma mesma consulta, aplicando todos os critérios simultaneamente.
- **Resultado esperado:** A listagem deverá retornar somente os registros que atendam a todos os filtros informados.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN09: Tratamento de filtros vazios

- **Regra:** A API deve ignorar filtros cujos valores não tenham conteúdo significativo, incluindo parâmetros não informados, strings vazias ("") ou strings contendo apenas espaços. Quando houver múltiplos filtros, os filtros vazios deverão ser ignorados, enquanto os demais deverão ser aplicados normalmente.
- **Resultado esperado:** A API deve retornar os registros considerando apenas os filtros válidos e não vazios, sem rejeitar a requisição devido à presença de filtros vazios.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Baixa.
- **Técnica sugerida:** Partição de equivalência.

### PAG-RN10: Validação dos parâmetros de paginação

- **Regra:** Os parâmetros page e limit deverão aceitar somente números inteiros positivos.
- **Resultado esperado:** Caso o usuário informe valores iguais a zero, negativos, decimais ou não numéricos, a API deverá retornar HTTP 400 Bad Request.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Alta.
- **Técnica sugerida:** Partição de equivalência e valores-limite.

### PAG-RN11: Limite máximo de registros por página

- **Regra:** O parâmetro limit deverá aceitar no máximo 100 registros por página.
- **Resultado esperado:** Quando o usuário informar um valor superior a 100, a API deverá retornar HTTP 400 (Bad Request). Valores entre 1 e 100 deverão ser aceitos.
- **Endpoint:** GET /authors, GET /books e GET /libraries.
- **Origem:** Decisão.
- **Situação:** Confirmada.
- **Prioridade:** Média.
- **Técnica sugerida:** Valores-limite.

## Melhorias futuras

### MEL-RN01: Pesquisa de livros pela nacionalidade do autor

**Descrição:** Permitir a filtragem de livros pela nacionalidade de seus respectivos autores na listagem de livros.
**Comportamento proposto:** Disponibilizar o parâmetro nationality em GET /books, utilizando a associação entre livros e autores para filtrar os resultados.
**Situação atual:** Não implementado.
**Observação:** A funcionalidade não faz parte do escopo atual da API e não constitui uma regra de negócio confirmada.

## Pendências de implementação

### PEN-RN01: Ordenação dinâmica da listagem

**Descrição:** Implementar a ordenação dinâmica dos registros nas listagens de autores, livros e bibliotecas, conforme definido nas regras de paginação.
**Parâmetros:** sortBy, responsável por selecionar o campo de ordenação, e order, responsável por definir a direção da ordenação (ASC ou DESC).
**Módulos afetados:** Autores, Livros e Bibliotecas.
**Referências:** Regras de paginação RN06 e RN07.
**Situação atual:** Não implementado.
**Observação:** A ausência dessa funcionalidade representa uma lacuna entre o comportamento documentado e a implementação atual da API. Os casos de teste correspondentes deverão ser executados após sua implementação.
