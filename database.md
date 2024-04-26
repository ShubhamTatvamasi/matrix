# Database

Connect to postgresql:
```bash
kubectl -n matrix-synapse exec -it matrix-synapse-postgresql-0 -- \
  bash -c 'PGPASSWORD=synapse psql -U synapse -d synapse'
```
