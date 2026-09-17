# Deployments

## O que é um Deployment?

Um `Deployment` é um recurso do Kubernetes utilizado para gerenciar aplicações que precisam manter uma quantidade desejada de Pods em execução.

Ele utiliza um `ReplicaSet` internamente para garantir a quantidade de réplicas definida.

```text
Deployment
    ↓
ReplicaSet
    ↓
Pods
```

Além de manter as réplicas, o Deployment permite realizar atualizações controladas da aplicação, substituindo Pods antigos por novos.

---

## Criando um Deployment declarativo

Arquivo:

```text
deployments/nginx-deployment.yaml
```

Exemplo:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

### Aplicar o Deployment

```powershell
kubectl apply -f deployments/nginx-deployment.yaml
```

O comando informa ao Kubernetes que ele deve criar ou atualizar os recursos definidos no arquivo YAML.

Resultado esperado:

```text
deployment.apps/nginx created
```

---

## Verificar o Deployment

```powershell
kubectl get deployments
```

Resultado esperado:

```text
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   3/3     3            3            ...
```

### O que observar

* `READY 3/3` → 3 réplicas estão prontas.
* `UP-TO-DATE 3` → as 3 réplicas estão utilizando a configuração atual.
* `AVAILABLE 3` → existem 3 Pods disponíveis para atender a aplicação.

---

## Verificar os Pods

```powershell
kubectl get pods
```

Deve aparecerem 3 Pods criados pelo Deployment.

Os nomes serão semelhantes a:

```text
nginx-xxxxxxxxxx-xxxxx
nginx-xxxxxxxxxx-xxxxx
nginx-xxxxxxxxxx-xxxxx
```

O nome não será simplesmente `nginx`, porque os Pods são gerenciados pelo ReplicaSet criado pelo Deployment.

---

## Verificar o ReplicaSet

```powershell
kubectl get replicasets
```

Resultado esperado:

```text
NAME               DESIRED   CURRENT   READY   AGE
nginx-xxxxxxxxxx   3         3         3       ...
```

Isso demonstra a relação:

```text
Deployment
    ↓
ReplicaSet
    ↓
3 Pods
```

---

## Ver todos os recursos relacionados

```powershell
kubectl get deployment,replicaset,pods
```

Esse comando permite visualizar o Deployment, o ReplicaSet e os Pods relacionados em uma única consulta.

---

## Ver detalhes do Deployment

```powershell
kubectl describe deployment nginx
```

Mostra informações detalhadas do Deployment, incluindo:

* quantidade de réplicas;
* estratégia de atualização;
* ReplicaSet utilizado;
* eventos;
* condições atuais;
* Pods controlados.

---

## Conceito importante: estado desejado

Quando usamos:

```yaml
replicas: 3
```

estamos declarando:

> O estado desejado da aplicação é possuir 3 Pods disponíveis.

O Kubernetes trabalha continuamente para manter esse estado.

Por exemplo, se um dos Pods for removido:

```text
Estado desejado:
3 Pods

Estado atual:
2 Pods

Kubernetes:
→ cria outro Pod

Estado final:
3 Pods
```

Essa é uma das principais vantagens de utilizar um Deployment.

---

## Deployment x Pod

### Pod

Representa a unidade que executa o container.

```text
Pod
 └── Container nginx
```

### Deployment

Gerencia os Pods e mantém o estado desejado da aplicação.

```text
Deployment
    ↓
ReplicaSet
    ↓
Pods
    ↓
Containers
```

---

## Deployment x ReplicaSet

O `ReplicaSet` é responsável principalmente por manter a quantidade desejada de Pods.

O `Deployment` fica acima dele e também fornece recursos para gerenciamento de versões e atualizações da aplicação.

```text
Deployment
   │
   ├── controla atualizações
   │
   └── ReplicaSet
          │
          ├── Pod
          ├── Pod
          └── Pod
```

---

## Comandos principais

```powershell
# Criar/atualizar Deployment a partir do YAML
kubectl apply -f deployments/nginx-deployment.yaml

# Listar Deployments
kubectl get deployments

# Listar Pods
kubectl get pods

# Listar ReplicaSets
kubectl get replicasets

# Ver Deployment, ReplicaSet e Pods juntos
kubectl get deployment,replicaset,pods

# Ver detalhes do Deployment
kubectl describe deployment nginx
```
