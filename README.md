# Baozi Store — API REST

API REST simples para o controle de clientes, produtos e pedidos de uma pequena loja de pão chinês.
Atividade prática da disciplina de Desenvolvimento Web Back-End.

## Tecnologias

- Java 21
- Spring Boot 4.2.0 (Web MVC, Data JPA, Validation)
- MySQL 8.4
- Docker Compose

## Estrutura

```
src/main/java/br/com/baozistore
├── model        entidades JPA (Cliente, Produto, Pedido)
├── repository   interfaces do Spring Data JPA
└── controller   endpoints REST
```

## Como executar

### Com Docker (não precisa de Java, Maven nem MySQL instalados)

```bash
docker compose up
```

A API fica em `http://127.0.0.1:8080`. Na primeira execução as imagens e dependências são baixadas.

Para parar:

```bash
docker compose down
```

### Com Java 21 instalado

Suba apenas o banco e execute a aplicação com o Maven Wrapper:

```bash
docker compose up -d db
```

```bash
./mvnw spring-boot:run
```

Para usar outro MySQL ou MariaDB, defina as variáveis `DB_URL`, `DB_USER` e `DB_PASSWORD`.
As tabelas são criadas automaticamente pelo Hibernate.

## Entidades

| Entidade | Campos |
|---|---|
| Cliente | `id` (Long), `nome` (String), `clienteDesde` (LocalDate) |
| Produto | `id` (Long), `nome` (String), `preco` (BigDecimal), `estoque` (Boolean) |
| Pedido | `id` (Long), `clienteId` (Long), `produtoId` (Long), `quantidade` (Integer) |

## Endpoints

As três entidades seguem o mesmo padrão, em `/clientes`, `/produtos` e `/pedidos`.

| Método | Caminho | Ação | Respostas |
|---|---|---|---|
| POST | `/{recurso}` | Criar | 201, 400 |
| GET | `/{recurso}` | Listar todos | 200 |
| GET | `/{recurso}/{id}` | Consultar por ID | 200, 404 |
| PUT | `/{recurso}/{id}` | Atualizar | 200, 400, 404 |
| DELETE | `/{recurso}/{id}` | Apagar | 204, 404, 409 |

Regras:

- Todos os campos são obrigatórios; preço e quantidade devem ser maiores que zero.
- Um pedido só pode ser registrado para um cliente e um produto existentes (400).
- Um cliente ou produto com pedidos não pode ser apagado (409).

## Exemplos de corpo (JSON)

```json
{ "nome": "Maria Silva", "clienteDesde": "2026-01-15" }
```

```json
{ "nome": "Baozi Tradicional", "preco": 8.50, "estoque": true }
```

```json
{ "clienteId": 1, "produtoId": 1, "quantidade": 5 }
```
