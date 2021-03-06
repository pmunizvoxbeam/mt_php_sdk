MagicTelecomAPILib
=================

INSTALATION VIA COMPOSER
========================

```composer require magictelecom/magictelecomapi```

How To Configure:
=================
The generated code might need to be configured with your API credentials. To do that,
provide the credentials and configuration values as a constructor parameters for the controllers

How To Build: 
=============
The generated code uses a PHP library namely UniRest. The reference to this
library is already added as a composer dependency in the generated composer.json
file. Therefore, you will need internet access to resolve this dependency.

How To Use:
===========
For using this SDK do the following:

    1. Open a new PHP >= 5.3 project and copy the generated PHP files in the project
       directory.
    2. Use composer to install the dependencies. Usually this can be done through a 
       context menu command "Instal (dev)".
    3. Import classes from your file in your code where needed for example,
           use MagicTelecomAPILib\Controllers\UsersController;
    4. You can now instantiate controllers and call the respective methods.

Magic SDK examples
==================

## 1. Get all accounts

```php
try {
    ...

    // Create an AccountsController for account actions like:
    // create an account, a cart, cart items, checkout cart and so on
    $objController = new AccountsController();

    // Get the list of accounts
    objResponse = $objController->getAccounts();
    $arrAccount = objResponse->data->results;
    …
    
} catch (APIException $e) {
    …
}
```

You can get a limit result using parameters like page, limit, and filters. Take a look to the API doc. Here you have an example.

```php
try {
    ...

    // Create an AccountsController
    $objController = new AccountsController();

    // Get a list of  accounts using pagination and filters
    // We are going to limit to first five elements with some requirements like firstname and email
    // The filters can be "number, email, contact_number, firstname, lastname"
    objResponse = $objController->getAccounts(1, 5 "firstname::John|lastname::Doe");
    $arrAccount = objResponse->data->results;
    …
    
} catch (APIException $e) {
    …
}
```

## 2. Create an account

```php
try {
    ...

    // Create an AccountsController
    $objController = new AccountsController();

    // Create an account "99674698002" with roles "USER"
    $objAccount = new Account("99674698002", array("USER"), "john@example.com", "14079876543", "John", "Smith");
    $objAccountForm = new AccountForm($objAccount);
    
    $objResponse = $objController->createAccount($objAccountForm);
    $strAccount = $objResponse->number;
    …
    
} catch (APIException $e) {
    …
}
```

This is a full response example.

```
class stdClass#13 (5) {
  public $number =>
  string(11) "99674698003"
  public $email =>
  string(19) "john@example.com"
  public $contact_number =>
  string(10) "4079876543"
  public $firstname =>
  string(4) "John"
  public $lastname =>
  string(6) "Smith"
}
```

## 3. Get an account

```php
try {
    ...
    
    // Create an AccountsController
    $objController = new AccountsController();

    // Get account "99674698003"
    $objResponse = $objController->getAccount("99674698003");
    $objAccount = $objResponse->data;
    …
    
} catch (APIException $e) {
    echo "Can't accomplish this action, exception: [{$e->getMessage()}]";
}
```
If the account doesn't exist, you gonna get this message:
`Can't accomplish this action, exception: [Resource not found]`

## 4. Update an account

```php
try {
    ...
    
    // Create an AccountsController
    $objController = new AccountsController();

    // Create the account to update
    $objAccount = new Account("99674698004", array("USER"), "johns_new@test.com", "4079876543", "John", "New");
    $objAccountForm = new AccountForm($objAccount);
    
    // Update account "99674698002"
    $objController->updateAccount("99674698002", $objAccountForm);
    
    // Get the account with the new account value
    $objResponse = $objController->getAccount("99674698004");
    …
    
} catch (APIException $e) {
    echo "Can't accomplish this action, exception: [{$e->getMessage()}]";
}
```
If the account doesn't exist, you gonna get this message:
`Can't accomplish this action, exception: [Resource not found]`

## 5. Delete an account

```php
try {
    ...
    
    // Create an AccountsController
    $objController = new AccountsController();

    // Delete account "99674698003"
    $objController->deleteAccount("99674698003");
    …
    
} catch (APIException $e) {
    …
}
```

## 6. Get account access tokens

```php
try {
    ...
    
    // Create an AccountsController
    $objController = new AccountsController();

    // Get account "997766554" access tokens
    $objResponse = $objController->getAccessTokens("997766554");
    $arrToken = $objResponse->data->results;
    
    // Get the first access token
    if(isset($arrToken[0]))
    {
        $strFirstToken = $arrToken[0]->token;
    }
    …
    
} catch (APIException $e) {
    …
}
```

## 7. Create account access tokens

```php
try {
    ...
    
} catch (APIException $e) {
    …
}
```

## 6. Create a cart

```php
try {
    ...
    
    // Create an AccountsController
    $objController = new AccountsController();

    // Create Cart for account "997766554"
    $objCart = $objController->createCarts("997766554");

    // Getting cart id
    $intCartId = $objCart->cart_id;
    …
    
} catch (APIException $e) {
    …
}
```

