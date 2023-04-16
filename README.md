# :hammer_and_wrench: Fiolet API

The Fiolet API is organized around REST. Our API has predictable resource-oriented URLs, accepts JSON-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

## Endpoint

The API endpoint is `https://api.fiolet.xyz`

## Authentication

Add your API key to the `x-api-key` header of each request.

```
x-api-key: YOUR_API_KEY
```

## Errors

**400 Bad Request** - The request was unacceptable, often due to missing a required parameter.

**401 Unauthorized** - No valid API key provided.

## API Reference

### :money_with_wings: Charges

The Charge object is the core resource of the Fiolet API. It can be used to create and manage payments.

```json
{
  "id": "9m4e2mr0ui3e8a215n4g",
  "name": "Best NFT ever",
  "description": "This is the best NFT ever",
  "amount": "6.9",
  "token": "usd",
  "amount_captured": "6.9",
  "metadata": {
    "customer_id": "1",
    "customer_name": "John Doe",
    "purchase_id": "34",
    "product_id": "2"
  },
  "success_url": "https://fiolet.xyz/success",
  "cancel_url": "https://fiolet.xyz/cancel",
  "status": "new",
  "updated_at": "2021-05-24T12:00:00.000Z",
  "created_at": "2021-05-24T12:00:00.000Z",
  "link": "https://checkout.fiolet.xyz/pay/9m4e2mr0ui3e8a215n4g"
}
```

**Attributes:**

- **name** - The name of the charge (required)
- **description** - The description of the charge
- **pricing_type** - The pricing type of the charge. Can be only `fixed_price` for now (required)
- **amount** - The amount of the charge (required)
- **token** - The token of the charge (required)
- **amount_captured** - The amount captured of the charge (always in USD)
- **metadata** - The metadata of the charge. Avaliable fields (use them as you want):
  - **customer_id** - The ID of the customer
  - **customer_name** - The name of the customer
  - **purchase_id** - The ID of the purchase
  - **product_id** - The ID of the product
- **success_url** - The URL to redirect to after a successful payment
- **cancel_url** - The URL to redirect to after a cancelled payment

#### POST /v1/charges

Create a charge

**Body:**

```json
{
  "name": "Best NFT ever",
  "description": "This is the best NFT ever",
  "amount": "6.9",
  "token": "usd",
  "metadata": {
    "purchase_id": "12"
  },
  "success_url": "https://fiolet.xyz/success",
  "cancel_url": "https://fiolet.xyz/cancel"
}
```

**Response**

```json
{
  "id": "9m4e2mr0ui3e8a215n4g",
  "name": "Best NFT ever",
  "description": "This is the best NFT ever",
  "amount": "6.9",
  "token": "usd",
  "amount_captured": "0",
  "metadata": {
    "purchase_id": "12"
  },
  "success_url": "https://fiolet.xyz/success",
  "cancel_url": "https://fiolet.xyz/cancel",
  "status": "new",
  "updated_at": "2021-05-24T12:00:00.000Z",
  "created_at": "2021-05-24T12:00:00.000Z",
  "link": "https://checkout.fiolet.xyz/pay/9m4e2mr0ui3e8a215n4g"
}
```

#### GET /v1/charges/:id

Retrieve a charge

**Path parameters:**

- **id** - The ID of the charge to retrieve

**Response**

```json
{
  "id": "9m4e2mr0ui3e8a215n4g",
  "name": "Best NFT ever",
  "description": "This is the best NFT ever",
  "pricing_type": "fixed_price",
  "amount": "6.9",
  "token": "usd",
  "amount_captured": "6.9",
  "metadata": {
    "purchase_id": "12"
  },
  "success_url": "https://fiolet.xyz/success",
  "cancel_url": "https://fiolet.xyz/cancel",
  "status": "successed",
  "updated_at": "2021-05-24T12:00:00.000Z",
  "created_at": "2021-05-24T12:00:00.000Z",
  "link": "https://checkout.fiolet.xyz/pay/9m4e2mr0ui3e8a215n4g"
}
```

#### GET /v1/charges

List all Charges

**Query parameters:**

