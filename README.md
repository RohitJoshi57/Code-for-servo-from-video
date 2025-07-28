# Code-for-servo-from-video
This was a fun project that allowed me to get re-familiar with circuitry and works as an introduction to making robots. (This is the video I used as a tutorial https://www.youtube.com/watch?v=EAyzRmAKueE ) 

#include "AS5600.h"
#include "Wire.h"

#define IN_3 6
#define IN_4 5

AS5600 as5600;

int power = 0;
int encoder = 0;
int setpoint = 1000;
int diff = 0;

void setup() {
  pinMode(IN_3, OUTPUT);
  pinMode(IN_4, OUTPUT);

  Serial.begin(9600);
  Wire.begin();
  Serial.println(__FILE__);

  as5600.begin(4);
  as5600.setDirection(AS5600_CLOCK_WISE);
  int b = as5600.isConnected();
  Serial.print("Connect: ");
  Serial.println(b);
  delay(1000);
}

void loop() {
encoder = as5600.readAngle();
Serial.print("Encoder angle: ");
Serial.println(encoder);

diff = encoder - setpoint;

power = 0;

if (abs(diff) < 82){
    power = 0;
} 
else { 
    power = map(abs(diff), 0, 4096-setpoint, 40,255);
}
if (diff > 0){

    digitalWrite(IN_3, LOW);
    analogWrite(IN_4, power);
}
else{
    digitalWrite(IN_3, power);
    analogWrite(IN_4, LOW);  
}
}

