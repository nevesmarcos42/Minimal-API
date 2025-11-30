# Minimal API

![C#](https://img.shields.io/badge/C%23-7.0-purple?style=for-the-badge&logo=csharp)
![.NET](https://img.shields.io/badge/.NET-7.0-512BD4?style=for-the-badge&logo=dotnet)
![Entity Framework](https://img.shields.io/badge/Entity%20Framework-7.0-512BD4?style=for-the-badge)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-Auth-000000?style=for-the-badge&logo=jsonwebtokens)
![Swagger](https://img.shields.io/badge/Swagger-API-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)

API RESTful para gerenciamento de veículos e administradores com autenticação JWT e controle de acesso baseado em perfis. Desenvolvida com ASP.NET Core Minimal APIs e Entity Framework Core.

[Funcionalidades](#funcionalidades) • [Tecnologias](#tecnologias) • [Instalação](#instalação) • [Uso](#uso) • [API](#documentação-da-api) • [Testes](#testes)

---

## Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [Funcionalidades](#funcionalidades)
- [Tecnologias](#tecnologias)
- [Arquitetura](#arquitetura)
- [Instalação](#instalação)
- [Uso](#uso)
- [Documentação da API](#documentação-da-api)
- [Testes](#testes)
- [Variáveis de Ambiente](#variáveis-de-ambiente)
- [Regras de Negócio](#regras-de-negócio)
- [Contribuindo](#contribuindo)
- [Licença](#licença)

---

## Sobre o Projeto

Minimal API é uma aplicação backend desenvolvida com **ASP.NET Core Minimal APIs**, oferecendo uma solução moderna e eficiente para gerenciamento de veículos e administradores. O projeto implementa autenticação JWT, controle de acesso baseado em perfis (Roles) e operações CRUD completas com validações robustas.

### Principais Características

- **Autenticação JWT** - Sistema seguro de login com tokens JWT
- **Controle de Acesso** - Perfis hierárquicos (Adm e Editor)
- **Gestão de Veículos** - CRUD completo com validações
- **Gestão de Administradores** - Controle de usuários do sistema
- **Paginação** - Listagem otimizada de recursos
- **Documentação Swagger** - Interface interativa para testes
- **Entity Framework Core** - ORM com migrations e seed data
- **Minimal APIs** - Arquitetura moderna e performática

---

## Funcionalidades

### Backend (API REST)

#### Autenticação

- Login com geração de token JWT
- Validação de credenciais
- Tokens com expiração configurável (1 dia)
- Claims customizadas (Email, Perfil, Role)
- Proteção de rotas sensíveis

#### Administradores

- Criar novos administradores
- Listar administradores (paginado)
- Buscar administrador por ID
- Controle de acesso baseado em perfil
- Perfis disponíveis: **Adm** (Admin) e **Editor**
- Validações:
  - Email obrigatório
  - Senha obrigatória
  - Perfil obrigatório

#### Veículos

- Criar veículos
- Listar veículos (paginado)
- Buscar veículo por ID
- Atualizar veículo (Adm)
- Deletar veículo (Adm)
- Validações:
  - Nome obrigatório
  - Marca obrigatória
  - Ano >= 1950

---

## Tecnologias

### Backend

| Tecnologia            | Versão | Descrição                |
| --------------------- | ------ | ------------------------ |
| C#                    | 11     | Linguagem de programação |
| .NET                  | 7.0    | Framework backend        |
| ASP.NET Core          | 7.0    | Minimal APIs             |
| Entity Framework Core | 7.0.14 | ORM e Migrations         |
| JWT (JwtBearer)       | 7.0.14 | Autenticação             |
| MySQL                 | 8.0+   | Banco de dados           |
| Pomelo EF MySQL       | 7.0.0  | Provider MySQL           |
| Swagger/OpenAPI       | 6.5.0  | Documentação interativa  |

### Testes

| Tecnologia                | Descrição               |
| ------------------------- | ----------------------- |
| xUnit                     | Framework de testes     |
| Moq                       | Mocking de dependências |
| Entity Framework InMemory | Testes de integração    |

---

## Arquitetura

### Estrutura do Projeto

```
Api/
├── Dominio/
│   ├── DTOs/                    # Data Transfer Objects
│   │   ├── AdministradorDTO.cs
│   │   ├── LoginDTO.cs
│   │   └── VeiculoDTO.cs
│   ├── Entidades/              # Modelos de domínio
│   │   ├── Administrador.cs
│   │   └── Veiculo.cs
│   ├── Enuns/                  # Enumerações
│   │   └── Perfil.cs
│   ├── Interfaces/             # Contratos de serviços
│   │   ├── IAdministradorServico.cs
│   │   └── IVeiculoServico.cs
│   ├── ModelViews/             # View Models
│   │   ├── AdministradorLogado.cs
│   │   ├── AdministradorModelView.cs
│   │   ├── ErrosDeValidacao.cs
│   │   └── Home.cs
│   └── Servicos/               # Implementação dos serviços
│       ├── AdministradorServico.cs
│       └── VeiculoServico.cs
├── Infraestrutura/
│   └── Db/                     # Contexto do banco de dados
│       └── DbContexto.cs
├── Migrations/                 # Migrations do EF Core
│   ├── AdministradorMigration.cs
│   ├── SeedAdministrador.cs
│   └── VeiculosMigration.cs
├── Program.cs                  # Entry point
└── Startup.cs                  # Configurações da aplicação

Test/
├── Domain/
│   ├── Entidades/             # Testes de entidades
│   └── Servicos/              # Testes de serviços
├── Requests/                   # Testes de endpoints
├── Helpers/                    # Utilitários de teste
└── Mocks/                      # Mocks de serviços
```

### Banco de Dados - Modelo de Dados

```
┌─────────────────┐           ┌─────────────────┐
│  Administrador  │           │    Veiculo      │
├─────────────────┤           ├─────────────────┤
│ Id (PK)         │           │ Id (PK)         │
│ Email           │           │ Nome            │
│ Senha           │           │ Marca           │
│ Perfil          │           │ Ano             │
└─────────────────┘           └─────────────────┘
```

### Padrão de Arquitetura

- **Minimal APIs** - Endpoints leves e performáticos
- **Repository Pattern** - Abstração da camada de dados
- **Service Layer** - Lógica de negócio isolada
- **DTOs** - Separação entre modelos de domínio e transporte
- **Dependency Injection** - Inversão de controle nativa do .NET

---

## Instalação

### Pré-requisitos

- [.NET SDK 7.0+](https://dotnet.microsoft.com/download)
- [MySQL 8.0+](https://dev.mysql.com/downloads/)
- IDE recomendada: [Visual Studio 2022](https://visualstudio.microsoft.com/) ou [VS Code](https://code.visualstudio.com/)

### Instalação Manual

#### 1. Clone o repositório

```bash
git clone https://github.com/nevesmarcos42/Minimal-API.git
cd Minimal-API
```

#### 2. Configure o banco de dados

Edite `Api/appsettings.json` com suas credenciais MySQL:

```json
{
  "ConnectionStrings": {
    "MySql": "Server=localhost;Database=minimalapi;Uid=root;Pwd=sua_senha;"
  },
  "Jwt": "sua_chave_secreta_aqui_minimo_32_caracteres"
}
```

#### 3. Execute as migrations

```bash
cd Api
dotnet ef database update
```

#### 4. Rode a aplicação

```bash
dotnet run
```

A API estará disponível em:

- API: `https://localhost:7076` ou `http://localhost:5179`
- Swagger: `https://localhost:7076/swagger`

#### 5. Executar testes

```bash
cd Test
dotnet test
```

---

## Uso

### Primeiro Acesso

1. **Acesse o Swagger**: `https://localhost:7076/swagger`

2. **Login com usuário seed** (criado automaticamente):

   - Email: `administrador@teste.com`
   - Senha: `123456`

3. **Obtenha o token JWT**:

   ```bash
   POST /administradores/login
   {
     "email": "administrador@teste.com",
     "senha": "123456"
   }
   ```

4. **Autentique no Swagger**:
   - Clique em "Authorize"
   - Insira: `Bearer SEU_TOKEN_JWT`
   - Agora você pode testar endpoints protegidos

### Funcionalidades Principais

#### Criar um Administrador

```bash
# Como ADM
POST /administradores
Authorization: Bearer {token}
{
  "email": "editor@teste.com",
  "senha": "senha123",
  "perfil": 1  # 0 = Adm, 1 = Editor
}
```

#### Cadastrar um Veículo

```bash
# Como ADM ou Editor
POST /veiculos
Authorization: Bearer {token}
{
  "nome": "Civic",
  "marca": "Honda",
  "ano": 2024
}
```

#### Listar Veículos (com paginação)

```bash
# Como qualquer usuário autenticado
GET /veiculos?pagina=1
Authorization: Bearer {token}
```

#### Atualizar um Veículo

```bash
# Apenas ADM
PUT /veiculos/1
Authorization: Bearer {token}
{
  "nome": "Civic EX",
  "marca": "Honda",
  "ano": 2024
}
```

#### Deletar um Veículo

```bash
# Apenas ADM
DELETE /veiculos/1
Authorization: Bearer {token}
```

---

## Documentação da API

A documentação interativa está disponível via **Swagger UI** após iniciar a aplicação:

**URL**: `https://localhost:7076/swagger`

### Principais Endpoints

#### Autenticação

```
POST   /administradores/login    # Login (público)
```

#### Administradores

```
POST   /administradores           # Criar (ADM)
GET    /administradores           # Listar (ADM)
GET    /administradores/{id}      # Buscar por ID (ADM)
```

#### Veículos

```
POST   /veiculos                  # Criar (ADM/Editor)
GET    /veiculos                  # Listar (autenticado)
GET    /veiculos/{id}             # Buscar por ID (ADM/Editor)
PUT    /veiculos/{id}             # Atualizar (ADM)
DELETE /veiculos/{id}             # Deletar (ADM)
```

### Exemplo de Requisição

#### Login

```bash
curl -X POST https://localhost:7076/administradores/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "administrador@teste.com",
    "senha": "123456"
  }'
```

**Resposta:**

```json
{
  "email": "administrador@teste.com",
  "perfil": "Adm",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Criar Veículo

```bash
curl -X POST https://localhost:7076/veiculos \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer SEU_TOKEN_JWT" \
  -d '{
    "nome": "Corolla",
    "marca": "Toyota",
    "ano": 2024
  }'
```

---

## Testes

### Backend - Testes Unitários e de Integração

O projeto possui testes cobrindo as principais camadas:

```bash
cd Test
dotnet test
```

**Cobertura de testes:**

- ✅ Testes de Entidades
- ✅ Testes de Serviços (com Mocks)
- ✅ Testes de Endpoints (Requests)
- ✅ Validações de negócio
- ✅ Autenticação e Autorização

### Executar testes com cobertura

```bash
dotnet test /p:CollectCoverage=true
```

---

## Variáveis de Ambiente

### appsettings.json

```json
{
  "ConnectionStrings": {
    "MySql": "Server=localhost;Database=minimalapi;Uid=root;Pwd=senha;"
  },
  "Jwt": "chave_secreta_para_gerar_tokens_jwt_minimo_32_caracteres",
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}
```

### appsettings.Development.json

Para desenvolvimento, você pode usar configurações específicas:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "Microsoft.AspNetCore": "Information"
    }
  },
  "ConnectionStrings": {
    "MySql": "Server=localhost;Database=minimalapi_dev;Uid=root;Pwd=dev;"
  }
}
```

---

## Regras de Negócio

### Administradores

- Email deve ser único e obrigatório
- Senha é obrigatória (mínimo 1 caractere)
- Perfil é obrigatório (Adm ou Editor)
- Apenas **Adm** pode:
  - Criar outros administradores
  - Listar administradores
  - Atualizar veículos
  - Deletar veículos

### Veículos

- Nome é obrigatório (máximo 150 caracteres)
- Marca é obrigatória (máximo 100 caracteres)
- Ano deve ser >= 1950
- Apenas **Adm** pode atualizar e deletar
- **Adm** e **Editor** podem criar e visualizar

### Autenticação

- Token JWT expira em 24 horas
- Token contém claims: Email, Perfil e Role
- Rotas protegidas exigem `Authorization: Bearer {token}`
- Perfis hierárquicos: Adm tem todas as permissões

### Paginação

- Parâmetro `?pagina=1` (padrão: 1)
- Retorna 10 itens por página
- Páginas começam em 1

---

## Contribuindo

Contribuições são bem-vindas! Siga os passos:

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/MinhaFeature`)
3. Commit suas mudanças (`git commit -m 'Adiciona MinhaFeature'`)
4. Push para a branch (`git push origin feature/MinhaFeature`)
5. Abra um Pull Request

### Padrões de Código

#### Backend

- Seguir convenções do C# e .NET
- Usar async/await para operações assíncronas
- Documentar endpoints complexos
- Escrever testes para novas funcionalidades
- Validar DTOs antes de processar
- Tratar exceções adequadamente

#### Commits

- Usar mensagens claras e descritivas
- Padrão: `tipo: descrição`
  - `feat`: nova funcionalidade
  - `fix`: correção de bug
  - `docs`: documentação
  - `test`: testes
  - `refactor`: refatoração

---

## Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

**Desenvolvido como projeto de estudo de ASP.NET Core Minimal APIs**

**Versão**: 1.0.0

**Última Atualização**: Novembro 2023

---

## Contato

Marcos Neves - [@nevesmarcos42](https://github.com/nevesmarcos42)

Link do Projeto: [https://github.com/nevesmarcos42/Minimal-API](https://github.com/nevesmarcos42/Minimal-API)
