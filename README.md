# program-for-obstacle-avoiding-robot-using-IR-sensor-that-moves-in-straight-direction-and-changes-pat
int left_wheelA = 27; int left_wheelB = 26;
int right_wheelA = 25; int right_wheelB = 33;
int EN_L = 14; int EN_R = 32;
const int freq = 5000;
const int Channel_1 =0;
const int Channel_2 = 1;
const int resolution=8;
int Input_Pin = 24;
int noObstacle = HIGH;
void setup(){
pinMode(left_wheelA, OUTPUT); pinMode(left_wheelB, OUTPUT);
pinMode(right_wheelA, OUTPUT); pinMode(right_wheelB, OUTPUT);
// configure PWM functionalitites
ledcSetup(Channel_1, freq, resolution);
ledcSetup(Channel_2, freq, resolution);
// attach the channel to the GPIO to be controlled
ledcAttachPin(EN_L, Channel_1);
ledcAttachPin(EN_R, Channel_2);
delay(2000);
pinMode(Input_Pin, INPUT);
}
void Drive_Far(){
digitalWrite(left_wheelA, HIGH);
digitalWrite(left_wheelB, LOW);
digitalWrite(right_wheelA, HIGH);
digitalWrite(right_wheelB, LOW);
ledcWrite(Channel_1, 130);
ledcWrite(Channel_2, 150);
}
void Drive_Back(){
digitalWrite(left_wheelA, LOW);
digitalWrite(left_wheelB, HIGH);
digitalWrite(right_wheelA, LOW);
digitalWrite(right_wheelB, HIGH);
ledcWrite(Channel_1, 140);//130 on EN_L where as 150 on EN_R makes the speed of both motors equal
ledcWrite(Channel_2, 160);
}
void Stop(){
digitalWrite(left_wheelA, LOW);
digitalWrite(left_wheelB, LOW);
digitalWrite(right_wheelA, LOW);
digitalWrite(right_wheelB, LOW);
delay(500);
}
void Turn_Right(){
ledcWrite(Channel_1, 180); //to turn right speed of left wheel is increased
ledcWrite(Channel_2, 150);
digitalWrite(left_wheelA, HIGH);
digitalWrite(left_wheelB, LOW);
digitalWrite(right_wheelA, HIGH);
digitalWrite(right_wheelB, LOW);
delay(200);
}
void ObstacleAvoid_IR(){
noObstacle = digitalRead(Input_Pin);
if(noObstacle == LOW){
Serial.println("OBSTACLE !! ");
Stop();
Drive_Back();
delay(2000);
Stop();
Turn_Right();
Stop();
}
else {
Serial.println("CLEAR !! ");
Drive_Far();
}
