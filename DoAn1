#include "DHT.h"  
#include <SoftwareSerial.h>  
  
const int DHTPIN = 7;  
const int DHTTYPE = DHT11;  
int relaydat = 2;  
int cbdat = A4;  
int doc_cb, TBcb;  
int relaykk = 3;  
DHT dht(DHTPIN, DHTTYPE);  
  
// Bluetooth control variables  
char mode = 't';  
char relaydatState = 'b';  
char relaykkState = 'y';  
  
void setup()  
{  
  Serial.begin(9600);  
  pinMode(cbdat, INPUT);  
  pinMode(relaydat, OUTPUT);  
  pinMode(relaykk, OUTPUT);  
  digitalWrite(relaydat, LOW);  
  digitalWrite(relaykk, LOW);  
  dht.begin();  
}  
  
void loop()  
{  
  // Bluetooth control  
  if (Serial.available() > 0)  
  {  
    char command = Serial.read();  
    handleBluetoothCommand(command);  
  }  
  
  for (int i = 0; i <= 9; i++)  
    {  
      doc_cb += analogRead(cbdat);  
    }  
    TBcb = doc_cb / 10;  
    int phantramao = map(TBcb, 0, 1023, 0, 100);  
    int phantramthuc = 100 - phantramao;  
    doc_cb = 0;  
  
    int t1 = dht.readTemperature();  
  
  // Automatic mode  
  if (mode == 't')  
  {  
    Serial.println(t1);  
    Serial.println(phantramthuc);  
    if (phantramthuc >= 55)  
    {  
      digitalWrite(relaydat, LOW);  
      Serial.println("OFF");  
    }  
    else  
    {  
      digitalWrite(relaydat, HIGH);  
      Serial.println("ON");  
    }  
  
    if (t1 < 35)  
    {  
      digitalWrite(relaykk, LOW);  
      Serial.println("OFF");  
    }  
    else  
    {  
      digitalWrite(relaykk, HIGH);  
      Serial.println("ON");  
    }  
  }  
  
  // Manual mode  
  if (mode == 'h')  
  {  
    Serial.println(t1);  
    Serial.println(phantramthuc);  
    if (relaydatState == 'a')  
    {  
      digitalWrite(relaydat, HIGH);  
      Serial.println("ON");  
    }  
    else if (relaydatState == 'b')  
    {  
      digitalWrite(relaydat, LOW);  
      Serial.println("OFF");  
    }  
  
    if (relaykkState == 'x')  
    {  
      digitalWrite(relaykk, HIGH);  
      Serial.println("ON");  
    }  
    else if (relaykkState == 'y')  
    {  
      digitalWrite(relaykk, LOW);  
      Serial.println("OFF");  
    }  
  }  
  delay(1000);  
}  
  
void handleBluetoothCommand(char command)  
{  
  switch (command)  
  {  
    case 't': // Automatic mode  
      mode = 't';  
      break;  
    case 'h': // Manual mode  
      mode = 'h';  
      break;  
    case 'a': // Turn on relaydat  
      relaydatState = 'a';  
      break;  
    case 'b': // Turn off relaydat  
      relaydatState = 'b';  
      break;  
    case 'x': // Turn on relaykk  
      relaykkState = 'x';  
      break;  
    case 'y': // Turn off relaykk  
      relaykkState = 'y';  
      break;  
    default:  
      break;  
  }  
}  
