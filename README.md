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
int x=0;
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
pinMode(14, OUTPUT);
pinMode(15, OUTPUT); 
pinMode(16, OUTPUT);
straight1();
core();
}

void loop() {
walking();

 }

void walking(){

sweepright();
core();
extendleft1();
core();
extendleft2();
core();
returnleft();
core();
moveforwardleft();
core();
sweepleft();
core();
extendright1();
core();
extendright2();
core();
returnright();
core();
moveforwardright();
core();
sweepright2();
core();
interconnect();
core();
//straight2();
//delay(5000);



//sweepleft();
//delay(5000);
//extendleft3();
//sweepright();
//delay(5000);
//extendright2();
}
void core(){
for (int m = 0; m<=17; m++){  
digitalWrite(14, HIGH);
delay(50);
digitalWrite(14, LOW);
delay(50);
digitalWrite(15, HIGH);
delay(50);
digitalWrite(15, LOW);
delay(50);
digitalWrite(16, HIGH);
delay(50);
digitalWrite(16, LOW);
delay(50);
}

digitalWrite(14, HIGH);
digitalWrite(15, HIGH);
digitalWrite(16, HIGH);
}
  
void straight1(){
upperhipr.write(90);
upperhipl.write(105);
lowerhipr.write(90);
lowerhipl.write(75);
kneer.write(85);
kneel.write(60);
upperfootr.write(30);
upperfootl.write(55);
lowerfootr.write(95);
lowerfootl.write(80);
}
void bend(){
upperhipr.write(80);
upperhipl.write(95);
lowerhipr.write(30);
lowerhipl.write(140);
kneer.write(30);
kneel.write(110);
upperfootr.write(50);
upperfootl.write(30);
lowerfootr.write(95);
lowerfootl.write(80); 
}

void sweepright(){
for (int i = 0; i<=20; i++){
upperhipr.write(85+i);
upperhipl.write(105+i);
lowerhipr.write(90);
lowerhipl.write(75);
kneer.write(85);
kneel.write(60);
upperfootr.write(30);
upperfootl.write(55);
lowerfootr.write(95+i);
lowerfootl.write(80+i);
delay(100);
}
}
void extendleft1(){
for (int a = 0; a<=70; a++){ 
upperhipr.write(105);
upperhipl.write(125);
lowerhipr.write(90);
lowerhipl.write(75+a);
kneer.write(85);
kneel.write(60+a);
upperfootr.write(35-a/12);
upperfootl.write(50-a/14);
lowerfootr.write(115+((a/2)-3));
lowerfootl.write(100);
delay(100);
}

}

void extendleft2(){
 for (int b = 0; b<=30; b++){  
upperhipr.write(105);
upperhipl.write(125);
lowerhipr.write(90);
lowerhipl.write(145);
kneer.write(85);
kneel.write(130-b);
upperfootr.write(29-b/6);
upperfootl.write(45+b);
lowerfootr.write(138);
lowerfootl.write(100);
delay(100);
}
}
void returnleft(){
for (int c = 0; c<=40; c++){  
upperhipr.write(105);
upperhipl.write(125);
lowerhipr.write(90);
lowerhipl.write(145);
kneer.write(85);
kneel.write(100);
upperfootr.write(20+c/3);
upperfootl.write(75);
lowerfootr.write(138-c);
lowerfootl.write(100);
delay(100);
}
}

void moveforwardleft(){
for (int d = 0; d<=20; d++){  
upperhipr.write(105);
upperhipl.write(125);
lowerhipr.write(90+d);
lowerhipl.write(145-((8*d)/5));
kneer.write(85);
kneel.write(100);
upperfootr.write(38+d/2);
upperfootl.write(75-d);
lowerfootr.write(98-d/2);
lowerfootl.write(100-d/2);
delay(100);
}
}

void sweepleft(){
for (int e = 0; e<=25; e++){  
upperhipr.write(105-e);
upperhipl.write(125-e);
lowerhipr.write(115);
lowerhipl.write(105);
kneer.write(85);
kneel.write(100);
upperfootr.write(52+e/2);
upperfootl.write(45-e/2);
lowerfootr.write(86-e);
lowerfootl.write(88-e);
delay(100);
}
}

void extendright1(){
for (int f = 0; f<=80; f++){ 
upperhipr.write(80);
upperhipl.write(100-f/12);
lowerhipr.write(115-f);
lowerhipl.write(105+f/10);
kneer.write(85-f);
kneel.write(100);
upperfootr.write(65-f/5);
upperfootl.write(38-f/8);
lowerfootr.write(61);
lowerfootl.write(63-f/7);
delay(100);
}

}
void extendright2(){
for (int g = 0; g<=35; g++){ 
upperhipr.write(80);
upperhipl.write(93);
lowerhipr.write(35);
lowerhipl.write(113);
kneer.write(5+g);
kneel.write(100);
upperfootr.write(49-g);
upperfootl.write(28);
lowerfootr.write(61);
lowerfootl.write(52);
delay(100);
}
}
void returnright(){
for (int h = 0; h<=18; h++){ 
upperhipr.write(80+h);
upperhipl.write(93+h);
lowerhipr.write(35);
lowerhipl.write(113);
kneer.write(40);
kneel.write(100);
upperfootr.write(14);
upperfootl.write(28-h/6);
lowerfootr.write(61+((2.2)*h));
lowerfootl.write(52+h);
delay(100);
}
}

void moveforwardright(){
for (int i = 0; i<=20; i++){ 
upperhipr.write(95);
upperhipl.write(108);
lowerhipr.write(35+i);
lowerhipl.write(113-i);
kneer.write(40);
kneel.write(100);
upperfootr.write(14+i);
upperfootl.write(26+i/4);
lowerfootr.write(91);
lowerfootl.write(67);
delay(100);
}
}
void sweepright2(){
for (int k = 0; k<=20; k++){ 
upperhipr.write(95+k);
upperhipl.write(108+k);
lowerhipr.write(55);
lowerhipl.write(93);
kneer.write(40);
kneel.write(100);
upperfootr.write(34);
upperfootl.write(31);
lowerfootr.write(91+k);
lowerfootl.write(67+k);
delay(100);
}
}
void interconnect(){
for (int l = 0; l<=20; l++){
upperhipr.write(110+((-1/4)*l));
upperhipl.write(123+((1/10)*l));
lowerhipr.write(55+((7/4)*l));
lowerhipl.write(93+((-5/10)*l));
kneer.write(40+((9/4)*l));
kneel.write(100+((-2)*l));
upperfootr.write(34+((-1/2)*l));
upperfootl.write(31+((6/5)*l));
lowerfootr.write(106+((12/20)*l));
lowerfootl.write(82+((1.15)*l));
delay(200);
}
}

  
void straight2(){
for (int j = 0; j<=20; j++){   
upperhipr.write(100+((-1/2)*j));
upperhipl.write(113+((-2/5)*j));
lowerhipr.write(55+((7/4)*j));
lowerhipl.write(93+((-9/10)*j));
kneer.write(40+((9/4)*j));
kneel.write(100+(-2*j));
upperfootr.write(34+((-1/5)*j));
upperfootl.write(3+((16/5)*j));
lowerfootr.write(101+((-3/10)*j));
lowerfootl.write(72+((2/5)*j));
delay(50);
}
}
