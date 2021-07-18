create table if not exists banco(
	numero integer not null,
	nome varchar(50) not null,
	ativo boolean not null default true,
	data_criacao timestamp not null default current_timestamp,
	primary key (numero)
);

create table if not exists agencia(
	banco_numero integer not null,
	numero integer not null,
	nome varchar(80) not null,
	ativo boolean not null default true,
	data_criacao timestamp not null default current_timestamp,
	primary key (banco_numero, numero),
	foreign key (banco_numero) references banco
);

create table cliente (
	numero bigserial primary key,
	nome varchar(120) not null,
	email varchar(220) not null,
	ativo boolean not null default true,
	data_criacao timestamp not null default current_timestamp
);

create table conta_corrente (
	banco_numero integer not null,
	agencia_numero integer not null, 
	numero bigint not null,
	digito smallint not null,
	cliente_numero bigint,
	ativo boolean not null default true,
	data_criacao timestamp not null default current_timestamp,
	primary key (banco_numero, agencia_numero, numero, digito, cliente_numero),
	foreign key (banco_numero, agencia_numero) references agencia (banco_numero, numero),
	foreign key (cliente_numero) references cliente (numero)
);

create table tipo_transacao (
 	id smallserial primary key,
	nome varchar(50) not null,
	ativo boolean not null default true,
	data_criacao timestamp not null default current_timestamp
);

create table cliente_transacoes (
	id bigserial primary key,
	banco_numero integer not null,
	agencia_numero integer not null,
	conta_corrente_numero bigint not null,
	conta_corrente_digito smallint not null,
	cliente_numero bigint not null,
	tipo_transacao_id smallint not null,
	valor numeric(15,2) not null,
	data_criacao timestamp not null default current_timestamp,
	foreign key (banco_numero, agencia_numero, conta_corrente_numero, conta_corrente_digito, 
				 cliente_numero) references conta_corrente (banco_numero, agencia_numero, 
															numero, digito, cliente_numero)	
);

insert into banco (numero, nome) values ( 001 , ' Banco Santander');

insert into agencia (banco_numero, numero, nome) values ( 001 , 15 , ' Agência número 15 do Banco Santander' );

insert into cliente (nome, email) values ( 'Diego Ferreira' , ' diego@teste.com' );

insert into conta_corrente (banco_numero, agencia_numero, numero, digito, cliente_numero) 
			values ( 001 , 15 , 000454125 , 5 , 1 );

INSERT INTO tipo_transacao (nome) VALUES ( ' Débito ' );
INSERT INTO tipo_transacao (nome) VALUES ( ' Crédito ' );
INSERT INTO tipo_transacao (nome) VALUES ( ' Transferência ' );
INSERT INTO tipo_transacao (nome) VALUES ( ' Empréstimo ' );

insert into cliente_transacoes (banco_numero, agencia_numero, conta_corrente_numero, conta_corrente_digito,
			cliente_numero,tipo_transacao_id, Valor) values ( 001 , 15 , 000454125 , 5 , 1 , 2 , 150.20 );
			