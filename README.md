# Arduino-distanceSensor
This code tells how we can measure distance between two objects using an UltraSonic sensor and display the distance on an I2C lcd.

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd = LiquidCrystal_I2C(0x3F, 16, 2);

int trigPin = 2;

int echoPin = 4;

long duration;

long distance_cm;

void setup() {
  lcd.begin();
  lcd.backlight();
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  lcd.setCursor(0, 0);
  lcd.print("   Welcome to ");
  lcd.setCursor(0, 1);
  lcd.print("   RAYTECH");
  delay(2000);

}

void loop() {
  ULTRASONIC();
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distance cm: ");
  lcd.print(distance_cm);

  if (distance_cm <= 6){
      lcd.setCursor(0,1);
      lcd.print("Too Close!");
    }

  if (distance_cm >= 200){
       lcd.setCursor(0,1);
       lcd.print("Too High!");
    }

}

void ULTRASONIC(){
  digitalWrite(trigPin, LOW);
  digitalWrite(trigPin, HIGH);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);

  distance_cm = duration / 29 / 2;
}
