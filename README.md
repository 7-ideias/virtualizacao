# Virtualização e Exposição da Máquina

Este projeto contém os manifestos necessários para expor um serviço local (localhost:8080) para a internet usando Kind (Kubernetes in Docker).

## Estrutura do Projeto

- `k8s/deployment.yaml`: Manifesto para o deployment da aplicação (usando a imagem Docker Hub carlospc1978/demo:tagname)
- `k8s/service.yaml`: Manifesto para o serviço que expõe a aplicação
- `k8s/ingress.yaml`: Manifesto para o ingress que expõe o serviço para a internet
- `kind-config.yaml`: Configuração do cluster Kind com mapeamento de portas

## Como Usar 

### 1. Criar o cluster Kind
```bash

kind create cluster --config kind-config.yaml
```

### 2. Aplicar os manifestos
```bash

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

> **Nota**: O deployment utiliza a imagem `carlospc1978/demo:tagname` do Docker Hub.

### 3. Verificar se tudo está funcionando 
```bash

kubectl get pods
kubectl get services
kubectl get ingress
```

### 4. Acessar a aplicação

A aplicação estará disponível em:
- http://localhost:8080

## Expondo para a Internet

Para expor o serviço para a internet, via ngrok: 
```bash

ngrok http 8080
```
