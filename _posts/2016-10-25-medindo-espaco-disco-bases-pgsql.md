---
layout: post
title:  "Medindo o espaço em disco consumido pelas bases do PostgreSQL"
date:   2016-10-25
categories: postgresql sql
author: marcos
---
Dica rápida para quem precisa medir o espaço em disco que cada base de dados está consumindo:

{% highlight sql %}
SELECT db.db_name, pg_size_pretty(db_size) AS db_size
FROM (
   SELECT datname AS db_name, pg_database_size(datname) AS db_size
   FROM pg_database
) AS db
-- WHERE db.db_name <> 'rdsadmin' /* Adicione esta linha se estiver usando o RDS do Amazon Web Services */
ORDER BY db_size DESC;
{% endhighlight %}
