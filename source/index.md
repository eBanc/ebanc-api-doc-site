---
title: eBanc API Reference

language_tabs:
  - php: PHP
  - ruby: Ruby
  - shell: Shell

search: false
---

# Introduction

Welcome to the eBanc API! You can use our API to access eBanc API endpoints. That will allow your to submit transactions, 
check the status of a transaction, add customers, update customers, and view customers.

We have language bindings in Shell, Ruby, and PHP! You can view code examples in the dark area to the right, and you can 
switch the programming language of the examples with the tabs in the top right.

The following libraries work with the eBanc API:
 
 * PHP [https://github.com/eBanc/ebanc-php](https://github.com/eBanc/ebanc-php)
 * Ruby [https://github.com/eBanc/ebanc-ruby](https://github.com/eBanc/ebanc-ruby)

# Authentication

> To authorize, use this code:

```php
require_once('Ebanc.php');

$apiKey    = 'keykeykey';
$gatewayId = 'gatewayid';

$ebanc = new Ebanc($apiKey, $gatewayId);
```

```ruby
require 'ebanc'

api_key = 'keykeykey'
gateway_id = 'gatewayid'

ebanc = Ebanc.new(api_key, gateway_id)
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  "Authorization: Token token=\"keykeykey\""
```

> Make sure to replace `keykeykey` with your API key and `gatewayid` with the gateway id you have been assigned.

eBanc uses API keys to allow access to the API. You should have recieved this key during the signup process.

eBanc expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Token token="keykeykey"`

eBanc also uses a gateway id to tell the library which gateway server to connect to. In the shell you simply include this as 
the subdomain that you want to connect to. For example if your gateway id was '1ab2' you would send your API requests to 
https://1ab2.ebanccorp.com

<aside class="notice">
You must replace `keykeykey` with your personal API key and `gatewayid` with the gateway id you have been assigned.
</aside>

# Customers

Customers represent sensitive data that you are giving to eBanc to store. You can then use the UUID of this object to
send transactions through at a later date without needing to store their banking information on your servers. A good use 
for this would be for a billing system that needs to send transactions through on a regular schedule.

## Get All Customers

```php
require_once('Ebanc.php');

$apiKey    = 'keykeykey';
$gatewayId = 'gatewayid';

$ebanc = new Ebanc($apiKey, $gatewayId);
$customers = $ebanc->getCustomers();
```

```ruby
require 'ebanc'

api_key = 'keykeykey'
gateway_id = 'gatewayid'

ebanc = Ebanc.new(api_key, gateway_id)
customers = ebanc.customers
```

```shell
curl "https://<GatewayId>.ebanccorp.com/api/v2/customers" \
  -H "Authorization: Token token=\"keykeykey\""
```

> The above command returns JSON structured like this:

```json
"customers":[
  {
    "uuid":"ae656b00-27d7-0132-54e2-1040f38cff7c",
    "first_name":"Bob",
    "last_name":"Thompson",
    "account_number_last_4":2345,
    "control_number":1817,
    "created_at":"08/26/2014",
    "updated_at":"08/29/2014"
  },
  {
    "uuid":"fe556b12-56d7-0982-59e2-1140118cff7c",
    "first_name":"Steve",
    "last_name":"Johns",
    "account_number_last_4":2345,
    "control_number":1817,
    "created_at":"09/01/2014",
    "updated_at":"09/01/2014"
  },
  {
    "uuid":"ae656b00-27d7-0132-54e2-1040f38cff7c",
    "first_name":"Tamy",
    "last_name":"Anderson",
    "account_number_last_4":2345,
    "control_number":1817,
    "created_at":"09/15/2014",
    "updated_at":"09/25/2014"
  }
]
```

This endpoint retrieves all of your account's customers.

### HTTP Request

`GET https://<GatewayId>.ebanccorp.com/customers`

## Get a Specific Customer

```php
require_once('Ebanc.php');

$apiKey    = 'keykeykey';
$gatewayId = 'gatewayid';

$ebanc = new Ebanc($apiKey, $gatewayId);
$customer = $ebanc->getCustomer('123456789');
```

```ruby
require 'ebanc'

api_key = 'keykeykey'
gateway_id = 'gatewayid'

ebanc = Ebanc.new(api_key, gateway_id)
ebanc.get_customer('73607e90-2bdb-0132-80aa-1040f38cff7c')
```

```shell
curl "https://<GatewayId>.ebanccorp.com/api/v2/customers/<CustomerUUID>" \
  -H "Authorization: Token token=\"keykeykey\""
```

> The above command returns JSON structured like this:

```json
{
  "uuid":"73607e90-2bdb-0132-80aa-1040f38cff7c",
  "first_name":"Tamy",
  "last_name":"Anderson",
  "account_number_last_4":2345,
  "control_number":1817,
  "created_at":"09/15/2014",
  "updated_at":"09/25/2014"
}
```

This endpoint retrieves a specific customer.

### HTTP Request

`GET https://<GatewayId>.ebanccorp.com/customers/<CustomerUUID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the customer to retrieve

## Create New Customer

```php
require_once('Ebanc.php');

$apiKey    = 'keykeykey';
$gatewayId = 'gatewayid';

$ebanc = new Ebanc($apiKey, $gatewayId);

$firstName     = 'Billy';
$lastName      = 'Bob';
$routingNumber = '123456789';
$accountNumber = '123456';
$customer = $ebanc->createCustomer($firstName, $lastName, $routingNumber, $accountNumber);

if($customer){
	echo 'Created customer '.$customer['first_name'].' '.$customer['last_name'].' with the UUID of '.$customer['uuid'];
}else{
	echo 'Error: '.$ebanc->getError();
}
```

```ruby
require 'ebanc'

api_key = 'keykeykey'
gateway_id = 'gatewayid'

ebanc = Ebanc.new(api_key, gateway_id)
customer = ebanc.create_customer(first_name: 'Billy', last_name: 'Bob', account_number: '123456', routing_number: '123456789')

if customer
	puts 'Created custoemr' + customer['first_name'] + ' ' + customer['last_name'] + ' with the UUID of ' + customer['uuid']
else
	puts ebanc.error
end
```

```shell
curl -X POST "https://<GatewayId>.ebanccorp.com/api/v2/customers/?first_name=Billy&last_name=Bob&account_number=123456&routing_number=123456789" \
     -H "Authorization: Token token=\"keykeykey\""

```

> The above command returns JSON structured like this:

```json
{
  "uuid":"73607e90-2bdb-0132-80aa-1040f38cff7c",
  "first_name":"Billy",
  "last_name":"Bob",
  "account_number_last_4":3456,
  "control_number":1817,
  "created_at":"09/25/2014",
  "updated_at":"09/25/2014"
}
```

This endpoint creates a customer.

### HTTP Request

`POST https://<GatewayId>.ebanccorp.com/customers`

### URL Parameters

Parameter | Description
--------- | -----------
first_name | The first name of the new customer
last_name | The last name of the new customer
account_number | Bank account number
account_number | Bank routing number

## Update Existing Customer

```php
require_once('Ebanc.php');

$apiKey    = 'keykeykey';
$gatewayId = 'gatewayid';

$ebanc = new Ebanc($apiKey, $gatewayId);

$uuid          = '73607e90-2bdb-0132-80aa-1040f38cff7c';

//Update only the required fields first_name and last_name
$firstName     = 'Billy';
$lastName      = 'Bob';
$customer = $ebanc->updateCustomer($uuid, $firstName, $lastName);

//Making updates to the account and routing numbers is optional
$routingNumber = '123456789';
$accountNumber = '123456';
$customer = $ebanc->updateCustomer($uuid, $firstName, $lastName, $routingNumber, $accountNumber);

if($customer){
	echo 'Updated customer '.$customer['first_name'].' '.$customer['last_name'].' with the UUID of '.$customer['uuid'];
}else{
	echo 'Error: '.$ebanc->getError();
}
```

```ruby
require 'ebanc'

api_key = 'keykeykey'
gateway_id = 'gatewayid'

ebanc = Ebanc.new(api_key, gateway_id)
#Update only the required fields first_name and last_name
customer = ebanc.update_customer(uuid: '73607e90-2bdb-0132-80aa-1040f38cff7c',first_name: 'Billy', last_name: 'Bob')

#Making updates to the account and routing numbers is optional
customer = ebanc.update_customer(uuid: '73607e90-2bdb-0132-80aa-1040f38cff7c',first_name: 'Billy', last_name: 'Bob', account_number: '123456', routing_number: '123456789')

if customer
	puts 'Created custoemr' + customer['first_name'] + ' ' + customer['last_name'] + ' with the UUID of ' + customer['uuid']
else
	puts ebanc.error
end
```

```shell
#account_number and the routing_number fields are optional
curl --request PATCH "https://<GatewayId>.ebanccorp.com/api/v2/customers/<CustomerUUID>?first_name=Billy&last_name=Bob&account_number=123456&routing_number=123456789" \
     -H "Authorization: Token token=\"keykeykey\""

```

> The above command returns JSON structured like this:

```json
{
  "uuid":"73607e90-2bdb-0132-80aa-1040f38cff7c",
  "first_name":"Billy",
  "last_name":"Bob",
  "account_number_last_4":3456,
  "control_number":1817,
  "created_at":"09/25/2014",
  "updated_at":"09/25/2014"
}
```

This endpoint creates a customer.

### HTTP Request

`PATCH https://<GatewayId>.ebanccorp.com/customers/<CustomerUUID>`

### URL Parameters

Parameter | Description
--------- | -----------
first_name | The first name of the new customer
last_name | The last name of the new customer
account_number (optional) | Bank account number
account_number (optional) | Bank routing number

# Transactions

A transaction represents the impact that this action will have on this current account.

Once a trasaction has been created, you can also get updates on the current status of the transaction. The possible 
statuses of a transaction are "recieved", "processing", "paid", and "returned". If the status is set to "returned" you 
can also get the returned reason of the object.

Status | Description
--------- | -----------
recieved | The eBanc gateway has successfully recieved this transaction.
processing | This transaction has started the processing cycle. The transaction information has been sent to the other account's bank.
paid | The transaction has successfully gone through and your account has been paid. You should be able to see the funds show up in your bank account within 24 hours.
returned | We were unable to secure the funds for this transaction. There are a number of reasons that this can happen. You can access this information in the "returned_reason" part of the transaction object.

