
# Install kind
*DICA DE ESPECIALISTA: A versão mais indicada do Kind para este desafio é a 0.19.0. Se você optar por instalar uma versão mais recente do Kind, tenha em mente que pode exigir atualizações no sistema operacional do host, o que pode consumir tempo. Portanto, é aconselhável usar a versão recomendada para evitar atrasos desnecessários.*
```
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.19.0/kind-linux-amd64 && chmod +x ./kind && sudo mv ./kind /usr/local/bin/kind && kind --version
```

# Install kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && kubectl version --client
```

# Create meu-primeiro-cluster.yaml file
```
cat <<EOT > /root/meu-primeiro-cluster.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: cluster-desafio-day1
nodes:
  - role: control-plane
  - role: worker
  - role: worker
  - role: worker
EOT
```
# Run kind cluster
```
kind create cluster --config meu-primeiro-cluster.yaml
```
# Create meu-primeiro-pod.yaml file
```
cat <<EOT > /root/meu-primeiro-pod.yaml
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
        memory: "450Mi"
        cpu: "500m"
      requests:
        memory: "440Mi"
        cpu: "300m"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
EOT
```
# Run Pod
```
kubectl apply -f meu-primeiro-pod.yaml 
```

# Debug commands
```
kubectl get nodes
kubectl describe node cluster-desafio-day1-control-plane
kubectl describe node cluster-desafio-day1-worker
kubectl describe node cluster-desafio-day1-worker2
kubectl describe node cluster-desafio-day1-worker3
kubectl get pods
kubectl delete pod nginx-giropops
kind delete cluster --name cluster-desafio-day1
```