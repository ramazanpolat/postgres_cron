# postgres_cron
Dockerfile for building postgresql:11 with pg_cron extension

This image is derived from official [postgres:11](https://hub.docker.com/_/postgres) docker image.

Dockerfile installs [CitusData pg_cron extension installed](https://github.com/citusdata/pg_cron) for `postgres` database.

To change the database for `pg_cron` to be installed, edit `Dockerfile` line:

`RUN echo "cron.database_name = '[enter-your-database-name-here]'" >> /var/lib/postgresql/data/postgresql.conf`

`Dockerfile` and `docker-entrypoint.sh` files reside in [https://github.com/ramazanpolat/postgres_cron](https://github.com/ramazanpolat/postgres_cron)


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

# Testing pg_cron

```sh
$ docker exec --it [container-id] bash
$ su - postgres
$ psql
psql$ CREATE TABLE public.cron_test(a int);
psql$ INSERT INTO public.cron_test values (1), (2), (3) RETURNING *;
psql$ SELECT * FROM public.cron_test;
# Will return 1,2,3
psql$ select pg_sleep(60);
psql$ INSERT INTO cron.job (schedule, command, nodename, nodeport, database, username) VALUES ('* * * * *', $$DELETE FROM public.cron_test;$$, '', 5432, 'postgres', 'postgres') RETURNING jobid;
psql$ SELECT * FROM public.cron_test;
# Must return nothing since we deleted it with pg_cron
psql$ DELETE FROM cron.job;
psql$ DROP TABLE public.cron_test;
```
