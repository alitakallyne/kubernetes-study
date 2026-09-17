# Comandos Kubernetes

Anotações dos comandos utilizados durante os estudos de Kubernetes.

---

## 1. Minikube

### Iniciar o cluster usando Docker

```bash
minikube start --driver=docker
```

Inicia um cluster Kubernetes local utilizando o Docker como driver.

Fluxo utilizado:

```text
Windows
   ↓
Docker Desktop
   ↓
Minikube
   ↓
Kubernetes
```

---

### Verificar o status do Minikube

```bash
minikube status
```

Mostra o estado atual dos principais componentes do cluster Minikube.

---

### Excluir o cluster

```bash
minikube delete
```

Remove o cluster Minikube criado localmente.

---

# 2. Nodes

### Listar Nodes

```bash
kubectl get nodes
```

Lista os Nodes disponíveis no cluster.

Exemplo:

```text
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   ...   v1.37.0
```

### Conceitos

* `NAME` → nome do Node
* `STATUS` → estado do Node
* `ROLES` → função do Node
* `AGE` → tempo desde sua criação
* `VERSION` → versão do Kubernetes

No cluster local utilizado neste estudo existe inicialmente um Node:

```text
minikube
```

---

# 3. Pods

## O que é um Pod?

Pod é a menor unidade que o Kubernetes gerencia para executar workloads.

Um Pod pode conter um ou mais containers.

Exemplo:

```text
Pod
└── Container
    └── Aplicação
```

---

## Criar Pod de forma imperativa

```bash
kubectl run php-pod --image=php:8.0.7-fpm-alpine3.13
```

Cria um Pod chamado `php-pod` utilizando a imagem:

```text
php:8.0.7-fpm-alpine3.13
```

### Estrutura do comando

```text
kubectl
  ↓
run
  ↓
php-pod
  ↓
--image=php:8.0.7-fpm-alpine3.13
```

* `kubectl` → ferramenta de comunicação com o Kubernetes
* `run` → solicita a criação de um Pod
* `php-pod` → nome do Pod
* `--image` → imagem utilizada pelo container

---

## Listar Pods

```bash
kubectl get pods
```

Lista os Pods do namespace atual.

Por padrão, o comando utiliza o namespace:

```text
default
```

Se não houver Pods:

```text
No resources found in default namespace.
```

---

## Listar Pods de todos os namespaces

```bash
kubectl get pods -A
```

Lista os Pods existentes em todos os namespaces.

`-A` é uma forma abreviada de:

```text
--all-namespaces
```

Exemplo:

```text
NAMESPACE     NAME                         READY   STATUS
kube-system   coredns-...                 1/1     Running
kube-system   kube-apiserver-minikube     1/1     Running
```

---

# 4. Inspecionando um Pod

### Descrever um Pod

```bash
kubectl describe pod php-pod
```

Exibe informações detalhadas sobre o Pod.

Entre as informações apresentadas estão:

* Namespace
* Node
* IP
* Containers
* Imagem
* Estado do container
* Restarts
* Volumes
* Conditions
* Events

### Events

Os Events ajudam a entender o ciclo de criação do Pod.

Exemplo:

```text
Scheduled
    ↓
Pulling
    ↓
Pulled
    ↓
Created
    ↓
Started
```

Representação simplificada:

```text
Scheduler escolhe o Node
        ↓
Imagem é baixada
        ↓
Container é criado
        ↓
Container é iniciado
        ↓
Pod fica Running
```

---

# 5. Acompanhar Pods em tempo real

```bash
kubectl get pods --watch
```

Lista os Pods e continua acompanhando alterações em tempo real.

Também pode ser utilizado:

```bash
kubectl get pods -w
```

### Exemplo

Um Pod pode passar por estados como:

```text
ContainerCreating
       ↓
Running
```

Para interromper o `watch`:

```text
Ctrl + C
```

---

# 6. Criação declarativa

Na abordagem declarativa, a configuração desejada é descrita em um arquivo YAML.

Exemplo:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: php-pod-dec
spec:
  containers:
    - name: php
      image: php:8.0.7-fpm-alpine3.13
```

Arquivo:

```text
pods/php-pod.yaml
```

---

## Aplicar um arquivo YAML

```bash
kubectl apply -f pods/php-pod.yaml
```

O comando aplica ao cluster o estado definido no arquivo YAML.

Exemplo de resultado:

```text
pod/php-pod-dec created
```

---

# 7. Imperativo x Declarativo

## Imperativo

```bash
kubectl run php-pod --image=php:8.0.7-fpm-alpine3.13
```

A instrução informa diretamente uma ação:

```text
"Crie este Pod."
```

## Declarativo

```bash
kubectl apply -f pods/php-pod.yaml
```

O arquivo descreve o estado desejado:

```text
"Este é o estado que quero no cluster."
```

### Comparação

```text
IMPERATIVO

kubectl run
     ↓
"Faça isso"
```

```text
DECLARATIVO

YAML
 ↓
Estado desejado
 ↓
kubectl apply
 ↓
Kubernetes
```

---

# 8. Namespaces

Um namespace é uma divisão lógica dentro do cluster.

Exemplo:

```text
Cluster
│
├── kube-system
│   ├── CoreDNS
│   ├── API Server
│   └── outros componentes
│
└── default
    └── Pods criados durante os estudos
```

Para consultar apenas o namespace atual:

```bash
kubectl get pods
```

Para consultar todos:

```bash
kubectl get pods -A
```

---

# 9. Conceitos aprendidos até aqui

```text
Kubernetes
    ↓
Cluster
    ↓
Node
    ↓
Pod
    ↓
Container
    ↓
Imagem
```

Ferramentas:

```text
Minikube
→ cria/executa um cluster Kubernetes local

kubectl
→ permite interagir com o cluster

Docker
→ utilizado como ambiente/driver do Minikube neste estudo

Docker Hub
→ origem das imagens utilizadas pelos containers
```

---

# 10. Próximos estudos

* [ ] Deployment
* [ ] ReplicaSet
* [ ] Services
* [ ] ConfigMap
* [ ] Secret
* [ ] Volumes
* [ ] Probes
* [ ] Requests e Limits
* [ ] Escalabilidade
* [ ] Ingress
* [ ] Helm
