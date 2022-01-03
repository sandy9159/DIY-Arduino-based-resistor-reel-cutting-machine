# DIY-Arduino-based-resistor-reel-cutting-machine

![image](https://user-images.githubusercontent.com/19898602/147902191-9c92a902-9a80-4975-a0c5-6c8acd36197b.png)

Hello guys in this video I have made a Arduino based resistor reel cutting machine this machine can cut reels in small quantities. how many resistor you need to cut in single set you can provide the input to the HMI and machine do it for you. two vertical blades are connected with stepper motor and resistor is feed horizontally via roller arrangement with is powered with stepper motor.


# Material used

> Nema 17 Stepper motor 3 nos.

> Multipurpose PCB

> A4988 stepper driver 2 nos

> L2988N motor driver

> Arduino nano

# Circuit drawing

I have used my multipurpose PCB for Arduino based resistor reel cutting machine project. If you want to have this PCB
Please find the Gerber file from the link below so that you can order your own PCB
I suggest you JLCPCB.COM to choose your PCB manufacturer they really have very good service and low price
Multipurpose PCB link https://oshwlab.com/sharmaz747/multipurpose-pcb

![image](https://user-images.githubusercontent.com/19898602/147902257-944dde5c-a1d4-4965-8f19-e7afe7c55594.png)


![image](https://user-images.githubusercontent.com/19898602/147902264-771d306d-764e-43c0-839d-90a81e485e16.png)


![FTQFHXZKLBNXU2X](https://user-images.githubusercontent.com/19898602/122632825-db9b8e80-d0f2-11eb-8281-3239f1275adc.jpg)
![147494540_1146948692400891_5797782675789162173_o](https://user-images.githubusercontent.com/19898602/122632834-ee15c800-d0f2-11eb-9385-0bcb4b05119a.jpg)

Making such projects without PCB is night mare yes trust me
you cannot get wanted result and professional touch in your project if you ignore PCB
So some days back I have developed my Multipurpose PCB.
This PCB is used to build wide range of arduino projects 

followings are the some features of PCB

1. Wide range of power input 9V to 24V DC
2. Cross polarity protection
3. DC motor voltage selection 9V or 12 V DC
4. Servo motor voltage selection 5V or 9V DC
5. Manual microstepping setting for stepper motor
6. Power indication LED
7. L298N IC for heavier DC motor
8. ON board 5V and 9V regulator no need to arrange different power sources
9. Header pins and screw terminals for easy connections

List of the Components you can connect to the PCB

1. 2 DC motor ( 9V to 24V DC)
2. 2 Potentiometer
3. 2 Servo motors ( 5V to 9V DC)
4. 1 Serial device (BT module, HMI, Communication module, RX, TX)
5. 1 Encoder (2 interrupt pin & 1 PB pin)
6. 1 I2C device (SCL/SDA Device, display, MPU6050 etc)
7. 2 Stepper motors

![147560983_1146948889067538_4990854912671411429_o](https://user-images.githubusercontent.com/19898602/122632848-fff76b00-d0f2-11eb-955e-207472be636d.jpg)
![component](https://user-images.githubusercontent.com/19898602/122632849-01289800-d0f3-11eb-970a-53fc1b6e0b58.jpg)


I have design circuit and PCB in [easyEDA](https://easyeda.com/) and ordered PCB from [JLCPCB](https://jlcpcb.com/IAT )

This is the link of [PCB editabl file](https://oshwlab.com/sharmaz747/multipurpose-pcb)

If you seriously need quality PCB quickly in your hand then you must have to try [JLCPCB](https://jlcpcb.com/IAT ) PCB manufacturing service.
They have Special offer of $2 for 1-4 Layer PCBs, free SMT assembly monthly.
If you get yourself registered today at [JLCPCB](https://jlcpcb.com/IAT ) you get 27$ welcome coupon from [JLCPCB](https://jlcpcb.com/IAT ).


![image](https://user-images.githubusercontent.com/19898602/147902413-8c479fb7-86f1-4295-b4b9-e4c6cfd24a4a.png)
![image](https://user-images.githubusercontent.com/19898602/147902429-2381ce25-9def-4ef0-b7bf-b4dddc201822.png)

first of all I cut the 12mm wooden sheet in reuired length

then i placed rubber legs at the bottom of the wooden sheet

![image](https://user-images.githubusercontent.com/19898602/147902463-bb8ade28-9079-4cd0-959e-91fd8a79292c.png)
![image](https://user-images.githubusercontent.com/19898602/147902483-122b47b6-f7a8-4d28-a620-77b7b445a302.png)

Now I prepare a resistor reel pulling mechanism using some threaded rod and 
bearing

I 3D Printed yellow part in PLA material the I place the roller mechanism in the grove made in 3D printed parts
then I place the pillow bearing on the 3D printed part 8mm SS rod will went inside to this roller

![image](https://user-images.githubusercontent.com/19898602/147902770-7d411902-4a60-4ff0-a3f4-f272bfb1f0c1.png)
![image](https://user-images.githubusercontent.com/19898602/147902790-c85a6ac5-c018-4d19-a2d7-b7c2a72b21b8.png)

then I cut the acrylic sheet in to make resistor reel guide 
resistor reel will pass through this acrylic guid and reach to the cutting blade.

![image](https://user-images.githubusercontent.com/19898602/147902865-633e2789-f022-4119-92fa-3103ce09d031.png)

This are the 3D printed parts used to hold the blade and connected with stepper motor shaft 
at the one end, 

When stepper motor rotate the blade will move up adn down


![image](https://user-images.githubusercontent.com/19898602/147902927-e3839d79-8aef-4292-8714-fc71d8e50f66.png)


Then I placed the stepper motor and all white 3D printed parts on 4mm wooden sheet 
and the this wooden sheet is vertically placed on the 20x20 extrustion profile 

in this way basic cunstruction of machien is completed now we will look for code

````

#include <Stepper.h>
#include <Arduino.h>
#include "BasicStepperDriver.h"
#include "MultiDriver.h"
#include "SyncDriver.h"
#include <SoftwareSerial.h>
SoftwareSerial mySerial(2, 3); // RX, TX
int A = 0;
int B = 0;
int state = 0;
String message;
int QTY, numMessages, endBytes;
byte inByte;
int flag = 0;


Stepper myStepper(200, 8, 7, 10, 11);
#define MOTOR_STEPS 200
#define DIR_X 14
#define STEP_X 15
#define DIR_Y 16
#define STEP_Y 17
#define MICROSTEPS 16
#define EN 9
int nos = 35;
int UP = 80;
int DOWN = -80;
int count = 1;
BasicStepperDriver stepperX(MOTOR_STEPS, DIR_X, STEP_X);
BasicStepperDriver stepperY(MOTOR_STEPS, DIR_Y, STEP_Y);
SyncDriver controller(stepperX, stepperY);


void setup()
{
  numMessages, endBytes = 0;
  pinMode(EN, OUTPUT);
  digitalWrite(EN, HIGH);
  myStepper.setSpeed(60);
  Serial.begin(9600);
  mySerial.begin(9600);
  stepperX.begin(20, MICROSTEPS);
  stepperY.begin(20, MICROSTEPS);
  delay(500);
  controller.rotate(UP, UP);
  delay(500);
}

void loop()
{
  data();
  count=1;
  if (A > 0 && B > 0) {
    delay(1000);
    

    for (int i = 0; i < B; i++) {

      myStepper.step(A * nos);
      delay(500);
      stepperX.begin(1000, MICROSTEPS);
      stepperY.begin(1000, MICROSTEPS);
      controller.rotate(DOWN, DOWN);
      delay(500);
      stepperX.begin(75, MICROSTEPS);
      stepperY.begin(75, MICROSTEPS);
      controller.rotate(UP, UP);
      delay(500);
//Serial.print("done = ");
mySerial.print("n2.val=");
mySerial.print(count);
mySerial.write(0xff);
mySerial.write(0xff);
mySerial.write(0xff);
count++;

    }
    A=0;
    B=0;

  }
}

  void data() {
    if (state < 1) {
      if (numMessages == 1) { //Did we receive the anticipated number of messages? In this case we only want to receive 1 message.
        A = QTY;
       // Serial.println(A);//See what the important message is that the Arduino receives from the Nextion
        numMessages = 0; //Now that the entire set of data is received, reset the number of messages received
        state = 1;
      }
    }

    if (state > 0) {
      if (numMessages == 1) { //Did we receive the anticipated number of messages? In this case we only want to receive 1 message.
        B = QTY;
       // Serial.println(B);//See what the important message is that the Arduino receives from the Nextion
        numMessages = 0; //Now that the entire set of data is received, reset the number of messages received
        state = 0;
      }
    }






    if (mySerial.available()) { //Is data coming through the serial from the Nextion?
      inByte = mySerial.read();

      // Serial.println(inByte); //See the data as it comes in

      if (inByte > 47 && inByte < 58) { //Is it data that we want to use?
        message.concat(char(inByte)); //Cast the decimal number to a character and add it to the message
      }
      else if (inByte == 255) { //Is it an end byte?
        endBytes = endBytes + 1; //If so, count it as an end byte.
      }

      if (inByte == 255 && endBytes == 3) { //Is it the 3rd (aka last) end byte?
        QTY = message.toInt(); //Because we have now received the whole message, we can save it in a variable.
        message = ""; //We received the whole message, so now we can clear the variable to avoid getting mixed messages.
        endBytes = 0; //We received the whole message, we need to clear the variable so that we can identify the next message's end
        numMessages  = numMessages + 1; //We received the whole message, therefore we increment the number of messages received.

        //Now lets test if it worked by playing around with the variable.

      }
    }
  }
  
  ````
  
  


