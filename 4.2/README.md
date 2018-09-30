# Random notes
## db in another container
create role engine with login encrypted password 'password';
create database engine owner engine template template0 encoding 'UTF8' lc_collate 'en_US.UTF-8' lc_ctype 'en_US.UTF-8';

create role ovirt_engine_history with login encrypted password '<password>';
          create database ovirt_engine_history owner ovirt_engine_history
           template template0
           encoding 'UTF8' lc_collate 'en_US.UTF-8'
           lc_ctype 'en_US.UTF-8';

\connect engine
CREATE EXTENSION "uuid-ossp";

docker run -d --name ovirt-psql -e POSTGRESQL_USER=engine -e POSTGRESQL_PASSWORD=engine -e POSTGRESQL_DATABASE=engine -v ovirt-psql:/var/lib/pgsql/data -p 5432:5432 postgres:9.5.14 -c 'autovacuum_vacuum_scale_factor=0.01' -c 'autovacuum_analyze_scale_factor=0.075' -c 'autovacuum_max_workers=6' -c 'work_mem=8192' -c 'max_connections=150'

## engine
`docker run --rm --name ovirt-engine --link ovirt-psql:postgres -e OVIRT_PKI_ORGANIZATION=mydomain.net -e OVIRT_FQDN=helmsdeep.mydomain.net -e OVIRT_PASSWORD=engine -v ovirt-engine:/etc/pki/ovirt-engine -v ovirt-engine-backups:/var/lib/ovirt-engine/backups -v ovirt-engine-logs:/var/log/ovirt-engine -p 9443:8443 madchap/ovirt-container-engine:0.1`
