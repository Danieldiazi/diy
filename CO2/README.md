# OBJETIVO
Construir un sensor de CO2 y poder ver mediante una pantalla los datos.
Para evitar que la pantalla esté siempre encendida, se usará un detector de movimiento, de tal manera que cuando detecte presencia encienda la pantalla y el resto del tiempo esté apagada.

También se conectará con Home Assistant, de tal manera que quien lo tenga, podrá ver sus valores desde esta plataforma. 

# COMPONENTES NECESARIOS

A continuación puedes ver los componentes principales. He puesto el enlace dónde los he comprado por si te resulta de interés. Cómpralos dónde tu consideres, sólo te doy un punto de partida.

## ESP32-C3 Supermini:   
 * [Aliexpress](https://es.aliexpress.com/item/1005005967641936.html)

## Detector de movimiento SR602
  * [Aliexpress](https://es.aliexpress.com/item/32921030810.html)

## Pantalla
  * [Aliexpress](https://es.aliexpress.com/item/32896971385.html)
  

## Sensor de CO2: MH-Z19C
  Hay muchos sensores de CO2, en este caso he elegido el MH-Z19C porque tiene bastante buena precisión.
  * [Aliexpress](https://es.aliexpress.com/item/4001296615950.html)
  

## Caja
  Puedes hacer tu una, con marquetería, con impresora 3D, o como tu imaginación y talento te permita.
  En mi caso he optado por una caja de superficie para mecanismos eléctricos. No es la mejor opción, porque es mucho más grande de lo que necesito.

  * [Leroy Merlin](https://www.leroymerlin.es/productos/electricidad-y-domotica/cajas-y-conexiones-electricas/cajas-de-registro/caja-de-mecanismos-universal-85x85-mm-15918413.html)


# PLATAFORMA 
Se usa ESPHOME, pues permite de una forma muy cómoda la construcción de este proyecto sin siquiera tener conocimientos de programación.

En su web hay mucha documentación, para el caso que nos ocupa:

* La pantalla: [ESPHOME - SSD1306](https://esphome.io/components/display/ssd1306)
* El sensor de CO2: [ESPHOME - MH Z19C](https://esphome.io/components/sensor/mhz19.html?highlight=mh+z19c)


# CONEXIONADO

| ESP32-C3 Supermini | MH-Z19C | Detector movimiento SR602 | SSD1309 OLED I2C |
| :---:| :----: | :---: | :---: |
| GND | | -  |  | 
|3.3V |  | + |  |
|GPIO0 | | OUT | |
|GND | | | GND |
|Vcc | |  | 3,3v|
|GPIO9 | |  | SCL |
|GPIO8 |   |  | SDA  |
|5V | Vin | | |
|GND | GND | | |
|GPIO20 | TX | | |
|GPIO21 | RX | | |



# CODIGO FUENTE
En este caso es el fichero con extensión yaml que hay que incorporar en ESPHOME.
* [co2.yaml](co2.yaml)
