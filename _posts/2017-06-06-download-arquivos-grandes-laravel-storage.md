---
layout: post
title:  "Download de arquivos grandes com Laravel Storage"
date:   2017-06-06
categories: php laravel aws
author: ulisses
---
Recentemente me deparei com um problema ao tentar baixar arquivos grandes com PHP e Laravel.

Os arquivos estavam alocados no Amazon S3 e eu estava usando o Storage padrão do Laravel. A solução que encontrei foi bem mais simples do que todas as ideias mirabolantes que estava tentando, e aqui eu compartilho essa solução:

{% highlight php %}
$resource = fopen('caminho/do/arquivo', 'r');
Storage::put('caminho/para/salvar/o/arquivo', $resource);
{% endhighlight %}
