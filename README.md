# SmartStep
This project aims to reduce the footboard in the buses in India. 

Abstract:
      Smart Step is an innovative safety system designed to enhance bus safety in India by focusing on the footboard. It uses a weight-sensing mechanism to detect passengers standing on the footboard. If a passenger stands for more than 20 seconds with a weight exceeding 10kg, the system activates and sends data to an Arduino board. This board then engages the bus clutch, preventing acceleration and reducing the risk of accidents caused by sudden movements or overcrowding. By integrating hardware and software, Smart Step aims to improve safety standards and provide a more secure commuting experience.


Arduino Code

#include "HX711.h" // Include the HX711 library
#define LOADCELL_DOUT_PIN 3 // DOUT pin of HX711 connected to digital pin 3
#define LOADCELL_SCK_PIN 2 // SCK pin of HX711 connected to digital pin 2
#define RELAY_PIN 4 // Relay control pin connected to digital pin 4
#define MOTOR_DELAY 5000 // Delay in milliseconds for motor activation (5 seconds)

HX711 scale; // Create an instance of the HX711 library

void setup() {
 Serial.begin(9600); // Initialize serial communication
 pinMode(RELAY_PIN, OUTPUT); // Set relay pin as output
 scale.begin(LOADCELL_DOUT_PIN, LOADCELL_SCK_PIN); // Initialize the HX711 load cell amplifier
 scale.set_scale(1.0); // Set calibration factor (change if necessary)
}
void loop() {
 // Read the weight from the load cells
 float weight = scale.get_units(4); // Read the average weight from 4 load cells

 // Check if the weight exceeds 4kg for 5 seconds
 if (weight >= 4.0 && weight < 8.0) { // Check if weight is between 4kg and 8kg (to account for small variations)
 unsigned long startTime = millis(); // Record the start time
 
 while ((millis() - startTime) < MOTOR_DELAY) { // Wait for 5 seconds
 float currentWeight = scale.get_units(4); // Read the current weight

 if (currentWeight < 4.0) { // Check if weight drops below 4kg during the delay
 break; // Exit the loop if weight decreases
 }
 }
 // If weight remained above 4kg for 5 seconds, turn on the wipermotor
 digitalWrite(RELAY_PIN, HIGH); // Activate the relay to turn on the wiper motor
 } else {
 // If weight is below 4kg or exceeds 8kg, turn off the wiper motor
 digitalWrite(RELAY_PIN, LOW); // Deactivate the relay to turn off the wiper motor
 }
}
}

Conclusion:
      In conclusion, the Smart Step project marks a major advancement in public transportation safety by integrating weight-sensing technology with an Arduino-based control system. This system proactively addresses safety concerns related to standing passengers on bus footboards, potentially reducing accidents caused by overcrowding and sudden movements. Smart Step has broad applications across various public transportation modes, including buses, trains, and shuttles. Future efforts should focus on optimizing the system for different environments and collaborating with transportation authorities to facilitate widespread adoption. Overall, Smart Step represents a crucial development in enhancing passenger safety and improving the commuting experience.




