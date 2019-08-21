# postgres_cron
Dockerfile for building postgresql:11 with pg_cron extension

This image is derived from official [postgres:11](https://hub.docker.com/_/postgres) docker image.

It comes with CitusData pg_cron extension installed for `postgres` database.

To change database, edit `Dockerfile` line:

`RUN echo "cron.database_name = '[enter-your-database-name-here]'" >> /var/lib/postgresql/data/postgresql.conf`



# Building image

```sh
$ git clone https://github.com/ramazanpolat/postgres_cron.git
$ cd postgres_cron
$ docker build -t postgres_cron:11 .
```

# Running image

```sh
$ docker run -d ramazanpolat/postgres_cron:11
```


