# linefollowing
follow the line
// LCD pins: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(8, 9, 4, 5, 6, 7);

// L298N motor driver pins
const int ENA = 3;  // Enable pin for right motor (PWM)
const int IN1 = 13;   // right motor direction 1
const int IN2 = 2;   // right motor direction 2
const int ENB = 11;   // Enable pin for left motor (PWM)
const int IN3 = 12;   // left motor direction 1
const int IN4 = 1;  // left motor direction 2

// 5 IR sensors
const int IRLeft = A1;
const int left2 = A3;
const int IRRight = A2;
const int right2 = A4;
const int center = A5;

// Variables
int LeftIRInput = 0;
int RightIRInput = 0;

// Speed for motors
int speedL = 85;
int speedR = 85;
int speedL1 = 80;
int speedR1 = 70;
int speedL2 = 70;
int speedR2 = 80;

void forward();
void backward();
void left();
void right();

void setup() {

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  pinMode(IRLeft, INPUT);
  pinMode(IRRight, INPUT);

  Serial.begin(9600);
}

void loop() {
  LeftIRInput = digitalRead(IRLeft);
  RightIRInput = digitalRead(IRRight);

  // Print the sensor states to the serial monitor
  Serial.print("Left Sensor: ");
  Serial.print(LeftIRInput);
  Serial.print("Right Sensor: ");
  Serial.println(RightIRInput);

  if (LeftIRInput == LOW && RightIRInput == LOW){
    forward();
  }

  else if (LeftIRInput == HIGH && RightIRInput == LOW){
    right();
  }

    else if (LeftIRInput == LOW && RightIRInput == HIGH){
    left();
  }
    else if (LeftIRInput == HIGH && RightIRInput == HIGH){
    backward();
  }
  delay(100);
}



void forward() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  analogWrite(ENA, speedL); // Speed between 0–255
  analogWrite(ENB, speedR);
}

void backward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, speedL); // Speed between 0–255
  analogWrite(ENB, speedR);
}

void left() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, speedL1); // Speed between 0–255
  analogWrite(ENB, speedR1);
}

void right() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  analogWrite(ENA, speedL2); // Speed between 0–255
  analogWrite(ENB, speedR2);
}

void stopCar() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 0);
  analogWrite(ENB, 0);
}
