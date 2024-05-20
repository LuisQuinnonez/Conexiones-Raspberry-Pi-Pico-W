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
![]()
### Código

```C
void setup() {
  // Inicializa el monitor serial a una velocidad de 9600 baudios
  Serial.begin(9600);
}

void loop() {
  // Imprime "Hola mundo" en el monitor serial
  Serial.println("Hola mundo");
  // Espera 1 segundo antes de repetir el bucle
  delay(1000);
}
```

## Práctica 1 - Desplegar "Hello World" C#
---
![]()

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
![]()

### Código

```C

```

## Práctica 5 - ChatGPT
---
![]()
### Código

```C

```

