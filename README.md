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
 servo.write(260); // Initialize servo at 90 degrees (closed lid)
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
 // Check if an object is close (within 30 cm range)
 if (distance < 30) {
 digitalWrite(ledPin, HIGH); // Turn on LED
 servo.write(0); // Move to 0 degrees (open lid)
 delay(2000); // Keep the lid open for 2 seconds
 servo.write(260); // Move back to 90 degrees (closed lid)
 digitalWrite(ledPin, LOW); // Turn off LED
 }

 delay(500); // Small delay for stable readings
}
