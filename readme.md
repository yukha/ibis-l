### Create app folder
on ibis-l:
```
mkdir /app
cd /app
```
### Get scripts from git
on ibis-l:
```
git clone https://github.com/yukha/ibis-l.git .
```
### Set TLS certificate
on local machine:
get certificate from windows CA, export it as file.pfx
```
openssl pkcs12 -in file.pfx -out pem -nokeys
openssl pkcs12 -in file.pfx -out file.withkey.pem
openssl rsa -in file.withkey.pem -out key
rm file.withkey.pem
```
copy the certificate to server:
```
scp ./pem username@ibis-l:/app/nginx/certs/pem
scp ./key username@ibis-l:/app/nginx/certs/key
```
### Create network
on ibis-l:
```
docker network create -d bridge backend-ibis-l
```
### Set youtrack right
on ibis-l:
```
mkdir -p -m 750 ./youtrack-ibas/backups ./youtrack-ibas/conf ./youtrack-ibas/data ./youtrack-ibas/logs

chown -R 13001:13001 ./youtrack-ibas/backups ./youtrack-ibas/conf ./youtrack-ibas/data ./youtrack-ibas/logs

```

### Run Services (nginx, sql, graylog, ...):
on ibis-l:
```
docker-compose up -d
```

### stop all
on ibis-l:
```
docker exec youtrack-ibas stop

docker container stop $(docker container ls -q)
```