# web3-plus.php
![Licensed under the MIT License](https://img.shields.io/badge/License-MIT-blue.svg)


A php interface for interacting with the Ethereum blockchain and ecosystem.

# Install

Set minimum stability to dev
```
"minimum-stability": "dev"
```

Then
```
composer require chuangyeshuo/web3-plus.php dev-master
```

Or you can add this line in composer.json

```
"chuangyeshuo/web3-plus.php": "dev-master"
```


# Usage

### New instance
```php
use Web3\Web3;

$web3 = new Web3('http://localhost:8545');
```

### Using provider
```php
use Web3\Web3;
use Web3\Providers\HttpProvider;
use Web3\RequestManagers\HttpRequestManager;

$web3 = new Web3(new HttpProvider(new HttpRequestManager('http://localhost:8545')));

// timeout
$web3 = new Web3(new HttpProvider(new HttpRequestManager('http://localhost:8545', 0.1)));
```

### You can use callback to each rpc call:
```php
$web3->clientVersion(function ($err, $version) {
    if ($err !== null) {
        // do something
        return;
    }
    if (isset($client)) {
        echo 'Client version: ' . $version;
    }
});
```

### Eth
```php
use Web3\Web3;

$web3 = new Web3('http://localhost:8545');
$eth = $web3->eth;
```

Or

```php
use Web3\Eth;

$eth = new Eth('http://localhost:8545');
```

### Net
```php
use Web3\Web3;

$web3 = new Web3('http://localhost:8545');
$net = $web3->net;
```

Or

```php
use Web3\Net;

$net = new Net('http://localhost:8545');
```

### Batch

web3
```php
$web3->batch(true);
$web3->clientVersion();
$web3->hash('0x1234');
$web3->execute(function ($err, $data) {
    if ($err !== null) {
        // do something
        // it may throw exception or array of exception depends on error type
        // connection error: throw exception
        // json rpc error: array of exception
        return;
    }
    // do something
});
```

eth

```php
$eth->batch(true);
$eth->protocolVersion();
$eth->syncing();

$eth->provider->execute(function ($err, $data) {
    if ($err !== null) {
        // do something
        return;
    }
    // do something
});
```

net
```php
$net->batch(true);
$net->version();
$net->listening();

$net->provider->execute(function ($err, $data) {
    if ($err !== null) {
        // do something
        return;
    }
    // do something
});
```

personal
```php
$personal->batch(true);
$personal->listAccounts();
$personal->newAccount('123456');

$personal->provider->execute(function ($err, $data) {
    if ($err !== null) {
        // do something
        return;
    }
    // do something
});
```

### Contract

```php
use Web3\Contract;

$contract = new Contract('http://localhost:8545', $abi);

// deploy contract
$contract->bytecode($bytecode)->new($params, $callback);

// call contract function
$contract->at($contractAddress)->call($functionName, $params, $callback);

// change function state
$contract->at($contractAddress)->send($functionName, $params, $callback);

// estimate deploy contract gas
$contract->bytecode($bytecode)->estimateGas($params, $callback);

// estimate function gas
$contract->at($contractAddress)->estimateGas($functionName, $params, $callback);

// get constructor data
$constructorData = $contract->bytecode($bytecode)->getData($params);

// get function data
$functionData = $contract->at($contractAddress)->getData($functionName, $params);
```

# Assign value to outside scope(from callback scope to outside scope)
Due to callback is not like javascript callback, 
if we need to assign value to outside scope, 
we need to assign reference to callback.
```php
$newAccount = '';

$web3->personal->newAccount('123456', function ($err, $account) use (&$newAccount) {
    if ($err !== null) {
        echo 'Error: ' . $err->getMessage();
        return;
    }
    $newAccount = $account;
    echo 'New account: ' . $account . PHP_EOL;
});
```

# Examples

To run examples, you need to run ethereum blockchain local (testrpc).

If you are using docker as development machain, you can try [ethdock](https://github.com/chuangyeshuo/ethdock) to run local ethereum blockchain, just simply run `docker-compose up -d testrpc` and expose the `8545` port.

# Develop

### Local php cli installed

1. Clone the repo and install packages.
```
git clone https://github.com/chuangyeshuo/web3-plus.php.git && cd web3.php && composer install
```

2. Run test script.
```
vendor/bin/phpunit
```

### Docker container

1. Clone the repo and run docker container.
```
git clone https://github.com/chuangyeshuo/web3-plus.php.git
```

2. Copy web3.php to web3.php/docker/app directory and start container.
```
cp files docker/app && docker-compose up -d php
```

3. Enter php container and install packages.
```
docker-compose exec php ash
```

4. Run test script
```
vendor/bin/phpunit
```

# Code of Conduct

We aim to share our knowledge and findings as we work daily to improve our product, for our community, in a safe and open space. We work as we live, as kind and considerate human beings who learn and grow from giving and receiving positive, constructive feedback. We reserve the right to delete or ban any behavior violating this base foundation of respect.

# Donations

I do this because I love it, but if you want to buy me a coffee, I won't say no. :o)
```php
Ethereum: 0x104F8FE69dF59fe4c27dd487779D49A9Ec5caCC9
```
# License

Completely MIT Licensed. Including ALL dependencies. If you love or like it ！Please jion us!
```go
E-mail:lucklidi@126.com，WechatID:adi1427569517
```
### 如感兴趣，请直接加入:
```
or you can scan it
```

![Image text](https://github.com/fomo3d-wiki/books/blob/master/images/weixinGZ.jpg)
