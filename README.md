/* Sweep
 by BARRAGAN <http://barraganstudio.com>
 This example code is in the public domain.

 modified 8 Nov 2013
 by Scott Fitzgerald
 http://www.arduino.cc/en/Tutorial/Sweep
*/

#include <Servo.h>

Servo lowerfootl;// create servo object to control a servo
// twelve servo objects can be created on most boards
Servo lowerfootr;
Servo upperfootl;
Servo upperfootr;
Servo upperhipr;
Servo upperhipl;
Servo lowerhipr;
Servo lowerhipl;
Servo kneel;
Servo kneer;

int pos = 0;    // variable to store the servo position

void setup() {
  lowerfootl.attach(6); // attaches the servo on pin 9 to the servo object
lowerfootr.attach(11);
upperfootr.attach(10);
upperfootl.attach(5);
upperhipr.attach(7);
upperhipl.attach(2);
lowerhipr.attach(8);
lowerhipl.attach(3);
kneel.attach(4);
kneer.attach(9); 
}

void loop() {

straight();
delay(1000);
stand();
delay(1000);

extendforward();
delay(1000);
extendbackward();  
delay(1000);  
}


void stand(){
upperhipr.write(40);
upperhipl.write(70);
kneer.write(140);
kneel.write(10);
lowerhipl.write(170);
lowerhipr.write(60);
upperfootr.write(30);
upperfootl.write(100);
lowerfootl.write(60);
lowerfootr.write(90);
}
  
void straight(){
upperhipr.write(40);
upperhipl.write(65);
kneer.write(60);
kneel.write(60);
lowerhipl.write(110);
lowerhipr.write(110);
upperfootr.write(60);
upperfootl.write(70);
lowerfootl.write(80);
lowerfootr.write(90);
  
}
void extendforward(){
upperhipr.write(40);
upperhipl.write(70);
kneer.write(20);
kneel.write(120);
lowerhipl.write(170);
lowerhipr.write(60);
upperfootr.write(30);
upperfootl.write(100);
lowerfootl.write(60);
lowerfootr.write(90);
}
void extendbackward(){
upperhipr.write(40);
upperhipl.write(70);
kneer.write(110);
kneel.write(30);
lowerhipl.write(40);
lowerhipr.write(160);
upperfootr.write(60);
upperfootl.write(60);
lowerfootl.write(60);
lowerfootr.write(90);
}






