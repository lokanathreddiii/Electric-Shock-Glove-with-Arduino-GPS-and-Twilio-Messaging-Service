#include <TinyGPS++.h>
#include <SoftwareSerial.h>

#define BUTTON_PIN 12 // Pin connected to the push button
#define RELAY_PIN  9 // Pin connected to the relay module

#define GPS_RX_PIN 3  // GPS module RX pin connected to Arduino TX pin
#define GPS_TX_PIN 4  // GPS module TX pin connected to Arduino RX pin

bool buttonState = HIGH; // Variable to store the current state of the button
bool lastButtonState = HIGH; // Variable to store the previous state of the button
bool buttonClicked = false; // Flag to indicate if the button was clicked since last state change

bool relayState = false; // Variable to store the state of the relay

SoftwareSerial gpsSerial(GPS_RX_PIN, GPS_TX_PIN); // Create a SoftwareSerial object for GPS communication
TinyGPSPlus gps; // Create a TinyGPS++ object to handle GPS data

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP); // Set button pin as input with internal pull-up resistor
  pinMode(RELAY_PIN, OUTPUT); // Set relay pin as output
  
  // Initialize relay state to OFF
  digitalWrite(RELAY_PIN, LOW);
  
  Serial.begin(9600); // Initialize serial communication
  gpsSerial.begin(9600); // Initialize GPS serial communication
}

void loop() {
  // Read the state of the push button
  int reading = digitalRead(BUTTON_PIN);
  
  // Check if the button state has changed
  if (reading != lastButtonState) {
    // Reset the button clicked flag
    buttonClicked = false;
  }
  
  // If the button is pressed
  if (reading == LOW && !buttonClicked) {
    // Toggle the relay state
    relayState = !relayState;
    
    // Update the relay
    digitalWrite(RELAY_PIN, relayState ? HIGH : LOW);
    
    // Set the button clicked flag
    buttonClicked = true;
    
    // Print emergency signal and GPS data to serial port
    if (gps.location.isValid()) {
      Serial.print("1,"); // Emergency signal
      Serial.print(gps.location.lat(), 6); // Latitude
      Serial.print(",");
      Serial.println(gps.location.lng(), 6); // Longitude
    } else {
      Serial.println("0,0,0"); // Invalid GPS data
    }
  }
  
  // Save the current button state for the next iteration
  lastButtonState = reading;
  
  // Process GPS data
  while (gpsSerial.available() > 0) {
    if (gps.encode(gpsSerial.read())) {
      // Optionally, you can handle more GPS data here if needed
    }
  }
  
  // Delay for stability
  delay(100);
}
