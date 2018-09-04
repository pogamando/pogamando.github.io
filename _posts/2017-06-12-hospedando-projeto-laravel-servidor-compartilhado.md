---
layout: post
title:  "Hospedando um projeto Laravel em um servidor de hospedagem compartilhado"
date:   2017-06-12
categories: php laravel ssh gambiarra
author: ulisses
---
Nem sempre podemos arcar com os custos de um servidor dedicado, então na maioria das vezes optamos por uma hospedagem compartilhada. Porém, nessas hospedagens o que recebemos em mãos nem sempre atende nossas necessidades, por exemplo a configuração de diretório ROOT do domínio, onde podem variar entre "/public_html" e "/html".

Quem conhece o Laravel sabe que o diretório ROOT do domínio tem que estar apontando para a pasta "public" do projeto, então como hospedar um projeto Laravel num servidor compartilhado?

Já fiz muitas gambiarras pra isso, mas existe uma maneira simples e limpa:

Suba seu projeto para um diretório no mesmo nível da pasta "public_html" ou "html", dependendo de como estiver o ROOT do seu domínio. Supomos que você coloque o projeto numa pasta chamada "project", então teríamos a seguinte estrutura:

```
/
public_html/
project/
    app/
    public/
    routes/
    …
```

Feito isso, acesse seu servidor por SSH, entre na pasta "public_html" e rode o comando `shell ln -s ../project/public/*`. Este comando cria um link simbólico na pasta "public_html" apontando para a pasta "public" do seu projeto Laravel, e dessa forma evitamos arquivos ".htaccess", configurações mirabolantes ou até mesmo jogar os arquivos e pastas do seu projeto de forma desorganizada no servidor.
