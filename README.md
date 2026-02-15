## 📌 Project Overview

Designed and developed a 3D-printed Arduino-based autonomous robot featuring ultrasonic obstacle detection, DC motor navigation, servo-actuated hand gestures, and an integrated audio feedback system for interactive response.


---

## 🖼️ Project Image
[Robot](https://drive.google.com/file/d/1mh2VcMyGt_ZOIplin4JrBmI_d4WT3zTK/view)

---

## 🎥 Demo Video
[▶ Watch Robot Demo](https://drive.google.com/file/d/1L8jFxVd42CmjOmvcttKNTzPIrc6u4P4Q/view)

---

## ⚙️ Hardware Used
- Arduino Uno
- HC-SR04 Ultrasonic Sensor
- Servo Motor (Hand Movement)
- L298N Motor Driver
- 3D Printed Body (PLA)
- DFPlayer Mini

---

## 🧠 Features
✅ Autonomous obstacle detection  
✅ Automatic stop & navigation  
✅ Servo-based hand gestures  
✅ Custom fabricated structure  

---

## 💻 Code
See `robot_code.ino

#include <Servo.h>
int trigPin = 8;
int echoPin = 4;

int motor1 = 3;
int motor2 = 5;
int motor3 = 6;
int motor4 = 9;

int servoPin = 10;   // Hand servo

long time;
int distance;

Servo handServo;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(motor1, OUTPUT);
  pinMode(motor2, OUTPUT);
  pinMode(motor3, OUTPUT);
  pinMode(motor4, OUTPUT);

  handServo.attach(servoPin);
  handServo.write(90);  // Neutral position

  Serial.begin(9600);
}

void loop() {

  // Trigger ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo time
  time = pulseIn(echoPin, HIGH);

  // Calculate distance
  distance = (time * 0.034) / 2;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  delay(50);

  if (distance <= 40) {
    stopRobot();
    waveHand();        // 
    moveBackward();
    delay(1000);
    turnRight();
    delay(800);
  } 
  else {
    moveForward();
    handServo.write(90); // Keep hand neutral
  }
}

void moveForward() {
  digitalWrite(motor1, HIGH);
  digitalWrite(motor2, LOW);
  digitalWrite(motor3, HIGH);
  digitalWrite(motor4, LOW);
}

void moveBackward() {
  digitalWrite(motor1, LOW);
  digitalWrite(motor2, HIGH);
  digitalWrite(motor3, LOW);
  digitalWrite(motor4, HIGH);
}

void turnRight() {
  digitalWrite(motor1, HIGH);
  digitalWrite(motor2, LOW);
  digitalWrite(motor3, LOW);
  digitalWrite(motor4, LOW);
}

void stopRobot() {
  digitalWrite(motor1, LOW);
  digitalWrite(motor2, LOW);
  digitalWrite(motor3, LOW);
  digitalWrite(motor4, LOW);
}

void waveHand() {
  // Simple waving motion
  for (int angle = 60; angle <= 120; angle += 5) {
    handServo.write(angle);
    delay(30);
  }
  for (int angle = 120; angle >= 60; angle -= 5) {
    handServo.write(angle);
    delay(30);
  }
  handServo.write(90);  // Return to neutral
}
`
