## Instalación y configuración de Telegraf

Mediante un fichero de configuración Telegraf recolecta los datos y los envia a InfluxDB.

![](./Imagenes/InfluxDBesquema.png)

Tenemos que configurar la instancia de Telegraf para que lea los datos del servidor TTN (The Things Network). El TTN incluye un bróker MQTT, así que todo lo que tenemos que hacer es lo siguiente.

El servicio de Telegraf coge todas las configuraciones del fichero ubicado en **/etc/telegraf/telegraf.conf,** y en él encontramos las siguientes opciones:

\-    [Input Plugins:](https://github.com/influxdata/telegraf#input-plugins) recolecta métricas del sistema, servicios, dispositivos IOT, etc.

\-    [Processor Plugins:](https://github.com/influxdata/telegraf#processor-plugins) Transforma, procesa, decora y filtra las métricas

\-    [Aggregator Plugins:](https://github.com/influxdata/telegraf#aggregator-plugins) Crea conjuntos de métricas, por ejemplo permite hacer una media, conseguir mínimos y máximos partiendo de datos, etc.

\-    [Output Plugins:](https://github.com/influxdata/telegraf#output-plugins) Escribe las métricas a diferentes destinos, en mi caso a InfluxDB.

En nuestro caso los apartados que modificamos son las de Input Plugins y Output Plugins. Es mejor no modificar el archivo original del **telegraf.conf (**[**https://github.com/influxdata/telegraf/blob/master/etc/telegraf.conf**](https://github.com/influxdata/telegraf/blob/master/etc/telegraf.conf)**).** Realizamos una copia donde guardaremos la configuración original:

```
cd /etc/telegraf/
sudo mv telegraf.conf telegraf.conf.old
```

Ahora creamos un fichero con el nombre de **telegraf.conf** en el mismo directorio y lo editamos para ponerlo a nuestro gusto con el siguiente comando:

```
nano /etc/telegraf/telegraf.conf
```

para que desde el servidor de TTN (The Things Network) leamos los datos del nodo en cuestión. Por suerte TTN corre un simple bróker MQTT, así que lo único que tenemos que hacer es pegar lo siguiente:

```
[agent]

  flush_interval = "15s"
  interval = "15s"

[[inputs.mqtt_consumer]]

  servers = ["tcp://eu.thethings.network:1883"]
  qos = 0
  connection_timeout = "30s"
  topics = [ "+/devices/+/up" ]
  client_id = "ttn"
  username = "XXX"
  password = "ttn-account-XXX"
  data_format = "json"

[[outputs.influxdb]]

  database = "telegraf"
  urls = [ "http://localhost:8086" ]
  username = "telegraf"
  password = "superpa$$word"

```

En este archivo sustituiremos los siguientes valores: “username” y “password” por los valores que obtenemos del TTN.

![](./Imagenes/Username.png)

![](./Imagenes/Password.png)

Después de cambiar el archivo hay que **reiniciar Telegraf** para que empiece a recoger las métricas y éstas sean enviadas a InfluxDB:

```
service telegraf restart
```

Para ver si los datos son enviados desde Telegraf a InfluxDB debemos de entrar en influxdb con el siguiente comando:

```
influx
```

Y después, utilizando el lenguaje de consulta InfluxQL y la base de datos antes configurada “telegraf”:

```
use telegraf
select * from "mqtt_consumer"
```

Deberías de poder ver algo como lo siguiente, cogiendo como ejemplo la aplicación de “dragino” que mide la **temperatura_exterior**, **temperatura_interior** y la **humedad**:

![](./Imagenes/InfluxMQTT.png)

Los datos vistos en el TTN, después de pasar el decoder al Formato del Payload:

![](./Imagenes/Fields.png)

Vemos que los datos son los mismo que han sido leídos por Telegraf y se han guardado correctamente en InfluxDb.

Pasa al [siguiente paso Grafana](./Grafana.md)