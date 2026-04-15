Debug Potential Problems:

- **The motor driver outputs are not actually being driven**
- **The driver is not powered correctly**
- **The control pins are wrong / not switching**
- **The motor is not connected to the correct motor-output pins**
- **You may be using the wrong understanding of M1/M2 vs AOUT1/AOUT2**

Goals: 

- Get the motor to run without powering direct voltage. And have it work with a functionuing Arduino Code

What Was Accomplished:

Motor now runs with a motor test code that moves it forward, stops, moves it in reverse, stops, and repeats !

What we realized was that the motor driver had internal damage (maybe a short was caused at some point during power delivery testing). We tested whether the M1 and M2 terminals, which are our A1OUT and A2OUT pins on the motor driver, were receiving power at all by measuring them with a DMM (multimeter). We put the red pin on one of the M1/M2 pins and the black on the shared common ground of the motor driver. It should have been receiving some kind of voltage (oscillating between 3.2 - 0 V, as we verified that the code and ESP-32 board were functioning properly and receiving the appropriate signals).

The reason why it would oscillate between that voltage is that the motor driver board itself delivers a voltage of 3.2 V, but with the code, if the pins were working, the M1 and M2 pins would be 3.2 V while the motor was moving either forward or in reverse, and then become 0 V when it stops in between movements. We noted that the voltage was just 0 V the whole time the code was running, confirming our suspicions that the motor driver board itself was the hardware issue. 

We soldered a new motor driver, and this one worked with the code immediately. So now every component of the hardware has been properly diagnosed, debugged, and resolved! The code below was a sample test code, and will not be our final iteration of the code. It was just used to test the movement of the DC motor.

#define PWM 4
#define IN1 27
#define IN2 26
#define STBY 14

const int freq = 20000;
const int resolution = 8;

void setup() {
  Serial.begin(115200);
  delay(1000);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(STBY, OUTPUT);

  digitalWrite(STBY, HIGH);
  Serial.println("STBY HIGH");

  bool ok = ledcAttach(PWM, freq, resolution);
  if (ok) {
    Serial.println("PWM attached");
  } else {
    Serial.println("PWM attach failed");
  }

  stopMotor();
  Serial.println("Motor test starting");
}

void loop() {
  Serial.println("Forward");
  forwardMotor(255);
  delay(2000);

  Serial.println("Stop");
  stopMotor();
  delay(1500);

  Serial.println("Reverse");
  reverseMotor(255);
  delay(2000);

  Serial.println("Stop");
  stopMotor();
  delay(1500);
}

void forwardMotor(int pwmVal) {
  pwmVal = constrain(pwmVal, 0, 255);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(PWM, pwmVal);
}

void reverseMotor(int pwmVal) {
  pwmVal = constrain(pwmVal, 0, 255);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  ledcWrite(PWM, pwmVal);
}

void stopMotor() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  ledcWrite(PWM, 0);
}

What We Learned:

- If there is a hardware issue, I recommend testing if voltage is being sent across all the input/output terminals of the designated board we are using. Most times why it could be a hardware problem because the boards may have a connectivity issue internally (short or open circuit).
- Test if the microcontroller board (ESP-32) is receiving the correct signals from the code. We initially thought the ESP-32 board was not receiving the right data input, so we ran a test code to flash the LEDs on the board to double-check.
- Wires and even the breadboard itself can have connectivity issues when delivering voltage, current, or signals, so always check with the DMM (multimeter) every input/output terminal.
