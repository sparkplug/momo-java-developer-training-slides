---


# Speaker

### Moses Mugisha

- Author of `mtn-momo` library
- Senior Software Engineer At Andela


---

### What is an API?

- Application Programming Interface
- interface that a software program presents to other programs, to humans & internet
- mostly intended to be understood and used by humans writing those other programs

---

### Examples Of APIS

- Momo API ;)
- GITHUB API
- Facebook graph API

---

### What makes a great API
- Usability
- Scalability 
- Performance
- Documentation and developer resources

---

### API Paradigms

- REST API
- RPC
- GraphQL
- Websockets
- Webhooks

---

### Which can be Divided into

 
- Request/Response APIs
- Event Driven APIs

---
### Request/Response APIS 

- APIs define a set of endpoints
- Clients make HTTP requests for data to those endpoints and the server returns responses
- The response is typically sent back as JSON or XML or binary.
          - Examples REST, GraphQL,RPC

---

### Event Driven APIS
- An API will send  a message to the configured URL when something happens
- Request/Response APIs can implement this through continuous polling of the server.

       - Examples Websockets, webhooks
 
---

### REST APIs
- Representational State Transfer (REST) 
- Its about resources
- A resource is an entity that can be identified, named,addressed, or handled on the web. 
- REST APIs expose data as resources and use standard HTTP methods to represent CRUD
Create, Read,Update, and Delete (CRUD) transactions against these resources

---

### General Guidelines for REST

- Resources are part of URLs, like /users .

- For each resource, two URLs are generally implemented: 

          - one   for the collection, like /users ,
          - and one for a specific element, like /users/U123 .
          
---          
### Rules Cont ....

- Nouns are used instead of verbs for resources. 

       For example, instead of /getUserInfo/U123  , use /users/U123 
       
 - HTTP methods like GET , POST , UPDATE , and DELETE inform the
    server about the action to be performed
    
    
---

### Rules cont ...

- Different HTTP methods invoked on the same URL provide different functionality

      POST -> for creating new resources
      GET -> for retriving a resource
      DELETE -> for deleting a resource
      PUT  -> for replacing a resource
      PATCH -> for partial update of a resource

---
### What MTN Mobile Money provides?

- sim card as a financial account
- sending and receiving money
- **withdraw and deposit through agents**
- payments (utilities, momo pay, online)
- network services (airtime, bundles)
- **remittances**

---

### with the Open API
 - Your software is a virtual agent

- withdraw from an account with the Collections API
- deposit with Disbursements API
- send money abroad with the Remittances API (NOT COVERED)
- ability to collect payments using MoMo Pay (NOT COVERED)

---

###  API Archtecture

- Leverages Azure API management
- To understand how the API works, we need to understand azure API management

---
### MOMO API Archtecture

![Logo](assets/img/api_gateway.jpg)

---

## Archtecture Cont ...

API Gateway and the portal. 
The portal provides two different experiences, publisher and developer.

---

### API Gateway

 - API Gateway is the core engine
 - receives payment requests, processes them, connects to the backend service,
    and responding to requests.
    
  ---

### Products

- Products are the consumable packages in Azure API Management. 
- Products contain APIs, and one product can have multiple APIs.
- Also contain usage quota and the terms of use.

![Logo](assets/img/product-subscription.jpg)

---

### Products

![Logo](assets/img/products.jpg)
---

### Subscriptions
- Primary or secondar and are interchangable

- Subscription Keys are associated with a single prodduct 

- The subscription keys act as a first-level security feature, but more importantly, 
they are used to track developers, usage, and policy configurations.
---
### Subscriptions ....

- Developers can also register their applications using the developer portal
- A developer can have more than one subscription to each product. 
- Each subscription is identified by a subscription key, and each subscription key is scoped to a product.
 - A developer cannot use a single subscription key for two different products

---
### Developer Account

- Sign up on https://momodeveloper.mtn.com/

- Subscribe to Collections and Disbursement and obtain primary key for each

---


### Installation

Add the latest version of the library to your project using pip:


```sh
 pip install mtnmomo
```

This will install

- the latest `mtnmomo` as a dependency
- the `mtnmomo` command line tool

---


## Sandbox credentials

Generate sandbox credentials using the command line tool;

```sh
$ mtnmomo --provider example.com --key 028b71f923f24df9a3d9fe90a6453
Here is your User Id and API secret : {'apiKey': 'b0431db58a9b41faa8f5860230xxxxxx',
'UserId': '053c6dea-dd68-xxxx-xxxx-c830dac9f401'}
```

- provider is your application's domain

- Primary key is your subscription key from momodeveloper account

- You will get a user secret and user id which we will use later

- We have to do this separately for collections and disbursement

---

```sh
$ mtnmomo --provider example.com --key 028b71f923f24df9a3d9fe90a6453
Here is your User Id and API secret : {'apiKey': 'b0431db58a9b41faa8f5860230xxxxxx', 
'UserId': '053c6dea-dd68-xxxx-xxxx-c830dac9f401'}
```

---


### Configuration

- Best practice to configure as environment variables

