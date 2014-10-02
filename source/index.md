---
title: eBanc API Reference

language_tabs:
  - php: PHP
  - ruby: Ruby
  - shell: Shell

toc_footers:
  - <a href='mailto: info@ebanccorp.com'>Contact us at info@ebanccorp.com</a>

search: false
---

# Introduction

Welcome to the eBanc API! You can use our API to access eBanc API endpoints. That will allow your to submit transactions, 
check the status of a transaction, add customers, update customers, and view customers.

We have language bindings in Shell, Ruby, and PHP! You can view code examples in the dark area to the right, and you can 
switch the programming language of the examples with the tabs in the top right.

The following libraries work with the eBanc API:
 
 * PHP http://blahblah
 * Ruby [https://github.com/eBanc/ebanc-ruby](https://github.com/eBanc/ebanc-ruby)

# Authentication

> To authorize, use this code:

```ruby
#If you are using bundler include 'ebanc' into your gemfile
#otherwise run 'gem install ebanc'
require 'ebanc'

ebanc_client = Ebanc::APIClient.new('keykeykey', 'gatewayid')
```

```php
import 'eBanc'

api = eBanc.authorize('keykeykey', 'gatewayid')
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

# Kittens

## Get All Kittens

```ruby
require 'eBanc'

api = eBanc::APIClient.authorize!('keykeykey')
api.kittens.get
```

```php
import 'eBanc'

api = eBanc.authorize('keykeykey')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: keykeykey"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'eBanc'

api = eBanc::APIClient.authorize!('keykeykey')
api.kittens.get(2)
```

```php
import 'eBanc'

api = eBanc.authorize('keykeykey')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: keykeykey"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">
	If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they 
	are hidden for admins only.
</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

