# OBJETIVO
Construir un buzón inteligente de tal manera que podrá avisarnos cuando haya una carta o entrega nueva en el buzón.
La idea es hacer uso de la LoRaWan y la red TTN. 

* Detecta aviso de entrega nueva en el buzón.
* Se conecta mediante LoRa a la red TTN
* Desde Home Assistant se descarga la información recibida para poder actuar sobre ella creando automatizaciones o paneles según se prefiera.
 

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
## HOME ASSISTANT
TODO

# CODIGO FUENTE
* TODO.