## 7.  Create cart items
#### Creating a trunk item

```php
try {
    ...

    // Create routing object        
    $objRouting = new Routing("load-balanced", 
                                array(new Endpoint("108.188.149.100", "maxChannels=100")), 
                                "Sip_user_1"
                             );

    // Create Trunk Item
    $objTrunk = new TrunkItem(10, "WORLD_WIDE", "108.188.149.121", "sms,fax",  $objRouting);

    // Set the item type ("TRUNK", "LOCATION", "DID")
    $objTrunk->__set("itemType", "TRUNK");
    ...

} catch (APIException $e) {
    ...
}
```

#### Create Location Item

```php
try {
    ...

    // Create a caller location
    $objCallerLocation = new CallerLocation(
                                    "John Smith", 
                                    "123 Street Name", 
                                    "Orlando", 
                                    "FL", 
                                    "32819", 
                                    "UNIT", 
                                    "123", 
                                    "US"
                                 );

    // Create Location Item for the trunk id 23
    $objLocation = new LocationItem("ORLANDO__407___FL", 3, "sms,fax", "STANDARD", 23, objCallerLocation);

    // Setting the item type
    $objLocation->__set("itemType", "LOCATION");
    ...

} catch (APIException $e) {
    ...
}
```

#### Create Did Item

```php
try {
    ...

    // Create a caller location
    $objCallerLocation = new CallerLocation(
                                    "John Smith", 
                                    "123 Street Name", 
                                    "Orlando", 
                                    "FL", 
                                    "32819", 
                                    "UNIT", 
                                    "123", 
                                    "US"
                                 );

    // Creating a Did Item for trunk id 5
    $objDid = new DidItem("14701234567", 5, "STANDARD", $objCallerLocation);
    $objDid->__set("itemType", "DID");
    ...

} catch (APIException $e) {
    ...
}
```

## 8. Add an Item to cart

```php
try {

    // Create an AccountsController for account actions like:
    // create a cart, add cart items, checkout cart
    $objController = new AccountsController();

    // Create a item like the examples before
    // Could be an TrunkItem, LocationItem or DidItem
    $objTrunk = …

    //Create an form item
    $objForm = new ItemForm($objTrunk);

    // Add the item to the cart 3
    $response = $objController->createItems("997766554", 3, $objForm);

    // Get Item id from the response
    $intItemId = response->item_id;
    ...

} catch (APIException $e) {
    ...
}
```

This is the full response (Trunk Item) example

```
object(stdClass)#15 (6) {
  ["item_id"]=>
  int(4)
  ["channels"]=>
  int(10)
  ["sip_uri"]=>
  string(15) "108.188.149.125"
  ["attributes"]=>
  string(7) "sms,fax"
  ["_routing"]=>
  string(115) "{"sip_user":"sip_user_1","logic":"load-balanced",
                "endpoints":[{"uri":"108.188.149.100","attrs":"maxChannels=100"}]}"
  ["item_type"]=>
  string(5) "TRUNK"
}
```

## 9. Checkout a Cart

```php
try {
    ...

    // Create Checkout object with using and external reference
    // "1234567899000dfhdfhdf1234"

    // External reference could be a generated string
    $objCheckout = new Checkout("1234567899000dfhdfhdf1234");

    // Create a checkout form using the checkout object
    $checkoutForm = new CartCheckoutForm($objCheckout);

    // Checkout Cart with id 3 in account "997766554"
    $response  = $objController->createCartCheckout("997766554", 3, $checkoutForm);

    // Getting Order id generated by the cart checkout
    $intOrderId = $response->order_id;
    ...

} catch (APIException $e) {
    ...
}
```

This is the full response (Checkout Cart) example

```
object(stdClass)#17 (5) {
  ["external_order_reference"]=>
  string(28) "1234567899000dfhdfhdf1234eee"
  ["created"]=>
  string(24) "2016-03-11T21:16:25+0000"
  ["order_id"]=>
  string(1) "2"
  ["_items"]=>
  array(1) {
    [0]=>
    object(stdClass)#18 (9) {
      ["item_id"]=>
      string(1) "1"
      ["status"]=>
      string(8) "COMPLETE"
      ["item_type"]=>
      string(5) "TRUNK"
      ["channels"]=>
      int(10)
      ["sip_uri"]=>
      string(15) "108.188.149.125"
      ["attributes"]=>
      string(7) "sms,fax"
      ["_routing"]=>
      string(115) "{"sip_user":"sip_user_1","logic":"load-balanced",
                    "endpoints":[{"uri":"108.188.149.100","attrs":"maxChannels=100"}]}"
      ["trunk_handle"]=>
      string(10) "WORLD_WIDE"
      ["trunk_id"]=>
      int(7)
    }
  }
  ["account_number"]=>
  string(9) "997766554"
}
```
