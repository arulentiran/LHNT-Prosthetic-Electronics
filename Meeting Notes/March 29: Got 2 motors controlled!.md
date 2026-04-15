March 29: Got 2 motors controlled!

- Summary:
    - were able to connect 2 DC motors to the circuit and control it with code
    - confirmed we are able to power circuit with 3.7V lipo battery boosted to ~5.5V
    - updated PCB schematic to include new connections (only is based off of connections for a 1 DC motor schematic from where we left off on thursday)
    - need to solder motor wires

Since the last meeting where we knew we could move 1 DC motor, we decided to update the previously made schematic to include the new connections (PWMA, STBY, and Vcc) in order to have a proper record. 

Then we charged up a 3.7V lipo battery with a battery charger Vic found from the cabinet and boosted it to 5.5V to power the circuit. This was important since a fair amount of our testing thus far, including the last meeting on Thursday, was done with the DC power generator. Now we know we can power the circuit without a generator. 

Then, Jorge used the code from the last meeting on Thursday to control 2 DC motors. This was a success! Video of implementation and code is below. Jorge also wrote code to get 4 DC motors moving which we can test in the upcoming week after we finishing wiring.

**NOTE: the DC motor wires kept on getting disconnected from the circuit. Next meeting we need to solder them to more solid wires in order to prevent loose connections.

//code to move 2 DC motors

#define D1_PWMA 4
#define D1_PWMB 15
#define D2_PWMA 2
#define D2_PWMB 5 
#define D3_PWMA 18
#define DC1_IN1 27
#define DC1_IN2 26
#define DC2_IN1 25
#define DC2_IN2 33
#define DC3_IN1 32
#define DC3_IN2 35
#define DC4_IN1 34
#define DC4_IN2 19
#define DC5_IN1 22
#define DC5_IN2 23
#define STBY 14

const int freq = 20000;
const int resolution = 8;

void setup() {
  Serial.begin(115200);
  delay(1000);

  //pinMode DC1_IN1, OUTPUT);
  pinMode(DC1_IN1, OUTPUT);
  pinMode(DC1_IN2, OUTPUT);
  pinMode(DC2_IN1, OUTPUT);
  pinMode(DC2_IN2, OUTPUT);
  pinMode(DC3_IN1, OUTPUT);
  pinMode(DC3_IN2, OUTPUT);
  pinMode(DC4_IN1, OUTPUT);
  pinMode(DC4_IN2, OUTPUT);
  pinMode(DC5_IN1, OUTPUT);
  pinMode(DC5_IN2, OUTPUT);
  pinMode(STBY, OUTPUT);

  digitalWrite(STBY, HIGH);
  Serial.println("STBY HIGH");

  // Don't think is necessary
  
  bool ok1 = ledcAttach(D1_PWMA, freq, resolution);
  bool ok2 = ledcAttach(D1_PWMB, freq, resolution);

  /*
  bool ok3 = ledcAttach(D2_PWMA, freq, resolution);
  bool ok4 = ledcAttach(D2_PWMB, freq, resolution);
  bool ok5 = ledcAttach(D3_PWMA, freq, resolution);
  */

  if (ok1) {
    Serial.println("D1_PWMA attached");
  } else {
    Serial.println("D1_PWMA attach failed");
  }

  Serial.println("Motor test starting");
}


void loop() {

  // function call is speed, which pwm pin, In1, In2 of a specific dc motor
  forwardMotor(250, D1_PWMA, DC1_IN1, DC1_IN2);
  reverseMotor(250, D1_PWMB, DC2_IN1, DC2_IN2);
  delay(2000);

  // function call is In1, In2, and pwm pin
  stopMotor(DC1_IN1, DC1_IN2, D1_PWMA);
  stopMotor(DC2_IN1, DC2_IN2, D1_PWMB);
  delay(1500);

  reverseMotor(90, D1_PWMA, DC1_IN1, DC1_IN2);
  forwardMotor(210, D1_PWMB, DC2_IN1, DC2_IN2);
  delay(2000);

  stopMotor(DC1_IN1, DC1_IN2, D1_PWMA);
  stopMotor(DC2_IN1, DC2_IN2, D1_PWMB);
  delay(1500);

  forwardMotor(220, D1_PWMA, DC1_IN1, DC1_IN2);
  reverseMotor(90, D1_PWMB, DC2_IN1, DC2_IN2);
  delay(2000);
}

// formatted as speed, which pwm pin, M1, and M2 of a specific dc motor
void forwardMotor(int pwm_val, int dc_pwm, int dc_in1, int dc_in2) {
  pwm_val = constrain(pwm_val, 0, 255);
  digitalWrite(dc_in1, HIGH);
  digitalWrite(dc_in2, LOW);
  ledcWrite(dc_pwm, pwm_val);
}

void reverseMotor(int pwm_val, int dc_pwm, int dc_in1, int dc_in2) {
  pwm_val = constrain(pwm_val, 0, 255);
  digitalWrite(dc_in1, LOW);
  digitalWrite(dc_in2, HIGH);
  ledcWrite(dc_pwm, pwm_val);
}

void stopMotor(int dc_in1, int dc_in2, int dc_pwm) {
  digitalWrite(dc_in1, LOW);
  digitalWrite(dc_in2, LOW);
  ledcWrite(dc_pwm, 0);
}

//code to move 4 DC motors (needs to be tested as of 3/29!)
// can change as needed

