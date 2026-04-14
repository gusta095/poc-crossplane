# azure-crossplane

# 1. Namespace e Functions
  kubectl apply -f bootstrap/namespaces.yaml
  kubectl apply -f bootstrap/functions.yaml

  # 2. Providers
  kubectl apply -f providers/resourcegroup.yaml
  kubectl apply -f providers/storageaccount.yaml

  # 3. Aguardar os providers ficarem healthy
  kubectl get provider -w

  # 4. Credenciais e ProviderConfig
  kubectl apply -f provider-config/secret.yaml
  kubectl apply -f provider-config/providerconfig.yaml

  # 5. EnvironmentConfig
  kubectl apply -f environments/gusta-labs-azure-sandbox.yaml

  # 6. XRDs e Compositions
  kubectl apply -f compositions/resourcegroup/definition.yaml
  kubectl apply -f compositions/resourcegroup/composition.yaml

  kubectl apply -f compositions/storageaccount/definition.yaml
  kubectl apply -f compositions/storageaccount/composition.yaml

  # 7. Testar com os exemplos
  kubectl apply -f compositions/resourcegroup/examples/resourcegroup.yaml
  kubectl apply -f compositions/storageaccount/examples/storageaccount.yaml
