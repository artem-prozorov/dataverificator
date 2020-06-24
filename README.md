# Data Locker: библиотека для защиты информации одноразовым паролем

[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/artem-prozorov/data-locker/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/artem-prozorov/data-locker/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/artem-prozorov/data-locker/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/artem-prozorov/data-locker/?branch=master)
[![Build Status](https://scrutinizer-ci.com/g/artem-prozorov/data-locker/badges/build.png?b=master)](https://scrutinizer-ci.com/g/artem-prozorov/data-locker/build-status/master)
[![Code Intelligence Status](https://scrutinizer-ci.com/g/artem-prozorov/data-locker/badges/code-intelligence.svg?b=master)](https://scrutinizer-ci.com/code-intelligence)

## Установка


## Использование

### Configuration

### MessageInterface

### TransportInterface


## Интеграции из коробки

### Laravel

### Bitrix


## Примеры

### Bitrix
Сперва в файле `local/php_interface/init.php` загрузите конфигурацию
```
\Prozorov\DataVerification\App\Configuration::getInstance()->loadConfig([
    'code_repository' => CodeRepo::class,
    'transport_config' => ['sms' => \Brosto\Loyalty\Integrations\SimpleSms\Transport::class],
]);
```

Пример сохранения данных для защиты
```
use Prozorov\DataVerification\App\Controller;
use Prozorov\DataVerification\Types\Phone;
use Prozorov\DataVerification\Integrations\Bitrix\Repositories\CodeRepo;

$request = \Bitrix\Main\Application::getInstance()->getContext()->getRequest();

$phone = $request->get('phone');
if (empty($phone)) {
    throw new \OutOfRangeException('Не указан номер телефона');
}

$controller = new Controller();

$address = new Phone($phone);

$code = $controller->lockData(['some data to secure'], $address, 'sms');

return $code->getVerificationCode();

```

Пример проверки одноразового пароля
```
use Prozorov\DataVerification\App\Controller;
use Prozorov\DataVerification\Integrations\Bitrix\Repositories\CodeRepo;

$request = \Bitrix\Main\Application::getInstance()->getContext()->getRequest();

$pass = $request->get('pass');
if (empty($pass)) {
    throw new \OutOfRangeException('Не указан одноразовый пароль');
}

$verificationCode = $request->get('verification_code');
if (empty($verificationCode)) {
    throw new \OutOfRangeException('Не указан верификационный код');
}

$controller = new Controller();

$data = $controller->getUnlockedData($verificationCode, $pass);

```
