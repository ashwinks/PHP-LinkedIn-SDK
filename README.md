PHP-LinkedIn-SDK
================

A PHP wrapper for the LinkedIn API


Here's a quick way to get started with this wrapper:
---

Instantiate our class
```php
$li = new LinkedIn(
  array(
    'api_key' => 'yourapikey',
    'api_secret' => 'yourapisecret',
    'callback_url' => 'https://yourdomain.com/redirecthere' // OPTIONAL
  )
);
```

Get the login URL - this accepts an array of SCOPES
```php
$url = $li->getLoginUrl(
  array(
    LinkedIn::SCOPE_BASIC_PROFILE,
    LinkedIn::SCOPE_EMAIL_ADDRESS,
    LinkedIn::SCOPE_NETWORK
  )
);
```

LinkedIn will redirect to 'callback_url' with an access token as the 'code' parameter.
You might want to store the token in your session so the user doesn't have to log in again
```php
$token = $li->getAccessToken($_REQUEST['code']);
$token_expires = $li->getAccessTokenExpiration();
```

Make a request to the API
```php
$info = $li->get('/people/~:(first-name,last-name,positions)');
```

## Overwrite curl options
```php
$li = new LinkedIn(
  array(
    'api_key' => 'yourapikey',
    'api_secret' => 'yourapisecret',
    'callback_url' => 'https://yourdomain.com/redirecthere',
    'curl_options' => array(
        CURLOPT_PROXY => '127.0.0.1:80',
    ),
  )
);
```

## [Advanced] Make the callback url optional
> **Note:** You must have the `callback_url` set for use authentication methods. Be careful :)

If you already have a valid access token stored, you may omit the setting ```callback_url``` from your config array on instantiation. Depending of the logic you have used on your code it might be useful. You may use two ways:

#### 1. Use ```setCallbackUrl()``` for set the callback url after the instantiation.
```php
$li->setCallbackUrl('https://yourdomain.com/redirecthere');
```

#### 2. Use as parameter on methods while in authentication flow.

> The passed parameter must be the same for both methods below!

```php
$callback_url = 'https://yourdomain.com/redirecthere';

$url = $li->getLoginUrl($scopes, $callback_url); // $scopes is the array of SCOPES
$token = $li->getAccessToken($_REQUEST['code'], $callback_url);
```
