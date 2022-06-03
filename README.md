# Johny Silva Mendes
# PRACTICA 6: Buses de comunicación II (SPI)

**Ejercicio Practico 2 LECTURA DE ETIQUETA RFID**

### Código

```cpp
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN 9 //Pin 9 para el reset del RC522
#define SS_PIN 10 //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522

void setup() {
  Serial.begin(9600); //Iniciamos la comunicación serial
  SPI.begin(); //Iniciamos el Bus SPI
  mfrc522.PCD_Init(); // Iniciamos el MFRC522
  Serial.println("Lectura del UID");
}

void loop() {
  // Revisamos si hay nuevas tarjetas presentes
  if ( mfrc522.PICC_IsNewCardPresent())
    {
    //Seleccionamos una tarjeta
      if ( mfrc522.PICC_ReadCardSerial())
      {
          // Enviamos serialemente su UID
          Serial.print("Card UID:");
          for (byte i = 0; i < mfrc522.uid.size; i++) {
            Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
            Serial.print(mfrc522.uid.uidByte[i], HEX);
          }
          Serial.println();
          // Terminamos la lectura de la tarjeta actual
          mfrc522.PICC_HaltA();
      }
    }
}
```

### Funcionamiento del programa

Para leer la tarjeta RFID usaremos la librería *MFRC522.h*
Se especifican los pines Reset y SDA del módulo. 

```cpp
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN 9 //Pin 9 para el reset del RC522
#define SS_PIN 10 //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522

```

**Setup:** en primer lugar se inicia el puerto serie. Luego se inicia el bus SPI y se llama a la función mfrc522.PCD_Init(), la cual se encarga de iniciar y configurar el RC522 para su lectura. Finalmente se imprime por pantalla.

```cpp

void setup() {
  Serial.begin(9600); //Iniciamos la comunicación serial
  SPI.begin(); //Iniciamos el Bus SPI
  mfrc522.PCD_Init(); // Iniciamos el MFRC522
  Serial.println("Lectura del UID");
}
```
**Loop:** primeramente se revisa si se ha leído la trarjeta RFID correctamente con la sentencia *mfrc522.PICC_IsNewCardPresent()*. Luego, *mfrc522.PICC_ReadCardSerial()* devuelve si se ha podido seleccionar una tarjeta RFID parar ser utilizada. True indica que se ha encontrado la tarjeta, se crea un bucle "for" para imprimir por pantalla el el código de identificación y finalmente se cierra la lectura de la tarjeta RFID.

```cpp
void loop() {
  // Revisamos si hay nuevas tarjetas presentes
  if ( mfrc522.PICC_IsNewCardPresent())
    {
    //Seleccionamos una tarjeta
      if ( mfrc522.PICC_ReadCardSerial())
      {
          // Enviamos serialemente su UID
          Serial.print("Card UID:");
          for (byte i = 0; i < mfrc522.uid.size; i++) {
            Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
            Serial.print(mfrc522.uid.uidByte[i], HEX);
          }
          Serial.println();
          // Terminamos la lectura de la tarjeta actual
          mfrc522.PICC_HaltA();
      }
    }
}
```

### Foto del Montaje 
(carpeta .MD)
