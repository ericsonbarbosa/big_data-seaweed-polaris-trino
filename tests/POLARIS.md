


### Limpeza das credênciais bootstrap (necessário executar na VM seaweed)

```bash
# Limpeza do Ambiente
sudo rm -f /etc/polaris/bootstrap-credentials.env
```

## Renovação de TOKEN:
```bash
# 1. Execute o request para renovar e salvar na variável TOKEN (Python)
TOKEN=$(curl -s -X POST http://192.168.56.101:8182/api/catalog/v1/oauth/tokens \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -d "grant_type=client_credentials&client_id=root&client_secret=root_secret_changeme&scope=PRINCIPAL_ROLE:ALL" \
    | python3 -c "import sys,json; print(json.load(sys.stdin)['access_token'])")

# Confirme se funcionou
echo $TOKEN

# Endpoints do REST Catalog (específicos do warehouse iceberg)
curl -s "http://127.0.0.1:8182/api/catalog/v1/config?warehouse=iceberg" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Polaris-Realm: POLARIS" | python3 -m json.tool

# Referência definitiva para interagir com o Polaris via curl:
# http://192.168.56.101:8182/api/catalog/v1/iceberg/{recurso}
```