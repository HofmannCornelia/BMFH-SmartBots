# Teil 1 - Motoren steuern
> [!NOTE]
> Du lernst, wie du deinen Roboter in Bewegung bringst, und welche Befehle für welche Bewegung nötig sind.

## Einführung
Das kleinere blaue Board auf der Oberseite des SmartBots ist ein "Sensor Shield". Das ist quasi eine Verteilzentrale. Viele Pins stehen zur Verfügung, um unterschiedliche Geräte (Motoren, Sensoren, etc) zu verbinden, ohne ein zusätzliches Breadboard zu benötigen. 

Das Sensor Shield hat an der Unterseite Pins, welche zum Arduino Board passen.

Um unser Roboter in Schwung zu bringen, benutzen wir 2 Gleichstrom Motoren die mit 3 bis 6V gespeist werden können. Um den Strom zu den Motoren weiterzuleiten und um die Motoren in verschiedene Richtungen zu bewegen, brauchen wir einen Kontroller. Das ist das rote Board an der Unterseite.
> [!WARNING]
> Vorsicht beim hochheben/umdrehen, das blaue Board an oder Oberseite ist nicht festgemacht.

<img alt="ArduinoBoard" src="https://github.com/user-attachments/assets/586bad59-0197-4562-bc5e-73aed8b9d8bc"  style="width:60%; height:auto;">

## Aufgabe 1.1

**Vorgehen**
1. Verbinde das Arduino Uno Board mit dem "Sensor Shield". Achte darauf, dass du bei den ersten Pins (Digital 0, Analog A5) beginnst.
<img alt="ArduinoBoard" src="https://www.electronicajnc.com/web/wp-content/uploads/2023/11/shiel-v5.jpg6_-600x577.jpg"  style="width:50%; height:auto;">

3. Starte ein neuer Sketch und Speichere ihn als "MotorenAnsteuern".

```
// Motor 1: welche Seite?

int in1 = 9;
int in2 = 8;

// Motor 2: welche Seite?
int in3 = 7;
int in4 = 6;



void setup()

{
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
}

void loop()

{

  digitalWrite(in1, HIGH);  // Motor 1 beginnt zu rotieren
  digitalWrite(in2, LOW);
  delay(1000);

  digitalWrite(in1, LOW);  // Motor 1 stoppt
  digitalWrite(in2, LOW);

  digitalWrite(in3, HIGH);  // Motor 2 beginnt zu rotieren
  digitalWrite(in4, LOW);
  delay(1000);

  digitalWrite(in3, LOW);  // Motor 2 stoppt
  digitalWrite(in4, LOW);

  digitalWrite(in1, LOW);   // Durch die Veränderung von HIGH auf LOW (bzw. LOW auf HIGH) wird die Richtung der Rotation verändert.
  digitalWrite(in2, HIGH);  
  delay(1000);

  digitalWrite(in1, LOW);  // Motor 1 stoppt
  digitalWrite(in2, LOW);

  digitalWrite(in3, LOW);   // Durch die Veränderung von HIGH auf LOW (bzw. LOW auf HIGH) wird die Richtung der Rotation verändert.
  digitalWrite(in4, HIGH);
  delay(1000);

  digitalWrite(in1, LOW);   // Anschließend sollen die Motoren 2 Sekunden ruhen.
  digitalWrite(in2, LOW);  
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
  delay(2000);

}
```
3. Kontrolliere, dass dein aktuelles Arduino Board verbunden und richtig gewählt ist.
4. Überprüfe deinen Sketch mit der Häkchen-Taste und lade den Code auf das Board hoch.
5. Trenne die USB-Verbindung, und schalte danach die Batterie-Versorgung an.
6. Beobachte was mit den Motoren passiert.

> [!WARNING]
> Aufpassen dass dein Roboter entweder auf dem Tisch auf den zwei Bechern hochgestellt ist, oder setze ihn auf den Boden. Allerdings fährt er aktuell noch blind dein vorgegebenes Programm ab, und könnte daher in ein Hindernis fahren.

## Aufgabe 1.2
Wenn wir kompliziertere Wege fahren, ist es mühsam immer alle 4 Pins einzeln richtig zu setzen. Statdessen definieren wir Funktionen, welche für die verschiedenen Bewegungen die nötigen Befehle auf die Pins zusammenfassen.

