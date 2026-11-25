# Plano de testes: Library Management API

- **Sistema:** node-sequelize-library-api (tag `v1-para-teste`)
- **Autor:** Dyonatan
- **Versão do documento:** 1.0
- **Última atualização do documento:** 2026-11-26

## 1. Objetivo

Este plano tem como objetivo verificar se a Library Management API atende aos requisitos funcionais e às regras de negócio documentadas, por meio da execução de testes nos seus endpoints. Ao final, será elaborado um parecer técnico com os resultados obtidos, as falhas identificadas e a avaliação geral da qualidade e conformidade da API.

## 2. Sistema sob teste

- Stack: Node.js, Express, Sequelize ORM, Sequelize CLI, JavaScript (ES Modules) e PostgreSQL.
- Módulos e quantidade de endpoints: 
Autores: 5 endpoints.
Livros: 5 endpoints.
Bibliotecas: 5 endpoints.
Estoque (library-books): 4 endpoints.
Total: 19 endpoints.
- Versão testada (tag e commit): v1-para-teste. Hash do commit a confirmar.
- Documentos de referência: README da API e regras-de-negocio.md.

## 3. Escopo

### Dentro do escopo

- **Autores:** operações CRUD, validação dos dados, consultas e regras de negócio do módulo.

- **Livros:** operações CRUD, validação dos dados, associação com autores e regras de negócio do módulo.

- **Bibliotecas:** operações CRUD, validação dos dados e regras de negócio do módulo.

- **Estoque (library-books):** associação entre livros e bibliotecas, controle de exemplares e validação das quantidades.

- **Listagens:** paginação, filtros, combinação de filtros e validação dos parâmetros de consulta.

- **Respostas da API:** códigos HTTP, mensagens de erro e consistência dos dados retornados.


### Fora do escopo

- **Testes de desempenho e carga:** não fazem parte dos objetivos desta primeira rodada.

- **Testes especializados de segurança:** não serão realizados testes aprofundados de vulnerabilidade, autenticação ou autorização.

- **Interface gráfica:** a API será testada diretamente por meio de requisições HTTP, sem testes de interface.

- **Ordenação dinâmica:** será testada posteriormente, após sua implementação na API.


## 4. Tipos de teste

- **Funcional de API:** verifica se os endpoints executam corretamente as operações de cadastro, consulta, atualização e exclusão.
- **Validação de dados (SQL):** verifica se os dados são persistidos corretamente no PostgreSQL e se as restrições de integridade são respeitadas.
- **Negativo e de borda:** verifica o comportamento da API diante de dados inválidos, campos ausentes, valores extremos e parâmetros incorretos.
- **Regressão (reteste de correções):** verifica se correções de defeitos não comprometeram funcionalidades anteriormente aprovadas.


## 5. Técnicas aplicadas

- **Partição de equivalência:** dividir os dados de entrada em grupos válidos e inválidos. Exemplo: testar publication_year com um número inteiro positivo e com valores inválidos, como texto ou número negativo.
- **Valor limite:** testar valores próximos aos limites estabelecidos pelas regras. Exemplo: verificar limit com os valores 1, 100 e 101.
- **Tabela de decisão:** combinar diferentes condições para verificar seus resultados. Exemplo: testar a criação de um livro considerando a presença ou ausência dos campos obrigatórios (title, genre e publication_year).

## 6. Ambiente e massa de dados

### Ambiente

- Node: pendente de confirmação.
- PostgreSQL: pendente de confirmação.
- Postman: pendente de confirmação.
- Cliente SQL: pendente de confirmação.
- URL base: pendente de confirmação.

### Massa de dados

A massa de dados será composta por registros de autores, livros, bibliotecas e associações entre livros e bibliotecas. Os registros serão preparados para contemplar cenários válidos, inválidos e situações de limite.

O banco de dados deverá ser restaurado a um estado inicial conhecido entre as rodadas de teste, garantindo a consistência e a reprodutibilidade dos resultados.

//LEMBRAR DE CONSULTAR AS VERSÕES DAS FERRAMENTAS!!!////

## 7. Priorização

**1.Prioridade alta:** executar primeiro as regras classificadas como alta, pois representam comportamentos essenciais para o funcionamento da API e têm maior impacto em caso de falha.

**2. Prioridade média:** executar em seguida as regras classificadas como média, verificando comportamentos importantes para a consistência e o uso correto dos módulos.

**3.Prioridade baixa:** executar por último as regras classificadas como baixa, priorizando comportamentos de menor impacto sem deixar de verificar sua conformidade.

## 8. Critérios de entrada e saída

### Entrada (posso começar quando...)

- API instalada e configurada, com banco de dados PostgreSQL acessível.

- Ambiente de testes funcional e endpoints disponíveis.

- Regras de negócio e plano de testes documentados.

- Massa de dados inicial preparada.

### Saída (posso encerrar quando...)

- 100% dos casos de teste planejados foram executados.

- 100% dos casos de prioridade alta foram aprovados ou possuem defeitos registrados.

- Todos os defeitos identificados foram documentados e classificados por severidade.

- Os resultados da execução e o parecer técnico foram registrados.



## 9. Classificação de defeitos

- **Crítica:** falha que compromete gravemente a integridade dos dados ou impede o funcionamento geral da API. Exemplo: exclusão ou alteração indevida de registros em massa.

- **Alta:** falha em funcionalidade essencial que impede uma operação importante. Exemplo: impossibilidade de cadastrar livros ou atualizar o estoque.

- **Média:** falha que compromete parcialmente uma funcionalidade, mas permite contornar o problema. Exemplo: filtro de listagem que não retorna corretamente determinados registros.

- **Baixa:** falha de baixo impacto, sem comprometimento relevante das operações. Exemplo: mensagem de erro com texto pouco claro, mas com código HTTP e comportamento corretos.

## 10. Riscos

- **Ambiente indisponível:** falhas na configuração da API ou no acesso ao PostgreSQL podem impedir a execução dos testes. Mitigação: validar previamente a conexão com o banco e o funcionamento dos endpoints.

- **Massa de dados inconsistente:** registros antigos ou alterações de testes anteriores podem interferir nos resultados. Mitigação: restaurar o banco a um estado inicial conhecido antes de cada rodada.

- **Alterações no código durante os testes:** mudanças na implementação podem invalidar resultados anteriores. Mitigação: manter a versão testada identificada e registrar alterações realizadas.

- **Regras de negócio não implementadas:** funcionalidades pendentes podem gerar resultados diferentes dos esperados. Mitigação: identificar previamente as regras pendentes e não classificá-las como defeitos da versão atual.

- **Cobertura incompleta:** a quantidade de cenários pode não contemplar todas as combinações relevantes. Mitigação: utilizar as regras de negócio e as técnicas de teste documentadas para orientar a elaboração dos casos.

## 11. Entregas

- README.md: documentação principal da API.

- regras-de-negocio.md: documentação das regras de negócio utilizadas como referência para os testes.

- plano-de-testes.md: planejamento, escopo, técnicas, critérios e estratégia de execução dos testes.

- casos-de-teste.md: cenários de teste, dados de entrada e resultados esperados.

- evidencias/: registros das execuções dos testes, incluindo requisições, respostas e consultas ao banco de dados.

- relatorio-de-testes.md: resultados da execução, defeitos identificados e parecer técnico final.
