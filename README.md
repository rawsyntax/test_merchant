Test Merchant
================

Setup
-------------

This application requires:

- Ruby 2.4.3
- Rails 5.1.5

Run this command to get started:

```
bundle install
```

By default the Billing mode is set to `:test` (not `:production`). This is set in the Thorfile.

Docker
-------------------

Once docker is installed run:

```
docker-compose build
```

Then the following commands can be run as is by prefixing them with:

```
docker-compose run web ...
```

For example:

```
docker-compose run web bundle exec thor gateway:purchase BraintreeBlueGateway merchant_id:123 public_key:456 private_key:789 amount:1000 number:4111111111111111 month:3 year:2018 first_name:Bob last_name:Smith verification_value:123 brand:visa
```

Find Your Gateway
-------------------

First you must find the ActiveMerchant representation of your gateway. In this example we are using Braintree Blue

```sh
bundle exec thor gateway:search braintree
```

Find Credentials Format
---------------

Refer to [active_merchant](https://github.com/activemerchant/active_merchant/blob/master/test/fixtures.yml) to understand how to format your gateway specific credentials.

In this example BraintreeBlue is formatted like:

```
braintree_blue:
  merchant_id: X
  public_key: Y
  private_key: Z
```

So those arguments are appended after specifying the command and the gateway to use. Note the below command isn't complete yet.

```
bundle exec thor gateway:some_command BraintreeBlueGateway merchant_id:123 public_key:456 private_key:789
```

Purchase
--------------

When purchasing an amount is included. The command below is a purchase for $10.

```
bundle exec thor gateway:purchase BraintreeBlueGateway merchant_id:123 public_key:456 private_key:789 amount:1000 number:4111111111111111 month:3 year:2018 first_name:Bob last_name:Smith verification_value:123 brand:visa
```

Authorize
---------------

When authorizing an amount is included. The command below is an authorization for $10.

Furthermore the details of the credit card must be specified via:

```
number:4111111111111111
month:3
year:2018
first_name:Bob
last_name:Smith
verification_value:123
brand:visa
```

For a list of accepted brands refer to [credit_card](https://github.com/activemerchant/active_merchant/blob/master/lib/active_merchant/billing/credit_card.rb#L83)

```
bundle exec thor gateway:authorize BraintreeBlueGateway merchant_id:123 public_key:456 private_key:789 amount:1000 number:4111111111111111 month:3 year:2018 first_name:Bob last_name:Smith verification_value:123 brand:visa
```

Capture
-------------

An amount and authorization is required when performing a capture.

```
bundle exec thor gateway:capture BraintreeBlueGateway merchant_id:123 public_key:456 private_key:789 amount:1000 authorization:123
```

Void
-------------

An authorization id is required when voiding a transaction.

```
bundle exec thor gateway:void BraintreeBlueGateway merchant_id:123 public_key:456 private_key:789 authorization:0fw94a1d
```

Refund
-------------

An authorization id and amount is required when refunding a transaction.

```
bundle exec thor gateway:refund BraintreeBlueGateway merchant_id:123 public_key:456 private_key:789 amount:1000 authorization:e33z96tk
```