Studiere dazu den folgenden Sketch "MotorenFunktionen":
```
/**********************************************
  Motoren Funktionen
  Zwei DC Motoren kontrollieren
**********************************************/

//---------------------------------------------
// Globale Konstanten für Motoren
//---------------------------------------------

// Digitale Output Pins für die Motoren
//   Jeder Motor wird über zwei Pins gesteuert.

// Motor 1, links
int in1 = 9;  // Vorwärts
int in2 = 8;  // Rückwärts

// Motor 2, rechts
int in3 = 7;  // Vorwärts
int in4 = 6;  // Rückwärts


//---------------------------------------------
// Setup Funktion
//  Wird zur Initialisierung zu Beginn des
//  Programms ausgeführt
//---------------------------------------------
void setup() {
  // // Die Motoren-Pins für Output initialisieren.
    pinMode(in1, OUTPUT);
    pinMode(in2, OUTPUT); 
    pinMode(in3, OUTPUT);
    pinMode(in4, OUTPUT);
}


//---------------------------------------------
// Loop Funktion
//   Wird nach setup() immer wieder ausgeführt.
//---------------------------------------------
void loop() {
  
// vorwärts fahren
  goAhead();
  delay(1000);
  
  // anhalten
  stopRobot();
  delay(1000);

// rückwärts fahren
  goBack();
  delay(1000);

  // anhalten
  stopRobot();
  delay(1000);

  // nach rechts drehen
  turnRight();
  delay(200);
  
  // anhalten
  stopRobot();
  delay(1000);

  // nach links drehen
  turnLeft();
  delay(200);
  
  // anhalten
  stopRobot();
  delay(1000);


}

/*****************************  Steuerbefehle  *********************************/
          //---------------------------------------------
          // goAhead Funktion  
          //   Beide Motoren drehen vorwärts
          //---------------------------------------------
            void goAhead(){ 
                 digitalWrite(in1, HIGH); 
                 digitalWrite(in2, LOW);
                 digitalWrite(in3, HIGH);    
                 digitalWrite(in4, LOW);
                 }

          //---------------------------------------------
          // goBack Funktion  
          //   Beide Motoren drehen rückwärts
          //---------------------------------------------
            void goBack(){ 
                 digitalWrite(in1, LOW); 
                 digitalWrite(in2, HIGH);
                 digitalWrite(in3, LOW);    
                 digitalWrite(in4, HIGH);
                 }

          //---------------------------------------------
          // turnRight Funktion
          //   Linker Motor vorwärts, rechter Motor rückwärts
          //---------------------------------------------
            void turnRight(){ 
                 digitalWrite(in1, HIGH); 
                 digitalWrite(in2, LOW);
                 digitalWrite(in3, LOW);    
                 digitalWrite(in4, HIGH);
                 }

          //---------------------------------------------
          // turnLeft Funktion
          //   Linker Motor rückwärts, rechter Motor vorwärts
          //---------------------------------------------     
            void turnLeft(){
                 digitalWrite(in1, LOW); 
                 digitalWrite(in2, HIGH);
                 digitalWrite(in3, HIGH);    
                 digitalWrite(in4, LOW);
                 }

          //---------------------------------------------
          // stopRobot Funktion
          //   Hält beide Motoren an.
          //---------------------------------------------
            void stopRobot(){  
                 digitalWrite(in1, LOW); 
                 digitalWrite(in2, LOW);
                 digitalWrite(in3, LOW);    
                 digitalWrite(in4, LOW);
                 }
```

## Aufgabe 1.3

Dein Roboter soll jetzt Slalom fahren. 

Ändere den Inhalt der `loop` Funktion im Sketch aus Aufgabe 1.2 so ab, dass dein Roboter jeweils eine Sekunde geradeaus fährt, dann eine Linksdrehung macht, wieder geradeaus fährt und dann eine Rechtsdrehung macht. Dann soll die Abfolge wieder von vorne beginnen. 

Überprüfe deinen Sketch mit der Häkchen-Taste und lade den Code auf das Board hoch.

Hilfe: geradeaus fährt der Roboter, wenn beide Räder vorwärts drehen. Eine Linksdrehung entsteht, wenn das rechte Rad vorwärts dreht und das linke Rad rückwärts. Umgekehrt für die Rechtsdrehung. 
