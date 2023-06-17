# Practica ESP32 con un boton virtual.

Este repositorio muestra como podemos programar una ESP32 con el sensor HC-SR-04 para saber la distancia y un sensor de temperatura y humedad, posteriormente se visualizara en un dashboard en tiempo real los datos simulados.

## Introducción

### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos y un sensor (```Modulo rele```) Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/), y utilizando node red, para realizar la visualización en tiempo real de los datos simulados.


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Programa instalado Node Red
- Modulo rele
- Y muchas ganas de chambear :D


## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).

Tener instalado el programa de Node Red (en este proyecto no se muestra la instalación así que puede buscar un tutorial a parte y regresar para finalizar el proyecto una vez lo tenga listo).


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación de Wokwi y colocar la siguente programación:


```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "44.195.202.69";
const int mqttPort = 1883;
const char* mqttUser = "ElUriel";
const char* mqttPassword = "230397";
const char* mqttTopic = "ElUriel";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}

```

2. Hacer la conexion de **Modulo rele** con la **ESP32** como se muestra en la siguente imagen.


![](https://github.com/UrielMastache/ESP32-BOTON-VIRUAL/blob/main/Conexiones.png?raw=true)


3. Agregaremos los siguientes bloques en nuestro espacio de trabajo de Nod Red. (mqtt out, switch)

![](https://github.com/UrielMastache/ESP32-BOTON-VIRUAL/blob/main/bloques%20node%20red.png?raw=true)

4. Configuraremos el primer bloque mqtt de acuerdo al server que pusimos en el codigo.En este ejemplo usamos el server 44.195.202.69

![](https://github.com/UrielMastache/ESP23-Node-Red/blob/main/server%20codigo.png?raw=true)

Aquí se ve en la configuración del bloque en la parte del server (dando doble clic al bloque).

![](https://github.com/UrielMastache/ESP23-Node-Red/blob/main/server%20mqt.png?raw=true)

En la parte de TOPIC colocaremos lo que escribimos en la parte del codigo const char* mqttTopic = "ElUriel"; **ElUriel**



**Cabe aclarar que cuando le des a "Done" se agregara automaticamente a tu apartado de dashboard dentro de tu proyecto, en este caso como en una sub carpeta en "El Uriel"**


### Instrucciónes de operación

1. Iniciar simulador.
2. Para poder ver los datos en tiempo real tienes que darle al cuadro superior derecho (cuadro con una flecha apuntando hacia arriba-derecha) para que te mande a tu dashboard.

## Resultados


Así tienes que ver tus resultados
Cambiando en tiempo real en el simulador

Switch prendido

![](https://github.com/UrielMastache/ESP32-BOTON-VIRUAL/blob/main/boton%20prendido.png?raw=true)

Switch apagado

![](https://github.com/UrielMastache/ESP32-BOTON-VIRUAL/blob/main/boton%20apagado.png?raw=true)

## Comentarios

Recuerda que esto es demostrativo, y puedes modificar todo a como mejor te adaptes y lo necesites.


Gracias por ver :D 

Ciao.