Create admin

docker run -v /var/lib/manictimeserver/Data:/app/Data --rm --entrypoint dotnet  manictime/manictimeserver ManicTimeServer.dll addadmin -u konovalov.rus@gmail.com -p KONOvalov2

Start Server

docker run -v /var/lib/manictimeserver/Data:/app/Data -p 8080:8080 manictime/manictimeserver