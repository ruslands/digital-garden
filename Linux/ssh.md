ssh-keygen -t ed25519 -C "your_email@example.com"

ssh-keygen -t rsa -b 4096

1.cd ~/.ssh  
2.chmod 700 ~/.ssh  
3.s # делаем локально  
4.ssh-copy-id name@server_ip # зарузит ключ на сервер в authorized_keys  
5.touch ~/.ssh/config  
Host torch  
 HostName 202.120.58.  
 Port  
 User clsilya  
 IdentityFile ~/.ssh/id_rsa  
6.remove public key from local machine

pip3 install smth --user # установит только для меня не поломает чужие зависимости  
ssh r.konovalov@10.0.4.  
scp -r r.konovalov@10.0.4.222:/media_old/r.konovalov/instagram/yyourgurll /home/rus/Downloads  
sshfs r.konovalov@10.0.4.221:/media/r.konovalov ~/server  
fusermount -u ~/server

sudo su # зайти из под root  
su konovalov.rus # сделать супреюзером

ssh cpspark01 # переключаться между машинами, из под root

sudo scp -r 01_Task_data_searching/ cpspark02:/home/jupyter/notebooks/ # перекинуть данные между машинами