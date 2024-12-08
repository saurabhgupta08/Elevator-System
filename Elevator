// Programmed By Nishant Gupta
// ============================================================================================
// This program controls a stepper motor-based elevator system using an Arduino.
// It detects floor requests via IR sensors and moves the elevator to the requested floor,
// while storing and retrieving the last floor from the EEPROM.

#include <EEPROM.h> // Library to access and use EEPROM for storing non-volatile data

// Define the EEPROM address to store the current floor number
const int EEPROM_Address = 0; 

// Define the pins used for controlling the stepper motor
const int stepPin = 3; // Pin connected to the step signal of the stepper driver
const int dirPin = 4;  // Pin connected to the direction signal of the stepper driver

// Define the pins for IR sensors, each representing a floor
const int irSensorPin1 = 6; // IR sensor for floor 1
const int irSensorPin2 = 7; // IR sensor for floor 2
const int irSensorPin3 = 8; // IR sensor for floor 3

int currentFloor = 1;  // Variable to track the current floor (default is floor 1)
int stepsPerFloor = 1150; // Number of steps required by the stepper motor to move one floor

// ============================================================================================
// setup() function: Initializes the hardware and retrieves the stored floor number
void setup() {
  pinMode(stepPin, OUTPUT); // Set the step pin as output for motor pulses
  pinMode(dirPin, OUTPUT);  // Set the direction pin as output to control motor direction

  // Set IR sensor pins as input to detect floor requests
  pinMode(irSensorPin1, INPUT); 
  pinMode(irSensorPin2, INPUT);
  pinMode(irSensorPin3, INPUT);  

  // Retrieve the last floor stored in EEPROM
  EEPROM.get(EEPROM_Address, currentFloor);
  
  // If the stored value is invalid (not between 1 and 3), reset to default (floor 1)
  if (currentFloor <= 0 || currentFloor > 3) {
    currentFloor = 1; // Set default floor
    EEPROM.put(EEPROM_Address, currentFloor); // Save the default floor in EEPROM
  }
}

// ============================================================================================
// goToFloor() function: Moves the elevator to the target floor
// Parameters:
// - targetFloor: The desired floor to move the elevator to
void goToFloor(int targetFloor) {
  if (targetFloor > currentFloor) { // Check if the elevator needs to move up
    digitalWrite(dirPin, HIGH); // Set direction pin HIGH to move up
    int stepsToMove = (targetFloor - currentFloor) * stepsPerFloor; // Calculate steps required
    
    // Step the motor the calculated number of times
    for (int i = 0; i < stepsToMove; i++) {
      digitalWrite(stepPin, HIGH); // Generate a HIGH pulse
      delayMicroseconds(2000);    // Small delay to control speed
      digitalWrite(stepPin, LOW); // Generate a LOW pulse
      delayMicroseconds(2000);
    }
    currentFloor = targetFloor; // Update the current floor
    EEPROM.put(EEPROM_Address, currentFloor); // Save the current floor to EEPROM
  } else if (targetFloor < currentFloor) { // Check if the elevator needs to move down
    digitalWrite(dirPin, LOW); // Set direction pin LOW to move down
    int stepsToMove = (currentFloor - targetFloor) * stepsPerFloor; // Calculate steps required
    
    // Step the motor the calculated number of times
    for (int i = 0; i < stepsToMove; i++) {                   
      digitalWrite(stepPin, HIGH); // Generate a HIGH pulse
      delayMicroseconds(2000);    // Small delay to control speed
      digitalWrite(stepPin, LOW); // Generate a LOW pulse
      delayMicroseconds(2000);
    }
    currentFloor = targetFloor; // Update the current floor
    EEPROM.put(EEPROM_Address, currentFloor); // Save the current floor to EEPROM
  }
}

// ============================================================================================
// loop() function: Continuously checks for floor requests via IR sensors
void loop() {
  // If IR sensor for floor 1 detects a request, move to floor 1
  if (digitalRead(irSensorPin1) == LOW) { // LOW indicates sensor is triggered
    int targetFloor = 1; // Set target floor to 1
    goToFloor(targetFloor); // Call function to move elevator
  }

  // If IR sensor for floor 2 detects a request, move to floor 2
  if (digitalRead(irSensorPin2) == LOW) {
    int targetFloor = 2; // Set target floor to 2
    goToFloor(targetFloor);
  }
  
  // If IR sensor for floor 3 detects a request, move to floor 3
  if (digitalRead(irSensorPin3) == LOW) {
    int targetFloor = 3; // Set target floor to 3
    goToFloor(targetFloor);
  }
}
