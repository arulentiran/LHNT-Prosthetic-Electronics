March 24: Trying to get motor moving

we realized that the motor M1 and M2 were connected to the input power and ground would make it move manually. this means that the motor wouldn’t be moving according to the signals we send (forward, stop, and then move backwards on loop).

we tried to lengthen the delay that the motor would be stopped for and it seemed like the motor would slow down, but it wouldn’t properly stop or change directions. it seemed like that the motor was recieving the delay signal, but it was being overwritten with the way we had the hardware wired for M1 and M2. for instance, if m1 is connected the power rail and m2 is connected to the ground, it turns counterclockwise, but when we swap m1 and m2 it turns counterclockwise. 

so then we wired M1 and M2 back to aout1 and aout2, but then the motor wouldn’t be moving at all. this is the code we ended with below.

in conclusion, it seems like the motor is able to recieve signals with our current setup, but it’s not recieving power properly through M1 and M2.

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
  setMotor(-1, 255, PWM, IN1, IN2);
  delay(1000); // Increased delay so you can see it move
  Serial.print("backward: ");
  Serial.println(pos);

//  setMotor(0, 0, PWM, IN1, IN2);
//  delay(3000);
//
//  setMotor(1, 150, PWM, IN1, IN2);
//  delay(1000);
//  Serial.print("forward: ");
//  Serial.println(pos);

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
}

