===CRON===

5 4 * * *

min hour month_day month week_day

crontab -l # list all jobs

1 * * * * /usr/bin/python3 /scripts/job_1.py  >> /var/log/cron.log 2>&1

1 * * * * /usr/bin/python3 /scripts/test.py  >> /var/log/cron.log 2>&1

1 * * * * echo "Log test"  >> /var/log/cron.log 2>&1

1 * * * * echo "Log test2"  > /var/log/cron.log

* * * * * echo "Log test2"  > /var/log/cron.log

pgrep crond

service cron status

service cron restart

service cron reload # reload after changes

cat /var/log/cron.log

Cron has own environment

[https://stackoverflow.com/questions/2229825/where-can-i-set-environment-variables-that-crontab-will-use](https://stackoverflow.com/questions/2229825/where-can-i-set-environment-variables-that-crontab-will-use)

crontab -e # edit crontab  
sudo run-paths /etc/cron.daily/  
sudo grep -a cron /var/log/syslog  
chmod a+x script.py # give execute permissions to script

===JOBBER===

[https://hub.docker.com/_/jobber](https://hub.docker.com/_/jobber)

[https://dshearer.github.io/jobber/doc/v1.4/#jobfile](https://dshearer.github.io/jobber/doc/v1.4/#jobfile)

[https://dshearer.github.io/jobber/doc/v1.4/](https://dshearer.github.io/jobber/doc/v1.4/)

docker exec -it vpakete-jobber sh

Time:  sec min hour month_day month week_day

export ACCESS_TOKEN=f4L3yUrZEAAFsllgKpnjcqM

jobber reload

jobber list

jobber test TEST