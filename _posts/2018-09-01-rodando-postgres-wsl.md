---
layout: post
title:  "Rodando o PostgreSQL no WSL (Windows Subsystem for Linux)"
date:   2018-09-01
categories: postgres linux windows wsl
author: marcos
background: /img/2018/09/postgres_rodando.png
---
Algum tempo atrás a Microsoft estava trabalhando com a Canonical para permitir rodar o Ubuntu dentro do Windows sem a necessidade de virtualização. O resultado do esforço das duas empresas foi o [WSL (Windows Subsystem for Linux)](https://www.linuxdescomplicado.com.br/2017/01/wsl-entenda-o-subsistema-que-permite-o-linux-ser-executado-no-windows.html). Com isso, você teria um Ubuntu e poderia instalar aplicações de terminal via apt-get.

Posteriormente o WSL passou a suportar mais distribuições Linux. No entanto, o WSL ainda não permitia que os softwares instalados nestas distribuições subissem um servidor socket. Ou seja, nada de usar o WSL para rodar servidores web, banco de dados, etc.

![microsoft_store_linux_distro](/img/2018/09/microsoft_store_linux_distro.png){:class="img-fluid"}

A Microsoft liberou com o Windows 10 Fall Creators Update uma melhoria que permite o WSL rodar softwares que subam um servidor socket. Agora é possível instalar servidores dentro do WSL.

Antes de começar, certifique-se que seu Windows 10 está rodando o Fall Creators Update e possui todas as atualizações instaladas. Para habilitar o WSL e instalar uma das distros disponíveis, sugiro você assistir esse vídeo do DioLinux: https://www.youtube.com/watch?v=HSLpH43hL-o

Se a janela do Bash não estiver aberta, digite "Linux" no menu iniciar. No meu caso, eu escolhi o Debian por ser a menor distro disponível na Microsoft Store.

![menu_iniciar_linux_app](/img/2018/09/menu_iniciar_linux_app.png){:class="img-fluid"}

Para instalar o postgres, vou usar o instalador interativo da EnterpriseDB, conforme eu descrevi no [Instalação rápida do PostgreSQL no Linux]({% post_url 2016-10-25-instalacao-rapida-pgsql-linux %}). Entretanto, é possível usar método tradicional com o apt-get.

![debian_install_postgres](/img/2018/09/debian_install_postgres.png){:class="img-fluid"}

Após a instalação, execute o comando `ps aux | grep postgres` para confirmar se o serviço está rodando corretamente. Se o resultado for semelhante ao da tela abaixo, tente acessar a base de dados com o PgAdmin (ou seu frontend de postgres favorito).

![postgres_rodando](/img/2018/09/postgres_rodando.png){:class="img-fluid"}

Ao reiniciar sua máquina, o postgres não iniciará automaticamente como aconteceria se estivesse instalado diretamente no Windows ou em um Linux tradicional. Para subir a instancia, abra novamente o Bash e digite `sudo service postgresql-10 start`.

![linux_iniciando_postgres](/img/2018/09/linux_iniciando_postgres.png){:class="img-fluid"}

Se você não souber com qual nome está instalado o serviço do postgres, basta olhar na pasta "/etc/init.d/"

![debian_servicos_inicializacao](/img/2018/09/debian_servicos_inicializacao.png){:class="img-fluid"}
