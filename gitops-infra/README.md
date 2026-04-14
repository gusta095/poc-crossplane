# gitops-infra

Repositório GitOps centralizado para gerenciar infraestrutura e add-ons de múltiplos clusters Kubernetes. Utiliza ArgoCD com o padrão **App of Apps**.

## Estrutura do Repositório

```
gitops-infra/
├── k8s-local/          # Kubernetes local (Docker for Windows)
│   ├── project-infra.yaml    # ArgoCD AppProject
│   ├── root-app.yaml         # App raiz (App of Apps)
│   └── templates/            # Applications gerenciadas pelo root-app
│       └── *.yaml
│
├── eks/                # (futuro) Amazon EKS
├── aks/                # (futuro) Azure AKS
├── gke/                # (futuro) Google GKE
└── oks/                # (futuro) Oracle OKE
```

Cada pasta de cluster segue a mesma estrutura: `project-infra.yaml`, `root-app.yaml` e `templates/`.

## Padrão App of Apps

O `root-app.yaml` aponta para a pasta `templates/` do cluster. Qualquer arquivo `.yaml` adicionado em `templates/` é automaticamente detectado e implantado pelo ArgoCD (sync automático com `prune` e `selfHeal`).

## Adicionando uma nova aplicação

1. Crie um arquivo em `<cluster>/templates/<nome>.yaml` como ArgoCD `Application`
2. Referencie o project `infra`
3. Defina `automated.prune: true` e `automated.selfHeal: true`
4. Faça push — o ArgoCD sincroniza automaticamente

## Roadmap

### Concluído

- [x] EKS Control Plane provisionado
- [x] ArgoCD validado localmente
- [x] App of Apps funcionando
- [x] gitops-infra estruturado
- [x] helm-charts próprio funcionando
- [x] AppProject infra configurado
- [x] Waves planejados

### Em andamento — Terraform bootstrap ArgoCD no EKS

```
helm_release.argocd
kubectl_manifest.argocd_project_infra
kubectl_manifest.argocd_repo_secret
kubectl_manifest.root_app
```

Após o bootstrap, o ArgoCD assume `gitops-infra/eks/` e executa os waves:

| Wave | Add-on            |
|------|-------------------|
| 0    | metrics-server    |
| 1    | aws-lb-controller |
| 2    | external-dns      |
| 3    | karpenter + keda  |

## Tecnologias

- **ArgoCD** — GitOps controller
- **Helm** — empacotamento de aplicações
- Helm charts externos: `https://kubernetes-sigs.github.io/metrics-server`, `https://charts.bitnami.com/bitnami`
- Helm charts próprios: `https://github.com/gusta-lab/helm-charts`
