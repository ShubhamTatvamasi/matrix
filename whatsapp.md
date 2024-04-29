# WhatsApp Bridge

Deploy Whatsapp bridge:
```bash
helm upgrade -i mautrix-whatsapp halkeye/mautrix-whatsapp \
  --namespace matrix-synapse \
  --values mautrix-whatsapp-values.yml
```

