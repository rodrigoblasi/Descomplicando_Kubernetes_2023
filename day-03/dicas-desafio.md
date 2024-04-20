## **O desafio consiste em:**
- Corrigir o arquivo deployment.yaml para atender ao enunciado.
```
Missões 🎯
- Utilize o arquivo /root/deployment.yaml para realizar o deploy do nosso Deployment;
- Corrija todos os erros;
- Configure o limite de utilização de recursos da seguinte maneira:
    Requests:
      64Mi de Memória;
      0.1 de CPU;
    Limits:
      128 Mi de Memória;
      0.3 de CPU;
- Use a estratégia de atualização do Deployment que atualiza todos os pods de uma vez;
- A versão do Nginx deve ser a 1.16.0.
```

## Resolução:
#### Primeiramente instale a ferramenta kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && kubectl version --client
```

#### Abaixo segue o deployment.yaml original entregue no desafio:
```yaml
apiVersion: app/v1
kind: Deployment
metadata:
  label:
    app: nginx-girus
    opa: sensacional-juvenal
name: nginx-girus
specs:
  replicas: 5
    selector:
      matchLabels:
      app: nginx-girus
  strategies:
    type: recreate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  strategies: {}
  replicas: 2
  template:
    metadata:
    label:
      app: nginx
    specs:
      containers:
      - image: nginx 1.15.0
        name: nginx
        resource: {}
```

#### deployment.yaml corrigido para o desafio
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-girus
    opa: sensacional-juvenal
  name: nginx-girus
spec:
  replicas: 2
  selector:
    matchLabels:  # Corrigido'matchLabels' para corresponder aos rótulos do template
      app: nginx  # Corrigido para 'app: nginx' para corresponder aos rótulos do template
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: nginx  # Mantive 'app: nginx' para corresponder ao seletor
    spec:
      containers:
      - image: nginx:1.16.0
        name: nginx
        resources:
          requests:
            cpu: "0.1"
            memory: "64Mi"
          limits:
            cpu: "0.3"
            memory: "128Mi"
```