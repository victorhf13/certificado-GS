###README

### Pré-requisitos

Antes de rodar a aplicação, é necessário garantir que as seguintes ferramentas estejam instaladas:

- *Node.js*: Para rodar os servidores da API e Worker.
- *Docker*: Para conteinerizar a aplicação e orquestrar os serviços com Docker Compose.

### Etapas de Configuração

1. *Clone o repositório*:

   ```bash
   git clone <https://github.com/victorhf13/certificado-GS.git>
   cd certificado-GS
   ```

2. *Configuração de variáveis de ambiente*:

   Crie arquivos .env nas pastas /api e /worker com as seguintes variáveis:

   **Arquivo .env para a API**:

   ```plaintext
   DATABASE_HOST=db
   DATABASE_USER=user
   DATABASE_PASSWORD=pass
   DATABASE_NAME=certificates
   RABBITMQ_HOST=rabbitmq
   REDIS_HOST=redis
   ```

   **Arquivo .env para o Worker**:

   plaintext
   DATABASE_HOST=db
   DATABASE_USER=user
   DATABASE_PASSWORD=pass
   DATABASE_NAME=certificates
   RABBITMQ_URL=amqp://rabbitmq:5672
   REDIS_URL=redis://redis:6379
   

3. *Inicie a aplicação com Docker Compose*:

   Após configurar as variáveis de ambiente, execute o comando abaixo para iniciar todos os serviços:

   bash
   docker-compose up --build
   

---

## Como Utilizar a API

### Endpoint Principal

- *POST /api/v1/certificate*: Envia os dados do aluno e do curso para emissão de certificado.
- http://localhost:3000/api/v1/certificate


### Headers no Postman

Adicione o header *Content-Type* com o valor *application/json*.

#### Exemplo de corpo da requisição (JSON):


```json
{
    "nome_aluno": "Andre Alves",
    "nacionalidade": "Brasileiro",
    "estado": "SP",
    "data_nascimento": "2004-01-01",
    "data_conclusao": "2024-11-13",
    "rg": "987654321",
    "nome_curso": "Sistemas de informação",
    "carga_horaria": 2000
}
```

#### Exemplo cURL de chamada:
```curl
curl -X POST http://localhost:3000/api/v1/certificate \
-H "Content-Type: application/json" \
-d '{
    "nome_aluno": "Andre Alves",
    "nacionalidade": "Brasileiro",
    "estado": "SP",
    "data_nascimento": "2004-01-01",
    "data_conclusao": "2024-11-13",
    "rg": "987654321",
    "nome_curso": "Sistemas de informação",
    "carga_horaria": 2000
}'
```
---

## Detalhes dos Serviços 📡

### API Service

- *Responsabilidade*: Recebe as requisições de emissão de certificados, valida os dados e enfileira os pedidos no RabbitMQ.
- *Localização*: Diretório /api.

### Worker Service 👨‍🏭

- *Responsabilidade*: Consome as mensagens da fila RabbitMQ, gera o PDF do certificado com base no template HTML e atualiza o status da solicitação no banco de dados.
- *Localização*: Diretório /worker.
- *Template de PDF*: O modelo HTML utilizado para gerar o certificado está na pasta /worker/templates.

### Banco de Dados 🗄

- *Função*: Armazena os dados dos alunos e registra o status da geração de cada certificado.
- *Banco de Dados: **PostgreSQL*.
- *Configuração*: Configurado via Docker Compose.

### RabbitMQ 🐰

- *Função*: Fila de mensagens que organiza as requisições de emissão de certificados, garantindo que sejam processadas de forma assíncrona.
- *Configuração*: Configuração no docker-compose.yml.

### Redis ⚡

- *Função*: Cache para armazenar os PDFs gerados, melhorando o desempenho e a resposta para consultas repetidas.
- *Configuração*: Configuração no docker-compose.yml.

---

## Fluxo de Uso 🚶‍♀➡🖨

1. *Envie uma requisição POST* para /api/v1/certificate com os dados do aluno e do curso.
2. A *API processa e enfileira* o pedido para processamento.
3. O *Worker* consome a fila, gera o PDF do certificado e atualiza o banco de dados com o status da emissão.
4. O *PDF gerado é armazenado em cache no Redis*, permitindo acessos rápidos em futuras consultas.

---

## Tecnologias e Ferramentas 🛠

- *Node.js*: Framework para execução da API e Worker.
- *Express*: Framework para criação da API.
- *PostgreSQL*: Banco de dados relacional para persistência dos dados.
- *RabbitMQ*: Sistema de fila de mensagens para garantir a comunicação assíncrona.
- *Redis*: Sistema de cache para melhorar o desempenho de leitura dos PDFs gerados.
- *Docker*: Ferramenta de conteinerização e orquestração de serviços.
- *Puppeteer*: Biblioteca para geração de PDFs a partir de templates HTML.

---

## Desenvolvimento Local 💻

1. *Instale as dependências*:

   Navegue até as pastas /api e /worker e execute:

   ```bash
   npm install
   ```

2. *Inicie a API e o Worker localmente* para testes:

   ```bash
   npm start
   ```

3. *Testes e ajustes: Utilize ferramentas como **Postman* para testar os endpoints da API e verificar a geração dos certificados.

---

## Integrantes

- *RM93598* - VICTOR HENRIQUE FERNANDES
- *RM76172* - LEONARDO MINNITI CANDIDO DA SILVA

---