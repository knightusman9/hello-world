/* Sweep
 by BARRAGAN <http://barraganstudio.com>
 This example code is in the public domain.

 modified 8 Nov 2013
 by Scott Fitzgerald
 http://www.arduino.cc/en/Tutorial/Sweep
*/

#include <Servo.h>

Servo myservo;// create servo object to control a servo
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
  myservo.attach(6); // attaches the servo on pin 9 to the servo object
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

upperhipr.write(40);
upperhipl.write(70);
kneer.write(60);
kneel.write(60);
  for (pos = 0; pos <= 180; pos += 5) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    // tell servo to go to position in variable 'pos'
    lowerhipl.write(pos);
   lowerhipr.write(pos);
    delay(75);                       // waits 15ms for the servo to reach the position
  }
  for (pos = 180; pos >= 0; pos -= 5) { // goes from 180 degrees to 0 degrees
    lowerhipl.write(pos);
    lowerhipr.write(pos);
    
    delay(75);                       // waits 15ms for the servo to reach the position
  }







  
}
