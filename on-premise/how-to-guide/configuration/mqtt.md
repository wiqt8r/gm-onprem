# MQTT

**MQTT** — это популярный протокол связи, используемый для устройств Интернета вещей (IoT).  
Он обеспечивает обмен сообщениями с минимальными задержками и низким потреблением трафика, что делает его идеальным решением для IoT-приложений.

Платформа **ГдеМои — Локальная версия** поддерживает использование протокола MQTT для подключения IoT-устройств и получения данных в режиме реального времени без необходимости сложной настройки.  
Эта возможность доступна для всех типов развертывания — **ServerMate (PaaS)**, **Облачная версия** и **Локальная версия (On-Premise)**.  

С помощью MQTT пользователи также могут отправлять команды на устройства и отслеживать их состояние, что делает этот инструмент мощным средством управления IoT-инфраструктурой.

## Примеры настройки устройств MQTT

Ниже приведены примеры конфигурации MQTT для конкретных типов устройств:

* [Xirgo Global (ранее BCE)](mqtt.md#xirgo-global)
* [Globalmatix](mqtt.md#globalmatix)

Процесс добавления устройств MQTT в платформу **ГдеМои** одинаков для всех типов развертывания (ServerMate, Cloud и On-Premise).  
Различия могут присутствовать только в настройке самих устройств. Примеры приведены ниже.

### Xirgo Global

Чтобы настроить устройства **Xirgo Global** для работы по протоколу MQTT:

1. Войдите в [BCE Configuration Manager FMSET](https://xdm.xgfleet.eu/login)  
2. Перейдите в **Connectivity → Telemetry server → MQTT broker address settings** и укажите:
   * **Host:** домен вашей платформы (например, `ui.mydomain.com`)
   * **Port:** `1883`

   ![](../../../on-premise/on-premise/configuration/attachments/image-20230810-133722.png)

3. Добавьте пользователя по умолчанию в разделе **MQTT Security → Authorization**:
   * **Client ID:** `%IMEI%`
   * **Username:** `bce_device`
   * **Password:** `secretword`

   ![](../../../on-premise/on-premise/configuration/attachments/image-20230810-133739.png)

4. Проверьте правильность конфигурации топиков:
   * Все топики, кроме **Output control topic name**, должны иметь значения по умолчанию.
   * **Output control topic name** должно быть установлено как `%IMEI%/OUTC`.

   ![](../../../on-premise/on-premise/configuration/attachments/image-20230810-133800.png)

5. Сохраните конфигурацию устройства.

### Globalmatix

Чтобы настроить устройство **Globalmatix** для работы по протоколу MQTT, укажите следующие параметры:

* **Host:** домен вашей платформы (например, `ui.mydomain.com`)
* **Port:** `1883`
* **User:** `globalmatix_device`
* **Password:** `secretword`
* **Topic:** `globalmatix/in`

![](../../../on-premise/on-premise/configuration/attachments/image-20230810-133819.png)
