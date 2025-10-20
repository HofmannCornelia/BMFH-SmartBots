# Teil 3 - alles zusammensetzen
> [!NOTE]
> Du lernst, wie dein Roboter zu einem SmartBot wird, und Hindernissen ausweicht.

## Einführung
Der Roboter soll gerade aus fahren, solange kein Hindernis in der Nähe ist. Bevor er aber in ein Hindernis fährt, soll er stoppen, die freie Distanz nach rechts und links messen, und dann passend reagieren. Das kann bedeuten: 
- nach rechts oder links drehen, wenn dort mehr freier Platz ist
- oder rückwärts fahren (wenn keine Seite ideal ist)

## Aufgabe 3.1 
Starte ein neuer Sketch "Erkunden"
```
/**********************************************
  Distanz in verschiedene Richtungen Erkunden
  beste Richtung auswählen, und fahren
**********************************************/

// Servo Motor braucht zusätzlicher Code
#include <Servo.h>

//---------------------------------------------
// Globale Konstanten für Motoren/Sensoren
//---------------------------------------------

// Digitale Output Pins für die Motoren
//   Jeder Motor wird über zwei Pins gesteuert.

// Motor 1, links
int in1 = 9;  // Vorwärts
int in2 = 8;  // Rückwärts

// Motor 2, rechts
int in3 = 7;  // Vorwärts
int in4 = 6;  // Rückwärts

// Digitale Output/Input Pins für Ultraschall-Sensor
#define TRIG 4
#define ECHO 5

// Werte für Distanzmessung
int lookLeft = 0;
int lookRight = 0;
int lookAhead = 0;
int timeEcho;

// Maximale und minimale Distanz
const int US_MAX_DISTANZ = 60;
const int US_MIN_DISTANZ = 2;
const int US_PULSEIN_TIMEOUT = US_MAX_DISTANZ * 58.2;

// Servo Definitionen
Servo servo;
int angle = 90;


//---------------------------------------------
// Setup Funktion
//  Wird zur Initialisierung zu Beginn des
//  Programms ausgeführt
//---------------------------------------------
void setup() {
  // Die Motoren-Pins für Output initialisieren.
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);

  // Servo ist an Pins 10 angeschlossen
  servo.attach(10);

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
  // Serial.println(lookAhead);

  if (lookAhead < 30) {
    stopRobot();
    delay(200);
    goBack();
    delay(200);
    stopRobot();
    delay(200);

    for (angle = 90; angle >= 10; angle--) {
      servo.write(angle);
      delay(10);
    }
    delay(500);
    distanceRight();
    // Serial.println(lookRight);
    delay(500);


    for (angle = 10; angle <= 170; angle++) {
      servo.write(angle);
      delay(10);
    }
    delay(500);
    distanceLeft();
    // Serial.println(lookLeft);
    delay(500);

    for (angle = 170; angle >= 90; angle--) {
      servo.write(angle);
      delay(10);
    }
    delay(500);

    if (lookLeft < lookRight) {
      turnRight();
      delay(200);
      stopRobot();
    } else {
      turnLeft();
      delay(200);
      stopRobot();
    }
  } else {
    goAhead();
  }
}




/*****************************  Erkunden  *********************************/

void distanceAhead() {
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

  // Dauert es zu lange, bis der Puls zurückkommt, dann ist
  // dauer=0, sollte aber US_PULSEIN_TIMEOUT sein:
  if (timeEcho <= 0)
  {
    timeEcho = US_PULSEIN_TIMEOUT;
  }

  // Distanz (cm) mithilfe der Schallgeschwindigkeit berechnen
  lookAhead = timeEcho / 58;
}

void distanceLeft() {
  digitalWrite(TRIG, LOW);
  delayMicroseconds(5);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);

  timeEcho = pulseIn(ECHO, HIGH);
  if (timeEcho <= 0)
  {
    timeEcho = US_PULSEIN_TIMEOUT;
  }
  lookLeft = timeEcho / 58;
}

void distanceRight() {
  digitalWrite(TRIG, LOW);
  delayMicroseconds(5);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);

  timeEcho = pulseIn(ECHO, HIGH);
  if (timeEcho <= 0)
  {
    timeEcho = US_PULSEIN_TIMEOUT;
  }
  lookRight = timeEcho / 58;
}


/*****************************  Steuerbefehle  *********************************/
//---------------------------------------------
// goAhead Funktion
//   Beide Motoren drehen vorwärts
//---------------------------------------------
void goAhead() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}

//---------------------------------------------
// goBack Funktion
//   Beide Motoren drehen rückwärts
//---------------------------------------------
void goBack() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
}

//---------------------------------------------
// turnRight Funktion
//   Linker Motor vorwärts, rechter Motor rückwärts
//---------------------------------------------
void turnRight() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
}

//---------------------------------------------
// turnLeft Funktion
//   Linker Motor rückwärts, rechter Motor vorwärts
//---------------------------------------------
void turnLeft() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}

//---------------------------------------------
// stopRobot Funktion
//   Hält beide Motoren an.
//---------------------------------------------
void stopRobot() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}
```

Verstehst du alle Teile darin? 


## Aufgabe 3.2 
**Vorgehen**
1. Der Aufbau bleibt gleich wie zuvor, wir haben den Servomotor dort auch bereits verbunden.
3. Kontrolliere wie immer, dass dein aktuelles Arduino Board richtig verbunden und gewählt ist
4. Überprüfe den Sketch mit der Häkchen-Taste und lade den Code auf den Arduino hoch.
5. Trenne die USB-Verbindung, lass deinen SmartBot aber noch auf den Bechern hochgestellt, und schalte danach die Batterie-Versorgung an.
6. Nun kannst du zum Beispiel mit deiner Hand "Hindernisse" vor dem Ultraschall Sensor kreieren, und so überprüfen, ob dein SmartBot tatsächlich so reagiert, wie du erwartest.
7. Wenn nötig, studiere den Sketch nochmals, und passe ihn eventuell an.
8. Sobald das Austesten erfolgreich ist, kannst du versuchen, ob das auch frei auf dem Boden wie erhofft klappt, und dein SmartBot den zahlreichen Hindernissen ausweicht.
9. Eventuell musst du den Sketch nochmals optimieren, für diese Realsituation.
