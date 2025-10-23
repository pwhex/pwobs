### Check activity:

SELECT
    pid,
    usename AS username,
    datname AS database,
    client_addr AS client_address,
    client_hostname AS client_hostname,
    client_port,
    application_name,
    backend_start,
    state
FROM
    pg_stat_activity
WHERE
    client_addr IS NOT NULL;

### psql in linux:
login: psql -h db.levylab.org -U llab_admin -d levylab
list databases: \l

SELECT pid, usename, application_name, client_addr, client_port, backend_start, state                                                                                                                  FROM pg_stat_activity; 