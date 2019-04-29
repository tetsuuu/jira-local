# Local JIRA Service
It's Atlassian docker compose file, to run Atlassian products with docker on one single machine.

```
jira.ownlocal.com   wiki.ownlocal.com   bitbucket.ownlocal.com
       +                   +                    +
       |                   |                    |
       +----------------------------------------+
                           |
                           v
                         Nginx
                           +
       +-----------------------------------------+
       |                   |                     |
       v                   v                     v
   Atlassian Jira    Atlassian Confluence   Atlassian Bitbucket
 [host:jira:8080]   [host:confluence:8090]  [host:bitbucket:7990]
       +                   +                     |
       |                   |                     |
       +-----------------------------------------+
                           |
                           v
                        Postgres
                   [host:database:5432]
                           +
                           |
       +------------------------------------------+
       |                   |                      |
       v                   v                      v
   [db:jira]           [db:wiki]          [db:bitbucket]
```


Atlassian supported products:

- Jira `latest`
- Confluence `6.9.0`
- Bitbucket `5.10.1`

With:
- Postgres `9.4`
- Nginx `latest`

Requirements:

- Docker version 1.13.1+
- docker-compose version 1.10.0+

Docker image source files:

- [cptactionhank/atlassian-jira](https://hub.docker.com/r/cptactionhank/atlassian-jira/)
- [cptactionhank/atlassian-confluence](https://hub.docker.com/r/cptactionhank/atlassian-confluence/)
- [atlassian/bitbucket-server](https://hub.docker.com/r/atlassian/bitbucket-server/)
- [nginx](https://hub.docker.com/_/nginx/)
- [postgres](https://hub.docker.com/_/postgres/)

Create local mount volume
  ```
  ./genelate.sh
  ```
Run docker compose
  ```
  $ docker-compose -p JIRA up
  ```

Set `DNS` according to the above `DOMAIN` value, on somewhere that you want to connect to host of `docker-compose`

 ```
 $ vim /etc/hosts
     127.0.0.1 jira.ownlocal.com www.jira.ownlocal.com
     127.0.0.1 wiki.ownlocal.com www.wiki.ownlocal.com
     127.0.0.1 bitbucket.ownlocal.com www.bitbucket.ownlocal.com
 ```
Replace `127.0.0.1` with IP of host that `docker-compose` command run on it.

Create Databases

 ```
 $ docker exec -it atlassian_database_1  psql -U postgres
    postgres=# CREATE DATABASE jira;
    postgres=# CREATE DATABASE wiki;
    postgres=# CREATE DATABASE bitbucket;
    postgres=# \l
    postgres-# \q
 ```

Browse JIRA or others


 ```
 http://jira.ownlocal.com
 http://wiki.ownlocal.com
 http://bitbucket.ownlocal.com
 ```

Data volumes following
 ```
 /home/docker/data/jira
 /home/docker/data/confluence 
 /home/docker/data/bitbucket
 /home/docker/data/postgres
 ```
 