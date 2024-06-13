# PRACTICA6b

## Codi sencer
```cpp
#include <Arduino.h>
#include <MFRC522.h>

#define RST_PIN 9    //Pin 9 para el reset del RC522 
#define SS_PIN 10   //Pin 10 para el SS (SDA) del RC522 
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522 

void setup() {
  Serial.begin(9600); //Iniciamos la comunicación  serial 
  SPI.begin();        //Iniciamos el Bus SPI 
  mfrc522.PCD_Init(); // Iniciamos  el MFRC522 
  Serial.println("Lectura del UID"); 
}

void loop() {
// Revisamos si hay nuevas tarjetas  presentes 
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
        // Terminamos la lectura de la tarjeta  actual 
        mfrc522.PICC_HaltA();
      }
  }
}
```

## Explicació del codi per parts
### Biblioteques
```cpp
#include <Arduino.h>
#include <MFRC522.h>
```
Aquestes línies inclouen les llibreries necessàries per utilitzar l'ecosistema d'Arduino i la llibreria MFRC522 per treballar amb el lector de targetes RFID.

### Definició de Pins i Creació de l'Objecte MFRC522
```
#define RST_PIN 9    // Pin 9 per al reset del RC522 
#define SS_PIN 10    // Pin 10 per al SS (SDA) del RC522 
MFRC522 mfrc522(SS_PIN, RST_PIN); // Creem l'objecte per al RC522 
```
- RST_PIN defineix el pin de reset del lector RC522.
- SS_PIN defineix el pin de selecció de xip (SDA) del lector RC522.
- mfrc522 és un objecte de la classe MFRC522 creat amb els pins de selecció de xip i de reset especificats.

### Setup
```cpp
void setup() {
  Serial.begin(9600); // Iniciem la comunicació sèrie 
  SPI.begin();        // Iniciem el bus SPI 
  mfrc522.PCD_Init(); // Iniciem el MFRC522 
  Serial.println("Lectura del UID"); 
}
```
- Serial.begin(9600) inicia la comunicació sèrie amb una velocitat de 9600 bauds.
- SPI.begin() inicia la interfície SPI.
- mfrc522.PCD_Init() inicialitza el mòdul MFRC522.
- Serial.println("Lectura del UID") escriu un missatge a la consola sèrie per indicar que s'iniciarà la lectura de l'UID.

### Loop
```cpp
void loop() {
  // Revisem si hi ha noves targetes presents 
  if (mfrc522.PICC_IsNewCardPresent()) {   
    // Seleccionem una targeta 
    if (mfrc522.PICC_ReadCardSerial()) { 
      // Enviem serialment el seu UID 
      Serial.print("Card UID:"); 
      for (byte i = 0; i < mfrc522.uid.size; i++) { 
        Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "); 
        Serial.print(mfrc522.uid.uidByte[i], HEX);  
      }
      Serial.println(); 
      // Acabem la lectura de la targeta actual 
      mfrc522.PICC_HaltA();
    }
  }
}
```
- mfrc522.PICC_IsNewCardPresent() comprova si hi ha una nova targeta RFID present.
- Si es detecta una nova targeta, mfrc522.PICC_ReadCardSerial() selecciona la targeta i llegeix el seu UID.
- Si es llegeix correctament l'UID, aquest es mostra a la consola sèrie caràcter per caràcter en format hexadecimal.
- Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "); afegeix un zero inicial si el valor és menor que 0x10 per assegurar que cada byte es mostri amb dos dígits.
- mfrc522.PICC_HaltA() atura la comunicació amb la targeta actual.
