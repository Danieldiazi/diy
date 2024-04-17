# OBJETIVO
Construir un buzón inteligente de tal manera que podrá avisarnos cuando haya una carta o entrega nueva en el buzón.
La idea es hacer uso de la LoRaWan y la red TTN. 

* Detecta aviso de entrega nueva en el buzón.
* Se conecta mediante LoRa a la red TTN
* Desde Home Assistant se descarga la información recibida para poder actuar sobre ella creando automatizaciones o paneles según se prefiera.
 
Podría usar la red LoRa sin TTN, pero entonces tendría que tener dos dispositivos LoRa, uno para enviar los avisos y otro para recibirlos y conectarlo con Home Assistant. 

De esta manera, sólo necesito un dispositivo LoRa, y una zona en la que haya cobertura de la red TTN. Si la hay, me ahorro un dispositivo.

# COMPONENTES NECESARIOS

A continuación puedes ver los componentes principales. He puesto el enlace dónde los he comprado por si te resulta de interés. Cómpralos dónde tu consideres, sólo te doy un punto de partida.

##  Heltec Lora 32 v2:   
 * [Aliexpress](https://es.aliexpress.com/item/1005005967763162.html)

## Detector de movimiento SR602
  * [Aliexpress](https://es.aliexpress.com/item/32921030810.html)

## Sensor de puerta MC-38
  * [Aliexpress](https://es.aliexpress.com/item/1005005396577514.html)

## Soporte de 2 baterías en paralelo
  
  * [Aliexpress](https://es.aliexpress.com/item/1005005449039449.html)
  
## Baterías (3,7V, 3500mAh):
* [Aliexpress](https://es.aliexpress.com/item/1005005544520613.html)

## Caja
 * [Aliexpress](https://es.aliexpress.com/item/33049905736.html): El modelo "Style 2-8" son 108mm de largo, por 56mm de ancho, y 40 de profundo

# PLATAFORMA 

* TTN: [Cobertura TTN](https://ttnmapper.org/heatmap/)
* LoRa
* Se usará PlatformIO y como IDE VS Code


# CONEXIONADO

| ESP32 Lora V2 |Sensor Puerta MC-38 | Detector movimiento SR602 | 
| :---:| :----: | :---: | 
| GND | | -  |  | 
|3.3V |  | + |  |
|GPIO36 | | OUT | |
|GND | Pin 1| |
|GPIO37 | Pin 2 |  |

#CONFIGURACIÓN
## Platform.IO
TODO.
## TTN
TODO.

## MQTT local
TTN proporciona la integración con MQTT, para ello podemos hacer lo siguiente.

- Instalamos MQTT en nuestro entorno. Por ejemplo MOSQUITTO
- Configuramos en nuestro MQTT un proxy con el servidor MQTT de TTN, de tal manera, que nos bajemos una copia de la información que ahí se recibe.

### Instalación MQTT Mosquito
Si tienes la posibilidad de instalarlo mediante un Addon en HA, perfecto. En el caso de que quieras instalarlo mediante Docker, no tienes más que seguir la página de [instalación en docker](https://hub.docker.com/_/eclipse-mosquitto)
### Configuración PROXY

En mosquito.conf deberás pegar esta configuración:

```
connection ttn
address #PUBLIC_ADDRESS#
bridge_protocol_version mqttv311
remote_username #REMOTE_USERNAME#
start_type automatic
notifications false
try_private false
remote_password #REMOTE_PASSWORD#
bridge_insecure true
topic # both 0 ttn/ v3/#REMOTE_USERNAME#/
cleansession true
```

Debes cambiar las "variables" por sus valores correspondientes que encontrarás dentro del panel de TTN en la sección integraciones y allí en MQTT.
* #PUBLIC_ADDRESS#:  Por ejemplo eu1.cloud.thethings.network:1883 es la dirección pública si tu aplicación está en Europa.
* #REMOTE_USERNAME#: En "Connection credentials" verás el usuario remoto.
* #REMOTE_PASSWORD#: La obtienes pulsando en "Generate New API"

Reemplaza las anteriores variables por las tuyas y la conexión con el MQTT de TTN estará lista una vez reinicies tu servidor MQTT.

Ten en cuenta que esta configuración puedes adaptarla como quieras, simplemente te propongo aquí un ejemplo sencillo y funcional.

## HOME ASSISTANT

### Integracion MQTT
1. Instala la [integracion con MQTT](https://www.home-assistant.io/integrations/mqtt/).

2. En el fichero  de configuración de Home Assistant (configuration.yaml) da de alta los siguientes sensores de MQTT:

```
mqtt:
  sensor:
  - name: Buzon_Hay_Entrega
    state_topic: "ttn/devices/#END_DEVICE#/up"
    value_template: "{{ value_json.uplink_message.decoded_payload.digital_in_0 }}"


  - name: Buzon_Numero_Entregas
    state_topic: "ttn/devices/#END_DEVICE#/up"
    value_template: "{{ value_json.uplink_message.decoded_payload.digital_in_1 }}"
```
* Tienes que reemplazar #END_DEVICE# por el dispositivo final dado de alta en la plataforma TTN
* Tienes más información del sensor de MQTT en su [página](https://www.home-assistant.io/integrations/sensor.mqtt/)

3. Reinicia Home Assistant
4. Vete a entidades y busca cualquiera de las creadas para ver que están funcionando.

5. Ahora configura un panel con esta información, o crea las automatizaciones que consideres, por ejemplo para enviar un aviso cuando te llegue una entrega.
# CODIGO FUENTE
* TODO. / PENDIENTE
