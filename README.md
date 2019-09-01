# stm32f103_sim800c

1. Install MDK-Core
   http://www2.keil.com/docs/default-source/default-document-library/mdk5-getting-started.pdf?sfvrsn=2[NC,L]
   https://www.keil.com/demo/eval/arm.htm#/DOWNLOAD

3. Install STM CUBE MX and create project for your controller
   https://www.st.com/en/development-tools/stm32cubemx.html#get-software

2. Install Keil uVision 5 and set it up   
   https://youtu.be/EX7g3_NUDgk
   
# SIM800C commands from terminal:

AT+CPIN?

1. Dial the number(recieve mode ASCII, tx mode with +CR):  
ATD+38--5---49--; 

2. Send SMS  
  AT+CMGF=1;  
  AT+CSCS="GSM";  
  AT+CMGS="+38--5---49--";  
   input text  
  []  
