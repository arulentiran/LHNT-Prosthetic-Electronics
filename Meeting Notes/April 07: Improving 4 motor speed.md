April 07: Improving 4 motor speed

- Goal: get the 4 motors moving at the same speed
- Summary: 2 motors continously move back and forth, but then the other 2 switch in when they turn (ex: motors A, B, and C turn then stop and then motors A, B, and D turn and then stop. Then A, B, and C turn again and the cycle continues)
    - one motor always turns notably slower than the other 3
- **note: the 3.7V w/850 mAh dies after an hour (even when fully charged)

We continued using Jorge’s code that he made for 4 motors and then played around with supplying more current. We found that currently, the circuit is receiving power from both the esp32 and the lipo input.

It seemed like the initial issue was the we weren’t supplying enough current. To solve this we connected the two 3.7V lipo batteries (one with 2000 mAh and the other 850mAh)  we have in series into the boost convertor, which ended up supplying 1A into the circuit. This did cause a notable difference in speed in the motors, making them move faster. One motors was still lagging behind in speed, and the same pattern of only 3 motors moving at once continued. This is odd since in the code, all 4 motors should move at once with 2 going forward and 2 going reverse. 

When we unplug the esp32, we noticed that the lights on all the motors turn off. Yet, the motors continue to turn when we unplug the lipo battery supply but leave the esp32 connected to the laptop. This leads up to believe that the motors are actually being mainly powered by the esp32 and not entirely by the lipo like we want. While the esp32 isn’t burning up and can make the motors move, it’s possible the motors aren’t moving like they should since they aren’t being powered properly.

Same code jorge wrote for us unchaged during our testing for the past week:

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
