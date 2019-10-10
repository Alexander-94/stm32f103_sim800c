# stm32f103_sim800c

1. Install MDK-Core
   http://www2.keil.com/docs/default-source/default-document-library/mdk5-getting-started.pdf?sfvrsn=2[NC,L]
   https://www.keil.com/demo/eval/arm.htm#/DOWNLOAD

3. Install STM CUBE MX and create project for your controller
   https://www.st.com/en/development-tools/stm32cubemx.html#get-software

2. Install Keil uVision 5 and set it up   
   https://youtu.be/EX7g3_NUDgk

3. Find st-link programmer with SWO out pin, connect SWO to STM32f103 PB3 pin.  
   The Debug (printf) Viewer window displays data streams that are transmitted sequentially via ITM Stimulus Port 0.  
   Open the Debug (printf) Viewer from View – Serial Windows - Debug (printf) Viewer.  
   
   ![Preview](https://github.com/Alexander-94/stm32f103_sim800c/blob/master/project_img/swo2.JPG)   
   (http://www.keil.com/support/man/docs/uv4/uv4_db_dbg_printf_viewer.htm)  
   
   Settings:  
   3.1. In Keil open Manage Run-Time Environment->Compiler->I/O, check STDERR, STDIN, STDOUT as ITM:  
     ![Preview](https://github.com/Alexander-94/stm32f103_sim800c/blob/master/project_img/swo1.JPG)   
   3.2. Set the ITM Port 0 to capture the information:  
     ![Preview](https://github.com/Alexander-94/stm32f103_sim800c/blob/master/project_img/swo3.JPG)   
   3.3. Use SWD debug:
     ![Preview](https://github.com/Alexander-94/stm32f103_sim800c/blob/master/project_img/swo4.JPG)   
   3.4  Allow sending debug information by SWO pin:
     ![Preview](https://github.com/Alexander-94/stm32f103_sim800c/blob/master/project_img/swo5.png)   
     
   3.5. Modify the code:  
   ```
      #include "stm32f10x.h"  
      #include <stdio.h>  
      #define ITM_Port8(n)    (*((volatile unsigned char *)(0xE0000000+4*n)))
      #define ITM_Port32(n)   (*((volatile unsigned long *)(0xE0000000+4*n)))

      #define DEMCR           (*((volatile unsigned long *)(0xE000EDFC)))
      #define TRCENA          0x01000000
      
      int fputc(int ch, FILE *f){
          if (DEMCR & TRCENA) {
              while (ITM_Port32(0) == 0);
              ITM_Port8(0) = ch;
          }
        return(ch);
      }  
   ```  
   
   Use ```printf("Message...\n");``` from the code.
   
 
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
