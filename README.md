#include <Servo.h>

#define trigPin 9
#define echoPin 8
#define ldrPin A0
#define ledPin 7

Servo servo;
int threshold = 70;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);
  servo.attach(6);
}

void loop() {
  long duration, distance;
  
  // Ultrasonic sensor part
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) / 29.1; 
  
  if (distance < 80) {
    Serial.print(distance);
    Serial.println(" cm");
    servo.write(90);
  } else if (distance < 180) {
    Serial.print(distance);
    Serial.println(" cm");
    servo.write(180);
  } else {
    Serial.println("The distance is more than 180cm");
  }

  // Light sensor part
  int data = analogRead(ldrPin);
  Serial.println("");
  Serial.print("Light Sensor ");
  Serial.print("Value = ");
  Serial.print(data);
  if (data <= threshold) {
    digitalWrite(ledPin, HIGH);
  } else {
    digitalWrite(ledPin, LOW);
  }

  delay(500);
}
