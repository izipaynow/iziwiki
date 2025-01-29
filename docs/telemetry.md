# Sistema de Telemetria

> **Status da aplicação:** Em produção
> **Última atualização:** 24/01/2025

## Sumário
1. [Visão Geral](#1-visão-geral)
2. [Banco de Dados](#2-banco-de-dados)
3. [Projeto de Migrations](#3-projeto-de-migrations)
4. [Tecnologias e Dependências](#4-tecnologias-e-dependências)
5. [Estrutura dos Projetos](#5-estrutura-dos-projetos)
6. [APIs e Serviços](#6-apis-e-serviços)
7. [Segurança](#7-segurança)
8. [Processos de Negócio](#8-processos-de-negócio)
9. [Configuração do Ambiente](#9-configuração-do-ambiente)
10. [Ambientes e Deploy](#10-ambientes-e-deploy)

## 1. Visão Geral

### 1.1 Contexto
A aplicação de telemetria é um sistema white-label utilizado pela Bankizi para atender clientes que desejam utilizar os serviços bancários como meios de pagamentos para seus negócios. Atualmente, a aplicação opera com um cliente que utiliza o serviço de PIX da Bankizi em máquinas de ursinhos de pelúcia.

### 1.2 Arquitetura do Sistema
- **Frontend:** Dashboard para visualização de telemetria e transações
- **Backend:** API para comunicação com serviços externos e gerenciamento de dados
- **Integrações:** APIs Izipay e Atera
- **Banco de Dados:** PostgreSQL

![Fluxo Telemetria](img/prints/fluxo-integracao-telemetria-2024-12-13-0950.png)

### 1.3 Infraestrutura AWS
![Arquitetura AWS](img/prints/aws-arquitetura-2024-11-08-0109.png)

## 2. Banco de Dados

### 2.1 Modelagem
![Modelagem Banco de dados](img/prints/diagrama-ER-Telemetria-v5_2024-12-13T17_39_55.865Z.png)

### 2.2 Configurações
- PostgreSQL com SSL em produção/homologação
- Pool de conexões:
  - Máximo: 10
  - Mínimo: 2
  - Idle: 10
- Suporte a múltiplos hosts (read/write)

### 2.3 Backup e Restore
- Backup automático diário às 03:00 AM (UTC-3)
- Retenção de backups: 30 dias
- Procedimento de restore:
  ```bash
  # Restore do último backup
  npm run db:restore --backup=latest
  
  # Restore de data específica
  npm run db:restore --backup=YYYY-MM-DD
  ```

## 3. Projeto de Migrations

### 3.1 Visão Geral
O projeto de migrations é mantido em um repositório separado (`telemetria-migrations`) para melhor organização e controle das alterações do banco de dados. Esta separação permite:

- Versionamento independente das alterações do banco
- Melhor rastreabilidade das mudanças
- Facilidade na gestão de ambientes
- Controle mais rigoroso das alterações estruturais

### 3.2 Estrutura do Projeto
```
telemetria-migrations/
├── migrations/                    # Arquivos de migração
├── seeds/                         # Seeds para população inicial
│   ├── homolog/                   # Seeds específicos para homologação
│   ├── 01_companies.js            # Dados de empresas
│   ├── 02_roles.js                # Perfis de usuário
│   ├── 03_data_profile.js         # Dados de perfil
│   ├── 04_users.js                # Usuários iniciais
│   ├── 05_user_roles.js           # Associação usuário-perfil
│   ├── 06_interfaces.js           # Interfaces iniciais
│   ├── 07_interface_users.js      # Associação interface-usuário
│   └── 08_receipt_refund.js       # Dados de reembolso
│
├── helpers/                       # Funções auxiliares
├── scripts/                       # Scripts de automação
├── knexfile.js                    # Configuração do Knex
├── docker-compose.yaml            # Configuração Docker
└── package.json                   # Dependências e scripts
```

### 3.3 Migrations Disponíveis
1. **Users Table** 
   - Estrutura base de usuários
   - Campos de autenticação e perfil

2. **Interface Users** 
   - Relacionamento entre interfaces e usuários
   - Permissões de acesso

3. **Interface Daily Report**
   - Relatórios diários de interface
   - Métricas e estatísticas

4. **Interface Readings**
   - Leituras de telemetria
   - Dados de sensores

5. **Reading Date Range**
   - Campos de período de leitura
   - Otimização de consultas

6. **Receipt Refund**
   - Estrutura de reembolsos
   - Comprovantes e histórico

### 3.4 Seeds
- **Produção**: Seeds essenciais para o funcionamento do sistema
  - Empresas e perfis básicos
  - Usuário administrador inicial
  - Configurações padrão

- **Homologação**: Seeds adicionais para testes
  - Dados de exemplo
  - Usuários de teste
  - Cenários de validação

### 3.5 Comandos e Scripts

```bash
# Executar todas as migrations
npm run migrate

# Reverter última migration
npm run migrate:rollback

# Criar nova migration
npm run migrate:make nome_da_migration

# Seeds de produção
npm run seed:prod

# Seeds de homologação
npm run seed:homolog

# Reverter seeds
npm run seed:rollback
```

## 4. Tecnologias e Dependências

### 4.1 Frontend
- React 18.3.1
- FUSE React 11.1.0
- Material UI (MUI) 6.1.6
- TypeScript 5.x
- Vite (build tool)
- Principais bibliotecas:
  - Redux Toolkit 2.2.5
  - React Router 6.23.1
  - Axios 1.7.4
  - React Hook Form 7.51.5
  - ApexCharts 3.54.1
  - Date-fns 4.1.0

### 4.2 Backend
- Node.js >= 14.15.0
- TypeScript 5.5
- Serverless Framework 3.39
- PostgreSQL 14
- Redis 6.2
- Principais bibliotecas:
  - Knex 3.1
  - JWT 9.0
  - Zod 3.23
  - AWS SDK 3.x
  - Express 4.18
  - Winston 3.10

### 4.3 DevOps e Ferramentas
- Docker 24.x
- Docker Compose 2.x
- Serverless Framework Plugins:
  - serverless-esbuild
  - serverless-offline
  - serverless-dotenv

## 5. Estrutura dos Projetos

### 5.1 Frontend
```
src/
├── @fuse/                       # Framework FUSE React core
│   ├── core/                    # Componentes core do framework
│   ├── hooks/                   # Hooks customizados
│   ├── utils/                   # Utilitários do framework
│   └── defaults/                # Configurações padrão
│
├── @lodash/                     # Utilitários Lodash customizados
│   └── _.ts                     # Funções lodash personalizadas
│
├── app/                         # Núcleo da aplicação
│   ├── auth/                    # Autenticação e autorização
│   │   ├── services/            # Serviços de autenticação
│   │   ├── user/                # Gerenciamento de usuário
│   │   ├── Authentication.tsx   # Componente principal de auth
│   │   ├── authRoles.ts         # Definição de papéis/roles
│   │   └── useAuth.tsx          # Hook de autenticação
│   │
│   ├── configs/                 # Configurações globais
│   │   ├── navigation/          # Configuração de navegação
│   │   ├── routes/              # Definição de rotas
│   │   └── themes/              # Temas da aplicação
│   │
│   ├── main/                    # Páginas e componentes principais
│   │   ├── dashboard/           # Dashboard principal
│   │   │   ├── components/      # Componentes do dashboard
│   │   │   ├── store/           # Estado local do dashboard
│   │   │   └── widgets/         # Widgets e gráficos
│   │   │
│   │   ├── telemetry/           # Módulo de telemetria
│   │   │   ├── components/      # Componentes de telemetria
│   │   │   ├── store/           # Estado da telemetria
│   │   │   └── types/           # Tipos e interfaces
│   │   │
│   │   ├── sales/               # Módulo de vendas/transações
│   │   ├── users/               # Gestão de usuários
│   │   ├── readings/            # Leituras de telemetria
│   │   └── profile/             # Perfil de usuário
│   │
│   ├── services/                # Serviços e integrações
│   │   ├── api/                 # Cliente HTTP e configurações
│   │   └── interceptors/        # Interceptadores de requisições
│   │
│   ├── shared-components/       # Componentes compartilhados
│   │   ├── tables/              # Componentes de tabela
│   │   ├── forms/               # Componentes de formulário
│   │   ├── dialogs/             # Modais e diálogos
│   │   └── charts/              # Componentes de gráficos
│   │
│   ├── store/                   # Gerenciamento de estado global
│   │   ├── middleware.ts        # Middlewares Redux
│   │   ├── store.ts             # Configuração da store
│   │   └── slices/              # Slices Redux
│   │
│   ├── theme-layouts/           # Layouts e temas
│   │   ├── layout1/             # Layout principal
│   │   ├── shared/              # Componentes compartilhados
│   │   └── components/          # Componentes de layout
│   │
│   └── utils/                   # Funções utilitárias
│       ├── formatters/          # Formatadores de dados
│       ├── validators/          # Validadores
│       └── helpers/             # Funções auxiliares
│
├── styles/                      # Estilos globais
│   ├── app-base.css             # Estilos base
│   └── tailwind.css             # Configuração Tailwind
│
└── node-scripts/                # Scripts de automação
    └── fuse-react-message.js    # Scripts de build
```

Principais diretórios e suas responsabilidades:

- **@fuse/**: Contém o core do framework FUSE React, incluindo componentes base, hooks e utilitários
- **app/main/**: Páginas principais da aplicação, incluindo dashboard, telemetria e transações
- **app/shared-components/**: Componentes reutilizáveis específicos do projeto
- **app/theme-layouts/**: Diferentes layouts e temas da aplicação
- **app/auth/**: Lógica de autenticação, guards e providers
- **app/store/**: Gerenciamento de estado global com Redux
- **app/configs/**: Configurações de rotas, navegação e ambiente

### 5.2 Backend
```
src/
├── @types                  # Tipos globais e declarações de tipos
│   └── knex                # Tipos personalizados para o Knex
├── config                  # Configurações globais
│   ├── db                  # Configuração do banco de dados
│   └── aws                 # Configurações AWS (S3, SNS, etc)
├── database                # Arquivos relacionados ao banco de dados
│   ├── migrations          # Migrações do banco de dados
│   ├── seeds               # Seeds para dados iniciais
│   └── repositories        # Camada de acesso ao banco
├── docs                    # Documentação adicional do projeto
│   ├── api                 # Documentação da API (OpenAPI/Swagger)
│   └── diagrams            # Diagramas e fluxos
├── functions               # Lambdas e código-fonte
│   ├── auth                # Autenticação e autorização
│   │   ├── handler.ts      # Handler da lambda
│   │   ├── index.ts        # Configuração Serverless
│   │   ├── routes.ts       # Definições de rotas
│   │   ├── controller.ts   # Controlador de requisições
│   │   ├── service.ts      # Lógica de negócios
│   │   └── validator.ts    # Validação de entrada
│   ├── metrics             # Métricas e telemetria
│   ├── transactions        # Gestão de transações
│   ├── users               # Gestão de usuários
│   └── interfaces          # Gestão de interfaces
├── libs                    # Código compartilhado
│   ├── @bankizi-sdk        # SDK de integração com a Bankizi
│   │   ├── adapters        # Adaptadores de dados
│   │   ├── errors          # Tratamento de erros específicos
│   │   ├── types           # Tipos e interfaces
│   │   └── index.ts        # Classe principal do SDK
│   ├── aws                 # Integrações AWS
│   ├── cache               # Gerenciamento de cache
│   ├── database            # Utilitários de banco de dados
│   ├── http                # Cliente HTTP e integrações
│   ├── logger              # Sistema de logging
│   └── validators          # Validadores compartilhados
├── middlewares             # Middlewares da aplicação
│   ├── auth                # Middleware de autenticação
│   └── validation          # Middleware de validação
├── templates               # Templates para emails e relatórios
│   ├── email               # Templates de email
│   └── reports             # Templates de relatórios
└── utils                   # Funções utilitárias
    ├── constants           # Constantes globais
    ├── dates               # Manipulação de datas
    ├── encryption          # Funções de criptografia
    ├── errors              # Tratamento de erros
    └── formatters          # Formatadores de dados
```

Cada lambda function segue a estrutura:
```
functions/[lambda-name]/
├── handler.ts      # Ponto de entrada da lambda
├── index.ts        # Configuração Serverless
├── routes.ts       # Definição de rotas e endpoints
├── controller.ts   # Controlador de requisições HTTP
├── service.ts      # Lógica de negócios
├── validator.ts    # Validação de entrada de dados
├── repository.ts   # Acesso ao banco de dados (opcional)
└── types.ts        # Tipos específicos da lambda
```

## 6. APIs e Serviços

### 6.1 Lambdas
1. **userLambda**
   - CRUD de usuários
   - Integração com Izipay para criação de usuários
   - Perfis: admin (2) e operador (3)

2. **authLambda**
   - Autenticação via email/username e senha
   - Gerenciamento de JWT

3. **metricLambda**
   - Dados para dashboard
   - Integração com Izipay e Atera
   - Filtros por data e período

4. **transactionLambda**
   - Listagem e estorno de transações
   - Filtros por ID, loja, cliente, status e período

5. **interfaceLambda**
   - Telemetria e gestão de interfaces
   - Associação operador-interface
   - Geração de relatórios

### 6.2 Endpoints

#### Auth API
```
POST /api/auth/login
POST /api/auth/refresh
POST /api/auth/logout
```

#### Users API
```
GET    /api/users
POST   /api/users
GET    /api/users/:id
PUT    /api/users/:id
DELETE /api/users/:id
```

#### Transactions API
```
GET    /api/transactions
POST   /api/transactions/refund
GET    /api/transactions/:id
GET    /api/transactions/metrics
```

#### Interface API
```
GET    /api/interfaces
POST   /api/interfaces
GET    /api/interfaces/:id
PUT    /api/interfaces/:id
DELETE /api/interfaces/:id
GET    /api/interfaces/:id/metrics
```

### 6.3 Cache (Redis)
- Dados de telemetria frequentes
- Informações de sessão
- Resultados de consultas comuns
- TTL padrão: 1 hora
- Estratégia de invalidação: por chave

### 6.4 SDK Bankizi

#### 6.4.1 Visão Geral
O SDK da Bankizi é uma biblioteca que facilita a integração com a API da Bankizi/Izipay. Ele fornece uma interface orientada a objetos para acessar os endpoints da API, com recursos de autenticação, tratamento de erros e adaptação de dados.

#### 6.4.2 Inicialização
```typescript
const bankizi = await BankiziSDK.Init('bankizi-user-uid');
```

#### 6.4.3 Recursos Disponíveis

##### Transações
- Listagem de transações com filtros
- Suporte a paginação
- Filtros por data, cliente, status e IDs

```typescript
const transactions = await bankizi.transactions({
  page: 1,
  limit: 10,
  startDate: '2024-01-01',
  endDate: '2024-01-31'
});
```

##### Sub-contas
- Criação de sub-contas
- Atualização de relação parent-child
- Gerenciamento de hierarquia

```typescript
const subAccount = await bankizi.createSubAccount({
  description: 'Nova sub-conta'
});
```

##### Métricas
- Consulta de métricas por período
- Dados agregados de transações
- Indicadores de performance

```typescript
const metrics = await bankizi.metrics({
  fromDate: '2024-01-01',
  toDate: '2024-01-31'
});
```

##### Estornos
- Processamento de estornos
- Validação de transações
- Rastreamento por ID

```typescript
const refund = await bankizi.refund({
  endToEndId: '123456789',
  transactionId: '987654321'
});
```

#### 6.4.4 Tratamento de Erros
O SDK inclui tratamento especializado para diferentes tipos de erros:

- `UnauthorizedError`: Falhas de autenticação (401/403)
- `BadRequestError`: Requisições inválidas (400)
- `InternalServerError`: Erros internos do servidor

#### 6.4.5 Configuração
O SDK requer as seguintes variáveis de ambiente:

- `X_API_KEY_IZIPAY`: Chave de API para autenticação
- `SERVICE_NAME`: Nome do serviço para identificação

## 7. Segurança

### 7.1 Autenticação
- JWT Token
- Expiração: 1 dia
- Salt rounds: 8
- Middleware de autenticação para rotas privadas

### 7.2 Políticas IAM
- Princípio do menor privilégio
- Roles específicas por lambda
- Acesso restrito ao S3 e DynamoDB
- Rotação automática de credenciais: 90 dias

## 8. Processos de Negócio

### 8.1 Fluxo de Transação PIX
1. Cliente seleciona produto na máquina
2. Interface gera QR Code PIX
3. Cliente efetua pagamento
4. Webhook Izipay confirma pagamento
5. Interface libera produto
6. Telemetria registra operação

### 8.2 Regras de Negócio
- Timeout de pagamento: 5 minutos
- Limite de tentativas: 3 por transação
- Valor mínimo: R$ 1,00
- Valor máximo: R$ 500,00
- Estorno disponível em até 30 dias

## 9. Configuração do Ambiente

### 9.1 Pré-requisitos
- Node.js >= 14.15.0
- PostgreSQL
- Docker e Docker Compose (opcional)
- AWS CLI configurado

### 9.2 Instalação
```bash
# Instalar dependências
npm install

# Configurar ambiente
cp .env.example .env

# Migrations
npm run migrate

# Seeds (opcional)
npm run seed
```

### 9.3 Scripts Disponíveis
```bash
# Desenvolvimento
npm run dev

# Build
npm run build

# Deploy
npm run deploy:homolog
npm run deploy:prod

# Banco de dados
npm run migrate
npm run migrate:rollback
npm run seed
```

## 10. Ambientes e Deploy

### 10.1 Ambientes
- Desenvolvimento: Local
- Homologação: [Acessar URL homologação](https://develop.d1wl150ymgj6k8.amplifyapp.com)
- Produção: [Acessar URL produção](https://kgk-telemetria.bankizi.com/)

### 10.2 Deploy
- Automatizado via AWS Amplify
- Branches:

    `develop` (homologação)

    `main` (produção)
