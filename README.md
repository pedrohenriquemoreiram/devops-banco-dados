# devops-banco-dados
Repositório do banco de dados do projeto de DevOps com Postgres.

# Banco de Dados DevOps - PostgreSQL

Este repositório contém a configuração para deploy de um banco de dados PostgreSQL dentro de um cluster Kubernetes, como parte da arquitetura integrada com backend FastAPI e frontend HTML + JS.

## 🧱 Tecnologias Utilizadas

- PostgreSQL 16
- Docker
- Kubernetes (com `Deployment`, `Service`, `Secrets`)
- ArgoCD (para GitOps)
- Kustomize

## 🗄️ Configuração do Banco

- **Nome do banco**: `meubanco`
- **Usuário**: `admin`
- **Senha**: `admin123`

Estas informações são injetadas como variáveis de ambiente a partir de um `Secret`.

## 🔐 Secrets

O acesso ao banco é controlado por `Secret` no Kubernetes:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: postgres-secret
type: Opaque
stringData:
  POSTGRES_DB: meubanco
  POSTGRES_USER: admin
  POSTGRES_PASSWORD: admin123


🚀 Deploy com Kubernetes
O banco é executado como um container a partir da imagem oficial postgres:16.

📦 Estrutura do diretório

├── deployment.yaml
├── service.yaml
├── secret.yaml
└── kustomization.yaml


🔁 Aplicar com Kustomize

kubectl apply -k k8s/


📤 Expondo o banco
O Service está configurado como NodePort para possibilitar acesso externo (ex: PGAdmin):

spec:
  type: NodePort
  ports:
    - port: 5432
      targetPort: 5432
      nodePort: 30000


📂 Organização do repositório
Copiar
Editar
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── secret.yaml
│   └── kustomization.yaml
├── Dockerfile (opcional para customização)