- **limit** - A limit on the number of objects to be returned, between 1 and 100.
- **created** - A filter on the list based on the object created_at field. The value can be a string with an integer Unix timestamp, or it can be a dictionary with the following options **Default is created.lte with now()**:
  - **gt** - Return objects where the created_at is after this timestamp.
  - **gte** - Return objects where the created_at is after or equal to this timestamp.
  - **lt** - Return objects where the created_at is before this timestamp.
  - **lte** - Return objects where the created_at is before or equal to this timestamp.
- **order** - Either asc or desc. **Default is desc**

**Query example:**

```
/v1/charges?limit=10&created.gte=1621872000&order=asc
```

**Response**

```json
[
    {
        "id": "9m4e2mr0ui3e8a215n4g",
        "name": "Best NFT ever",
        "description": "This is the best NFT ever",
        "pricing_type": "fixed_price",
        "amount": "6.9",
        "token": "usd",
        "amount_captured": "0",
        "metadata": {
            "purchase_id": "12",
        },
        "success_url": "https://fiolet.xyz/success",
        "cancel_url": "https://fiolet.xyz/cancel",
        "status": "new",
        "updated_at": "2021-05-24T12:00:00.000Z",
        "created_at": "2021-05-24T12:00:00.000Z",
        "link": "https://checkout.fiolet.xyz/pay/9m4e2mr0ui3e8a215n4g"
    },
    ...
]
```

#### POST /v1/charges/:id/cancel

Cancel a charge

**Path parameters:**

- **id** - The ID of the charge to cancel

**Response**

```json
{
  "id": "9m4e2mr0ui3e8a215n4g",
  "name": "Best NFT ever",
  "description": "This is the best NFT ever",
  "pricing_type": "fixed_price",
  "amount": "6.9",
  "token": "usd",
  "amount_captured": "6.9",
  "metadata": {
    "purchase_id": "12"
  },
  "success_url": "https://fiolet.xyz/success",
  "cancel_url": "https://fiolet.xyz/cancel",
  "status": "canceled",
  "updated_at": "2021-05-24T12:00:00.000Z",
  "created_at": "2021-05-24T12:00:00.000Z",
  "link": "https://checkout.fiolet.xyz/pay/9m4e2mr0ui3e8a215n4g"
}
```

## Webhooks

Webhooks are added to the app and automatically linked to your new charges. A webhook is sent for every status change to all URLs specified in the app.

**Webhook structure**

```json
{
  "t": "entity.status",
  "data": {
    ...
  }
}
```

**Attributes:**

- **t** - field contains the webhook entity and status in the "entity.status" format.

- **data** - field contains the entity itself. For now, we only support the Charge entity.

  Webhook types:

  - "charge.pending"
  - "charge.succeeded"

An example of a webhook when updating the status of a charge:

```json
{
  "t": "charge.succeeded",
  "data": {
    "id": "cgm20fh5gkpr20225h1g",
    "name": "My first charge",
    "description": "My first charge description",
    "amount": "0.01",
    "token": "usd",
    "amount_captured": "0.01",
    "metadata": {
      "customer_id": "12345",
      "customer_name": "John Doe"
    },
    "success_url": "",
    "cancel_url": "",
    "status": "succeeded",
    "updated_at": "2023-04-04T16:04:30.239457+03:00",
    "created_at": "2023-04-04T16:03:58.188801+03:00",
    "intent": {
      "id": "cgm20lh5gkpr20225h20",
      "charge_id": "cgm20fh5gkpr20225h1g",
      "to_address": "0xE108F10f6c5714A862C3fDAEaeccdd22034b4634",
      "from_address": "0x1190Ce8216DF82f7FAacB19b902289abBEeA5EA4",
      "network": "bsc",
      "amount": "0.01",
      "token": "usdt",
      "tx_id": "0x8e8f337ab9d813ceefed53d446ff7a765d89e75fc89b74b6b151032006ee4776",
      "block_number": 27056231,
      "status": "succeeded",
      "updated_at": "2023-04-04T16:04:29.900248+03:00"
    },
    "link": "https://checkout.fiolet.xyz/pay/cgm20fh5gkpr20225h1g"
  }
}
```

## Supported tokens

You can use any of the following tokens to create a charge.

**Fiat (Currency Equivalent):**

- **usd**

**Crypto:**

- **eth**
- **bnb**
- **matic**
- **usdc**
- **usdt**
- **dai**
- **busd**
- **weth**