#define D1_PWMA 4
#define D1_PWMB 15
#define D2_PWMA 2
#define D2_PWMB 5 
#define D3_PWMA 18
#define DC1_IN1 27
#define DC1_IN2 26
#define DC2_IN1 25
#define DC2_IN2 33
#define DC3_IN1 32
#define DC3_IN2 35
#define DC4_IN1 12
#define DC4_IN2 34
#define DC5_IN1 22
#define DC5_IN2 23
#define STBY 14

// not sure what these are but they work type
const int freq = 20000;
const int resolution = 8;

void setup() {
  Serial.begin(115200);
  delay(1000);

  pinMode(DC1_IN1, OUTPUT);
  pinMode(DC1_IN2, OUTPUT);
  pinMode(DC2_IN1, OUTPUT);
  pinMode(DC2_IN2, OUTPUT);
  pinMode(DC3_IN1, OUTPUT);
  pinMode(DC3_IN2, OUTPUT);
  pinMode(DC4_IN1, OUTPUT);
  pinMode(DC4_IN2, OUTPUT);
  pinMode(DC5_IN1, OUTPUT);
  pinMode(DC5_IN2, OUTPUT);
  pinMode(STBY, OUTPUT);

  digitalWrite(STBY, HIGH);
  Serial.println("STBY HIGH");

  // Don't think is necessary
  
  // ledCAttach for PWM pins for 4 dc motors
  bool ok1 = ledcAttach(D1_PWMA, freq, resolution);
  bool ok2 = ledcAttach(D1_PWMB, freq, resolution);
  bool ok3 = ledcAttach(D2_PWMA, freq, resolution);
  bool ok4 = ledcAttach(D2_PWMB, freq, resolution);

  /*
  bool ok3 = ledcAttach(D2_PWMA, freq, resolution);
  bool ok4 = ledcAttach(D2_PWMB, freq, resolution);
  bool ok5 = ledcAttach(D3_PWMA, freq, resolution);
  */

  // not sure if this is necessary
  if (ok1) {
    Serial.println("D1_PWMA attached");
  } else {
    Serial.println("D1_PWMA attach failed");
  }

  Serial.println("Motor test starting");
}


void loop() {

  //////////////////////////////////////////////////////////////////////////
  // function call for rotation is speed, which pwm pin, In1, In2 of a specific dc motor
  // to change speed, change first number to be between 0-255, where
  // 0 is lowest speed, 255 is highest speed
  // change delays to increase/decrease time spent rotating
  //////////////////////////////////////////////////////////////////////////


  forwardMotor(250, D1_PWMA, DC1_IN1, DC1_IN2);
  reverseMotor(250, D1_PWMB, DC2_IN1, DC2_IN2);
  forwardMotor(250, D2_PWMA, DC3_IN1, DC3_IN2);
  reverseMotor(250, D2_PWMB, DC4_IN1, DC4_IN2);
  delay(2000);

  // function call is In1, In2, and pwm pin
  
  stopMotor(DC1_IN1, DC1_IN2, D1_PWMA);
  stopMotor(DC2_IN1, DC2_IN2, D1_PWMB);
  stopMotor(DC3_IN1, DC3_IN2, D2_PWMA);
  stopMotor(DC4_IN1, DC4_IN2, D2_PWMB);
  delay(1500);

  reverseMotor(90, D1_PWMA, DC1_IN1, DC1_IN2);
  forwardMotor(210, D1_PWMB, DC2_IN1, DC2_IN2);
  reverseMotor(90, D2_PWMA, DC3_IN1, DC3_IN2);
  forwardMotor(210, D2_PWMB, DC4_IN1, DC4_IN2);
  delay(2000);

  stopMotor(DC1_IN1, DC1_IN2, D1_PWMA);
  stopMotor(DC2_IN1, DC2_IN2, D1_PWMB);
  stopMotor(DC3_IN1, DC3_IN2, D2_PWMA);
  stopMotor(DC4_IN1, DC4_IN2, D2_PWMB);
  delay(1500);

  forwardMotor(220, D1_PWMA, DC1_IN1, DC1_IN2);
  reverseMotor(90, D1_PWMB, DC2_IN1, DC2_IN2);
  forwardMotor(220, D2_PWMA, DC3_IN1, DC3_IN2);
  reverseMotor(90, D2_PWMB, DC4_IN1, DC4_IN2);
  delay(2000);
}

// formatted as speed, pwm pin, M1, and M2 of a specific dc motor
// rotates the motor forward by writing high to one pin and low to the other, sets speed
void forwardMotor(int pwm_val, int dc_pwm, int dc_in1, int dc_in2) {
  pwm_val = constrain(pwm_val, 0, 255);
  digitalWrite(dc_in1, HIGH);
  digitalWrite(dc_in2, LOW);
  ledcWrite(dc_pwm, pwm_val);
}

//formatted as speed, pwm pin, M1, and M2 of a specific dc motor
// writes high to other pin and low to the original, sets speed
void reverseMotor(int pwm_val, int dc_pwm, int dc_in1, int dc_in2) {
  pwm_val = constrain(pwm_val, 0, 255);
  digitalWrite(dc_in1, LOW);
  digitalWrite(dc_in2, HIGH);
  ledcWrite(dc_pwm, pwm_val);
}

// Stops motor by writing low to both pins + setting speed to 0
void stopMotor(int dc_in1, int dc_in2, int dc_pwm) {
  digitalWrite(dc_in1, LOW);
  digitalWrite(dc_in2, LOW);
  ledcWrite(dc_pwm, 0);
}

