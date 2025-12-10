# README – Pipeline CI/CD de Estudo com GitHub Actions

Este repositório demonstra três pipelines diferentes utilizando **GitHub Actions**, cada uma representando um cenário comum no ciclo de desenvolvimento moderno:

* **Branch `main`** → apenas testes (CI)
* **Branch `deploy-vm`** → CD com deploy em máquina virtual (simulado)
* **Branch `deploy-k8s`** → GitOps com ArgoCD para Kubernetes (simulado)

O objetivo deste projeto é **estudo e prática**, sem deploy real, mas respeitando as estruturas que seriam utilizadas em ambientes de produção.

---

# Estrutura do Projeto

```
/
├── app/                 # Aplicação Node.js
│   └── (código fonte)
├── k8s/                 # Manifests Kubernetes
│   └── deployment.yml
└── .github/workflows/  # Pipelines GitHub Actions
    ├── tests.yml
    ├── deploy-vm.yml
    └── deploy-k8s-gitops.yml
```

---

# Branch `main` — CI (Testes Automatizados)

A branch **main** contém apenas o processo de **Continuous Integration**, cujo foco é garantir que a aplicação está funcionando.

### O que essa pipeline faz?

1. Faz checkout do repositório
2. Instala Node.js 20
3. Acessa o diretório `/app`
4. Executa:

   * `npm install`
   * `npm test`

### Arquivo: `.github/workflows/tests.yml`

Esse workflow valida o código a cada push ou pull request na branch `main`.

---

# Branch `deploy-vm` — Build + Publicação da Imagem + Deploy Simulado em VM

A branch **deploy-vm** simula um pipeline real utilizado para publicar aplicações em uma **VM**, como um servidor Linux.

### O que essa pipeline faz?

1. Executa todos os testes da branch `main`
2. Faz build da imagem Docker
3. Publica no Docker Hub:

   ```
   celsojrss96/diolab:latest
   ```
4. Simula um deploy em uma VM usando SSH (sem acesso real)

### Observações

* A VM não existe: o deploy é **simulado**, apenas para estudo.
* O workflow mostra os comandos que seriam executados num ambiente real:

  ```
  ssh ...
  docker pull ...
  docker run ...
  ```

### Arquivo: `.github/workflows/deploy-vm.yml`

Esse arquivo demonstra um pipeline clássico de **CI + CD para VM**.

---

# Branch `deploy-k8s` — GitOps + ArgoCD (Simulado)

Essa branch demonstra o fluxo moderno GitOps usando **ArgoCD**, porém **sem cluster real**. Mesmo assim, a pipeline imita fielmente todas as etapas.

### O que essa pipeline faz?

1. Roda os testes
2. Faz build e push da imagem Docker
3. Simula o comportamento do ArgoCD:

   * valida os manifests Kubernetes
   * detecta a nova imagem publicada
   * simula a criação do *commit GitOps*
   * simula o webhook de sincronização
   * simula o `argocd app sync`
   * mostra como o deployment seria aplicado

### Manifesto Kubernetes real no repositório

`k8s/deployment.yml` contém:

* Deployment
* Service
* Exposição da porta 3000
* Uso da imagem `celsojrss96/diolab:latest`

### Arquivo: `.github/workflows/deploy-k8s-gitops.yml`

Esse arquivo demonstra como o GitHub Actions interage com um fluxo GitOps (mesmo que simulado).

---

# Comparação Entre as Branches

| Branch         | Pipeline                  | Build | Push Docker | Deploy Real  | Deploy Simulado | Kubernetes |
| -------------- | ------------------------- | ----- | ----------- | ------------ | --------------- | ---------- |
| **main**       | CI – Testes               | ❌     | ❌           | ❌            | ❌               | ❌          |
| **deploy-vm**  | CI/CD para VM             | ✔️    | ✔️          | ❌ (simulado) | ✔️              | ❌          |
| **deploy-k8s** | CI/CD + GitOps com ArgoCD | ✔️    | ✔️          | ❌ (simulado) | ✔️              | ✔️         |

---

# Objetivo do Projeto

Este repositório foi criado **para fins de estudo**, permitindo compreender:

* Como funciona o GitHub Actions
* Como estruturar pipelines separadas por branches
* Como realizar build e publicação de imagens Docker
* Como simular deploys em VM
* Como simular GitOps com ArgoCD para Kubernetes

A ideia é ensinar conceitos avançados sem necessidade de infraestrutura real.

---

# Próximos Passos / Sugestões

Se desejar expandir o estudo, você pode adicionar:

* Deploy real em VM usando SSH + Systemd
* Deploy real em Kubernetes com minikube
* Uso de Helm Charts
* ArgoCD real rodando em cluster local
* Argo Rollouts (canary e blue/green)
* Observabilidade com Prometheus + Grafana

---

# Autor

Projeto criado para estudos por **Celso Jr**
Docker Hub: `celsojrss96`
GitHub Actions • Docker • GitOps • Kubernetes
