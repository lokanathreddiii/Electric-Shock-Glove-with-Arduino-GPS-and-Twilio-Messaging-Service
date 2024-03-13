import serial
from twilio.rest import Client
import time

# Twilio credentials
account_sid = 'ACc4412bd167fd9e12813992899396b219'
auth_token = '4ee58769be237b84eaaee0aea938428c'
twilio_phone_number = '+18783029949'  # Twilio phone number
recipient_phone_number = ''  # Recipient phone number

# Initialize Twilio client
client = Client(account_sid, auth_token)

def send_sms_message(message):
    client.messages.create(
        body=message,
        from_=twilio_phone_number,
        to=recipient_phone_number
    )
    print("SMS sent successfully!")

try:
    # Establish serial connection with Arduino
    arduino_port = 'COM7'  # Change this to the appropriate serial port
    arduino_baudrate = 9600
    arduino_ser = serial.Serial(arduino_port, arduino_baudrate, timeout=1)

    while True:
        # Read serial monitor output from Arduino
        serial_output = arduino_ser.readline().decode().strip()
        
        # Check if Arduino sends '1' (emergency signal)
        if serial_output == '1':
            # Send SMS message with "hello" using Twilio
            send_sms_message("Help me I am Lokhanath")













            
            
            # Delay to prevent rapid message sending
            time.sleep(5)  # Adjust as needed
        
except KeyboardInterrupt:
    print("Exiting program.")
finally:
    # Close serial connection when program is terminated
    if arduino_ser.is_open:
        arduino_ser.close()
