# Установка SSL-сертификатов

На этой странице описан процесс первичной установки SSL-сертификатов и перевода веб-сайта на работу по HTTPS.  
Если у вас уже установлен SSL-сертификат, но он истёк, перейдите на страницу [Обновление сертификатов](ssl-certificates-update.md).

Платформа **ГдеМои** использует [Nginx](../nginx-web-server.md) в качестве веб-сервера. Он совместим как с Linux, так и с Windows, поэтому приведённые ниже шаги применимы для обеих систем.

## Шаг 1. Подготовка SSL-сертификатов

Для включения SSL-защиты сайта вам потребуется сертификат и соответствующий ему приватный ключ.  
Если на вашем сайте используется несколько доменов, потребуется отдельный сертификат для каждого или мультидоменный (wildcard) сертификат.

Чтобы веб-сервер работал корректно, сертификат и ключ должны быть парой. Без соответствующего приватного ключа Nginx не сможет запуститься.  
Обычно оба файла выдаются центром сертификации при выпуске SSL.

Подробнее о работе SSL в платформе можно узнать на странице [SSL-шифрование](../../requirements/ssl-encryption.md).

### Цепочка доверия

Для корректной работы сертификата необходимо наличие **полной цепочки доверия** — основного, промежуточных и корневого сертификатов.  
Если цепочка неполная, часть функций (например, мобильные приложения) может работать некорректно.

Если сертификат не содержит полную цепочку, обратитесь к вашему провайдеру сертификатов или воспользуйтесь онлайн-инструментом для её восстановления:

* [How certificate chains work](https://knowledge.digicert.com/solution/SO16297.html)
* [SSL certificates chain resolver](https://www.leaderssl.com/tools/cert_chain_resolver)

### Проверка приватного ключа

Приватный ключ должен соответствовать SSL-сертификату.  
Как правило, он выдаётся вместе с сертификатом, и при продлении сертификата у того же центра ключ остаётся тем же.

Проверить соответствие можно с помощью онлайн-инструмента:  
[Certificate and Key Matcher](https://www.sslshopper.com/certificate-key-matcher.html)

## Шаг 2. Установка SSL-сертификатов

### Автоматическая установка с помощью Let's Encrypt

Если платформа была установлена автоматически (см. [Автоматическая установка](../../installation/advanced-installation/)), можно воспользоваться бесплатными сертификатами Let's Encrypt, установив их через [Мастер настройки](../../installation/advanced-installation/ubuntu-20/configuration-wizard.md).

### Ручная установка сертификатов

Если вы устанавливаете сертификаты вручную, сначала получите файлы сертификата и ключа у любого удостоверяющего центра.  
Разместите их в стандартных каталогах:

* Linux — `/etc/nginx/ssl/`
* Windows — `C:\nginx\conf\ssl\`

Используйте короткие пути в конфигурации сайта, например `ssl/name.crt` и `ssl/name.key`, чтобы избежать ошибок при настройке.

## Шаг 3. Обновление конфигурации

### Настройка Nginx

Найдите файл конфигурации вашего сайта — обычно он называется **navixy.conf** и находится:

* Linux — `/etc/nginx/sites-available/`
* Windows — `C:\nginx\sites-enabled\`

Измените порт с `80` на `443 ssl`, добавьте строки для SSL-сертификатов:

```
listen 443 ssl; ## listen for ipv4
ssl_certificate /ssl/certificate.crt;
ssl_certificate_key /ssl/private.key;
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 10m;
```
Пример конфигурации для API-домена:

```
server {
listen 443 ssl; ## listen for ipv4
server_name api.domain.com;
access_log /var/log/nginx/api.domain.com_ssl.access.log;
client_max_body_size 20m;
ssl_certificate /ssl/certificate.crt;
ssl_certificate_key /etc/nginx/ssl/private.key;
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 10m;
location / {proxy_pass http://127.0.0.1:8084;
}
}
```

Перезапустите веб-сервер:

* Linux — `nginx -t && nginx -s reload`
* Windows — завершите все процессы Nginx и запустите `nginx.exe` из `C:\nginx`

Убедитесь, что сервер запущен и не выдаёт ошибок, связанных с SSL.

### Настройка файлов платформы

Необходимо указать платформе использовать HTTPS вместо HTTP.  
Откройте и измените следующие файлы:

* `/var/www/panel-v2/PConfig.js` — замените `http` на `https` в параметре `apiRoot`, а в `terminalHost` измените `ws` на `wss` и удалите порт 8383.  
* `/var/www/pro-ui/Config.js` — замените `http` на `https` в параметре `apiRoot`.  
* `/var/www/pro-ui/static/app_config.js` — измените `http` на `https` в `apiUrl` (если присутствует).  
* `/home/java/api-server/conf/config.properties` — в параметре `api.externalBaseUrl` измените `http` на `https`.

Перезапустите сервисы платформы (см. [Перезапуск платформы](../../maintenance/restarting-instance.md)), чтобы изменения вступили в силу.  
Если сайт всё ещё открывается по HTTP, очистите кэш браузера или попробуйте открыть страницу в режиме инкогнито.
