---
layout: post
title:  "Convertendo segundos para formato de hora (H:i:s) em PHP"
date:   2018-01-18
categories: php
author: ulisses
---
Quem nunca recebeu uma informação num formato e precisou converter para outro formato mais "legível" pelo usuário?

Em um projeto recente trabalhei com upload de vídeos, e um dos dados salvos era a duração desse vídeo em segundos. Porém ao apresentar a duração do vídeo para o usuário, era necessário formatá-lo como hora, ex: 00:03:24.

O que você faria nessa situação? Muitos provavelmente conhecem a melhor forma. Porém, independente ser a melhor ou não, não é incomum vermos algo do tipo:

{% highlight php %}
public function convertSecondsToHourMinutesSeconds ($seconds) {
    $hours = str_pad(intval($seconds / 60 / 60), 2, '0', STR_PAD_LEFT);
    $minutes = str_pad(abs(intval(($seconds % 3600) / 60)), 2, '0', STR_PAD_LEFT);
    $seconds = str_pad(abs($seconds % 60), 2, '0', STR_PAD_LEFT);

    return $hours . ':' . $minutes . ':' . $seconds;
}
{% endhighlight %}

Bom, como eu disse, muitos provavelmente conhecem uma forma muito mais "agradável" de se fazer isso. O PHP tem muitos métodos extremamente úteis, e um deles é o `gmdate`.

O método `gmdate` recebe dois parâmetros. O primeiro é o formato que deve ser retornado, e o segundo é o valor em segundos que deve ser convertido, da seguinte forma:

{% highlight php %}
gmdate('H:i:s', 220);
//return 00:03:40
{% endhighlight %}