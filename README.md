# windmill-selfhost


### REFERENCE 
- https://www.windmill.dev/docs/advanced/self_host

```
curl https://raw.githubusercontent.com/windmill-labs/windmill/main/docker-compose.yml -o docker-compose.yml
curl https://raw.githubusercontent.com/windmill-labs/windmill/main/Caddyfile -o Caddyfile
curl https://raw.githubusercontent.com/windmill-labs/windmill/main/.env -o .env

docker compose up -d
```



### Use an external database
```bash
curl https://raw.githubusercontent.com/windmill-labs/windmill/main/init-db-as-superuser.sql -o init-db-as-superuser.sql
psql <DATABASE_URL> -f init-db-as-superuser.sql
```

### SQL Example
```sql
CREATE ROLE windmill_user;

GRANT ALL
ON ALL TABLES IN SCHEMA windmill 
TO windmill_user;

GRANT ALL PRIVILEGES 
ON ALL SEQUENCES IN SCHEMA windmill 
TO windmill_user;

ALTER DEFAULT PRIVILEGES 
    IN SCHEMA windmill
    GRANT ALL ON TABLES TO windmill_user;

ALTER DEFAULT PRIVILEGES 
    IN SCHEMA windmill
    GRANT ALL ON SEQUENCES TO windmill_user;


CREATE ROLE windmill_admin WITH BYPASSRLS;
GRANT windmill_user TO windmill_admin;
```


- Make sure that the user used in the DATABASE_URL passed to Windmill has the role windmill_admin and windmill_user:
```
GRANT windmill_admin TO <user used in database_url>;
GRANT windmill_user TO <user used in database_url>;
```


