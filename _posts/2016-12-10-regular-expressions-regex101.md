---
layout: post
title:  "Regular Expressions 101: Testando expressões regulares do JavaScript, PHP, Python e Go direto no browser"
date:   2016-12-10
categories: java javascript php regexp
author: marcos
---
Expressões Regulares (RegExp) são muito uteis para validar dados ou mesmo localizar uma sequência de caracteres dentro de um texto. No entanto, quem não possui muita afinidade com RegExp acaba sofrendo um pouco na hora de desenvolver algo.

Faz algum tempo que encontrei o [Regular Expressions 101](https://regex101.com/). Desde então, passei a testar/debugar todas as minhas expressões regulares nesta ferramenta. Recomendo 😀.

[regex101
Exemplo de uso do Regex101: Procurando um email dentro do texto]

A interface do Regex101 é muito simples de usar, mas vou destacar alguns elementos:

* **Flavor:** Aqui você indica a linguagem de programação, vulgo “sabor”, que você está usando. O Regex101 suporta PHP, JavaScript (também funciona com Java), Python e Golang;
* **Regular Expressions:** Aqui você escreve sua expressão regular, além de informar as flags de busca;
* **Test String:** Aqui você insere o texto onde sua expressão regular deve procurar ou validar dados;
* **Explanation:** descreve o que cada elemento da expressão faz. Muito útil para debugar a regexp escrita por outra pessoa;
* **Match Information:** apresenta detalhes das sequencias de caracteres que a regexp encontrou dentro do texto;
* **Quick Reference:** um “dicionário” de todos tokens que podem compor a expressão regular.
 

That’s all folks!