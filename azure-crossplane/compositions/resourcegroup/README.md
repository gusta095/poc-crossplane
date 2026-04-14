# ResourceGroup Composition

Abstração para criação de Resource Groups no Azure via Crossplane.

## Recursos criados

| Managed Resource | Descrição |
|---|---|
| `azure.upbound.io/v1beta1/ResourceGroup` | Resource Group Azure |

## Inputs

| Campo | Tipo | Obrigatório | Default | Descrição |
|---|---|---|---|---|
| `subscription` | string | sim | — | Alias da subscription Azure |
| `location` | string | não | `eastus` (via EnvironmentConfig) | Região Azure |
| `tags` | object | não | — | Tags aplicadas ao Resource Group |

## Regiões disponíveis

- `eastus`
- `eastus2`
- `brazilsouth`

## Exemplo de uso

```yaml
apiVersion: azure.gusta-lab.com/v1
kind: ResourceGroup
metadata:
  name: rg-meu-projeto
spec:
  subscription: gusta-labs-azure-sandbox
  location: brazilsouth
  tags:
    env: production
    team: platform
```

## Aplicar

```bash
kubectl apply -f examples/resourcegroup.yaml
kubectl get resourcegroup
kubectl describe resourcegroup rg-gusta-lab-sandbox
```
