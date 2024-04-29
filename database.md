# Database

Connect to postgresql:
```bash
kubectl -n matrix-synapse exec -it matrix-synapse-postgresql-0 -- \
  bash -c 'PGPASSWORD=synapse psql -U synapse -d synapse'
```

Create database for WhatsApp:
```bash
kubectl -n matrix-synapse exec -it matrix-synapse-postgresql-0 -- \
  bash -c 'PGPASSWORD=synapse createdb -U synapse -E UTF8 -l C -T template0 -O synapse whatsapp'
```
