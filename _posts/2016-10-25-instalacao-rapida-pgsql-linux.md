---
layout: post
title:  "Instalação rápida do PostgreSQL no Linux"
date:   2016-10-25
categories: postgresql sql linux
author: marcos
background: /img/2016/10/wizard-linux-pg2.jpg
---
Ao instalar o PostgreSQL no Linux geralmente usamos os tradicionais gerenciados de pacotes apt-get, yum/dnf ou zyper. Em poucas situações necessitamos compilar e configurar o PostgreSQL manualmente.

No entanto, poucos sabem que a EnterpriseDB fornece um instalador cross-platform do PostgreSQL. Ele está disponível para Windows, Linux e macOS. No Linux, esse instalador funciona em distribuições baseadas no Debian/RedHat/SuSE.

**Observação**: Eu testei o instalador no Slackware Linux 14.2 64Bits, porém, no final do processo ocorre um erro e a instalação é abortada. Se alguém conseguir fazer uma instalação funcional, sinta-se livre para compartilhar conosco :).

## Baixando o instalador
------

Acesse o link <https://www.postgresql.org/download/linux/> e clique em "Download the Installer" no item "Interactive installer by EnterpriseDB".

![download-pg-linux1](/img/2016/10/download-pg-linux1.jpg){:class="img-fluid"}

Esse link aponta para a página de downloads da EnterpriseDB, onde estão disponíveis as últimas versões do Postgres. Selecione a versão desejada e faça o download clicando em "Linux x86-32" ou "Linux x86-64" de acordo com a arquitetura do seu sistema operacional.

![download-pg-linux2](/img/2016/10/download-pg-linux2.jpg){:class="img-fluid"}

## Instalação
------

Com o instalador em mãos, execute o comando abaixo para fornecer permissão de "execução" ao instalador:

{% highlight shell %}
sudo chmod +x postgresql-<versao-so-arquitetura>.run
{% endhighlight %}

Agora, você pode executar o instalador em ambiente gráfico ou modo texto, conforme os comandos abaixo:

{% highlight shell %}
# Modo grafico
sudo ./postgresql-<versao-so-arquitetura>.run

# Modo texto (útil para instalação via SSH)
sudo ./postgresql-<versao-so-arquitetura>.run --mode text
{% endhighlight %}

A partir deste ponto, o procedimento é o mesmo do instalador para Windows.

![wizard-linux-pg1.jpg](/img/2016/10/wizard-linux-pg1.jpg 'Instalador em ambiente gráfico'){:class="img-fluid"}

![wizard-linux-pg2](/img/2016/10/wizard-linux-pg2.jpg 'Instalador em modo texto'){:class="img-fluid"}

## Desinstalação
------
A tarefa de desinstalar é tão simples quanto a instalação. Dentro do diretório no qual o PgSQL foi instalado (default é /opt/PostgreSQL/<versão>) há um outro executável chamado "uninstall-postgresql". O desinstalador também fornece a opção de modo gráfico ou texto.

Para usá-lo, forneça permissão de "execução" ao arquivo:

{% highlight shell %}
sudo chmod +x /opt/PostgreSQL/9.5/uninstall-postgresql
{% endhighlight %}

Agora você pode executá-lo com o comando abaixo:

{% highlight shell %}
# Modo gráfico
sudo /opt/PostgreSQL/9.5/uninstall-postgresql

# Modo texto
sudo /opt/PostgreSQL/9.5/uninstall-postgresql --mode text
{% endhighlight %}

> #### Atualização em 11 de junho de 2017
> Se você encontrar problemas com encoding após a instalação do PostgreSQL, visite o nosso post sobre trocar o enconding do sistema: [Trocando o encoding SQL_ASCII para UTF-8 no PostgreSQL]({% post_url 2017-06-11-trocando-encoding-sqlascii-para-uf8-pgsql %})

> Para verificar o encoding do PostgreSQL, use a query:

> {% highlight sql %}SHOW SERVER_ENCODING{% endhighlight %}
