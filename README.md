# CodigoArduinoIDE
codigo implementado en Sistema de Control de LEDs mediante ESP32 y Bluetooth Clásico con interfaz Android

#include "BluetoothSerial.h"

BluetoothSerial SerialBT;

const int LED1 = 2;
const int LED2 = 4;
const int LED3 = 5;
const int LED4 = 18;
const int LED5 = 19;
const int LED6 = 21;

int leds[] = {LED1, LED2, LED3, LED4, LED5, LED6};

void encenderHasta(int cantidad) {
  for (int i = 0; i < 6; i++) {
    digitalWrite(leds[i], i < cantidad ? HIGH : LOW);
  }
}

void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32_LEDs");
  Serial.println("Bluetooth listo. Esperando conexión...");

  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
  pinMode(LED3, OUTPUT);
  pinMode(LED4, OUTPUT);
  pinMode(LED5, OUTPUT);
  pinMode(LED6, OUTPUT);

  digitalWrite(LED1, LOW);
  digitalWrite(LED2, LOW);
  digitalWrite(LED3, LOW);
  digitalWrite(LED4, LOW);
  digitalWrite(LED5, LOW);
  digitalWrite(LED6, LOW);
}

void loop() {
  if (SerialBT.available()) {

    String mensaje = SerialBT.readStringUntil('\n');
    mensaje.trim();

    Serial.print("Comando recibido: ");
    Serial.println(mensaje);

    if (mensaje.length() > 1) {
      int cantidad = mensaje.toInt() / 10;
      encenderHasta(cantidad);

    } else {

      char comando = mensaje.charAt(0);
      switch (comando) {
        case '1':
          digitalWrite(LED1, !digitalRead(LED1));
          Serial.println("LED 1 alternado");
          break;
        case '2':
          digitalWrite(LED2, !digitalRead(LED2));
          Serial.println("LED 2 alternado");
          break;
        case '3':
          digitalWrite(LED3, !digitalRead(LED3));
          Serial.println("LED 3 alternado");
          break;
        case '4':
          digitalWrite(LED4, !digitalRead(LED4));
          Serial.println("LED 4 alternado");
          break;
        case '5':
          digitalWrite(LED5, !digitalRead(LED5));
          Serial.println("LED 5 alternado");
          break;
        case '6':
          digitalWrite(LED6, !digitalRead(LED6));
          Serial.println("LED 6 alternado");
          break;
        case '0':
          digitalWrite(LED1, LOW);
          digitalWrite(LED2, LOW);
          digitalWrite(LED3, LOW);
          digitalWrite(LED4, LOW);
          digitalWrite(LED5, LOW);
          digitalWrite(LED6, LOW);
          Serial.println("Todos los LEDs apagados");
          break;
      }
    }
  }
}

