# SYTI-Bericht
---
### Fach: SYTI
### Name: Hauser Alexander
### Klasse: 4AHITS
### Datum: 06.11.2025
---

## Inhaltzverzeichnis

1.Aufgabe 1
   1.1 Aufgabenstellung
   1.2 Sourcecode
   1.3 Erklärung

2. Aufgabe 2
   2.1 Aufgabenstellung
   2.2 Sourcecode
   2.3 Erklärung

3. Aufgabe 3
   3.1 Aufgabenstellung
   3.2 Sourcecode
   3.3 Erklärung

4. Aufgabe 4
   4.1 Aufgabenstellung
   4.2 Sourcecode
   4.3 Erklärung


## Aufgaben 

![](../Images/Aufgabe_Bild)

## Aufgabe 1

#### Aufgabenstellung:
Der Zustand der Tasten soll durch die LED-Reihe angezeigt werden: Solange Taste 0 gedrückt ist,
leuchtet LED 0 usw.

#### Sourcecode:
```
void setup() {
  DDRF = 0xFF;
  DDRK  = 0x00;
}

void loop() {
  
  PORTF = ~PINK;
}
``` 
#### Erklärung:
Im setup() wird festgelegt, dass PORTF ein Ausgangsport ist und PORTK (PINK) als Eingang genutzt wird.
In der loop()-Funktion wird dann einfach der komplette Eingangswert von PINK eingelesen, bitweise invertiert und direkt auf PORTF ausgegeben.
Das bedeutet: Jedes Bit, das am Eingang 0 ist, wird am Ausgang zu 1, und jedes Eingangssignal 1 wird zu 0.

## Aufgabe 2

#### Aufgabenstellung:
Modifizierung der vorherigen Aufgabe: Der Zustand der Taste 0 wird durch LED 7 angezeigt, der
Zustand von Taste 1 von LED 6 usw. => soll auch bei mehreren Tastendrücken gleichzeitig
funktionieren!

#### Sourcecode:
```
void setup() {
  DDRF = 0xFF;
  DDRK  = 0x00;
}
void loop() {
  
  int i = 0;
  while (i < 8) {
    if ((PINK & (1 << i)) == 0) {
      PORTF |= (1 << (7 - i));
    }
    else {
      PORTF &= ~(1 << (7 - i));
    }
    i++;
  }
}
 
``` 
#### Erklärung:
Der Code geht alle acht Bitpositionen von PINK der Reihe nach durch. Für jedes Bit prüft er, ob es den Wert 0 hat.
Wenn ja, setzt er das entsprechende gespiegelte Bit in PORTF auf 1. Wenn das Bit in PINK den Wert 1 hat, löscht er das gespiegelte Bit in PORTF, also setzt es auf 0.
Auf diese Weise werden die Bits von PINK der Reihenfolge nach gelesen und in umgekehrter Reihenfolge – also gespiegelt – in PORTF eingetragen.

## Aufgabe 3

#### Aufgabenstellung:
Ausgabe von LED-Mustern: Solange Taste 0 gedrückt wird, wird das LED-Muster Nr. 0 ausgegeben, 
solange Taste 1 gedrückt wird, LED-Muster 1, usw. (z. B. Muster 0 = 00000000, Muster 1 = 10000000, 
… Muster 7 = 11111111)

#### Sourcecode:
```
void setup() {
  DDRF = 0xFF;
  DDRK  = 0x00;
}
uint8_t ArrayMuster[8] = {
    0b00000001,  
    0b00000011, 
    0b00000111,  
    0b00001111,  
    0b00011111,  
    0b00111111,  
    0b01111111,  
    0b11111111   
  };

  uint8_t m = PINK;

  for(int i = 0;i < 8; i++)
  {
    if(m & (1 << i))
    {
      PORTF &= ~ArrayMuster[i];
    }
    else{
      PORTF |= ArrayMuster[i];
    }
  }
```

#### Erklärung:
Das Array ArrayMuster speichert acht verschiedene LED-Muster als Binärzahlen.
Mit PINK wird ausgelesen, welche Taste gedrückt ist.
Die for-Schleife prüft nacheinander alle acht Tasten.
Wenn eine Taste gedrückt ist, wird das zugehörige LED-Muster aus dem Array genommen.
Dieses Muster wird dann über PORTF auf die LEDs ausgegeben.
So leuchtet immer genau das LED-Muster, das zur gedrückten Taste gehört.

### Aufgabe 4

#### Aufgabenstellung
Flip-Flop: Durch Drücken der Taste 6 wird LED 0 eingeschaltet, durch Drücken der Taste 7 
ausgeschaltet. 
Die Aufgabe soll mittels Zustandsdiagramm gelöst werden: 2 Zustände: LED leuchtet / LED ist 
gelöscht. Von einem Zustand zum anderen kommt man durch den entsprechenden Tastendruck.

#### Sourcecode
```
	DDRF |= (1 << 0);

    
    DDRK  &= ~((1 << 6) | (1 << 7));
    PORTK |=  (1 << 6) | (1 << 7);

    while(1)
    {
        switch(status)
        {
            case LED_AUS:
               
                PORTF &= ~(1 << 0);

              
                if (!(PINK & (1 << 6))) {
                    status = LED_AN;    
                }
                break;

            case LED_AN:
                PORTF |= (1 << 0);

                
                if (!(PINK & (1 << 7))) {
                    status = LED_AUS;   
                }
                break;
        }
    }

```

#### Erklärung:
Mit DDRF |= (1 << 0) wird LED 0 als Ausgang eingestellt.
Mit DDRK werden die Pins für Taste 6 und Taste 7 als Eingänge gesetzt.
PORTK aktiviert die Pull-Up-Widerstände für beide Tasten.
Die while-Schleife läuft dauerhaft.
Im Zustand LED_AUS wird die LED ausgeschaltet.
Wird Taste 6 gedrückt, wechselt der Zustand zu LED_AN.
Im Zustand LED_AN wird die LED eingeschaltet.
Wird Taste 7 gedrückt, wechselt der Zustand zurück zu LED_AUS.
