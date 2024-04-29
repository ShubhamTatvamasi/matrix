# Database

Connect to postgresql:
```bash
kubectl -n matrix-synapse exec -it matrix-synapse-postgresql-0 -- \
  bash -c 'PGPASSWORD=synapse psql -U synapse -d synapse'
```

Create database for WhatsApp:
```bash
createdb --encoding=UTF8 --locale=C --template=template0 --owner=synapse whatsapp
```
