# Kubernetes

## instalando Kubernetes no Linux

* `sudo apt-get install curl -y`
* `curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"`
* `chmod +x ./kubectl`
* `sudo mv ./kubectl /usr/local/bin/kubectl`
* `curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.12.1/minikube-linux-amd64 \ && chmod +x minikube sudo install minikube /usr/local/bin/`

* No linux é necessário instalar o kubectl e o minikube para inicializar um cluster localmente

* [Lista de integração do Kubernetes com plataformas em nuvem](https://v1-18.docs.kubernetes.io/docs/concepts/cluster-administration/cloud-providers/)



# Pods

* Pods são conjuntos de um ou mais containers
* Possui um IP para vário containers
* Como o Kubernetes armazena as imagens baixadas dentro do cluster?
* A resposta é simples: quando definimos que um Pod será executado, o scheduler definirá em qual Node isso acontecerá. O resultado então é que as imagens quando baixadas de repositórios como o Docker Hub, serão armazenadas localmente em cada Node, não sendo compartilhada por padrão entre todos os membros do cluster.
* Um Pod é encerrado quando todos os containers dentro dele param de trabalhar



# Kubectl

* Comandos Kubectl
    * Kubectl run
    * Kubectl apply
    * Kubectl edit
    * Kubectl describe

# arquivos YAML

* arquivos yaml servem para definir as configurações das tecnologias Kubernetes
* Usa o método declarativo ao invés do imperativo

* 
    ```
    apiVersion: v1
    kind: Pod
    spec:
    containers:
        - name: container-pod-1
        image: nginx:latest
    ```

# Services

* Os services são de outra categoria de tecnologia dentro do Kubernetes além dos Pods
* Utilizados para permitir o acesso externo ao cluster
* Possuem um DNS estável
* Faz o balanceamento de carga
* Criados de maneira declarativa
* Possuem IP's fixos para comunicação
* NodePort e ClusterIP são exemplos de Services
* Também executados com Kubectl
* Um Service se conecta com um Pod através das Labels definidas no metadata no arquivo yaml no Pod
* Labels são responsáveis por definir a relação Service x Pod
* Um ClusterIP funciona apenas dentro do cluster
* Um NodePort expõe Pods para dentro e fora do cluster
* Um LoadBalancer também é um NodePort e ClusterIP
* Um LoadBalancer é capaz de automaticamente utilizar um balanceador de carga de um cloud provider

``` 
Pod

apiVersion: v1
kind: Pod
metadata:
  name: news
  labels:
    app: news
spec:
  containers: 
    - name: news-container
      image: aluracursos/portal-noticias:1
      ports:
        - containerPort: 80


Service NodePort

apiVersion: v1
kind: Service
metadata: 
  name: svc-news
spec:
  type: NodePort
  ports: 
    - port: 80
      nodePort: 30002
  selector:
    app: news
```

* NodePort, ClusterIP e LoadBalance são categorias de Services

# Variaveis ambiente

* Um pod também possui variáveis de ambiente para configuração
* Um ConfigMap serve para definir variáveis de ambiente em um service
* 
```

Pod

apiVersion: v1
kind: Pod
metadata:
  name: sistema-noticias
  labels:
    app: sistema-noticias
spec:
  containers:
    - name: sistema-noticias-container
      image: aluracursos/sistema-noticias:1
      ports:
        - containerPort: 80
      envFrom:
        - configMapRef:
            name: sistema-configmap


apiVersion: v2
kind: ConfigMap
metadata:
  name: sistema-configmap
spec:
  MYSQL_ROOT_PASSWORD: q1w2e3r4
  MYSQL_DATABASE: empresa
  MYSQL_PASSWORD: q1w2e3r4
```
* Comandos mais usados
    * kubectl apply -f ./arquivo.yaml
    * kubectl run 
    * kubectl delete pod <nome-do-pod>
    * kubectl edit
    * kubectl get pods
    * kubectl get svc
    * kubectl get configmap