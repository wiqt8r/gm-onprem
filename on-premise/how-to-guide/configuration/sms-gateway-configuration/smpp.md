# SMPP

**SMPP** (Short Message Peer-to-Peer) — это протокол, используемый для обмена SMS-сообщениями между SMS-центрами (SMSC) и приложениями.  
Он обеспечивает надёжную и быструю доставку сообщений, поддержку подтверждений статуса, а также позволяет обрабатывать большие объёмы SMS-трафика.

Платформа **ГдеМои — Локальная версия** поддерживает протокол **SMPP v3.4** для отправки и приёма SMS-сообщений.  
Это даёт возможность использовать внешний SMS-центр и интегрировать его с платформой через базу данных.

Подключение выполняется через таблицу **google.sms_gates**, где указываются все параметры соединения в формате **JSON**.

---

## Минимальная JSON-конфигурация для SMPP-шлюза

```json
{
  "owner_id": 1,
  "label": "My_SMPP_Gateway",
  "type": "transceiver",
  "provider": "smpp",
  "params": {
    "addresses": ["smpp.server.com:1234"],
    "login": "username",
    "password": "password"
  },
  "enabled": 1,
  "class_filter": "*"
}
```

## Расширенная JSON-конфигурация
{
  "addresses": ["smpp.server.com:1234"],
  "login": "username",
  "password": "password",
  "default_charset": "GSM8",
  "source_ton": -1,
  "source_npi": 1,
  "dest_ton": 1,
  "dest_npi": 1,
  "alnum_ton": 5,
  "alnum_npi": 0,
  "num_ton": 1,
  "num_npi": 1,
  "override_originator": null,
  "long_sms_transmit_method": "udh",
  "interface_version": "v34",
  "default_coding": 0,
  "binary_coding": 4,
  "unicode_coding": 8,
  "null_padded_octet_strings": false,
  "connect_timeout": 10000,
  "bind_timeout": 10000,
  "request_expiry_timeout": 15000,
  "enquire_link_timeout": 3000,
  "enquire_link_interval": 15000,
  "submit_timeout": 10000,
  "reconnect_wait": 15000,
  "log_pdu": false,
  "log_bytes": false,
  "window_size": 10,
  "system_type": null,
  "registered_delivery_receipt_request": true,

  "support_binary": true
}

## Пояснение основных параметров

| Параметр | Назначение |
|-----------|------------|
| **addresses** | Адрес и порт сервера SMS-провайдера. Пример: `["sms.provider.ru:2775"]`. |
| **login** / **password** | Учётные данные для подключения к SMS-центру. |
| **source_ton**, **source_npi** | Параметры типа и нумерационного плана источника (обычно используются значения по умолчанию). |
| **alnum_ton**, **alnum_npi** | Используются для буквенно-цифровых отправителей. |
| **long_sms_transmit_method** | Метод отправки длинных сообщений: `udh` или `payload`. |
| **interface_version** | Версия протокола SMPP — `v33` или `v34`. |
| **registered_delivery_receipt_request** | Включает получение отчётов о доставке сообщений. |
| **reconnect_wait** | Интервал ожидания перед повторным подключением при обрыве связи. |
| **log_pdu** / **log_bytes** | Включение детализированного логирования (используется для диагностики). |

---

## Рекомендации по настройке

1. Все параметры указываются в таблице **google.sms_gates** в поле `params` в формате JSON.  
2. После добавления записи убедитесь, что шлюз активен (`enabled = 1`).  
3. Для тестирования подключения отправьте пробное SMS-сообщение из панели администратора.  
4. После внесения изменений перезапустите сервисы командой: `restart-navixy`
5. При необходимости уточнения параметров обратитесь к вашему SMS-провайдеру — он предоставит адрес, порт, логин и технические ограничения для подключения SMPP.

---

С помощью правильно настроенного SMPP-шлюза вы можете организовать надёжную отправку и приём SMS-сообщений прямо из платформы **ГдеМои**.

