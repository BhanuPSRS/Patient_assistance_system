# Patient_assistance_system
This project developed a voice-activated bedside control system for patient care. It responds to voice commands for help, water, and bed adjustments using off-the-shelf components. Specific commands like "help," "water," "lift," "down," and "stop" are programmed and recognized.
#include <Servo.h>
Servo myservo;
int pos;
String readvoice;
const int RELAY_PIN = A5; //define relay pin
const int BUZZER_PIN = A4; //define buzzer and LED pin
void setup() {
Serial.begin(9600);
pinMode(RELAY_PIN, OUTPUT); //set relay pin as output
pinMode(BUZZER_PIN, OUTPUT); //set buzzer and LED as output
myservo.attach(10);
}
void loop() {
digitalWrite(RELAY_PIN, HIGH); //initially set relay as HIGH
while (Serial.available()) //while Bluetooth is connected
{
delay(3);
char c = Serial.read(); //read input from HC-05
readvoice += c; //append the words
}
if(readvoice.length() > 0) //works if any word is read by HC-05 other than
null
{
Serial.println(readvoice);
if(readvoice == "lift") //checks if command is lift
{
for (pos = 0; pos <= 90; pos += 1) //rotate the servo from 0 to 90 degrees
{
myservo.write(pos);
delay(50);
}
}
if(readvoice == "down") //checks if command is down
{
for (pos = 90; pos >= 0; pos -= 1) //rotate the servo from 90 to 0 degrees
{
myservo.write(pos);
2
delay(50);
}
}
if(readvoice == "water") //checks if command is down
{
digitalWrite(RELAY_PIN, LOW); //dc motor pump runs for 5 seconds
delay(5000);
digitalWrite(RELAY_PIN, HIGH);
}
if(readvoice == "help") //checks if command is help
{
digitalWrite(BUZZER_PIN, HIGH); //led and buzzer turns on
}
if(readvoice == "stop") //checks if command is stop
{
digitalWrite(BUZZER_PIN, LOW); //led and buzzer turns off
}
}
readvoice=""; //read voice declared as string
}
