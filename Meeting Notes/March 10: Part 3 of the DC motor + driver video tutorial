March 10: Part 3 of the DC motor + driver video tutorial

Summary:

- Goal: get the motor moving
- Accomplished: erm motor didn’t move…

We started by following part 3 of the video we found on Sunday and copied it’s code:

#define ENCA 2
#define ENCB 4
#define PWM 5
#define IN2 14
#define IN1 12

int pos  = 0;

void setup() {
// put your setup code here, to run once:
Serial.begin(9600);
pinMode(ENCA, INPUT);
pinMode(ENCB, INPUT);
attachInterrupt(digitalPinToInterrupt(ENCA), readEncoder, RISING);
}

void loop() {
// put your main code here, to run repeatedly:
setMotor(1, 25, PWM, IN1, IN2);
delay(200);
Serial.println(pos);
setMotor(-1, 25, PWM, IN1, IN2);
delay(200);
Serial.println(pos);
setMotor(0, 25, PWM, IN1, IN2);
delay(200);
Serial.println(pos);
}

void setMotor(int dir, int pwmVal, int pwm, int in1, int in2){
analogWrite(pwm, pwmVal);
if (dir == 1){
digitalWrite(in1, HIGH);
digitalWrite(in2, LOW);
}
else if (dir == -1){
digitalWrite(in1, LOW);
digitalWrite(in2, HIGH);
}
else {
digitalWrite(in1, LOW);
digitalWrite(in2, LOW);
}
}

void readEncoder(){
int b = digitalRead(ENCB);
if(b>0){
pos++;
}
else{
pos--;
}
}

- It said that pins 0, 2, 12 can interfere with uploads → so we moved pins 2 and 12
    - ”If you have wires plugged into **GPIO 0**, **GPIO 2**, or **GPIO 12**, these are "strapping pins." If they are pulled High or Low by your motor driver/encoder while the board is trying to start up, the ESP32 will try to boot into the wrong mode and block the upload.”

Then we were able to upload but motor wasn’t moving still so we implemented these changes:

- Also that we needed to program Standby cause otherwise the ESP32 would be stuck in LOW mode and the motor wouldn’t move → so we connect STBY to ESP32 3.3V pin
- And that we needed to connect Vcc → so we conntected it to the ESP32 3.3V pin

This change in setup still didn’t work with the code from the video so used the new code gemini generated from the suggested changes (inserted below), but this still didn’t get the motor moving:

#define ENCA 23
#define ENCB 4
#define PWM 5
#define IN2 14
#define IN1 34

int pos = 0;

void setup() {
  Serial.begin(9600);

  // Encoder Pins
  pinMode(ENCA, INPUT);
  pinMode(ENCB, INPUT);
  attachInterrupt(digitalPinToInterrupt(ENCA), readEncoder, RISING);

  // Motor Control Pins - MUST be set to OUTPUT
  pinMode(PWM, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
}

void loop() {
  // Speed set to 150 (approx 60% power) to ensure the motor turns
  setMotor(1, 150, PWM, IN1, IN2);
  delay(1000); // Increased delay so you can see it move
  Serial.print("Forward: ");
  Serial.println(pos);

  setMotor(-1, 150, PWM, IN1, IN2);
  delay(1000);
  Serial.print("Backward: ");
  Serial.println(pos);

  setMotor(0, 0, PWM, IN1, IN2);
  delay(1000);
}

void setMotor(int dir, int pwmVal, int pwm, int in1, int in2){
  analogWrite(pwm, pwmVal);
  if (dir == 1){
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
  }
  else if (dir == -1){
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
  }
  else {
    digitalWrite(in1, LOW);
    digitalWrite(in2, LOW);
  }
}

void readEncoder(){
  int b = digitalRead(ENCB);
  if(b > 0){
    pos++;
  }
  else{
    pos--;
  }
}

But then this wasn’t working so we asked gemini for some help.

For issues with uploading, this helped us fix our issue:

the error message i got:
Arduino: 1.8.19 (Windows Store 1.8.57.0) (Windows 10), Board: "ESP32 Dev Module, Disabled, Disabled, Default 4MB with spiffs (1.2MB APP/1.5MB SPIFFS), 240MHz (WiFi/BT), QIO, 80MHz, 4MB (32Mb), 921600, Core 1, Core 1, None, Disabled, Disabled"

Sketch uses 294423 bytes (22%) of program storage space. Maximum is 1310720 bytes.

Global variables use 22752 bytes (6%) of dynamic memory, leaving 304928 bytes for local variables. Maximum is 327680 bytes.

esptool v5.1.0

Serial port COM3:

Connecting.....................................

A fatal error occurred: Failed to connect to ESP32: Invalid head of packet (0x00): Possible serial noise or corruption.

For troubleshooting steps visit: https://docs.espressif.com/projects/esptool/en/latest/troubleshooting.html

.

the selected serial port does not exist or your board is not connected



This report would have more information with
"Show verbose output during compilation"
option enabled in File -> Preferences.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------

- It said that pins 0, 2, 12 can interfere with uploads → so we moved pins 2 and 12
    - ”If you have wires plugged into **GPIO 0**, **GPIO 2**, or **GPIO 12**, these are "strapping pins." If they are pulled High or Low by your motor driver/encoder while the board is trying to start up, the ESP32 will try to boot into the wrong mode and block the upload.”

Then we were able to upload but motor wasn’t moving still so we implemented these changes:

- Also that we needed to program Standby cause otherwise the ESP32 would be stuck in LOW mode and the motor wouldn’t move → so we connect STBY to ESP32 3.3V pin
- And that we needed to connect Vcc → so we conntected it to the ESP32 3.3V pin

This change in setup still didn’t work with the code from the video so used the new code gemini generated from the suggested changes (inserted below), but this still didn’t get the motor moving:

#define ENCA 23
#define ENCB 4
#define PWM 5
#define IN2 14
#define IN1 34

int pos = 0;

void setup() {
  Serial.begin(9600);

  // Encoder Pins
  pinMode(ENCA, INPUT);
  pinMode(ENCB, INPUT);
  attachInterrupt(digitalPinToInterrupt(ENCA), readEncoder, RISING);

  // Motor Control Pins - MUST be set to OUTPUT
  pinMode(PWM, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
}

void loop() {
  // Speed set to 150 (approx 60% power) to ensure the motor turns
  setMotor(1, 150, PWM, IN1, IN2);
  delay(1000); // Increased delay so you can see it move
  Serial.print("Forward: ");
  Serial.println(pos);

  setMotor(-1, 150, PWM, IN1, IN2);
  delay(1000);
  Serial.print("Backward: ");
  Serial.println(pos);

  setMotor(0, 0, PWM, IN1, IN2);
  delay(1000);
}

void setMotor(int dir, int pwmVal, int pwm, int in1, int in2){
  analogWrite(pwm, pwmVal);
  if (dir == 1){
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
  }
  else if (dir == -1){
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
  }
  else {
    digitalWrite(in1, LOW);
    digitalWrite(in2, LOW);
  }
}

void readEncoder(){
  int b = digitalRead(ENCB);
  if(b > 0){
    pos++;
  }
  else{
    pos--;
  }
}
