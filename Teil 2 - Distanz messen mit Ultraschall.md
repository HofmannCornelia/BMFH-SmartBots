# Teil 2 - Distanz messen mit Ultraschall
> [!NOTE]
> Du lernst, wie der Ultraschallsensor funktioniert, so dass dein Arduino die Distanz zu möglichen Hindernissen messen kann.

## Einführung
Damit unser Roboter auch "Smart" wird, muss er seine Umgebung wahrnehmen und passend reagieren können. In diesem Teil beschäftigen wir uns mit dem "Wahrnehmen".

## Aufgabe 2.1 Ultraschall Sensor dazubauen

Der Sensor ist auf einem kleinen Servomotor aufgebaut, so dass der Roboter später in verschiedene Richtungen "schauen" kann. Für den Moment interessieren wir uns nur für die Distanz geradeaus.

**Vorgehen**
1. Setze den Servomotor vorne auf deinen Roboter, so dass der Sensor möglichst geradeaus zeigt. Unterstütze dabei die Plexiglas-Scheibe von unten, dass sie nicht bricht.
2. Studiere das folgende Schema, und verbinde die Kabel des Servos und des Sensors mit dem Sensor Shield:
    - Der Servo hat drei kombinierte Kabel, diese gehen alle auf Pin 10 (S = Signal, V = + Spannung, G = Grund).
    - Der Ultraschall Sensor benötigt auch Energie (VCC -> V, GND -> G)
    - Plus, der Sensor muss auf Befehl ein Signal aussenden (Trig -> 4 S) und wird danach ein reflektiertes Echo hören (Echo -> 5 S)

3. Teste nun den folgenden Sketch "DistanzMessung":
```
/**********************************************
  Distanzmessung mit Ultraschall Sensor
**********************************************/


//---------------------------------------------
// Globale Konstanten für Motoren/Sensoren
//---------------------------------------------

// Digitale Output/Input Pins für Ultraschall-Sensor
#define TRIG 4
#define ECHO 5

// Werte für Distanzmessung
int lookLeft = 0; 
int lookRight = 0; 
int lookAhead = 0; 
int timeEcho;  


//---------------------------------------------
// Setup Funktion
//  Wird zur Initialisierung zu Beginn des
//  Programms ausgeführt
//---------------------------------------------
void setup() {
    

    // Die Sensoren-Pins für Trigger und Echo
    pinMode(TRIG, OUTPUT); 
    pinMode(ECHO, INPUT); 

    // damit der SerialPlotter funktioniert
    Serial.begin(9600);
}


//---------------------------------------------
// Loop Funktion
//   Wird nach setup() immer wieder ausgeführt.
//---------------------------------------------
void loop() {

     distanceAhead();
     Serial.println(lookAhead);
     delay(10);

}




/*****************************  Erkunden  *********************************/

            float distanceAhead(){  
                 // Es wird ein Ultraschall Puls der Länge
                 //   10 Mikrosekunden abgesetzt und wieder
                 //   empfangen. Damit das Signal klar und sauber
                 //   ist, wird zuerst 5 Mikrosekunden aus sichergestellt,
                 digitalWrite(TRIG, LOW);
                 delayMicroseconds(5);  
                 digitalWrite(TRIG, HIGH);
                 delayMicroseconds(10);
                 digitalWrite(TRIG, LOW);

                 // Puls empfangen: Die Funktion pulseIn() misst die
                 // Dauer in Mikrosekunden, bis ein Signal der Stärke
                 // HIGH empfangen wird.
                   timeEcho = pulseIn(ECHO, HIGH);

                 // Distanz (cm) mithilfe der Schallgeschwindigkeit berechnen
                   lookAhead = timeEcho/58;
                   return lookAhead;
                   }

            float distanceLeft(){   
                 digitalWrite(TRIG, LOW);
                 delayMicroseconds(5);    
                 digitalWrite(TRIG, HIGH);
                 delayMicroseconds(10);
                 digitalWrite(TRIG, LOW);
                 timeEcho = pulseIn(ECHO, HIGH);
                 lookLeft = timeEcho/58;
                 return lookLeft;
                 }

            float distanceRight(){     
                 digitalWrite(TRIG, LOW);
                 delayMicroseconds(5);     
                 digitalWrite(TRIG, HIGH);
                 delayMicroseconds(10);
                 digitalWrite(TRIG, LOW);
                   timeEcho = pulseIn(ECHO, HIGH);
                   lookRight = timeEcho/58;
                   return lookRight;
                   }
```
Die Zeile `Serial.println(lookAhead);` definiert, dass der Wert von `lookAhead` auf den SerialPlotter gesendet wird. Wir können somit live sehen, welche Distanzen der Ultraschall Sensor gerade misst.  
<img alt="SerialPlotter" src="https://github.com/user-attachments/assets/15f9660d-4068-42a5-acf2-d7f8460b839c"  style="width:40%; height:auto;"/>


