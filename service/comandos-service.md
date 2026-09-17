# Service — Comandos e Estudos

## 1. Criar o Service

Aplicar o arquivo YAML do Service:

```powershell
kubectl apply -f services/nginx-service.yaml
```

Cria o Service definido no arquivo `nginx-service.yaml`.

---

## 2. Listar os Services

```powershell
kubectl get services
```

Ou usando a abreviação:

```powershell
kubectl get svc
```

Mostra os Services existentes no namespace atual.

Exemplo:

```text
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)
nginx-service   ClusterIP   10.96.xxx.xxx   <none>        80/TCP
```

---

## 3. Ver detalhes do Service

```powershell
kubectl describe service nginx-service
```

Mostra informações detalhadas sobre o Service, como:

* Tipo
* Cluster IP
* Porta
* TargetPort
* Selector
* Endpoints
* Eventos

---

## 4. Ver os Pods encontrados pelo Service

```powershell
kubectl get endpoints nginx-service
```

Mostra os endereços dos Pods que foram encontrados pelo `selector` do Service.

Exemplo:

```text
NAME            ENDPOINTS
nginx-service   10.244.0.4:80,10.244.0.5:80,10.244.0.6:80
```

Isso permite verificar se o Service está realmente encontrando os Pods.

---

## 5. Como o Service encontra os Pods

O Service possui um `selector`:

```yaml
selector:
  app: nginx
```

Os Pods precisam possuir a mesma label:

```yaml
labels:
  app: nginx
```

O Kubernetes relaciona os dois:

```text
Service
   │
   │ selector: app=nginx
   │
   ├── Pod nginx-1
   ├── Pod nginx-2
   └── Pod nginx-3
```

---

## 6. Port e TargetPort

Exemplo:

```yaml
ports:
  - port: 80
    targetPort: 80
```

### `port`

É a porta disponibilizada pelo Service.

### `targetPort`

É a porta para onde o Service encaminha o tráfego nos Pods.

Neste exemplo:

```text
Cliente
   ↓
Service :80
   ↓
Pod :80
```

---

## 7. Tipo padrão

Quando não informamos um `type`, o Kubernetes utiliza:

```yaml
type: ClusterIP
```

Portanto, este Service:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  selector:
    app: nginx

  ports:
    - port: 80
      targetPort: 80
```

é equivalente a:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  type: ClusterIP

  selector:
    app: nginx

  ports:
    - port: 80
      targetPort: 80
```

---

## 8. Conceito principal

O Service fornece um endereço estável para acessar um conjunto de Pods.

Os Pods podem ser recriados e seus IPs podem mudar.

O Service continua sendo o ponto de acesso:

```text
                ┌── Pod
                │
Service ────────┼── Pod
                │
                └── Pod
```

O Service utiliza o `selector` para descobrir quais Pods fazem parte dele.

---

## 9. Service x Pod

### Pod

Executa a aplicação.

### Service

Fornece um ponto de acesso estável para essa aplicação.

```text
Pod → executa
Service → direciona o acesso
```
