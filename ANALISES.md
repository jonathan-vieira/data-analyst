Impulsionar nossas buscas

1) Qual é a análise:
   - Analise feita em cima dos destinos internacionais mais procurados no periodo

2) Motivação da análise
   - Entender quais destinos internacionais as pessoas que usam o maxmilhas procuram para poder assim impulsionar ainda mais as buscas
    com alguma campanha especifica seja ela de cupons de desconto ou materias/blog sobre esses destinos, pois se temos várias
    pessoas querendo ir pra esse destino, temos mais chance de aumentar as buscas e com essas ações.

3) Como executar
   - Primeiro executamos uma query para contar quantas pesquisas foram feitas agrupadas por aeroporto internacional de volta e então ordenar do
    maior para o menor

```sql
select
  count(distinct id),
  nome_aeroporto_volta
from buscas
where voo_internacional = 'SIM' and pais_volta <> 'BR'
group by 2
order by 1 desc;
```

| Qtde  |  Aeroporto Volta |
| --- | --- |
| 153458	| Buenos Aires - Todos |
| 92133	| Santiago, Chile |
| 88202	| Miami Intl |
| 66430	| Orlando |
| 58261	| Lisboa, Portugal |
| 44381	| Nova Iorque - Todos |
| 41794	| Rosário |
| 30996	| Montevideo |
| 25510	| Paris - Todos |
| 22403 |	Lima, Peru |


Impulsionar nossas buscas

1) Qual é a análise:
- Analise feita em cima dos dias da semana com maior numero de buscas

2) Motivação da análise
- Entender qual é o dia da semana em que as pessoas pesquisam por passagens aéreas, encontrar quais os maiores dias e criar campanhas,
como por exemplo e-mail marketing para serem disparadas nesses dias onde existe maior procura de passagens

3) Como executar
- Criada uma tabela com os dias da semana e seus respectivos numeros para podermos usar na query e trazer o nome do dia ao inves do numero do dia
- Executamos uma query para contar quantas pesquisas foram feitas agrupadas por dia da semana e então ordenar do maior para o menor

| Id | Dia |
| --- | --- |
| 0  |	Dom |
| 1	| Seg |
| 2	| Ter |
| 3	| Qua |
| 4	| Qui |
| 5	| Sex |
| 6	| Sab |


```sql
select
  count(distinct buscas.id) as Qtde,
  dias.dia as dia
from buscas
join dias on dias.id = extract(dow from  data_busca)
group by 2
order by 1;
```

| Qtde | Dia |
| --- | --- |
| 1961435	| Seg |
| 1959036	| Qua |
| 1901204	| Ter |
| 1900939	| Sex |
| 1876104	| Qui |
| 1423362	| Dom |
| 1411690	| Sab |


Construir a marca

1) Qual é a análise:
- Analise feita do preço de voos

2) Motivação da análise
-Entender se as passagens de ida eram mais baratas na Maxmilhas do que nas companhias aereas, com esse numeros podemos ainda mais reforçar
o quão mais economico é comprar pela maxmilhas e construir ainda mais a marca através de postagens/anuncios/campanhas sobre essa questão
de economia na compra de passagens

- 3) Como executar

```sql
select
  count(distinct id),
  mais_barato_na_mm_ida
from buscas
group by 2
order by 1 desc;
 ```

| Qtde | Valor |
| --- | --- |
| 12433386 | true|
| 384 |	false |



Suportar as buscas

1) Qual é a análise:
- Analisar qual o diferença de resposta da data_fim_buscador e data_recebimento_busca

2) Motivação da análise
- Entender se temos algum problema de desempenho na parte de busca e entrega de informações sobre os voos pesquisados, para 
então tomar alguma ação nesse quesito de melhora de perfomance/ajustes no serviço

3) Como executar
- Fizemos uma query para buscar algumas informações sobre as pesquisa, mas com principal foco data fim da busca até a data da entrega
dessas informações para o usuário.


```sql
select id,
  data_ida,
  data_volta,
  data_busca,
  data_inicio_buscador,
  data_fim_buscador,
  data_recebimento_busca,
  diff_segundos_entre_fim_da_busca_e_recebimento
from buscas
  where diff_segundos_entre_fim_da_busca_e_recebimento > 180 and data_recebimento_busca is not null and data_fim_buscador is not null
order by  diff_segundos_entre_fim_da_busca_e_recebimento desc;
```

| id | data_fim_buscador | data_recebimento_busca | diff |
| --- | --- | --- | --- |
20915592 | 2016-12-03 12:16:25.000000 |	2017-01-06 17:57:17.000000 |	2958052 |
20915573 | 2016-12-03 12:16:27.000000 |	2017-01-06 17:57:10.000000 |	2958043 |
23968287 | 2016-12-28 09:28:39.000000 |	2017-01-23 11:18:26.000000 |	2252987 |
23968296 | 2016-12-28 10:03:39.000000 |	2017-01-23 11:21:22.000000 |	2251063 |
23968925 | 2016-12-28 10:04:00.000000 |	2017-01-23 11:21:21.000000 |	2251041 |
23968944 | 2016-12-28 10:39:03.000000 |	2017-01-23 11:30:20.000000 |	2249477 |
23970894 | 2016-12-28 10:39:12.000000	| 2017-01-23 11:30:22.000000 |	2249470 | 
23970912 | 2017-01-05 04:20:41.000000	| 2017-01-24 13:52:57.000000 |	1675936 |
24198830 | 2017-01-05 04:20:41.000000 | 2017-01-24 13:52:57.000000 |	1675936 |
20135808 | 2017-01-05 04:20:41.000000	| 2017-01-24 13:52:57.000000 |	1675936 |