```python
config = {
   "ENVIRONMENT": os.environ.get("ENVIRONMENT"), 
   "BASE_URL": os.environ.get("BASE_URL"), 
   "CALLBACK_HOST": os.environ.get("CALLBACK_HOST"), # Mandatory.
   "COLLECTION_PRIMARY_KEY": os.environ.get("COLLECTION_PRIMARY_KEY"), 
   "COLLECTION_USER_ID": os.environ.get("COLLECTION_USER_ID"),
   "COLLECTION_API_SECRET": os.environ.get("COLLECTION_API_SECRET"),
   "REMITTANCE_USER_ID": os.environ.get("REMITTANCE_USER_ID"), 
   "REMITTANCE_API_SECRET": os.environ.get("REMITTANCE_API_SECRET"),
   "REMITTANCE_PRIMARY_KEY": os.envieon.get("REMITTANCE_PRIMARY_KEY"),
   "DISBURSEMENT_USER_ID": os.environ.get("DISBURSEMENT_USER_ID"), 
   "DISBURSEMENT_API_SECRET": os.environ.get("DISBURSEMENTS_API_SECRET"),
   "DISBURSEMENT_PRIMARY_KEY": os.environ.get("DISBURSEMENT_PRIMARY_KEY"), 
}
```


---

class: center, middle

### For maximum security, implement the integration with MTN in your backend

This way, you will not need any secret keys in your client

---


### Collections

Withdraw money from your customer's account

---

### Initializing collections

```python
import os
from mtnmomo.collection import Collection

client = Collection({
        "COLLECTION_USER_ID": os.environ.get("COLLECTION_USER_ID"),
        "COLLECTION_API_SECRET": os.environ.get("COLLECTION_API_SECRET"),
        "COLLECTION_PRIMARY_KEY": os.environ.get("COLLECTION_PRIMARY_KEY"),
    })
```

---
### Requesting a payment

- Call `requestToPay`, it returns a transaction id
- You can store the transaction id for later use

```python
client = Collection({
    "COLLECTION_USER_ID": os.environ.get("COLLECTION_USER_ID"),
    "COLLECTION_API_SECRET": os.environ.get("COLLECTION_API_SECRET"),
    "COLLECTION_PRIMARY_KEY": os.environ.get("COLLECTION_PRIMARY_KEY"),
})

try:
    transaction_ref = client.requestToPay(
        mobile="256772123456", 
        amount="600",
        external_id="123456789",
        payee_note="dd", 
        payer_message="dd",
        currency="EUR")
except MomoError e:
    print(e)
```

---


### Requesting a payment

- In sandbox, use EUR as the currency ¯\\\_(ツ)\_/¯
- In production, use the currency of your country
- Use reference of your database transaction record as `externalId`
- `payerMessage` appears on your customer's statement
- `payeeNote` appears on your statement (as a virtual agent)

---


### But the payment is not yet complete

As an agent, you cannot exchange your goods until you are sure the payment has transferred to your account

So how can you be sure that the transaction has been completed?

---


### Polling

- Before exchanging goods, call `getTransaction` with the transaction id every few seconds until it succeeds or fails
- This technique is known as polling

---


### Callback

- if you do not want to poll, you can setup an API endpoint to receive requests from MTN when the transaction status changes,
- you can pass the endpoint url as part of `requestToPay`
- your endpoint should be called when the transaction fails or succeeds
- Note that callbacks do not currently work in the sandbox 

---


### Errors

- a transaction can fail immediately if;
  - credentials are incorrect/invalid/expired
  - the provided parameters are invalid (depending on the environment)
  - the phone number is not registered for mobile money
  - the customer has insufficient balance
- a transaction can fail eventually if;
  - the customer cancels the transaction
  - transaction times out

---


### Disbursements

Deposit money to a mobile money account

---


### Initializing disbursements

```python
import os
from mtnmomo.collection import Disbursement

client = Disbursement({
    "DISBURSEMENT_USER_ID": os.environ.get("DISBURSEMENT_USER_ID"),
    "DISBURSEMENT_API_SECRET": os.environ.get("DISBURSEMENT_API_SECRET"),
    "DISBURSEMENT_PRIMARY_KEY": os.environ.get("DISBURSEMENT_PRIMARY_KEY"),
})
```

NOTE: remember to use a generate new credentials using a disbursements primary key

---


### Making a payment

- Call `transfer`, it returns  a transaction id or fails with  an error

```python
import os
from mtnmomo.collection import Disbursement

client = Disbursement({
    "DISBURSEMENT_USER_ID": os.environ.get("DISBURSEMENT_USER_ID"),
    "DISBURSEMENT_API_SECRET": os.environ.get("DISBURSEMENT_API_SECRET"),
    "DISBURSEMENT_PRIMARY_KEY": os.environ.get("DISBURSEMENT_PRIMARY_KEY"),
})

client.transfer(amount="600", mobile="256772123456", external_id="123456789", payee_note="dd",      payer_message="dd", currency="EUR")
```

---


### Making a payment

- Same as collections;
  - In sandbox, use EUR as the currency ¯\\\_(ツ)\_/¯
  - In production, use the currency of your country
  - Use a reference of your database record as `externalId`
- `payerMessage` appears on your statement
- `payeeNote` appears on the receiver's statement

---


### Making a payment

- Since there is no approval process for disbursements, it safe to assume they complete immediately
- For 100% certainty, you can poll using `disbursements.getTransaction` or use the callback
- Error handling is same as collections
