# Microbit_tutorial    
-------------------
**Makecode and Microbit basics:**    
If you don't have makecode and microbit basics, you can follow the link to learn the basics: [Click Me](https://docs.mosiwi.com/en/latest/microbit/M1D0000_microbit_mainboard/M1D0000_microbit_mainboard.html)      

## Wiring diagram        
-----------------
![Img](../_static/microbit_tutorial/img/1img.jpg)   

The USB port of the Microbit must be connected to the usb port of the PC through a micro usb cable.   

## Create a new project      
-----------------------          
Open the link to create a new online project: <https://makecode.microbit.org>     
![Img](../_static/microbit_tutorial/img/2img.png)  
  
## Load extension library      
-------------------------
![Img](../_static/microbit_tutorial/img/3img.png)  

Fill in the link to the right of the chain and search for:    
<https://github.com/Mosiwi/Mosiwi-basic-learning-kit-for-microbit>     
![Img](../_static/microbit_tutorial/img/4img.png)         
Wait for the loading of the library. After the loading is successful, the following figure is shown:   
![Img](../_static/microbit_tutorial/img/5img.png)       

## Block code parsing        
---------------------
▶ 1: ![Img](../_static/microbit_tutorial/img/6img.png)     
The module returns a value that is the numeric input value for P11. When the keyboard on the sheild is clicked, a low level is returned, indicating that the keyboard value on the sheild can be read.        
![Img](../_static/microbit_tutorial/img/7img.jpg)     
Usage:      
Press any keyboard on the sheild, the microbit dot matrix displays 0, otherwise displays 1.    
![Img](../_static/microbit_tutorial/img/8img.png)   
Because the P11 is also mapped to the microbit Buttom B, the keyboard value of the sheild can also be read through the microbit Buttom B.     
![Img](../_static/microbit_tutorial/img/9img.png)       


▶ 2: ![Img](../_static/microbit_tutorial/img/10img.png)      
Read the keyboard values on the sheild.     
Usage:   
Press any key on the sheild, and the microbit dot matrix displays the key value.       
![Img](../_static/microbit_tutorial/img/11img.png)      
Key value: U = 16, D = 8, L = 4, R = 2, OK = 1   

▶ 3: ![Img](../_static/microbit_tutorial/img/12img.png)     
Clear the display on the 4-digit nixtube of the sheild.   

▶ 4: ![Img](../_static/microbit_tutorial/img/13img.png)   
4-digit nixie tube displays numeric values.   
Floating-point number display range: 0.0 to 999.9     
Integer range: 0 to 9999    
Usage:   
![Img](../_static/microbit_tutorial/img/14img.png)     

▶ 5: ![Img](../_static/microbit_tutorial/img/15img.png)   
Let one of the 4-digit nixie tubes display the number.   
Parameter 1: 0 to 3, the position of the nixie tube to be lit.   
Parameter 2: 0 to 15 (0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F), characters to display.   

Usage:   
![Img](../_static/microbit_tutorial/img/16img.png)   

▶ 6: ![Img](../_static/microbit_tutorial/img/17img.png)   
A display segment in a nixie tube.       
![Img](../_static/microbit_tutorial/img/18img.png)   

▶ 8: ![Img](../_static/microbit_tutorial/img/19img.png)    
Let one of the 4-digit nixie tubes display a segment.   
Parameter 1: 0 to 3, the position of the nixie tube to be lit.   
Parameter 2: ![Img](../_static/microbit_tutorial/img/17img.png)     

Usage:   
![Img](../_static/microbit_tutorial/img/20img.png)    
The 4th digit tube display 2 segments.    

▶ 9: ![Img](../_static/microbit_tutorial/img/21img.png)   
Light an LED in the strip.  
Parameter 1: 0 to 8, the position of the LED to be lit.   
Parameter 2: ![Img](../_static/microbit_tutorial/img/22img.png)(0: OFF, 1: ON)      

Usage:   
![Img](../_static/microbit_tutorial/img/23img.png)       

▶ 10: ![Img](../_static/microbit_tutorial/img/24img.png)    
Controls the brightness of the RGB LED on the sheild.    
Parameter 1: Red LED, green LED, blue LED.     
Parameter 2: 0 to 1023, brightness.   

▶ 11: ![Img](../_static/microbit_tutorial/img/25img.png)     
Initial temperature and hygrometer.    

▶ 12: ![Img](../_static/microbit_tutorial/img/26img.png)   
Read the temperature and humidity values and verify that the values are correct. Execute this block at a frequency greater than or equal to 2 seconds.        

▶ 13: ![Img](../_static/microbit_tutorial/img/27img.png)      
Get the temperature or humidity value.   

Usage:   
![Img](../_static/microbit_tutorial/img/28img.png)     

▶ 14: ![Img](../_static/microbit_tutorial/img/29img.png)   
When the ultrasonic module is connected to the Sensor interface of the sheild, the measurement distance value of the ultrasonic module can be read. The unit is centimeter.           
![Img](../_static/microbit_tutorial/img/30img.jpg)     

Usage:     
![Img](../_static/microbit_tutorial/img/31img.png)   

▶ 15: ![Img](../_static/microbit_tutorial/img/32img.png)     
The value of the infrared receiver, microphone or sliding resistor on the sheild is read through the [I2C protocol](https://docs.mosiwi.com/en/latest/common/C1E0000_3in1_basic_learning_shield/C1E0000_3in1_basic_learning_shield.html#io-expand).        

Usage:   
Infrared receiver ([NEC](https://docs.mosiwi.com/en/latest/various_resources/nec/nec.html))   
![Img](../_static/microbit_tutorial/img/33img.png)    
![Img](../_static/microbit_tutorial/img/34img.jpg)     
![Img](../_static/microbit_tutorial/img/40img.png)     

Microphone    
![Img](../_static/microbit_tutorial/img/35img.png)    

Sliding resistor   
![Img](../_static/microbit_tutorial/img/36img.png)    

▶ 16: ![Img](../_static/microbit_tutorial/img/44img.png)      
When the fan module is connected to the fan port on the sheild, it is used to control the steering and speed of the fan module.    
![Img](../_static/microbit_tutorial/img/45img.jpg)     
Parameter 1: CW = clockwise rotation.     
Parameter 2: CCW = counterclockwise rotation.    

▶ 17: ![Img](../_static/microbit_tutorial/img/37img.png)     
Read the key value on the sheild, return 0 when the key is pressed, otherwise return 1. This key is connected to the P5 pin of the microbit.   
![Img](../_static/microbit_tutorial/img/38img.jpg)     

▶ 18: ![Img](../_static/microbit_tutorial/img/41img.png)      
Reads a byte of data from the memory of the sheild.    
Parameter: 0 to 16, Address of the memory.     

▶ 19: ![Img](../_static/microbit_tutorial/img/42img.png)    
Write a byte of data to the memory of the sheild.   
Parameter 1: 0 to 16, Address of the memory.      
Parameter 2: 0 to 255, Data to be stored in memory.  

Usage:     
![Img](../_static/microbit_tutorial/img/43img.png)     
4 digit nixie tube display 255.   

**End!**    
   
