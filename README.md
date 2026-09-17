# Kubernetes Study 🚀

Repositório criado para registrar meu aprendizado prático de **Kubernetes**, utilizando **Minikube, Docker e kubectl**.

A proposta é aprender Kubernetes de forma progressiva, entendendo os conceitos por trás dos recursos e comandos, e não apenas reproduzindo configurações.

> **Objetivo:** entender como o Kubernetes funciona na prática, desde a criação de Pods até recursos mais avançados de orquestração.

---

## 🛠️ Ambiente de estudo

* Windows 11
* Docker Desktop
* Minikube
* Kubernetes
* kubectl
* Docker Hub

### Versões utilizadas

```text
Minikube:   v1.39.0
Kubernetes: v1.37.0
kubectl:    v1.34.1
```

> As versões podem mudar conforme a evolução do ambiente de estudos.

---

## 📚 Conteúdos estudados

### 01. Ambiente Kubernetes

* O que é Kubernetes
* O que é Minikube
* O que é kubectl
* Docker como driver do Minikube
* Criação de cluster local
* Nodes
* Namespaces

### 02. Pods

* Conceito de Pod
* Criação imperativa
* Criação declarativa
* Imagens de containers
* Estados de um Pod
* Inspeção de Pods
* Acompanhamento de mudanças

### 03. Deployments

Em breve.

### 04. Services

Em breve.

### 05. ConfigMaps e Secrets

Em breve.

### 06. Volumes e persistência

Em breve.

### 07. Comunicação entre aplicações

Em breve.

### 08. Escalabilidade

Em breve.

---

## 🧠 Abordagem de estudo

A ideia deste repositório é acompanhar a evolução do aprendizado de forma prática.

Para cada recurso estudado, busco entender:

```text
Conceito
   ↓
Por que existe?
   ↓
Como funciona?
   ↓
Como criar?
   ↓
Como verificar?
   ↓
Como alterar?
   ↓
Quais problemas resolve?
```

Os comandos utilizados durante os estudos estão documentados em [`comandos.md`](./comandos.md).

---

## 📁 Estrutura do projeto

```text
kubernetes-study/
│
├── README.md
├── comandos.md
│
└── pods/
    └── php-pod.yaml
```

---

## 🚀 Primeiros passos

Iniciar o cluster local utilizando Docker:

```bash
minikube start --driver=docker
```

Verificar o Node:

```bash
kubectl get nodes
```

Verificar os Pods:

```bash
kubectl get pods
```

Verificar Pods de todos os namespaces:

```bash
kubectl get pods -A
```

---

## 📌 Progresso

* [x] Criar cluster Kubernetes com Minikube
* [x] Utilizar Docker como driver
* [x] Entender Node
* [x] Entender Namespace
* [x] Criar Pod de forma imperativa
* [x] Criar Pod de forma declarativa
* [x] Utilizar arquivos YAML
* [x] Inspecionar Pods com `kubectl describe`
* [x] Acompanhar Pods com `--watch`
* [ ] Deployments


---

## 🎯 Objetivo

Construir uma base sólida em Kubernetes por meio de experimentos locais e documentação dos conceitos aprendidos.

Este repositório será atualizado conforme novos recursos forem estudados.
