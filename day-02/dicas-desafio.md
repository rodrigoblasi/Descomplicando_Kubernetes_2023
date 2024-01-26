## **O desafio consiste em:**
- Instalar o KUBECTL
- Corrigir o manifesto kubernetes, pod.yaml.
- Subir o pod com NGINX

## Resolução:
#### Install kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && kubectl version --client
```

#### Abaixo segue o pod.yaml original entregue no desafio:
```yaml
apiVersion: v1beta1
kind: pods
metadata:
  labels:
    run: nginx-giropops
    app: giropops-strigus
  name: nginx_giropops
spec:
  containers:
  - image: nginx
    name: nginx_giropops
    ports:
    - containerPort: 80
    resources:
      limits:
        memory:
        cpu: "0.5"
    requests:
        memory: "4400MB"
        cpu: "0,3"
  dnsPolicy: ClusterSecond
  restartPolicy: Always
  ```

  #### pod.yaml corrigido
  ```yaml
  apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx-giropops
    app: giropops-strigus
  name: nginx-giropops
spec:
  containers:
  - image: nginx
    name: nginx-giropops
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "500Mi"
        cpu: "0.5"
      requests:
        memory: "440Mi"
        cpu: "0.3"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  ```

||   
Agora, se você estiver enfrentando problemas, basta rodar este comando direto no terminal e seu desafio estará completo e finalizado em 5 segundos.
```bash
{
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
  && sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
  && kubectl version --client \
  && cat <<EOL > pod.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx-giropops
    app: giropops-strigus
  name: nginx-giropops
spec:
  containers:
  - image: nginx
    name: nginx-giropops
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "500Mi"
        cpu: "0.5"
      requests:
        memory: "440Mi"
        cpu: "0.3"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
EOL
} && kubectl apply -f pod.yaml
```
||

