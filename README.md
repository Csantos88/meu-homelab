# Meu Homelab

Este repositório contém a infraestrutura como código (IaC) do meu ambiente de Homelab, orquestrado utilizando **Docker Compose**. O ambiente foi projetado para ser modular e agrupa uma vasta gama de aplicações e serviços indispensáveis, focando em automação, produtividade, orquestração, gestão de dados e ferramentas empresariais.

## 🚀 Serviços e Aplicações

A infraestrutura está dividida nas seguintes categorias:

### 🌐 Dashboard

- **[Heimdall](https://heimdall.site/):** Painel de controle central para gerenciar e acessar facilmente todos os serviços e links do homelab.

### 🗄️ Bancos de Dados

- **MySQL 8.0:** Utilizado como banco de dados principal do ERP Dolibarr.
- **PostgreSQL 15:** Banco de dados relacional para serviços como Sure, n8n e Apache Airflow.
- **MongoDB 6.0:** Banco de dados NoSQL.
- **Redis:** Banco de dados em memória para cache e filas (suporte ao Sure, etc.).

### 💰 Finanças (Finance App)

- **[Sure](https://github.com/we-promise/sure):** Aplicação de gestão financeira pessoal e empresarial.

### ⚡ Low-Code & Automação

- **[Appsmith](https://www.appsmith.com/):** Plataforma para criação e desenvolvimento ágil de ferramentas e dashboards internos.
- **[Budibase](https://budibase.com/):** Construtor de aplicações web de baixo código.
- **[n8n](https://n8n.io/):** Ferramenta avançada para automação de fluxos de trabalho baseada em nós.

### ⚙️ CI/CD & Orquestração

- **[Jenkins](https://www.jenkins.io/):** Servidor de automação líder do mercado para integração e entrega contínuas (CI/CD).
- **[Apache Airflow](https://airflow.apache.org/):** Plataforma para agendamento, orquestração e monitoramento de fluxos de trabalho e pipelines de dados (Webserver e Scheduler configurados através de imagem customizada).

### 🏢 Business Apps

- **[Dolibarr](https://www.dolibarr.org/):** Sistema de ERP e CRM moderno e fácil de utilizar para gestão de negócios.

### 📊 Data Science (Big Data)

- **Apache Hadoop (HDFS):** Ecossistema de armazenamento de dados distribuídos (Namenode e Datanode).
- **Apache Spark:** Sistema de computação em cluster para processamento de Big Data (Arquitetura Master e Worker limitados a 4G de RAM e 4 Cores para otimização no homelab).

---

## 🛠️ Pré-requisitos

Antes de executar o ambiente, certifique-se de que sua máquina atenda aos seguintes requisitos:

1. **Docker e Docker Compose** instalados.
2. **Rede externa (Nginx Proxy):** O arquivo de composição espera que a rede `nginx-proxy-network` já exista. Se você usa um proxy reverso (como Nginx Proxy Manager), crie-a antes executando:

   ```bash
   docker network create nginx-proxy-network
   ```

   *Nota: Como os serviços estão conectados a esta rede e não expõem portas diretamente no `docker-compose.yml`, o roteamento para acesso web às aplicações deve ser configurado diretamente no seu proxy reverso (ex: Nginx Proxy Manager, Traefik).*

3. **Diretório customizado do Airflow:** É necessário existir um diretório `./airflow-custom` com o `Dockerfile` que define a imagem estendida do Airflow (para instalação de dependências, bibliotecas Python extras, etc.).

## 🔐 Variáveis de Ambiente

Muitos serviços dependem de credenciais seguras para inicializar. Você precisa criar um arquivo `.env` na raiz do diretório `meu-homelab` com as seguintes chaves preenchidas:

```env
MYSQL_ROOT_PASSWORD=sua_senha_mysql
POSTGRES_USER=seu_user_pg
POSTGRES_PASSWORD=sua_senha_pg
POSTGRES_DB=seu_db_sure
DB_HOST=db_postgres
SECRET_KEY_BASE=sua_secret_key_ruby
MONGO_ROOT_PASSWORD=sua_senha_mongo
AIRFLOW_DB_CONN=postgresql+psycopg2://<user>:<password>@db_postgres/airflow
```

*(Substitua os valores com as credenciais reais do seu ambiente e não comite este arquivo no git).*

## 🚀 Como Executar

Para subir todos os contêineres e redes:

```bash
# Inicia todos os serviços em segundo plano e constrói as imagens customizadas (ex: Airflow)
docker-compose up -d --build
```

Para verificar o log de um serviço específico (ex: Airflow):

```bash
docker logs -f homelab_airflow
```

Para desligar e remover todos os contêineres em execução:

```bash
docker-compose down
```

## 💾 Armazenamento e Volumes

Os dados dos bancos de dados, configurações de ferramentas e recursos das aplicações são garantidos através de Docker Volumes nomeados (como `mysql_data`, `postgres_data`, `jenkins_data`, `namenode_data`, etc.). Isso assegura que **não** ocorrerá perda de dados ao destruir ou recriar os contêineres, mantendo a persistência sempre funcional.
