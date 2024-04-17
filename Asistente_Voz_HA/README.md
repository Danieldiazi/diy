# OBJETIVO
Construir un asistente de voz en HA

# COMPONENTES HARDWARE NECESARIOS

A continuación puedes ver los componentes principales. He puesto el enlace dónde los he comprado por si te resulta de interés. Cómpralos dónde tu consideres, sólo te doy un punto de partida.

## M5Stack Atom Echo:   
 * [M5Stack.com](https://shop.m5stack.com/products/atom-echo-smart-speaker-dev-kit?variant=34577853415588)

# INSTALACIÓN Y CONFIGURACIÓN DE M5STACK ATOM ECHO
Una vez tengas tu dispositivo:
* Conéctalo a tu ordenador 
* desde un navegador compatible con Chromium vete a https://web.esphome.io/
* Te aparece la opción de elegir el firmware de Voice Assistant
* Una vez finalizado, te pide añadirlo a tu WIFI, pon tus credenciales y listo.
* En Home Assistant te aparecerá el dispositivo para añadir, házlo.
* Si tienes el contenedor con ESPHome, también lo detectará y no tienes más que añadirlo

## Problemas
En mi caso, no se conectaba a la wifi, así que:
* Como tengo el contenedor de ESPHome creé un nuevo dispositivo y le metí el software de forma manual (código aqui: https://github.com/esphome/firmware/blob/main/voice-assistant/m5stack-atom-echo.yaml) y le metí las lineas de configuración de la WIFI. 
* Compilé, y subí el .bin al aparato usando  https://web.esphome.io/ 

# INSTALACIÓN Y CONFIGURACIÓN DE COMPONENTES SOFTWARE NECESARIOS

## VERSIÓN ADDON
En el caso de que tengas instalado HAOS/HASSIO vete a addons e instala:
* Piper
* Whisper
* openWakeWord

Aquí te explica el proceso: [Wyoming integration](https://www.home-assistant.io/integrations/wyoming)

## VERSIÓN CONTENEDOR
Nota: Cambia la carpeta "/rhasspy" por la que tu consideres mejor.

## Piper
Es la parte que se encargará de traducir el texto a voz, para que en este caso el asistente nos pueda hablar lo que HA le está pasando como texto.

```
mkdir -p /rhasspy/piper/data
docker run --name=piper -d -p 10200:10200 -v /rhasspy/piper/data:/data rhasspy/wyoming-piper --voice  es_ES-davefx-medium
```
"davefx" es la voz que mejor me encajó. Luego también puedes cambiarla en HA.

## Whisper
Se encarga de convertir la voz en texto, para que nos pueda escuchar e interpretar luego el texto generado.
```
mkdir -p /rhasspy/whisper/data
docker run --name=whisper -d -p 10300:10300 -v /rhasspy/whisper/data:/data rhasspy/wyoming-whisper --model tiny-int8 --language es
```
En este caso el modelo a cargar es importante de cara a mejorar la precisión y por tanto nuestra comprensión. Cuánto más grande sea el modelo, mejor nos "entenderá", pero también más recursos hardware (cpu y memoria) necesitará. Igual la versión small (small-int8) o medium (medium-int8) es mejor opción que "tiny-int8". Más info sobre esto en el [proyecto github de whisper](https://github.com/openai/whisper)

## openWakeWord
Es la palabra de activación
```
docker run --name=openwakeword -d -p 10400:10400 rhasspy/wyoming-openwakeword --preload-model 'ok_nabu'

```
Aunque he puesto 'ok_nabu' desde HA luego parece que se puede cambiar.

# CONFIGURACIÓN EN HOME ASSISTANT


## Piper
Ir a integraciones, añadir integración "Piper". Te pedirá IP y puerto.

## Whisper
Ir a integraciones, añadir integración "Whisper". Te pedirá IP y puerto.

## openWakeWord
Ir a la integración llamada Wyoming, pulsar en "ADD Entry" y poner la IP y puerto de openwakeword

## Asistente de voz
Vete a Ajustes, y allí en "Asistente de voz". Pulsa en "Añadir asistente"

* Ponle un nombre, por ejemplo asistente local.
* En Voz a texto, elige "faster-whisper"
* en texto a voz, elige "piper" y voz "davefx"
* En palabra de activacion, te aparecerá "openWakeWord", así que escógelo.

# CONSIDERACIONES

* En HA, ten en cuenta que por cada dispositivo/sensor, en su configuración tienes una sección dónde puedes acceder a la parte de asistente de voz y añadir un alías más cómodo.

* Referente a Whisper y al contenedor: La parte de elegir el modelo puede marcar la diferencia entre que sea una interacción incómoda o no.


# MÁS INFORMACIÓN
Te recomiendo los siguientes enlaces:

* [Installing a local Assist pipeline ](https://www.home-assistant.io/voice_control/voice_remote_local_assistant/)


* [$13 voice assistant for Home Assistant] (https://www.home-assistant.io/voice_control/thirteen-usd-voice-remote/)

* Exponer dispositivos a Assist: [Exposing your devices](https://www.home-assistant.io/voice_control/voice_remote_expose_devices/#exposing-your-devices)