# Practica-3B
## Part B: Comunicació bluetooth amb el mòbil 
### Codi complet
```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"

#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

BluetoothSerial SerialBT;

void setup() {
Serial.begin(115200);
SerialBT.begin("ESP32test2"); //Bluetooth device name
Serial.println("The device started, now you can pair it with bluetooth!");
}

void loop() {
if (Serial.available()) {
SerialBT.write(Serial.read());
}
if (SerialBT.available()) {
Serial.write(SerialBT.read());
}
delay(20);
}
```
### Funcionament per parts del codi

__1. Inclusió de Biblioteques__
```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"

```
La nova biblioteca que utilitzem per primera vegada en aquesta pràctica es BluetoothSerial.
Aquesta biblioteca ens permet comunicar en sèrie dispositius disponivbles mitjançant Bluetooth, cas del ESP32 i un telèfon mòbil.

__2. Verificació de Configuració de Bluetooth__
```cpp
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
```
Aquesta part del codi verifica si les configuracions de Bluetooth estan habilitades (CONFIG_BT_ENABLED i CONFIG_BLUEDROID_ENABLED).
Si no es el cas, es genera un error i comunica a l'usuari que ha d'executar en el terminal `make menuconfig` per habilitar les configuracions de Bluetooth.

__3. Declaració de l'Objecte Bluetooth__
```cpp
BluetoothSerial SerialBT;
}
```
Es declara l'objecte SerialBT, objecte que esta definit dintre la biblioteca BluetoothSerial.Es declara aquest objecte per gestionar la comunicació Bluetooth.

__4. Setup__
```cpp
void setup() {
Serial.begin(115200);
SerialBT.begin("ESP32test2"); //Bluetooth device name
Serial.println("The device started, now you can pair it with bluetooth!");
}
}
```
S'estableix la configuració inicial del programa:
- Serial.begin(115200): Inicialitza la comunicació sèrie en 115200 bauds.
- SerialBT.begin("ESP32test2"): Inicialitza la conexió Bluetooth i ens indica el nom del dispositiu que hem de buscar.
- Serial.println: Imprimeix per pantalla que el dispositiu es disponible per linkar amb bluetooth.
  
__5. Loop__
```cpp
void loop() {
if (Serial.available()) {
SerialBT.write(Serial.read());
}
if (SerialBT.available()) {
Serial.write(SerialBT.read());
}
delay(20);
}
```
- if (Serial.available()) {...}: .Comprova si hi han dades disponibles en el port sèrie(com pot haver-hi en el monitor serial).
- SerialBT.write(Serial.read());: Si hi ha  dades disponibles, les llegeix des del port sèrie i les envía a través de Bluetooth mitjançant l'objecte SerialBT. 
- if (SerialBT.available()) {...}: Comprova si hhi han dades disponibles a través de Bluetooth (dades enviades des del dispositiu emparellat).
- Serial.write(SerialBT.read());: Si hi han dades disponibles ,les llegeix des del dispositiu connectat per Bluetooth i les envie al port sèrie per poder mostrarles pel monitor.
- delay(20);: Introduce un pequeño retraso de 20 milisegundos en cada iteración del loop para evitar una lectura o escritura excesivamente rápida, lo que ayuda a estabilizar la comunicación. Introduiex un petit retard de 20 msen cada iteració per evitar una lectura o escriptura massa ràpida i que sigui mes estable la comunicació entre dispositius.

Basicament les dades que escribim ja sigui pel monitor serial o per el dispositiu emparellat per Bluetooth, les sortides dels dos dispositius mostren el mateix, ja sigui llegint les dades d'un canal i fent la transmissió com rebent les dades i mostrant-les pel monitor del dispositiu.
