# Aplicar filtros a consultas SQL

## Descrição do projeto
Como profissional de segurança cibernética em uma organização, fui encarregada de investigar possíveis roblemas de segurança envolvendo tentativas de login e máquinas de funcionários. Para isso, utilizei consultas SQL com os operadores `AND`, `OR`, e `NOT`, além da palavra-chave `LIKE`, para filtrar registroa nas tabelas `log_in_attempts` e `employees`. O objetivo foi identificar tentativas de login suspeitas (fora do horário comercial, em datas específicas e fora do Brasil) e localizar funcionários de departamentos específicos para a  aplicação de atualizações de segurança.

## Recuperar tentativas de login malsucedidas fora do horário de expediente

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00'
AND success = 0;
```

**Explicação**: Esta consulta retorna todas as colunas (`*`) da tabela `log_in_attempts` em duas condições são verdadeiras ao mesmo tempo. O operador `AND` exige que ambas as condições sejam satisfeitas: a coluna `login_time` deve ter u maior maior que `'18:00'` (ou seja, a tentativa ocorreu após o horário comercial) e a coluna `sucess` deve ser igual a `0`, indicando que a tentativa de login falhou. Como o `AND` é usado, apenas os registros que atendem às duas condições simultaneamente são retornados.

## Recuperar tentativas de login em datas específicas

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2026-09-28'
OR login_date = '2022-05-27';
```

**Explicação**: Esta consulta usa o operador `OR` para retornar os registros que atendem a pelo menos uma das duas condições: a data de login (`login_date`) ser igual a `'2026-09-28'` ou igual a `'2026-09-27'`. Diferente do `AND`, o `OR` retorna uma linha se qualquer uma das condições for verdadeira, permitindo recuperar as tentativas de login ocorridas no dia do evento e no dia anterior

## Recuperar tentativas de login fora do Brasil

```sql
SELECT *
FROM log_in_attempts
WHERE country != 'BR'
AND country NOT LIKE 'BRAZIL%';
```

**Explicação:** Como o país "Brazil" pode aparecer na tabela tanto como `'BR'` quanto como valores que começam com `'BRAZIL'`, a palavra-chave `LIKE` com o caractere de porcentagem (`%`) é usada para capturar qualquer variação que comece com essa palavra (por exemplo, `'BRAZIL'`, `'BRAZIL '`, etc.), O `%` funciona como um curinga que representa qualquer sequência de caracteres após *"BRASIL"*. O operador `NOT` é aplicado junto ao `LIKE` (`NOT LIKE`) e à comparação de igualdade (`!=`) para excluir os registros cujo país seja o Brasil, retornando apenas as tentativas de login originadas em outros países. O `AND` garante que ambas as exclusões sejam aplicadas ao mesmo tempo.

## Recuperar funcionários do departamento de Marketing

```sql
SELECT *
FROM employees
WHERE department = 'Marketing'
AND office LIKE 'East%';
```

**Explicação:** Esta consulta combina duas condições com `AND`: o `department` deve ser igual a `'Marketing'` e o `office` deve começar com `'East'`. Como os escritórios do edifício East têm valores como `'East-170'` e `'East-320'`, a palavra LIKE com % no final do padrão (`'East%'`) é usada para corresponder a qualquer valor que que comece com "East", independentemente do número que vem depois. Assim, a cunsulta retorna somente os funcionários de Marketing que trabalham eem algum escritório do edifício East.


## Recuperar funcionários dos departamentos de Finanças ou Vendas

```sql
SELECT *
FROM employees
WHERE department = 'Finance'
OR department = 'Sales';
```

**Explicação:** O operador `OR` é usado ara retornar funcionários cujo `department` seja igual a `'Finance'` ou igual a `'Sales'`. Como pelo menos uma das condições precisa ser verdadeira (não as duas ao mesmo tempo), a consulta traz todos os funcionários que pertencem a qualquer um dessed dois departamentos, permitindo que a equipe aplique a atualização de segurança nas máquinas corretas.


## Recuperar todos os funcionários que não pertencem à TI

```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```

**Exlicação:** O operador `NOT` inverte a condição que o segue. Aqui, a condição original seria `department = 'Information Technology'`; com o `NOT` na frente, a consulta retorna exatamente o oposto: todos os regristros em que o departamento **não** é igual a *"Information Technology"*. Como os funcionários de TI já receberam a atualização, essa consulta identifica todos os demais funcionários que ainda precisam recebê-la.

## Resumo

Nesse projeto, utilizei filtros SQL para investigar problemas relacionados a tentativas de login e para localizar funcionários de departamentos específicos. Usei o operador `AND` para combinar múltiplas condições que precisavam ser verdadeiras simultaneamente (por exemplo, horário e status do login), o `OR` para retornar registros que atendiam a pelo menos uma de várias condições (como datas específicas ou departamentos diferentes), e o `NOT` para excluir registros que não deveriam fazer parte do resultado (como o departamento de TI). Também usei a palavra-chave `LIKE` com o caractere `%` para buscar padrões de texto, como variações do nome do Brasil e escritórios do edifício East. Esses filtros combinados permitiram atender às necessidades de investigação de segurança e de aplicação de atualizações descritas no cenário.