**RBAC & ABAC & ACL**
JOSE
- [https://docs.authlib.org/en/latest/jose/jwk.html](https://docs.authlib.org/en/latest/jose/jwk.html)
RSA key
- [https://medium.com/asecuritysite-when-bob-met-alice/so-whats-in-a-public-key-efe6357d3541](https://medium.com/asecuritysite-when-bob-met-alice/so-whats-in-a-public-key-efe6357d3541)
- [https://github.com/jpf/okta-jwks-to-pem](https://github.com/jpf/okta-jwks-to-pem)
- [https://8gwifi.org/jwkconvertfunctions.jsp](https://8gwifi.org/jwkconvertfunctions.jsp)
JWS
JWE
JWK (JSON Web Key) data structure that represents a cryptographic key. 
JWT  
- [https://www.openpolicyagent.org](https://www.openpolicyagent.org)
OPA - policy based control

**Cybersecurity**
- [https://standoff365.com/en-US](https://standoff365.com/en-US)

**OWASP**
Burp suite [https://portswigger.net/burp/community-download-thank-you](https://portswigger.net/burp/community-download-thank-you)

OWASP Top 10

Web Application Security Testing with OWASP ZAP

Web Application Security Testing with Burp Suite

Pentest(penetration testing) - тестирование на безопасность

HTTPS

Одностороннее хеширование паролей

Пароли никогда не должны храниться как обычный текст, так как в случае нарушения безопасности все учетные записи пользователей будут скомпрометированы.

В то же время следует строго избегать симметричного шифрования, так как любой злоумышленник, изобретательный и достаточно настойчивый, сможет их взломать.

Единственным предлагаемым вариантом является асимметричный алгоритм шифрования (или «односторонний») для хранения паролей.

Таким образом, ни злоумышленник, ни разработчик, ни системный администратор внутри компании не смогут прочитать пароли клиентов.

Сильная аутентификация

Теперь почти каждый API имеет форму аутентификации, но, на мой взгляд, система OAuth2 работает лучше всего.

В отличие от других методов проверки подлинности, он делит вашу учетную запись на ресурсы и разрешает только ограниченный доступ к каналу-маркеру аутентификации.

В то же время, еще одна очень хорошая практика — установить токены, которые истекают каждые, скажем, 24 часа, и их нужно будет обновить.

Применить ограничение скорости

Если у вас нет API, который используется миллионами людей каждую минуту, есть очень хорошая идея, чтобы обеспечить ограничение количества вызовов, которые клиент может сделать к  API в заданном временном окне.

Это в основном для предотвращения ботов, которые могут отсылать сотни одновременных запросов каждую секунду и заставлять ваш API потреблять системные ресурсы без каких-либо веских оснований.

Все рамки веб-разработки поставляются с промежуточным программным обеспечением, ограничивающим скорость (а если нет, их довольно легко добавить через библиотеку), который занимает всего минуту или около того, чтобы их полностью настроить.