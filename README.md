Write a sketch that allows a user to access data in EEPROM using the serial monitor. In the serial monitor the user should be able to type one of two commands: “read” and “write. "Read" takes one argument, an EEPROM address. "Write" takes two arguments, an EEPROM address and a value.

For example, if the user types “read 3” then the contents of EEPROM address 3 should be printed to the serial monitor. If the user types “write 3 10” then the value 10 should be written into address 3 of the EEPROM.

# Author: Niam Moltta
#include <EEPROM.h>

String command;

void setup() {
  
  Serial.begin(9600);
  
}

void loop() {

  if (Serial.available() > 0) {
    
    command = Serial.readStringUntil('\n'); 
    String commandRead = command;
    String commandWrite = command;
    String sRead = "read";
    String sWrite = "write";
    commandRead.remove(4);
    commandWrite.remove(5);

    if (commandRead.equals(sRead)) {
      
      String sreadArg1 = command.substring(5);
      
      int readArg1 = sreadArg1.toInt();
      
      if (readArg1 > 1023 || readArg1 < 0) {
        
        Serial.println("Invalid, please enter a value from 1 to 1023");
      
      }

      else { 
        int valueEEP = EEPROM.read(readArg1); 
        Serial.print("The value is ");
        Serial.println(valueEEP, DEC);
      }
    }

    if (commandWrite.equals(sWrite)) { 
      
      String swriteArgs = command;
      
      swriteArgs.remove(0, 6);
      
      int swriteSpace = swriteArgs.indexOf(' ');
      
      String swriteArg1 = swriteArgs.substring(0, swriteSpace);
      
      int writeArg1 = swriteArg1.toInt(); 
      
      String swriteArg2 = swriteArgs.substring(swriteSpace + 1); 
      
      int writeArg2 = swriteArg2.toInt(); 

      if (writeArg1 < 0 || writeArg1 > 1023 || writeArg2 < 0 || writeArg2 > 255) {
        
        Serial.println("Invalid, enter a first number from 0 to 1023 and the second number from 0 to 255");
        
      }

      else { 
        byte byte1Arg2 = writeArg2 & 0XFF;
        
        byte byte2Arg2 = (writeArg2 >> 8) & 0XFF;
        
        EEPROM.write(writeArg1, byte1Arg2);
        
        EEPROM.write(writeArg1 + 1, byte2Arg2);
        
        Serial.println("Success!");
        
   }
  }
 }
}
