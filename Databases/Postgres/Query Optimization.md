[https://habr.com/ru/post/509406/](https://habr.com/ru/post/509406/)

Avoid table bloating

- Используй Newrelic, Sentry или другой подобный сервис чтобы найти зацепки. Мы поняли что проблема именно в запросах к базе данных именно так.
- Используй практику PMDSC – это работает и в любом случае хорошо вовлекает.
- Используй PgHero чтобы найти подозреваемых и изучать улики в статистике SQL-запросов.
- Используй [explain.depesz.com](https://explain.depesz.com) – там удобно читать explain analyze запросов.
- Пробуй рисовать множества данных, когда не понимаешь что именно делает запрос.

PMDSC

Play it!

Measure it!

Draw it!

Suppose it!

Check it!

Newrelic/Sentry для отслеживания общего времени отклика за сутки. 

Best Practises and Tuning Tips

Check missed or unused indexes

Use Where Clause instead of having

Use Select instead of Select *

Use Exist() instead of Count()

Avoid using multiple OR in the FILTER predicate

Use wildcards at the end of a phrase only

Select for Update

Ide