# code-for-obstacle-avoiding-robot
//Subscribe to lets discover pro.
#include <AFMotor.h>  
#include <NewPing.h>
#include <Servo.h> 
#define TRIGPIN A5
#define ECHOPIN A4 
#define MAX_DISTANCE 250
#define MAX_SPEED 200 
#define MAX_SPEED_OFFSET 20

NewPing sonar(TRIGPIN, ECHOPIN, MAX_DISTANCE); 

AF_DCMotor motor1(1, MOTOR12_1KHZ); 
AF_DCMotor motor2(2, MOTOR12_1KHZ);
Servo myservo;   

boolean goesForward=false;
int distance = 100;
int speedSet = 0;

void setup() {

  myservo.attach(10);  
  myservo.write(90); 
  delay(2000);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
  Serial.begin(9600);
}

void loop() {
 int distanceRight = 0;
 int distanceLeft =  0;
 delay(40);
 if(distance<=20)
 {
  Stop();
  delay(100);
  Backward();
  delay(3000);
  Stop();
  delay(500);
  distanceRight = lookRight();
  delay(200);
  distanceLeft = lookLeft();
  delay(200);

  if(distanceRight>=distanceLeft)
  {
    turnRight();
    Stop();
  }else
  {
    turnLeft();
    Stop();
  }
 }else
 {
  Forward();
 }
 distance = readPing();
}

int lookRight()
{
    myservo.write(-10); 
    delay(500);
    int distance = readPing();
    delay(100);
    myservo.write(90); 
    return distance;
}

int lookLeft()
{
    myservo.write(220); 
    delay(500);
    int distance = readPing();
    delay(100);
    myservo.write(90); 
    return distance;
    delay(100);
}

int readPing() { 
  delay(70);
  int cm = sonar.ping_cm();
  if(cm==0)
  {
    cm = 250;
  }
  return cm;
}

void Stop() {
  motor1.run(RELEASE); 
  motor2.run(RELEASE);
  
  } 
  
void Forward() {

 if(!goesForward)
  {
    goesForward=true;
    motor1.run(FORWARD);      
    motor2.run(FORWARD);
  
   for (speedSet = 0; speedSet < MAX_SPEED; speedSet +=2) 
   {
    motor1.setSpeed(speedSet);
    motor2.setSpeed(speedSet);
    delay(5);
   }
  }
}

void Backward() {
    goesForward=false;
    motor1.run(BACKWARD);      
    motor2.run(BACKWARD);
    
  for (speedSet = 0; speedSet < MAX_SPEED; speedSet +=2) 
  {
    motor1.setSpeed(speedSet);
    motor2.setSpeed(speedSet);
    
    delay(5);
  }
}  

void turnRight() {
  motor1.run(FORWARD);
  motor2.run(BACKWARD);
  delay(3500);
  motor1.run(FORWARD);      
  motor2.run(FORWARD);
  
} 
 
void turnLeft() {
  motor1.run(BACKWARD);     
  motor2.run(FORWARD); 
  delay(3500);
  motor1.run(FORWARD);     
  motor2.run(FORWARD);
  
}  
