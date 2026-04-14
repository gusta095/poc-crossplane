# StorageAccount Composition

Abstração para criação de Storage Accounts no Azure via Crossplane.

## Recursos criados

| Managed Resource | Descrição |
|---|---|
| `storage.azure.upbound.io/v1beta2/Account` | Storage Account Azure |

## Inputs

| Campo | Tipo | Obrigatório | Default | Descrição |
|---|---|---|---|---|
| `subscription` | string | não | via EnvironmentConfig | Alias da subscription Azure |
| `resourceGroupName` | string | sim | — | Resource Group onde a Storage Account será criada |
| `location` | string | não | via EnvironmentConfig | Região Azure |
| `accountTier` | string | não | `Standard` | Tier da Storage Account (`Standard` ou `Premium`) |
| `accountReplicationType` | string | não | `LRS` | Tipo de replicação (`LRS`, `GRS`, `ZRS`, `GZRS`, `RA-GRS`, `RA-GZRS`) |
| `accountKind` | string | não | `StorageV2` | Tipo de Storage Account |
| `accessTier` | string | não | `Hot` | Tier de acesso para blobs (`Hot` ou `Cool`) |
| `tags` | object | não | — | Tags aplicadas à Storage Account |

## Restrições do nome (Azure)

O `metadata.name` do recurso é usado como nome da Storage Account no Azure.
O Azure exige:
- **3 a 24 caracteres**
- **Apenas letras minúsculas e números** (sem hífens ou underscores)
- Globalmente único em todo o Azure

## Regiões disponíveis

- `eastus`
- `eastus2`
- `southcentralus`
- `brazilsouth`

## Exemplo de uso

```yaml
apiVersion: azure.gusta-lab.com/v1
kind: StorageAccount
metadata:
  name: meuprojeto001
spec:
  subscription: sub-gusta-labs
  resourceGroupName: rg-meu-projeto
  location: brazilsouth
  accountTier: Standard
  accountReplicationType: GRS
  tags:
    env: production
    team: platform
```

## Aplicar

```bash
kubectl apply -f examples/storageaccount.yaml
kubectl get storageaccount
kubectl describe storageaccount gustalabsandbox
```
