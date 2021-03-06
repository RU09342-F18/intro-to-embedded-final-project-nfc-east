# NFC Security - Greek Freaks
## Abstract
This repository acts as a near field communication (NFC) security system. The project uses a MSP430F5529, Arduino Uno, PN532 breakout board, two LEDs, and a servo motor. This repository includes a manipulated Arduino library for configuring and displaying the PN532 breakout board NFC reading on Arduino's serial monitor and code for the MSP430F5529 that takes an input from a serial program and determines if the input matches a specific NFC ID using an algorithm to activate the servo motor, LEDs, and display. Output messages are displayed on the serial monitor. These two systems are independent of each other and need a user to read the ID and input it into a serial program such as Realterm. 
## Inputs
### MSP430F5529 
The inputs of this system are a series of five bytes. The first four bytes are the four corresponding bytes of the NFC tag ID. The fifth byte is an optional administrator byte that has the ability to change the specific tag able to drive the outputs. The format of the input must be 0xFF as the bytes are displayed and inputted in hexadecimal. The administrator byte is 0xCC. If this byte is entered after a set of four bytes is entered, the outputs will be activated and the new ID will be the only ID that can activate the system. The serial program used is called Realterm. It must be configured with the port the microcontroller is connected to and set to 9600 baud rate. This program uses UART to transmit and receive data from and to the MSP430F5529. 
### Arduino Uno/PN532 Breakout Board 
This system includes two parts. The PN532 breakout board communicates with the Arduino Uno based on an input. The input is an ISO14443A standard NFC tag, and in particular a Rowan University student ID. The breakout board reads data off of the ID using passive communication and sends it to the Arduino Uno by means of SPI. To do this, the student ID must be placed within 10 cm. of the PN532. The Arduino Uno takes this information and gives a readable output. 
## Outputs 
### MSP430F5529 
This part of the system includes multiple outputs. One, if the correct ID is inputted, a servo motor will activate and spin until another byte is sent. Once another byte is entered, the servo motor will stop. The servo motor is PWM controlled via P2.5 using the hardware capabilities of TimerA2. Two, via a GPIO pin on P1.6, two LEDs, red and green, are powered from a 5 V source using a converto box. The red LED is powered on when the system is not activated, or a correct ID or administrator byte is inputted. The green LED is powered on when the correct ID or administrator byte is inputted. Three, a series of characters are transmitted over UART back to the Realterm serial program. In the code, P4.4 and P4.5 are initialized, which are the UART Tx and Rx pins that communicate to a PC via USB. If an incorrect ID is entered, the letter "D" is displayed for denied access. If the correct ID is entered, the letter "G" is displayed for granted access. If the administrator byte is entered, the letter "C" is displayed for changed ID. 
### Arduino Uno/PN532 Breakout Board 
The NFC information from the student ID is displayed on Arduino serial monitor. The library will display a sequence of four bytes corresponding to the specific ID in the format (0xFF 0xFF 0xFF 0xFF). The table below shows the connections between the PN532 and Arduino Uno via jumper cables. 

PN532 Breakout Board | Arduino Uno |
| --- | --- |
| GND | GND |
| 5V | 5V |
| SDA  | A4 |
| SCL | A5 |
| RSTO  | 3 |
| IRQ | 2 |
