# Smart-and-Automatic-Wheelchair-For-Disable-Person-With-Hand-Gesture
# Arduino Coding
#include<Wire.h>
#include <SPI.h>

#define relay1 6
#define relay2 7
#define relay3 8
#define relay4 9

 
int minVal=265;
int maxVal=402;
 
double x;
double y;
double z;

void setup() {
  // put your setup code here, to run once:
  Wire.begin();
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x6B);
  Wire.write(0);
  Wire.endTransmission(true);
  Serial.begin(9600);

pinMode(relay1, OUTPUT);
pinMode(relay2, OUTPUT);
pinMode(relay3, OUTPUT);
pinMode(relay4, OUTPUT);

digitalWrite(relay1, LOW);
digitalWrite(relay2, LOW);  
digitalWrite(relay3, LOW);
digitalWrite(relay4, LOW);

}

void loop() {

  // put your main code here, to run repeatedly:

Wire.beginTransmission(MPU_addr);
Wire.write(0x3B);
Wire.endTransmission(false);
Wire.requestFrom(MPU_addr,14,true);
AcX=Wire.read()<<8|Wire.read();
AcY=Wire.read()<<8|Wire.read();
AcZ=Wire.read()<<8|Wire.read();
int xAng = map(AcX,minVal,maxVal,-90,90);
int yAng = map(AcY,minVal,maxVal,-90,90);
int zAng = map(AcZ,minVal,maxVal,-90,90);
 
x= RAD_TO_DEG * (atan2(-yAng, -zAng)+PI);
y= RAD_TO_DEG * (atan2(-xAng, -zAng)+PI);
z= RAD_TO_DEG * (atan2(-yAng, -xAng)+PI);
 
Serial.print("AngleX= ");
Serial.println(x);
 
Serial.print("AngleY= ");
Serial.println(y);
 
Serial.print("AngleZ= ");
Serial.println(z);
Serial.println("-----------------------------------------");

if(x>210 && x<250)
{
  drive('L');
}
else if(x>150 && x<160)
{
  drive('R');
}
else if(z>310 && z< 360)
{
  drive('B');
}
else if(z>150 && z< 200)
{
  drive('F');
}
else{
    drive('Z');
}
delay(500);

}

void drive(char inChar)
{
 
     if(inChar=='F'){
    digitalWrite(relay1, HIGH);
    digitalWrite(relay2, LOW);
    digitalWrite(relay3, HIGH);
    digitalWrite(relay4, LOW);
    Serial.println('F');
    }
    else if(inChar=='B'){
    digitalWrite(relay1, LOW);
    digitalWrite(relay2, HIGH);
    digitalWrite(relay3, LOW);
    digitalWrite(relay4, HIGH);
    Serial.println('B');
    }  
    else if(inChar=='R'){
    digitalWrite(relay1, LOW);
    digitalWrite(relay2, HIGH);
    digitalWrite(relay3, HIGH);
    digitalWrite(relay4, LOW);
    Serial.println('R');
    } 
    else if(inChar=='L'){
    digitalWrite(relay1, HIGH);
    digitalWrite(relay2, LOW);
    digitalWrite(relay3, LOW);
    digitalWrite(relay4, HIGH);
    Serial.println('L');
  }
    else{
    digitalWrite(relay1, LOW);
    digitalWrite(relay2, LOW);
    digitalWrite(relay3, LOW);
    digitalWrite(relay4, LOW);
  }

}
