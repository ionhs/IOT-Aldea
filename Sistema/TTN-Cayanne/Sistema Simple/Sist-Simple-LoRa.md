Para poder empezar a trabajar con LoRa, empezamos realizando una aplicación simple. Vamos a explicar el proceso para que desde el desconocimiento y sin entrar en detalles, poder enviar datos de un sensor mediante LoRa y que cualquier usuario pueda visualizar los datos. 

En la imagen de abajo se puede ver que la arquitectura  simplificada de un sistema de comunicación LoRa podría resumirse en 4 elementos interconectados de manera lineal.

![](./Imagenes/architecture-texto.png)

- [Dispositivo:](./Dispositivo.md) Denominado como Nodo dentro del ecosistema LoRa, generalmente suele ser el sensor con el que queremos medir algo o el actuador sobre el que queremos interactuar. Este elemento se comunica mediante una comunicación inalámbrica LoRa que usa un protocolo de comunicación LoRaWAN. Usaremos un sensor de temperatura y humedad [Dragino LHT65](https://www.dragino.com/products/lora-lorawan-end-node/item/151-lht65.html).

- [Gateway](./Gateway.md): Es una puerta de enlace la cual recibe los datos del módulo vía LoRa y los reenvía por Internet (Wifi, Ethernet, 4G, ect.) a los servidores o Network Server. En nuestro caso usaremos un gateway [RAK 7249](https://docs.rakwireless.com/Product-Categories/WisGate/RAK7249) para conectar los Dispositivos LoRa, mediante Ethernet y LTE 4G a Internet.

- [Network Server](./Network_Server.md): Un servidor o grupo de servidores que tienen el objetivo de unir los datos del gateway con los servicios que ofrecen las aplicaciones. Para simplificar este proceso usamos la infraestructura abierta [The Thing Network](https://www.thethingsnetwork.org/).

- [Aplicación](./Aplicacion): Teniendo acceso a los datos en bruto que hay en el Network Server se encarga de buscarle utilidad a esos datos para que tengan una finalidad que haya establecido algún usuario. Aunque hay múltiples aplicaciones con ciertas libertades en nuestro caso intentaremos identificar aplicaciones sencillas de poner en marcha como [Cayanne myDevices](https://developers.mydevices.com/cayenne/features/)  para visualizar los datos.

![](./Imagenes/architecturesimple.png)

