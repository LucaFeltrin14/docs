# Product API

## Visão Geral

A **Product API** é um serviço REST que faz parte de uma plataforma de loja virtual. Esta API permite a gestão de produtos e pedidos (**orders**), estando integrada ao restante da infraestrutura por meio de um **API Gateway** e sistema de autenticação.

Todas as requisições exigem autenticação via **JWT**.

---

## Endpoints de Produto

### `POST /product`

Cria um novo produto.

#### Requisição:

```json
{
    "name": "Tomato",
    "price": 10.12,
    "unit": "kg"
}
```

#### Resposta:

- `201 Created` – Produto criado com sucesso.
- `400 Bad Request` – Dados inválidos.

---

### `GET /product`

Retorna todos os produtos cadastrados.

#### Resposta:

```json
[
    {
        "id": "0195abfb-7074-73a9-9d26-b4b9fbaab0a8",
        "name": "Tomato",
        "price": 10.12,
        "unit": "kg"
    },
    {
        "id": "0195abfe-e416-7052-be3b-27cdaf12a984",
        "name": "Cheese",
        "price": 0.62,
        "unit": "slice"
    }
]
```

- `200 OK`

---

### `GET /product/{id}`

Busca um produto pelo seu ID.

#### Resposta:

```json
{
    "id": "0195abfb-7074-73a9-9d26-b4b9fbaab0a8",
    "name": "Tomato",
    "price": 10.12,
    "unit": "kg"
}
```

- `200 OK` – Produto encontrado.
- `404 Not Found` – Produto não encontrado.

---

### `DELETE /product/{id}`

Remove um produto pelo seu ID.

- `204 No Content` – Produto deletado com sucesso.
- `404 Not Found` – Produto não encontrado.

---

## Funcionalidades Adicionais

A API pode ser expandida com os seguintes recursos:

- **Busca por nome com filtro "like"**.
- **Autorização por papel**:
  - `admin`: pode criar, atualizar e deletar produtos.
  - `user`: pode apenas criar pedidos.
- **Validações de entrada** para garantir dados corretos.
- **Tratamento de erros** com mensagens claras.

---

## Integração com a Plataforma

### Fluxo:

```
Usuário → Gateway → Product API → Banco de Dados
```

### Componentes Envolvidos:

- **Gateway** – Repassa requisições e autentica o token JWT.
- **Auth Service** – Serviço de autenticação e autorização.
- **Account Service** – Gestão de identidade do usuário.
- **Banco de Dados** – Persistência dos produtos e pedidos.
- **API Externa** – (opcional, para integração com catálogos ou ERP externos).

---

## Segurança

- Requisições HTTPS obrigatórias.
- Autenticação JWT.
- Autorização baseada em função do usuário (admin/user).

---

## Observabilidade (Nice to Have)

Recomenda-se implementar:

- **Logs estruturados**.
- **Métricas com Prometheus**.
- **Visualização com Grafana**.

---

## Recomendação Técnica

- Utilizar **Redis** como banco em memória para microserviço de produto.
- Gerar documentação automática com **Swagger** via **SpringDoc** ou equivalente (se Java).
- Aplicar testes de integração com mocks de autenticação e banco.

---
