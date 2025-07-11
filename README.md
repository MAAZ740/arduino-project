# 🚮 Smart Dustbin Project

## 🎯 Objective
The objective of this project is to **design and build a smart dustbin** that automatically opens its lid upon detecting the presence of waste using an ultrasonic sensor.

This project will help students understand:
- **Automation concepts**
- **Sensor integration**
- **The importance of smart waste management** in urban environments

---

## 📦 List of Contents
1. Arduino Uno  
2. Ultrasonic Sensor (HC-SR04)  
3. Servo Motor (SG-90)  
4. Jumper Wires  
5. Breadboard  
6. Power Supply (12V Lithium-Ion Battery)  
7. Dustbin (any small container with a lid)  
8. Red LED  

---

## ⚙️ Working
The ultrasonic sensor detects an object (hand or waste) within 30 cm. If detected:
- The red LED turns ON.
- The servo motor rotates to open the lid.
- After a 2-second delay, the lid automatically closes.
- The LED turns OFF.

---

## 💻 Arduino Code

```cpp
#include <Servo.h> // Include Servo library

// Define Pins
const int trigPin = 5;
const int echoPin = 6;
const int servoPin = 9;
const int ledPin = 10;

Servo servo; // Create Servo object
long duration;
int distance;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);
  servo.attach(servoPin); // Attach the servo motor
  servo.write(260); // Initialize servo at 260 degrees (closed lid)
}

void loop() {
  // Measure distance using ultrasonic sensor
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2; // Calculate distance in cm

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // If object is within 30 cm
  if (distance < 30) {
    digitalWrite(ledPin, HIGH);     // Turn on LED
    servo.write(0);                 // Open lid
    delay(2000);                    // Wait 2 seconds
    servo.write(260);              // Close lid
    digitalWrite(ledPin, LOW);      // Turn off LED
  }

  delay(500); // Short delay for stable reading
}
