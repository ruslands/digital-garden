add_header: добавит header в ответ

proxy_set_header: добавит header запрос для следующего сервиса

Нельзя отправлять два header'a Access-Control-Allow-Origin на фронт

Ошибка: Одновременно сконфигурировать header Access-Control-Allow-Origin в nginx и в приложении

Rewrite rules

last: stop rewriting this request and search for a new matching location block

break: stop rewriting this request and continue execution within the current location block

proxy_x_forwarded_proto | http_x_forwarded_proto: http/https

proxy_x_forwarded_port | http_x_forwarded_port

proxy_add_x_forwarded_for:

server_port

http_upgrade

scheme

proxy_connection

scheme

proxy_x_forwarded_ssl

proxy_http_version 1.1;

proxy_buffering off;

proxy_set_header Host $http_host;

proxy_set_header Upgrade $http_upgrade;

proxy_set_header Connection $proxy_connection;

proxy_set_header X-Real-IP $remote_addr;

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

proxy_set_header X-Forwarded-Proto $proxy_x_forwarded_proto;

proxy_set_header X-Forwarded-Ssl $proxy_x_forwarded_ssl;

proxy_set_header X-Forwarded-Port $proxy_x_forwarded_port;

proxy_set_header Country $country;

client_max_body_size 60m;

client_body_buffer_size 128k;

proxy_request_buffering    off;

ignore_invalid_headers off;

Host: specifies the domain name that the client is trying to reach.

By default, Nginx will forward the original "Host" header to the backend server as-is, without making any modifications. This allows the backend server to use the "Host" header to determine which site or application to serve the request to.

However, Nginx also provides the ability to modify the "Host" header before forwarding it to the backend server. This can be done using the proxy_set_header directive in the location block of the Nginx configuration file.

For example:

proxy_set_header Host $host;

This line of code tells Nginx to forward the original "Host" header to the backend server, but replace it with the value of the $host variable. This can be useful in situations where the backend server is expecting a different hostname than the one that the client requested.

Alternatively, if you want to set the hostname to a specific value, you can use:

proxy_set_header Host example.com;

This line of code tells Nginx to forward the request to the backend server with the "Host" header set to "example.com", regardless of what the client requested.

So, it's up to you how you want to handle the host header and you can forward the original one or you can set it to a specific value.

Referer: ссылка с которой пришли(для статистики). Is an optional HTTP header field that identifies the address of the web page, from which the resource has been requested. By checking the referrer, the server providing the new web page can see where the request originated.

mime.types: Унифицированные названия для разных типов данных(тип/подтип)  
text/html  
text/css  
image/png  
application/json

NGINX файлы  
Конфиг /etc/nginx/nginx.conf  
Скрипт /etc/init.d/nginx  
PID-file /var/run/nginx.pid # содержит PID запущенного процесса  
Access-log /var/log/nginx/access.log  
Error-log /var/log/nginx/error.log

map and global variables

DR;TL Variables in NGINX are always global and once defined accessable from anywhere in the configuration. Therefore it would not make any sense to define a map in a server or location block.

map $geoip_country_code $country {default  $geoip_country_code;}

map creates new variable country and put the value to new variable

map $host $myvar {  
example.com "test";  
foo.com     "for";

}

As variables in NGINX are ALWAYS global and once defined available anywhere else in the configuration. So it wouldn't make any sense to move the map into a location or server block. The interesting fact with our map directive is when the variable myvar will receive its value or when it will be assigned?

map assigns the value to the variable once the variable will be used in your configuration

That means you can define the map in the http context but the value will be assigned at the point you are accessing $myvar in your nginx configuration.

Back to your question: As NGINX variables are always global having a map per server block would make sense as they would be global anyway.

----------

Nginx будет искать index_ru.html в папке /var/www/html при этом блок location не учитывается

location /ru {

# root /var/www/html/$http_Country; # убрал из-за проблем с проверкой в гугле

root /var/www/html;

index index_ru.html;

}

Nginx посмотрит блок location и отдаст file index_en.html без расширения html

location /en {

default_type "text/html";

alias /var/www/html/index_en.html;

}

API Management tools

[https://tyk.io](https://tyk.io)

[https://moesif.com](https://moesif.com)

[https://gravitee.io/](https://gravitee.io/)

[https://apiumbrella.io/](https://apiumbrella.io/)

[https://getkong.org/](https://getkong.org/)

health checks: passive, available in the open source version; and active, available only in NGINX Plus.

Content-Type: multipart/form-data; boundary=afsdgsgsg # отправялем запрос в теле которого данные разделены уникальным значением boundary

Expires: MON, 18 Oct 2010 # верни мне документ если он менялся после указанной даты

HTTPS - позволяет шифровавать и определять другую сторону общения, через SSL сертификат

Способы серверной работы с сокетами  
1. blocking IO - пока не выполнится текущий процесс, следующий не будет выполнятся  
2. fork - делается копия socket с новым pid  
3. prefork  
4. AIO - создается три переменных в которые пишется информация и по очереди они итерируются  
5. threads - все процессы делят между собой память  
AIO - select, kqueue, poll, epoll

Web-servers  
1. Apache - prefork, worker, C  
2. IIS, Tomcat, Jetty - threads, Java  
3. Starman, Hypnotoad - prefork  
4. nginx, lightppd - асинхронные, C  
5. haproxy - проксирование, С  
6. node.js, Tornado - асинхронные  
7. Erlang  
8. dev servers

Процессы web-server  
1. Master(root, 1 process)  
 - Чтение и валидация конфига  
 - Открытие сокетов  
 - Открытие файла логов  
 - Запуск и управление дочерними процессами  
 - Graceful restart  
2. Worker(requests, 1+ process)  
 - Обработка входящих запросов

nginx лучше, apache проще  
Nginx раздает статику все остальные запросы он проксирует дальше  
Проксирование - пришедший запрос, отправляем на другой сервис/сервер  
Nginx может отдавать только статику, если на nginx приходит запрос, который требует обработку на бэкенде, nginx проксирует его на бэкенд к запущенному серверу django.