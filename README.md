# :hammer_and_wrench: Fiolet API

The Fiolet API is organized around REST. Our API has predictable resource-oriented URLs, accepts JSON-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

## Getting Started
1. Signup through [App](https://app.fiolet.xyz/)
2. Choose **payment methods** (Networks and Tokens)
3. Copy **API key** (already created or create a new one)
4. Set up **webhook** receiving [(API)](https://github.com/fioletxyz/api#webhooks)
5. Create a **Customer** using the [API](https://github.com/fioletxyz/api#customer) **(on a regular basis)**
6. Create and finilize an **Invoice** using the [API](https://github.com/fioletxyz/api#invoice) **(on a regular basis)**
7. Send Customer/Invoice **link** to the user
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

### Customer 
Customer handlers help connect our payment solution and your metering system (e.g. Lago).

**Customer structure**
```json
{
    "id": "cus_cj1veqk41l36cgukp2c0oktX",
    "created_at": "2023-07-28T10:14:18.149804004-07:00",
    "link": "https://checkout.fiolet.xyz/customer/cus_cj1veqk41l36cgukp2c0oktX"
}
```

**Attributes:**

- **id** - unqiue id to connect user between our systems.
- **created_at** - time of creation.
- **link** - link to customer's checkout.

#### POST /v1/customers
Create customer.


**Response**

```json
{
    "id": "cus_cj1veqk41l36cgukp2c0oktX",
    "created_at": "2023-07-28T10:14:18.149804004-07:00",
    "link": "https://checkout.fiolet.xyz/customer/cus_cj1veqk41l36cgukp2c0oktX"
}
```

#### GET /v1/customers/
Get list of customers attached to API key.


**Response**

```json
[
    {
        "id": "cus_ZBNigiqKj2wxFR",
        "created_at": "2023-07-10T03:39:53.942584-07:00",
        "link": "https://checkout.fiolet.xyz/customer/cus_ZBNigiqKj2wxFR"
    },
    {
        "id": "cus_K65ybWw7k1GmdZ",
        "created_at": "2023-07-15T08:19:56.670284-07:00",
        "link": "https://checkout.fiolet.xyz/customer/cus_K65ybWw7k1GmdZ"
    }
]
```

#### GET /v1/customers/:id
Get customer object by customer id.


**Response**

```json
    {
        "id": "cus_ZBNigiqKj2wxFR",
        "created_at": "2023-07-10T03:39:53.942584-07:00",
        "link": "https://checkout.fiolet.xyz/customer/cus_ZBNigiqKj2wxFR"
    }
```

#### DELETE /v1/customers/:id
Delete a customer by id.


**Response**

```json
```

### Invoice
Invoice is what you send when you need to charge your customer.

**Invoice structure**

```json
{
    "id": "in_cj1vq4k41l36cgukp2cgUjTL",
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "10",
    "token": "eth",
    "number": "",
    "status": "draft",
    "fee": "0",
    "created_at": "2023-07-28T10:38:26.402622393-07:00",
    "updated_at": "2023-07-28T10:38:26.402622393-07:00",
    "link": "https://checkout.fiolet.xyz/invoice/in_cj1vq4k41l36cgukp2cgUjTL"
}
```

**Attributes:**
- **id** - id of invoice.
- **customer_id** - id of customer to who it's invoice.
- **amount** - amount to charge.
- **token** - currency.
- **number** - number of invoice if multiple invoices were made.
- **status** - status of the invoice.
     Invoice statuses:
  - "invoice.draft" - invoice after creation before finalizing.
  - "invoice.open" - finalized invoice.
  - "invoice.pending" - invoice in process of paying.
  - "invoice.paid" - paid invoice.
  - "invoice.void" - voided invoice(open only).

- **fee** - fee that has to be payed for proccesing invoice.
- **created_at** - time of creation.
- **updated_at** - time of updation.
- **link** - link to customer's chechkout.

#### POST /v1/invoice
Handler to create invoice.

**Body:**

```json
{
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "10",
    "token": "eth"
}
```

**Response**

```json
{
    "id": "in_cj1vq4k41l36cgukp2cgUjTL",
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "10",
    "token": "eth",
    "number": "",
    "status": "draft",
    "fee": "0",
    "created_at": "2023-07-28T10:38:26.402622393-07:00",
    "updated_at": "2023-07-28T10:38:26.402622393-07:00",
    "link": "https://checkout.fiolet.xyz/invoice/in_cj1vq4k41l36cgukp2cgUjTL"
}
```

#### POST /v1/invoice/:id
Update invoice by its id. Only for `draft` status.

**Body:**

```json
{
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "12",
    "token": "usd",
    "number": "0"
}
```

**Response**

```json
{
    "id": "in_cj2081s41l36cgukp2dgibZy",
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "12",
    "token": "usd",
    "number": "0",
    "status": "draft",
    "fee": "0",
    "created_at": "2023-07-28T11:08:07.609567-07:00",
    "updated_at": "2023-07-28T11:19:28.019762-07:00",
    "link": "https://checkout.fiolet.xyz/invoice/in_cj2081s41l36cgukp2dgibZy"
}
```

#### GET /v1/invoice/:id
Get invoice by id.

**Body:**
```json
```

**Response**

```json
{
    "id": "in_cj2081s41l36cgukp2dgibZy",
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "12",
    "token": "usd",
    "number": "0",
    "status": "draft",
    "fee": "0",
    "created_at": "2023-07-28T11:08:07.609567-07:00",
    "updated_at": "2023-07-28T11:19:28.019762-07:00",
    "link": "https://checkout.fiolet.xyz/invoice/in_cj2081s41l36cgukp2dgibZy"
}
```

#### GET /v1/invoice
Get list of all invoices connected to API key.

**Body:**
```json
```

**Response**

```json
[
    {
        "id": "in_cis17t8eulmigvuu21i0VT2R",
        "customer_id": "cus_cis14l0eulmi4ktj9hig3VON",
        "amount": "10",
        "token": "usd",
        "number": "",
        "status": "new",
        "fee": "0",
        "created_at": "2023-07-19T09:48:53.103233-07:00",
        "updated_at": "2023-07-19T09:48:53.103233-07:00",
        "link": "https://checkout.fiolet.xyz/invoice/in_cis17t8eulmigvuu21i0VT2R"
    },
    {
        "id": "in_cismdsoeulmkhebt58n0D3uk",
        "customer_id": "cus_cis14l0eulmi4ktj9hig3VON",
        "amount": "10",
        "token": "usdt",
        "number": "",
        "status": "draft",
        "fee": "0",
        "created_at": "2023-07-20T09:55:15.011704-07:00",
        "updated_at": "2023-07-20T09:55:15.011704-07:00",
        "link": "https://checkout.fiolet.xyz/invoice/in_cismdsoeulmkhebt58n0D3uk"
    }
]
```

#### DELETE /v1/invoice/:id
Delete invoice by id. Only for `draft` status.

**Body:**
```json
```

**Response**

```json
OK
```

#### POST /v1/invoice/:id/finalize
Change invoice status to `open`. 

**Body:**
```json
```

**Response**

```json
{
    "id": "in_cj20mdc41l38nj1jlos0rONT",
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "10",
    "token": "eth",
    "number": "",
    "status": "open",
    "fee": "0",
    "created_at": "2023-07-28T11:38:45.452712-07:00",
    "updated_at": "2023-07-28T11:38:55.834294-07:00"
}
```

#### POST /v1/invoice/:id/void
Change invoice status to `void`. Only for `open` status.


**Body:**
```json
```

**Response**

```json
{
    "id": "in_cj20mdc41l38nj1jlos0rONT",
    "customer_id": "cus_cj0vujs41l305cf64t0gz07P",
    "amount": "10",
    "token": "eth",
    "number": "",
    "status": "void",
    "fee": "0",
    "created_at": "2023-07-28T11:38:45.452712-07:00",
    "updated_at": "2023-07-28T11:42:10.658287-07:00"
}
```

### Webhooks

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
