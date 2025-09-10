# Virtualização e Exposição da Máquina

Este projeto contém os manifestos necessários para expor um serviço local (localhost:8080) para a internet usando Kind (Kubernetes in Docker).

## Estrutura do Projeto

- `k8s/deployment.yaml`: Manifesto para o deployment da aplicação
- `k8s/service.yaml`: Manifesto para o serviço que expõe a aplicação
- `k8s/ingress.yaml`: Manifesto para o ingress que expõe o serviço para a internet
- `kind-config.yaml`: Configuração do cluster Kind com mapeamento de portas

## Como Usar

> **Nota**: Os comandos abaixo são fornecidos para ambientes Bash (Linux/Mac) e PowerShell (Windows). Para Windows, certifique-se de usar a versão PowerShell dos comandos quando disponível.

### 1. Criar o cluster Kind

#### Bash (Linux/Mac) ou PowerShell (Windows)
```bash
kind create cluster --config kind-config.yaml
```

### 2. Instalar o Ingress NGINX Controller

#### Bash (Linux/Mac) ou PowerShell (Windows)
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

### 3. Aguardar o Ingress Controller estar pronto

#### Bash (Linux/Mac)
```bash
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```

#### PowerShell (Windows)
```powershell
kubectl wait --namespace ingress-nginx `
  --for=condition=ready pod `
  --selector=app.kubernetes.io/component=controller `
  --timeout=90s
```

Ou em uma única linha:
```powershell
kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=90s
```

### 4. Aplicar os manifestos

#### Bash (Linux/Mac) ou PowerShell (Windows)
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### 5. Verificar se tudo está funcionando

#### Bash (Linux/Mac) ou PowerShell (Windows)
```bash
kubectl get pods
kubectl get services
kubectl get ingress
```

### 6. Acessar a aplicação

A aplicação estará disponível em:
- http://localhost:8080

## Expondo para a Internet

Para expor o serviço para a internet, você pode usar:

1. **Port Forwarding**: Se você estiver em uma rede com IP público, configure o port forwarding no seu roteador para encaminhar as requisições da porta 80 para a porta 8080 do seu computador.

2. **Serviços de Tunnel**: Use serviços como ngrok, Cloudflare Tunnel ou Inlets para criar um túnel seguro:

   #### Bash (Linux/Mac) ou PowerShell (Windows)
   ```bash
   # Exemplo com ngrok
   ngrok http 8080
   ```

3. **VPN com IP Público**: Configure uma VPN com IP público e encaminhe o tráfego para o seu cluster Kind.

## Observações

- Este exemplo usa o Nginx como aplicação de exemplo
- O serviço está configurado para usar a porta 8080
- O ingress está configurado para rotear todo o tráfego para o serviço
