# Order API

## Visão Geral

A **Order API** é responsável por gerenciar os pedidos de usuários na plataforma. Esta API permite a criação de novos pedidos, listagem de todos os pedidos do usuário atual e visualização dos detalhes de pedidos específicos.

Todas as requisições exigem autenticação por **JWT Token**, sendo consumidas exclusivamente via **API Gateway**.

---

## Endpoints de Pedido

### `POST /order`

Cria um novo pedido para o usuário autenticado.

#### Requisição:

```json
{
    "items": [
        {
            "idProduct": "0195abfb-7074-73a9-9d26-b4b9fbaab0a8",
            "quantity": 2
        },
        {
            "idProduct": "0195abfe-e416-7052-be3b-27cdaf12a984",
            "quantity": 1
        }
    ]
}
```

#### Resposta:

- `201 Created` – Pedido criado com sucesso.
- `400 Bad Request` – Dados inválidos.

---

### `GET /order`

Retorna todos os pedidos do usuário autenticado.

#### Resposta:

```json
[
    {
        "id": "0195ac33-73e5-7cb3-90ca-7b5e7e549569",
        "date": "2025-09-01T12:30:00",
        "total": 26.44
    },
    {
        "id": "0195ac33-cbbd-7a6e-a15b-b85402cf143f",
        "date": "2025-10-09T03:21:57",
        "total": 18.6
    }
]
```

- `200 OK`

---

### `GET /order/{id}`

Retorna os detalhes de um pedido específico. O pedido deve pertencer ao usuário autenticado.

#### Resposta:

```json
{
    "id": "0195ac33-73e5-7cb3-90ca-7b5e7e549569",
    "date": "2025-09-01T12:30:00",
    "items": [
        {
            "id": "01961b9a-bca2-78c4-9be1-7092b261f217",
            "product": {
                "id": "0195abfb-7074-73a9-9d26-b4b9fbaab0a8"
            },
            "quantity": 2,
            "total": 20.24
        },
        {
            "id": "01961b9b-08fd-76a5-8508-cdb6cd5c27ab",
            "product": {
                "id": "0195abfe-e416-7052-be3b-27cdaf12a984"
            },
            "quantity": 10,
            "total": 6.2
        }
    ],
    "total": 26.44
}
```

- `200 OK` – Pedido encontrado.
- `404 Not Found` – Pedido não pertence ao usuário ou não existe.

---

## Funcionalidades Adicionais

Funcionalidades extras recomendadas incluem:

- **Validações de entrada** robustas.
- **Autorização por papel**:
  - `admin`: pode gerenciar produtos.
  - `user`: pode apenas criar e consultar pedidos.
- **Tratamento de erros** com mensagens apropriadas.
- **Busca de produtos por nome** (relevante para interfaces de pedidos).

---

## Integração com a Plataforma

### Fluxo de Requisição:

```
Usuário → Gateway → Order API → Banco de Dados
```

### Componentes Envolvidos:

- **Gateway**: Encaminha e autentica as requisições.
- **Auth Service**: Valida tokens e autoriza ações.
- **Order API**: Processa e gerencia os pedidos.
- **Banco de Dados**: Persistência dos pedidos e itens.
- **Product API**: Utilizada para validar produtos nos pedidos.

---

## Segurança

- Requisições via HTTPS.
- JWT Token obrigatório no header.
- Validação de propriedade do pedido (`order.user_id == authenticated_user.id`).

---

## Observabilidade (Nice to Have)

Sugestões de boas práticas:

- Logs estruturados por pedido.
- **Métricas com Prometheus**.
- **Painéis de monitoramento com Grafana**.
- **Swagger/OpenAPI** com SpringDoc ou FastAPI Docs.

---

## Considerações Finais

- O modelo de autorização garante que apenas o dono do pedido possa visualizá-lo.
- A Order API deve ser extensível para lidar com diferentes formas de pagamento no futuro.
- Ideal para compor microserviços orientados a domínio em uma arquitetura distribuída.

---
