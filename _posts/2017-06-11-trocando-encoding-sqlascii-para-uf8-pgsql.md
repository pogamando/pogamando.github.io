---
layout: post
title:  "Trocando o encoding SQL_ASCII para UTF-8 no PostgreSQL"
date:   2017-06-11
categories: postgresql sql linux
author: marcos
---
Nas últimas semanas encontrei um problema ao instalar o PostgreSQL 9.6 no Ubuntu Server 16.04 LTS. Mesmo definindo o locale/charset do sistema operacional como "pt_BR.UTF-8", o Postgres continuava definindo o encoding do sistema como "SQL_ASCII". Ao tentar criar uma nova base de dados, o postgres me retornava a seguinte mensagem:

[ERROR: new enconding (UTF8) is incompatible with the encoding of the template database (SQL_ASCII) HINT: Use the same encoding as in the template database, or use template0 as template.
Mensagem de erro mostrada pelo PgAdmin3]

Encontrei várias abordagens que sugeriam excluir o "template0" e "template1" do sistema e recria-las com o encoding "UTF-8". Essas abordagens, além de não resolverem meu problema, agora esses templates apareciam no ObjectBrowser do PgAdmin3 como base de dados (algum desavisado poderia deleta-las por engano).

Analisando melhor o problema, percebi que a base "postgres" foi criada com o tal enconding "SQL_ASCII", conforme mostra a imagem a seguir:

[sql pane database]


A solução foi muito simples, bastou rodar o SQL abaixo para fazer o update do encoding de todas as bases do sistema: "postgres", "template0" e "template1".

{% highlight sql %}
UPDATE pg_database SET encoding = pg_char_to_encoding('UTF8')
{% endhighlight %}

Após reiniciar o serviço do postgres, ao executar o SQL abaixo, o sistema deve retornar "UTF8" ao invés de "SQL_ASCII":

{% highlight sql %}
SHOW SERVER_ENCODING
{% endhighlight %}

> <strong style="color: red;">Atenção:</strong>

> **Este procedimento deve ser executado logo após a instalação do PostgreSQL. Não é aconselhável a execução do mesmo em uma instancia do Postgres com outras bases criadas, visto que podem acontecer perda de dados ou mal funcionamento dos softwares que acessam essas bases.**

> **NÃO EXECUTE este procedimento em um ambiente de PRODUÇÃO a menos que saiba o que está fazendo ou as implicações que isto pode ocasionar.**

> **USE POR SUA CONTA E RISCO!**

> **FAÇA SEMPRE BACKUP!** 😁