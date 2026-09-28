# Aplicação de filtros a consultas SQL

## Sobre o projeto

Projeto de portfólio em Cibersegurança — investigação de tentativas de login suspeitas e identificação de funcionários por departamento usando SQL.

Como parte de um cenário de segurança em uma organização fictícia, este projeto usa consultas SQL para investigar possíveis problemas de segurança envolvendo tentativas de login e para localizar máquinas de funcionários que precisam de atualizações de segurança. As consultas foram construídas sobre duas tabelas de um banco de dados organization: log_in_attempts e employees.

## Objetivo

- Investigar tentativas de login fora do horário comercial e que falharam;
- Investigar um evento suspeito ocorrido em uma data específica;
- Investigar tentativas de login originadas fora do Brasil;
- Localizar funcionários de departamentos específicos (Marketing, Finanças, Vendas) para aplicar atualizações de segurança em suas máquinas;
- ocalizar todos os funcionários fora do departamento de TI, que já havia recebido a atualização.

## Estrutura do repositório e ordem de leitura sugerida

```
├── README.md
├── docs
│   ├── example-apply-filters-to-sql-queries.md
    └── table-formats.pdf
└── result
    └── apply-filters-to-sql-queries.md
```

Nesse formato:

| Ordem | Arquivo | Propósito |
|---|---|---|
| 1 | [`example-apply-filters-to-sql-queries`](docs/example-apply-filters-to-sql-queries.md) | Exemplo de como deve ser o relatório final |
| 2 | [`table-formats.pdf`](docs/table-formats.pdf) | Este documento descreve a organização das tabelas utilizadas neste projeto |
| 3 | [`apply-filters-to-sql-queries.md`](result/apply-filters-to-sql-queries.md) | Relatório final, contendo a descrição da atividade, as consultas SQL, suas explicações detalhadas e o resumo final |

## Estrutura do banco de dados

### `log_in_attempts`

| Coluna | Descrição|
|---|---|
| `event_id` | ID do evento de login | 
| `username` | Nome de usuário do funcionário |
| `login_date` | Data da tentativa de login |
| `login_time` | Horário da tentativa de login |
| `country` | País de origem da tentativa |
| `ip_address` | Endereço IP da máquina |
| succe`ss | Resultado do login (0/FALSE = falha) |

### `employees`

| Coluna | Descrição|
|---|---|
| `employee_id` | ID do funcionário | 
| `device_id` | ID do dispositivo utilizado | 
| `username` | Nome de usuário do funcionário |
| `department` | Departamento do funcionário |
| `office` | Escritório/sala do funcionário |

### Consultas e conceitos demonstrados
| Consulta | Operadores/Palavras-chave |
|---|---|
| Tentativas de login com falha fora do horário comercial | AND | 
| Tentativas de login em datas específicas | OR |
| Tentativas de login fora do Brasil | NOT, LIKE com % | 
| Funcionários do Marketing no edifício East | AND, LIKE com % |
| Funcionários de Finanças ou Vendas | OR |
| Funcionários fora do departamento de TI | NOT |

As consultas completas, com o texto explicando o funcionamento de cada uma, estão em [`apply-filters-to-sql-queries.md`](result/apply-filters-to-sql-queries.md).

## Metodologia

Cada consulta foi desenvolvida seguindo o mesmo fluxo de trabalho:

- **Entender o cenário**: identificar qual problema de segurança está sendo investigado e qual tabela contém os dados relevantes (`log_in_attempts` ou `employees`).
- **Mapear as colunas**: consultar o formato das tabelas para localizar as colunas que serão usadas como filtro (por exemplo, `login_time`, `success`, `country`, `department`, `office`).
- **Traduzir o requisito em condições**: transformar a descrição do cenário em condições lógicas individuais (por exemplo, "após as 18:00" e "login com falha").
- **Escolher o operador adequado**: decidir como combinar as condições: AN`D quando todas devem ser verdadeiras, `OR` quando basta uma delas, `NOT` para excluir registros, e usar `LIKE` com `%` quando o valor segue um padrão em vez de uma correspondência exata.
- **Escrever a consulta**: montar o `SELECT ... FROM ... WHERE ...` com as condições definidas.
- **Revisar a lógica**: conferir se o operador escolhido produz o resultado esperado (por exemplo, evitar `AND` onde o correto é `OR`) e se os padrões `LIKE` cobrem todas as variações de valor.
- **Documentar**: registrar a consulta digitada e explicar seu funcionamento, para que o raciocínio possa ser compreendido por quem lê o portfólio.

## Habilidades demonstradas

- Escrita de consultas SQL com `SELECT` e `WHERE`
- Uso dos operadores lógicos `AND`, `OR` e `NOT` para combinar e excluir condições
- Uso da palavra-chave `LIKE` com caracteres de porcentagem (`%`) para correspondência de padrões de texto
- Filtragem de dados por data, horário e valores de texto
- Documentação técnica de consultas para fins de portfólio

## Aviso

Este projeto é baseado nos operadores lógicos (`AND`, `OR`, `NOT`) em SQL do *Google Cybersecurity Certificate* e tem finalidade exclusivamente de portfólio/estudo.

## Autoria

querycat — em transição de carreira de desenvolvimento de software para cibersegurança, com foco em unir experiência em pagamentos e antifraude ao conhecimento de segurança.

[GitHub](https://github.com/querycat) · [LinkedIn](https://www.linkedin.com/in/maysa-pereira/)

