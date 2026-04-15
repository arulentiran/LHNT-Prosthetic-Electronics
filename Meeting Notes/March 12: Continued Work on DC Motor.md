March 12: Continued Work on DC Motor

Link to YouTube Video-
https://www.youtube.com/watch?v=dTGITLnYAY0

MC Schematic:
<img width="1078" height="806" alt="image" src="https://github.com/user-attachments/assets/c8954b0d-4fb5-4996-969b-a71c955393f7" />

GP34 is an input only pin, so it cannot be used to transmit signals between the ESP32 and motor driver. The analogWrite() function also can’t be used with ESP32’s so we rewrote the initialization in the setupMotor function with digitalWrite().

New Code:
//Connecting the motor VCC to VIN on the ESP32 is what made the serial monitor produce outputs

//These pins should go from the ESP32 to C1 and C2. They help the ESP32 detect direction of rotation. 
#define ENCA 33
#define ENCB 32

//PWM required to control the amount of power given to the motor
#define PWM 4 

//IN pins connect to the ESP32 and tell the motor which direction to turn
#define IN2 26
#define IN1 27

//STBY needed to control whether motor is active or not 
#define STBY 14 

//int pwmChannel = 0; 
/* New Arduino versions don't require PWM to first be called from
a channel before being connected to a pin. Defining the PWM pin  
and initializing it is sufficient. */

//PWM settings for ESP32
volatile int pos = 0;
int freq = 20000;
int resolution = 8;

void setup() {
  Serial.begin(115200);

  // Encoder Pins
	//INPUT_PULLUP defaults the input pins to HIGH, making them more resistant to interference
  pinMode(ENCA, INPUT_PULLUP);
  pinMode(ENCB, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(ENCA), readEncoder, RISING);

  // Motor Control Pins - MUST be set to OUTPUT
  pinMode(PWM, OUTPUT);
  pinMode(STBY, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);

  digitalWrite(STBY, HIGH); 

	//This function is dated b/c we don't need to manually attach a PWM channel to a pin anymore. 
  //ledcAttachPin(PWM, pwmChannel);
  ledcAttach(PWM, freq, resolution);

  setMotor(0, 0, PWM, IN1, IN2);

  Serial.println("Starting motor test...");
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
  pwmVal = constrain(pwmVal, 0, 255);
  ledcWrite(PWM, pwmVal);
  
  //ledcWrite() is more specific to the PWM channel, while digitalWrite in more general.
  //digitalWrite(pwm, pwmVal);
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

# Power Connections (Correct Setup)

(This is what we believe to be the correct pin set up between all the components, however we are still a bit unsure if this is the true set up. Would need further testing done but we have modified the code at least)

## Motor Driver Pins

### Motor Power

- **VM → 5.6 V from your boost converter**
- **GND → common ground**

### Logic Power

- **VCC → ESP32 3.3 V**
- **GND → ESP32 GND**

### Motor Outputs

- **A01 → Motor wire 1**
- **A02 → Motor wire 2**

### Control Pins

- **AIN1 → ESP32 GPIO**
- **AIN2 → ESP32 GPIO**
- **PWMA → ESP32 PWM pin**
- **STBY → ESP32 GPIO (HIGH to enable)**

# Next Steps:

1. Consider soldering or using different wires for the setup to be more clean to reduce connectivity issues
2. Redraw circuit schematic
