# slavecode
#include <AFMotor.h>
#include <SoftwareSerial.h>
SoftwareSerial bluetoothSerial(9, 10); // RX, TX connected pins of the bluetooth module. 

//Assigning each motor           
AF_DCMotor leftBack(1);
AF_DCMotor rightBack(2);
AF_DCMotor rightFront(3);
AF_DCMotor leftFront(4);

char bluetoothsignal;
//The maximum speed of the rover.
byte RoverSpeed = 100;
//The speed when the rover is turning.                             
int turningSpeed = 50;
char command[1];
String instruct;
bool Time;  

void setup() {
  Serial.begin(9600);
  while(!Serial) {;}
  Serial.println("Serial Set");
  //setting the bluetooth module.
  bluetoothSerial.begin(9600);
  //Setting all the motors to the RoverSpeed.
  rightBack.setSpeed(RoverSpeed);                 
  rightFront.setSpeed(RoverSpeed);
  leftFront.setSpeed(RoverSpeed);
  leftBack.setSpeed(RoverSpeed);
}

void loop() {
String  F="F";
  Time=true;
  //Checking if the bluetooth module is working.
  if (bluetoothSerial.available()){
    bluetoothsignal = bluetoothSerial.read();
 //Stop the rover until a command is given.
    String string = String(bluetoothsignal);
    Serial.println(string);
    stopMove ();
   if(string.equals(F)&& Time == true){
    moveForward();
    delay(10000);
    stopMove ();
    delay(10000);
   }
   else if(string.equals("B") && Time == true){
    moveBackward();
    delay(10000);
    stopMove ();
    delay(10000);
   }
   else if(string.equals("L") && Time == true){
    turnLeft();
    delay(10000);
    stopMove ();
    delay(10000);
   }
   else if(string.equals("R") && Time == true){
    turnRight();
    delay(10000);
    stopMove ();
    delay(10000);
   }
  }
}

//Setting the rover to advance forward.
void moveForward()                                
{
  rightBack.setSpeed(RoverSpeed);                          
  rightFront.setSpeed(RoverSpeed);
  leftFront.setSpeed(RoverSpeed);
  leftBack.setSpeed(RoverSpeed);
  rightBack.run(FORWARD);
  rightFront.run(BACKWARD);
  leftFront.run(FORWARD);
  leftBack.run(BACKWARD);
}
//Setting the rover to move backward.
void moveBackward()                              
{
  rightBack.setSpeed(RoverSpeed);                         
  rightFront.setSpeed(RoverSpeed);
  leftFront.setSpeed(RoverSpeed);
  leftBack.setSpeed(RoverSpeed);
  rightBack.run(BACKWARD);
  rightFront.run(FORWARD);
  leftFront.run(BACKWARD);
  leftBack.run(FORWARD);
}
//Stopping the motor from moving.
void stopMove()    
{
  rightBack.run(RELEASE);
  rightFront.run(RELEASE);
  leftFront.run(RELEASE);
  leftBack.run(RELEASE);
}
//Setting the rover to turn right with the addtitional speed.
void turnRight()                                 
{
  rightBack.setSpeed(RoverSpeed+turningSpeed);                 
  rightFront.setSpeed(RoverSpeed+turningSpeed);
  leftFront.setSpeed(RoverSpeed+turningSpeed);
  leftBack.setSpeed(RoverSpeed+turningSpeed);
  rightBack.run(FORWARD);
  rightFront.run(FORWARD);
  leftFront.run(BACKWARD);
  leftBack.run(BACKWARD);
}
//Setting the rover to turn left with the additional speed, turn speed.
void turnLeft()                                
{
  rightBack.setSpeed(RoverSpeed+turningSpeed);                
  rightFront.setSpeed(RoverSpeed+turningSpeed);
  leftFront.setSpeed(RoverSpeed+turningSpeed);
  leftBack.setSpeed(RoverSpeed+turningSpeed);
  rightBack.run(BACKWARD);
  rightFront.run(BACKWARD);
  leftFront.run(FORWARD);
  leftBack.run(FORWARD);
}
