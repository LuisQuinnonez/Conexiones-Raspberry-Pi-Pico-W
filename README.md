<pre>
	<p align=center>
TECNOLÓGICO NACIONAL DE MÉXICO
INSTITUTO TECNOLÓGICO DE TIJUANA

SUBDIRECCIÓN ACADÉMICA
DEPARTAMENTO DE SISTEMAS Y COMPUTACIÓN

SEMESTRE:
ENERO – JUNIO 2024

CARRERA:
INGENIERÍA EN SISTEMAS COMPUTACIONALES

MATERIA:
Lenguajes de Interfaz

TÍTULO ACTIVIDAD:
Repositorio con evidencia de practicas

UNIDAD A EVALUAR:
UNIDAD 3

NOMBRES Y NÚMEROS DE CONTROL DEL ALUMNOS:
HUERTA RIVAS OLAF ALEXANDRO 21211966
Martínez Nava Blanca Yessenia
QUIÑONEZ ROCHA LUIS ARTURO 21212029

NOMBRE DEL MAESTRO (A):
Rene Solis

	</p>

</pre>

## Práctica 0 - Desplegar "Hello World" 
---
![image](https://github.com/LuisQuinnonez/Conexiones-Raspberry-Pi-Pico-W/assets/158366686/c86b40fc-4f1b-4523-b200-cd4c0ecd586a)
### Código

```C
void setup() {
  Serial.println("Rasperry Pi Pico W");
  // Inicializa el monitor serial a una velocidad de 9600 baudios
  Serial.begin(115200);
}

void loop() {
  // Imprime "Hola mundo" en el monitor serial
  Serial.println("Hola, Rasperry Pi Pico W!");
  // Espera 1 segundo antes de repetir el bucle
  delay(2000);
}
```

## Práctica 1 - Desplegar "Hello World" C#
---
![](holamundo.jpg)

### Código

```C#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.IO.Ports;

namespace LenguajesInterfaz
{

    public partial class Form1 : Form
    {
        SerialPort serialPuerto;

        public Form1()
        {
          InitializeComponent();
        }

        public void button1_Click(object sender, EventArgs e)
        {
            //boton de conectar 

            int bauRate = 9600;
            string nombrePuerto = "COM3";
            serialPuerto = new SerialPort();
            try
            {
                serialPuerto.Open();
                MessageBox.Show("Se conecto correctamente");
            }
            catch(Exception noConected)
            {
                MessageBox.Show("No se conecto" + noConected);
            }
        }

        private void enviar_Click(object sender, EventArgs e)
        {
            try
            {
                serialPuerto.Write(pantalla.ToString());
                //string arduinoResponse = serialPuerto.ReadLine();
                MessageBox.Show("Hola Mundo!!");
            }
            catch(Exception noConected)
            {
                MessageBox.Show("No se envio correctamente" + noConected);
            }
        }

        private void reset_Click(object sender, EventArgs e)
        {

            pantalla.Text=" ";
        }
    }
}
```

## Práctica 2 - OLED Dislpay
---
![](oled.jpg)
### Código

```C
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup() {
  Wire.begin();        // Iniciar comunicación I2C
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);  // Dirección I2C del OLED: 0x3C (para la mayoría de los módulos)
  display.display();   // Mostrar inicialmente
  delay(2000);         // Esperar 2 segundos
}

void loop() {
  display.clearDisplay();  // Borrar la pantalla

  // Escribir texto en la pantalla
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.println("que rollo asi nomas quedo");

  display.display();  // Mostrar texto
  delay(1000);       // Esperar 1 segundo antes de la próxima actualización
}
```

## Práctica 3 - Conectar WiFi
---
![](441237037_1144725820047098_8174869671412169517_n.jpg)
### Código

```C
#include <WiFi.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>


#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);


void setup() {
  Serial.begin(115200);


  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;);
  }


  display.clearDisplay();
}


const char *encToString(uint8_t enc) {
  switch (enc) {
    case ENC_TYPE_NONE: return "NONE";
    case ENC_TYPE_TKIP: return "WPA";
    case ENC_TYPE_CCMP: return "WPA2";
    case ENC_TYPE_AUTO: return "AUTO";
  }
  return "UNKN";
}


void loop() {
  delay(5000);
  Serial.printf("Beginning scan at %lu\n", millis());
  auto cnt = WiFi.scanNetworks();
  display.clearDisplay();
  if (!cnt) {
    Serial.printf("No networks found\n");
    display.setTextSize(0.5);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(0, 0);
    display.println("No networks found");
  } else {
    Serial.printf("Found %d networks\n\n", cnt);
    display.setTextSize(0.5);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(0, 0);
    display.println("Networks:");
    for (auto i = 0; i < cnt; i++) {
      Serial.printf("%s \n", WiFi.SSID(i));
      display.setCursor(0, (i + 1) * 8);
      display.printf("%s ", WiFi.SSID(i));
    }
  }
  Serial.printf("\n--- Sleeping ---\n\n\n");
  display.display();
  delay(5000);
}
```

## Práctica 4 - Conectar Bluetooth
---
![](bluetooth.jpg)

### Código

```C
from machine import Pin
import bluetooth
from ble_advertising import advertising_payload

ble = bluetooth.BLE()
ble.active(True)

def on_scan(addr_type, addr, adv_type, rssi, adv_data):
    print("Found device:", bluetooth.decode_name(adv_data))

ble.gap_scan(2000, 30000, 30000)
ble.irq(handler=on_scan)

def on_connect(addr_type, addr):
    print("Connected to", addr)

ble.gap_advertise(100000, advertising_payload(name='PicoW'))

while True:
    pass
```

## Práctica 5 - ChatGPT
---
![]()
### Código

```C
#include <WiFi.h>
#include <WiFiClientSecure.h>
// Replace with your network credentials
const char* ssid = "Olaf";
const char* password = "12356";
// Replace with your API Key and Model ID
const char* api_key = "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";
const char* model_id = "davinci";
// Set the hostname and URL for the API
const char* host = "api.openai.com";
const char* url = "/v1/engines/davinci-codex/
completions";
void setup() {
Serial.begin(115200);
delay(10);
// Connect to Wi-Fi
Serial.println();
Serial.print("Connecting to ");
Serial.println(ssid);
WiFi.begin(ssid, password);
while (WiFi.status() != WL_CONNECTED) {
delay(500);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected");
// Set up secure client
WiFiClientSecure client;
client.setInsecure();
client.setTimeout(10000);
// Connect to ChatGPT API
Serial.print("Connecting to API...");
if (!client.connect(host, 443)) {
Serial.println("failed");
return;
}
Serial.println("connected");
// Build request payload
String payload = "{";
payload += "\"temperature\": 0.7,";
payload += "\"max_tokens\": 50,";
payload += "\"n\": 1,";
payload += "\"stop\": \"\\n\"}";
// Build HTTP request
String request = "POST ";
request += url;
request += " HTTP/1.1\r\n";
request += "Host: ";
request += host;
request += "\r\n";
request += "Authorization: Bearer ";
request += api_key;
request += "\r\n";
request += "Content-Type: application/json\r\n";
request += "Content-Length: ";
request += payload.length();
request += "\r\n\r\n";
request += payload;
// Send HTTP request
Serial.println("Sending request...");
client.print(request);
// Wait for response from OpenAI API
Serial.println("Waiting for response...");
while (client.connected()) {
if (client.available()) {
String response = client.readString();
Serial.print(response);
}
}
}
void loop() {
}
```

## Proyecto Final
---
![20240521_061135](https://github.com/LuisQuinnonez/Conexiones-Raspberry-Pi-Pico-W/assets/158366686/2af309f7-1f94-4b4e-b628-011f5860db62)



### Código
```C
#include <TinyMLShield.h>

bool commandRecv = false; // flag used for indicating receipt of commands from serial port
bool liveFlag = false; // flag as true to live stream raw camera bytes, set as false to take single images on command
bool captureFlag = false;

// Image buffer;
byte image[176 * 144 * 2]; // QCIF: 176x144 x 2 bytes per pixel (RGB565)
int bytesPerFrame;

void setup() {
  Serial.begin(9600);
  while (!Serial);

  initializeShield();

  // Initialize the OV7675 camera
  if (!Camera.begin(QCIF, RGB565, 1, OV7675)) {
    Serial.println("Failed to initialize camera");
    while (1);
  }
  bytesPerFrame = Camera.width() * Camera.height() * Camera.bytesPerPixel();

  Serial.println("Welcome to the OV7675 test\n");
  Serial.println("Available commands:\n");
  Serial.println("single - take a single image and print out the hexadecimal for each pixel (default)");
  Serial.println("live - the raw bytes of images will be streamed live over the serial port");
  Serial.println("capture - when in single mode, initiates an image capture");
}

void loop() {
  int i = 0;
  String command;

  bool clicked = readShieldButton();
  if (clicked) {
    if (!liveFlag) {
      if (!captureFlag) {
        captureFlag = true;
      }
    }
  }

  // Read incoming commands from serial monitor
  while (Serial.available()) {
    char c = Serial.read();
    if ((c != '\n') && (c != '\r')) {
      command.concat(c);
    } 
    else if (c == '\r') {
      commandRecv = true;
      command.toLowerCase();
    }
  }

  // Command interpretation
  if (commandRecv) {
    commandRecv = false;
    if (command == "live") {
      Serial.println("\nRaw image data will begin streaming in 5 seconds...");
      liveFlag = true;
      delay(5000);
    }
    else if (command == "single") {
      Serial.println("\nCamera in single mode, type \"capture\" to initiate an image capture");
      liveFlag = false;
      delay(200);
    }
    else if (command == "capture") {
      if (!liveFlag) {
        if (!captureFlag) {
          captureFlag = true;
        }
        delay(200);
      }
      else {
        Serial.println("\nCamera is not in single mode, type \"single\" first");
        delay(1000);
      }
    }
  }
  
  if (liveFlag) {
    Camera.readFrame(image);
    Serial.write(image, bytesPerFrame);
  }
  else {
    if (captureFlag) {
      captureFlag = false;
      Camera.readFrame(image);
      Serial.println("\nImage data will be printed out in 3 seconds...");
      delay(3000);
      for (int i = 0; i < bytesPerFrame - 1; i += 2) {
        Serial.print("0x");
        Serial.print(image[i+1], HEX);
        Serial.print(image[i], HEX);
        if (i != bytesPerFrame - 2) {
          Serial.print(", ");
        }
      }
      Serial.println();
    }
  }
}
```

