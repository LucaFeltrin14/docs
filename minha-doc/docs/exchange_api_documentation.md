# Exchange API

## Visão Geral

A **Exchange API** é um serviço REST desenvolvido com **FastAPI** que permite a conversão de valores entre diferentes moedas. Ela integra-se com uma API externa para obter as taxas de câmbio em tempo real, fornecendo tanto a taxa de **compra** quanto de **venda**.

A API é parte de uma plataforma maior, sendo consumida através de um **API Gateway**, e requer **autenticação** para o uso.

---

## Endpoints

### `GET /exchange/{from}/{to}`

Consulta a taxa de câmbio atual entre duas moedas específicas.

#### Parâmetros:

| Parâmetro | Tipo   | Descrição                      |
|-----------|--------|--------------------------------|
| `from`    | string | Código da moeda de origem (ex: USD) |
| `to`      | string | Código da moeda de destino (ex: EUR) |

#### Exemplo de Requisição:

```
GET /exchange/USD/EUR
Authorization: Bearer <token>
```

#### Exemplo de Resposta:

```json
{
    "sell": 0.82,
    "buy": 0.80,
    "date": "2021-09-01 14:23:42",
    "id-account": "0195ae95-5be7-7dd3-b35d-7a7d87c404fb"
}
```

#### Código de Status:

- `200 OK` – Resposta bem-sucedida com as taxas de câmbio.
- `401 Unauthorized` – Caso o usuário não esteja autenticado.
- `500 Internal Server Error` – Falha na comunicação com a API externa ou outro erro interno.

---

## Integração com API Externa

A API consome dados de taxas de câmbio de uma fonte externa. As APIs sugeridas para isso são:

- [AwesomeAPI](https://docs.awesomeapi.com.br/)
- [ExchangeRate-API](https://www.exchangerate-api.com/)
- [Open Exchange Rates](https://openexchangerates.org/)
- [CurrencyLayer](https://currencylayer.com/)

> **Dica**: A biblioteca Python `requests` é utilizada para fazer chamadas HTTP à API de terceiros.

---

## Autenticação

Para utilizar esta API, o cliente precisa estar autenticado via **JWT Token**. O token é validado pelo sistema de autenticação central da plataforma através do Gateway.

Exemplo de uso no cabeçalho da requisição:

```http
Authorization: Bearer <token>
```

---

## Arquitetura

A Exchange API faz parte de uma arquitetura modular com múltiplos serviços integrados. Abaixo, um resumo da interação entre os componentes:

### Fluxo de Requisição:

```
Usuário -> Gateway -> Exchange API -> 3rd-Party API (Internet)
```

### Componentes Envolvidos:

- **Gateway**: Encaminha requisições, aplica políticas de segurança e autenticação.
- **Auth Service**: Responsável por autenticação e autorização dos usuários.
- **Account Service**: Associa o `id-account` ao usuário autenticado.
- **Exchange API**: Realiza a conversão de moedas e consulta externa.
- **Banco de Dados**: Pode ser utilizado para persistir logs ou histórico de conversões (opcional).
- **API Externa**: Fonte de dados de câmbio em tempo real.

---



## Segurança

- Requisições devem ser feitas via HTTPS.
- Autenticação obrigatória com **token JWT**.
- É recomendado limitar o número de requisições por usuário (rate limit).

---

## Considerações Finais

- Este serviço é essencial para aplicações financeiras que demandam informações de câmbio atualizadas.
- A confiabilidade da API depende da disponibilidade da fonte de dados externa.
- A estrutura modular facilita a manutenção e escalabilidade do serviço.

---
