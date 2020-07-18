### Install postgres db
kubectl create ns demo
kubectl apply -f postgres/db

PGPASSWORD=postgres psql -h example-postgresql.demo -Upostgres -d postgres

CREATE DATABASE exampledb;
\c exampledb
CREATE TABLE sample (col1 character(1024), col2 character(1024), col3 character(1024), col4 character(1024), col5 character(1024));

insert into sample
select
    md5(random()::text),
    md5(random()::text),
    md5(random()::text),
    md5(random()::text),
    md5(random()::text)
from generate_series(1, 10000);

### Install postgres exporter
kubectl apply -f postgres/exporter

### Install servicemonitor
kubectl apply -f postgres/servicemonitor

### Add postgres alerts
kubectl apply -f postgres/alerts

### Reference for Alerts:
https://awesome-prometheus-alerts.grep.to/rules