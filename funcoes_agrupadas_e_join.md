## Utilizando select, update e insert.



select * from banco;
update banco set nome = 'Banco do Brasil S.A.';
select * from agencia;
update agencia set numero = 9999;
select * from tipo_transacao;

create table if not exists teste (
	id serial primary key,
	nome varchar(50) not null,
	created_at timestamp not null default current_timestamp
);

drop table if exists teste;

create table if not exists teste (
	cpf varchar(11) not null,
	nome varchar(50) not null,
	created_at timestamp not null default current_timestamp,
	primary key (cpf)
);

insert into teste (cpf, nome) values (12345678909, 'douglas ferreira');
-- se tentar inserir novamente irá apresentar erro de campo duplicado não sendo feita nova inserção --

insert into teste (cpf, nome) values (12345678909, 'douglas ferreira') on conflict (cpf) do nothing;
-- desta forma não apresenta erro -- 



## Funções agrupadas.

-- exibindo os dados das tabelas -- 
select numero, nome from banco;
select banco_numero, numero, nome from agencia;
select numero, nome, email from cliente;
select banco_numero, agencia_numero, cliente_numero from cliente_transacoes;

-- listando todas as colunas de uma tabela --
select * from information_schema.columns where table_name = 'banco';

-- listando os tipos dos dados da tabela
select column_name, data_type from information_schema.columns where table_name = 'banco';

-- Funções agregadas
-- avg = media dos valores
select * from cliente_transacoes;
select avg (valor) from cliente_transacoes;

-- count = contagem total
select count (numero) from cliente;

select count (numero), email from cliente where email ilike '@gmail.com' group by email;

select count(id), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id;
select count(id), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id having count(id) > 150;

-- max e min retornam o maior e o menor numero na tabela
select max (numero) from cliente;
select min (numero) from cliente;

select max (valor) from cliente_transacoes;
select min (valor) from cliente_transacoes;

select max (valor), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id;
select min (valor), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id;

-- sum = soma de todos os registros
select sum(valor), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id;
select sum(valor), tipo_transacao_id from cliente_transacoes group by tipo_transacao_id order by tipo_transacao_id asc;
--asc (crescente) desc (decrescente)



## Trabalhando com joins.

-- trabalhando com joins
select numero, nome from banco;
select banco_numero, numero, nome from agencia;
select numero, nome from cliente;
select banco_numero, agencia_numero, numero, digito, cliente_numero from conta_corrente;
select id, nome from tipo_transacao;
select banco_numero, agencia_numero, conta_corrente_numero, conta_corrente_numero_digito, cliente_numero, valor from cliente_transacoes;

select count(1) from banco; -- 151
select count(1) from agencia; -- 296

-- pesquisando com join
select banco.numero, banco.nome, agencia.numero, agencia.nome from banco join agencia on agencia.banco_numero = banco.numero;

select banco.numero from banco join agencia on agencia.banco_numero = banco.numero group by banco.numero;

select count(distinct banco.numero) from banco join agencia on agencia.banco_numero = banco.numero;

-- pesquisando com left join
select banco.numero, banco.nome, agencia.numero, agencia.nome from banco left join agencia on agencia.banco_numero = banco.numero;

-- pesquisando com right join
select agencia.numero, agencia.nome, banco.numero, banco.nome from agencia right join banco on banco.numero = agencia.numero;

-- pesquisando com full join
select banco.numero, banco.nome, agencia.numero, agencia.nome from banco full join agencia on agencia.banco_numero = banco.numero;

-- relacionando mais tabelas na pesquisa com join
select 	banco.nome,
		agencia.nome,
		conta_corrente.numero, conta_corrente.digito,
		cliente.nome
from banco
join agencia on agencia.banco_numero = banco.numero
join conta_corrente on conta_corrente.agencia_numero = banco.numero
	and conta_corrente.agencia_numero = agencia.numero
join cliente on cliente.numero = conta_corrente.cliente_numero;	