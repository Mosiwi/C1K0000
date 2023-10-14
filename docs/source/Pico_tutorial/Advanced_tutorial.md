# Advanced tutorial     
-------------------
Learn the communication protocol, driver programming and MicroPyhotn module based on the "Pico board + expansion board" to learn MicroPython programming at the lowest cost.         

## Previous preparation    
-----------------------
**Pico and Thonny basics:**     
If you don't have Pico and Thonny basics, you can follow the link to learn the basics: [Click Me](../../../raspberry/R1D0001_raspberry_pico/R1D0001_raspberry_pico.md)    


## Chapter1 Module for Pico    
---------------------------       
When you have multiple xx.py files, you can put them in a single folder to form a module file.      
1\. Create a new folder with an empty "\_\_init\_\_.py" file inside:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/1img.png)       

2\. Then create a new file called "test.py" in that folder, and create a function and a class in that file:      
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/2img.png)       
```
def hello():
    print("hello!")
    
class prt:
    def __init__(self):
        pass
    def hello(self):
        print("class: hello!")
```

3\. Upload the folder to Pico:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/3img.png)       

4\. Create a new file called "module_test.py" and call the functions and classes from the module:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/4img.png)       
```
import time                             # Importing the time class
from mosiwi_lib.test import hello, prt  #  Import prt class and hello function from the machine module.

Class = prt()
while True:                      # Always empty loop
    hello()
    Class.hello()
    time.sleep(1)                # sleep for 1 second
```

5\. The code runs successfully as follows:    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/5img.png)       


## Chapter2 Serial port      
----------------------- 
**1. Puzzled**    
There are many serial port products on the market, such as RS232, RS485, TTL, which can be divided into simplex, half-duplex and full-duplex communication, but they cannot be used together, which can easily confuse everyone. This is because people don't know the difference between them. Now let's study them in detail and divide them into protocol layer, physical layer and communication to explain.       

**2. Protocol layer**      
The serial port communication protocol mentioned here is based on UART (Universal Asynchronous Receiver-Transmitter), which specifies the format of sending and receiving data between two devices. The device has two hardware units, one is the transmitter (TX) and the other is the receiver (RX). The connection between the two devices is as follows:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/21img.png)       

The main purpose of each device's transmitter and receiver is to send and receive data on the serial data line.    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/22img.png)       
The transmitting UART is connected to the control data bus that transmits data in parallel, and the parallel data is serially transferred bit by bit on the transmission line to the receiving UART. The receiver converts the serial data to parallel data and transmits it to the parallel device.     

For UART and most serial communications, the same baud rate needs to be set on the sending and receiving devices. In serial port communications, the baud rate determines the maximum number of bits transferred per second.    

UART summary:    
1. Number of wires: 2   
2. Baud rate: 9600, 19200, 38400, 57600, 115200, 230400, 460800, 921600, 1000000, 1500000    
3. Transmission method: asynchronous     
4. Maximum number of hosts: 1     
5. Maximum number of slaves: 1      

A UART device does not use a clock signal to synchronize the transmitter and receiver devices, it transmits data asynchronously. While the receiver uses its internal clock signal to sample the incoming data, the transmitter also generates the bit stream based on its internal clock signal, and no clock signal is required between the receiver and transmitter to synchronize the data. Use the same baud rate on both devices to manage the synchronization point, failure to do so may affect the time to send and receive data, resulting in discrepancies in data processing. Under the premise that the timing of the bits does not deviate too much, the maximum allowable difference in the baud rate is 10%.       

In UART devices, data is transferred in packets. The transmitter and receiver need to create serial data packets and control these physical hardware lines for data transfer. A data packet consists of start bits, data frames, parity bits, and stop bits.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/23img.png)       

Start bit:    
The data line of a UART device is normally held high when not transmitting data. To start a data transfer, the transmitting UART pulls the transmission line from high to low for 1 clock cycle. When the receiving UART detects a high-to-low transition, it starts reading the bits in the data frame at the set baud rate.    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/24img.png)       

Data frame:     
The data frame contains the actual data being transmitted. If parity bits are used, it can be 5 and up to 8 bits. If parity bits are not used, the data frame can be 9 bits. In most cases, the lowest bit of the data is sent first.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/25img.png)       

Parity bits:    
The parity bit describes whether a number is even or odd. The parity bit is a way for the receiving UART to determine whether the data has changed during transmission. Bits can be altered by electromagnetic radiation or mismatched baud rates.     
After the receiving UART reads the data frame, it calculates the sum of the values of the bits in the data frame that are 1 and checks whether the sum is even or odd.  If the parity bit is 0, the sum of bits that are 1 in the data frame should be even. If the parity bit is 1, the sum of bits that are 1 in the data frame should be odd.    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/26img.png)       

To signal the end of a data packet, the transmitting UART drives the data transmission line from low voltage to high voltage for a duration of 1 to 2 bits.    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/27img.png)       


**UART transmission steps:**      
1\. The sending UART receives data from the data bus in parallel.   
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/28img.png)       

2\. The sending UART adds the start, parity and stop bits to the data frame.    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/29img.png)       

3\. The entire data packet is sent serially to the receiving UART, which samples the data line at a preconfigured internal baud rate.   
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/30img.png)       

4\. After verifying the received data successfully, the receiving UART discards the start bit, parity bit and stop bit of the data frame.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/31img.png)       

5\. The receiving UART converts the serial data back to parallel data and transmits it to the parallel data bus on the receiving end.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/32img.png)       

**3. physical layer**      
The physical layer is to optimize the interface shape or communication data level voltage, so that the transmission distance is longer, and the data transmission is faster and more accurate.       

1\. RS232    
The RS-232 interface conforms to the serial data communication interface standard formulated by the Electronic Industry Alliance (EIA) of the United States, and its full name is EIA-RS-232. It defines the interface between DTE (data terminal devices such as computers) and DCE (data communication devices such as modems) devices using serial binary data exchange.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/33img.png)       

The line voltage range of RS232 is -25V to +25V. They are divided into signal voltage and control voltage.      
The signal voltage between +3V and +25V represents logic "1", and the signal voltage between -3V and -25V represents logic "0". However, the control voltage signal uses negative logic, that is, logic "1" represents -3 to -25 volts and logic "0" represents +3V to +25V. The voltage from -3V to +3V is considered as an uncertain state.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/34img.png)       

DB9 interface:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/35img.png)       

2\. RS485     
The RS-485 standard name is TIA/EIA-485-A, RS-485 makes up for the shortcomings of RS-232 short communication distance and low rate. RS-485 is differential transmission and uses A pair of twisted pair wires where one wire is defined as A and the other as B with R receiver and D transmitter inside. It is recommended to add pull-up and pull-down resistors to the terminals of the bus to eliminate interference signals generated externally when the bus is idle.      
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/36img.png)       

Pin Description:      
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/37img.png)       

Calculate the resistance formula (assuming that the terminal resistance Rt = 120 Ω, 200mV is the minimum threshold voltage of R receiver) :     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/38img.png)       

Truth table:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/39img.png)       

Wiring:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/40img.png)       
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/41img.png)       

3\. TTL Serial      
TTL (Transistor-Transistor Logic) is composed of three wires, which are the data transmitting line (TXD), the data receiving line (RXD), and the common ground (GND).     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Arduino_tutorial/Advanced_tutorial/42img.png)       

TTL level:      
| Output low (0) | Output high (1) | Input low (0) | Input high (1) |  
| :--: | :--: | :--: | :--: |  
| <0.8V | >2.4V	| <1.2V	| >2.0V |   

**4. Communication direction**       
With only one data line, data can only be transmitted in one direction.    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/43img.png)        
With only one data line, two devices can send data to each other, but data can only be sent from one device to the other at the same time.       
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/44img.png)     
There are two data lines, two devices can send data to each other, and the data of two devices can be sent to each other at the same time.           
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/45img.png)       

**5. Overview**     
The serial port we are talking about is an overview. The interface that can send data or receive data serially in a piece of data can be called a serial port.      
1. RS232, RS485, TTL serial ports, etc. can also send data or receive data serially in one piece of data, so they can all be called serial ports. It's just that they specify the electrical properties on the data line, and they don't specify how the data is transmitted.     
2. We can run different data transmission protocols on RS232, RS485, and TTL serial ports, such as the Universal Asynchronous Receiver-Transmitter (UART) protocol explained in the protocol layer above, and you can also run your own defined protocols.     
3. Simplex, half-duplex, and full-duplex communication only stipulate the direction of data transmission at the same time, and do not involve any data transmission protocol and electrical properties.      


## Chapter3 I2C communication protocol      
--------------------------------------
**1. Overview**      
I2C is a communication protocol developed by Philips Semiconductor for data transfer between a host and multiple slaves on the same circuit board using two common wires.    
This is a synchronous serial communication protocol where the data bits are transmitted one after the other according to a pulse signal set by a clock line.    

Here are some important features of the I2C communication protocol:     
1. Only two common buses are needed to control any device on the I2C network.   
2. I2C networks are easily extensible. The new device can simply be connected to two generic I2C bus lines.    
3. There is no need to agree on the data transmission rate in advance as in UART communication. Therefore, the data transfer speed can be adjusted at any time.     
4. Use 7-bit addressing to find a specific device on the I2C bus.      

**2. Wiring**       
The I2C bus consists of only two lines, called Serial Clock Line (SCL) and Serial Data line (SDA). The data to be transmitted is sent through the SDA line, and the clock signal of SCL is used to synchronize the data. All devices on the I2C network are connected to the same SCL and SDA lines as follows:    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/52img.png)        

Both I2C buses (SDA, SCL) operate as open-drain drivers. This means that any device on the I2C network can drive SDA and SCL to low, but cannot drive them to high. Therefore, a pull-up resistor is used for each bus to keep them high by default.    

Master and slave devices:     
1. Devices connected to the I2C bus are classified as master or slave. At any instant, only one host remains active on the I2C bus. It controls the SCL clock line and decides what operation to do on the SDA data line.    
2. To distinguish between multiple slaves connected to the same I2C bus, each slave is physically assigned a permanent 7-bit address.    
3. When a master device wants to transfer data to or read data from a slave device, it specifies this specific slave address on the SDA line and proceeds to transfer or read data. All other slave devices do not respond unless their address is the same as the address specified by the master device on the SDA line.   
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/53img.png)        

**3. Data stream**      
The I2C data is transmitted as a data stream. The data stream is decomposed into multiple data frames. Each data stream has an address frame containing the binary address of the slave device, and one or more data frames containing the data being transferred. The data stream also includes start and stop conditions, read/write bits, and ACK/NACK bits between each data frame:    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/54img.png)        

Validity analysis of data:    
In the protocol, there is a special requirement for the SDA line and SCL line to transfer data bits. When the SCL line is HIGH, the state (level) on the SDA line is stable. The state (level) of the data line can be changed only if the SCL line is LOW (low level). Each data bit needs to generate a clock pulse.      
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/55img.png)        

Starting condition:     
Whenever the master starts communication, it switches the SDA line from high to low before switching the SCL line from high to low. Once the master sends the start condition, all slave devices are activated even if they are in sleep mode and wait to receive the address bit.      
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/56img.png)        

Address frame:    
It consists of 7 bits and fills in the address of the slave device where the master device needs to send/receive data. All slave devices on the I2C bus compare these address bits to their addresses.    

Read/write bits:   
This bit specifies the direction of data transmission. This bit is set to "0" if the master device needs to send data to the slave device. Set to "1" if the master device needs to read data from the slave device.    

ACK/NACK bits:     
When the receiving device has successfully received the correct address or data frame and can send another byte, the bit is set to 0 (ACK).     

There are five cases in which the bit is NACK set by the receiving device:    
1. The receiving device received the wrong address.     
2. The receiver is busy.     
3. The receiving device receives commands or data it does not understand.    
4. The host has sent more bytes than the receiving device allows.    
5. When the host is reading a data frame (in read mode), it should generate a stop signal.   

![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/57img.png)        
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/58img.png)        

| Master sends an ACK signal | Master sends a NACK signal | Master reads ACK/NACK signal |
| :--: | :--: | :--: |
| ![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/59img.png) | ![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/60img.png) | ![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/61img.png) |

Data frames:     
It consists of 8 bits, which are generated by the master or slave device. This frame is followed by an ACK/NACK bit that is set to "0" if the next data frame is to be received; Otherwise, it is set to "1". Data frames are in a format followed by ACK/NACK bits so that multiple data frames can be sent or received repeatedly.     

Stop condition:    
When the SDA line has transmitted the required data block, the master switches the SCL line from low to high level, and then the SDA line from low to high level. At this point, the master is disconnected from the slave.    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/62img.png)        

More info: [I2C](../../../_static/pdf/communication_protocol/UM10204%28I2C_Bus_Specification_and_User_Manual_%29.pdf)


## Chapter4 SPI communication protocol   
--------------------------------------            
**1. Overview**       
The Serial Peripheral Interface (SPI), developed by MOTOROLA in the 1980s, is a master-slave synchronous serial communication protocol. The interface of this protocol enables full-duplex communication at a very high speed, providing a simple and low-cost interface between the microcontroller and peripheral devices.     

**2. Interface**     
The SPI bus has four signal lines:   
1. Master–Out/Slave–In (MOSI)
2. Master–In/Slave–Out (MISO)
3. Serial Clock (SCLK)
4. Chip Select (CS) or Slave Select (SS)

The following diagram depicts how the SPI bus master (processor) is connected to the slave (peripheral):     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/63img.png)        

**3. Internal principle**       
The hardware requirements for implementing SPI are very simple. For example, a master device and a slave device are connected using the SPI bus. The following figure shows the minimum configuration requirements for both devices.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/64img.png)          
As can be seen from the figure above, the master device consists of a shift register, a data latch, and a clock generator. Slave devices consist of similar hardware: shift registers and data latches. Two shift registers are connected to form a loop. It takes 8 clock cycles for the device to transmit one byte of data.       
During each SPI clock cycle, a full-duplex data transfer occurs. The master sends a bit on the MOSI line and the slave reads it, while the slave sends a bit on the MISO line and the master reads it. This ordering is maintained even if only one-way data transfer is intended.       

**4. Work mode**        
The job of the master device is to generate clock signals of a specific frequency and assign them to the slave devices in order to synchronize data between the master and slave devices.     

The master and slave must also agree on certain synchronization protocols. Thus, two features of the clock, namely clock polarity (CPOL or CKP) and clock phase (CPHA), enter the picture.     
■ Clock polarity determines the state of the clock. When CPOL is 0, the clock signal generated by the master device is low when idle. When CPOL is 1, the clock signal is high when idle.      
■ The clock phase determines at what time on the clock the data is sent. When CPHA is 0, the data is sent at the first edge of the periodic clock. When CPHA is 1, the second edge of the data cycle clock is sent.     
■ According to the clock polarity (CPOL) and clock phase (CPHA) values. SPI has 4 modes of operation: modes 0 to 3.      
| SPI mode | Clock polarity(CPOL/CKP) | Clock phase(CPHA) | SCK level at idle time | Sampling time |   
| :--: | :--: | :--: | :--: | :--: |   
| 0 | 0 | 0 | Low level | The first edge |  
| 1 | 0 | 1 | Low level | The second edge |  
| 2 | 1 | 0 | High level | The first edge |  
| 3 | 1 | 1 | High level | The second edge |   

![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/65img.png)        
<span style="color: rgb(255, 76, 65);">Note: The master and slave need to work in the same mode to communicate normally.</span>      

**5. Connection mode**     
Independent master-slave configuration:    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/66img.png)        

Daisy chain configuration:   
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/67img.png)        

More info: [SPI](../../../_static/pdf/communication_protocol/SPI_Block_Guide_V03.06.pdf)    


## Chapter5 3-wire communication          
--------------------------------             
**1. Overview**      
This chapter will not explain the communication protocol, is based on 74HC595 serial input parallel output chip, explain the 3-wire communication mode.   

**2. 74HC595 internal hardware**       
Pin description:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/68img.png)        

Built-in function:    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/69img.png)        
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/70img.png)        

**3. Working principle**      
The 74HC595 has one 8-bit shift register, one memory register, and one 3-state parallel output bus. When OE is enabled (low level), data on the Ds enters the shift register at the rising edge of the SHcp clock, the storage register at the rising edge of the STcp clock, and outputs to the parallel bus. When the serial output (Q7 ') pin is on the eighth rising edge of the SCHcp clock, it will output the data of the first rising edge of the SCHcp clock, and so on, and then output the data of the second rising edge of the SCHcp clock.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/71img.png)        


## Chapter6 Infrared NEC communication protocol     
-----------------------------------------------            
Please refer to: [NEC communication protocol](../../../common_resource/nec_communication_protocol/nec_communication_protocol.md)      


## Chapter7 OneWire communication protocol     
------------------------------------------           
**1. Overview**      
1-Wire is a bidirectional, half-duplex slow serial communication standard developed by Dallas Semiconductor Corp (now Maxim Integrated) that uses a single signal data line for communication. The standard data rate is 15.4kbps. But it is possible to overdrive 1-Wire communication to up to 125kbps.    

**2. Hardware wiring**     
The slave device on the 1-Wire bus can be powered by an external power source and also by a signal line. Most 1-Wire slave devices require only very low power or no power pins, and when powered by a parasitic power supply, the current on the bus is charged through diodes inside the slave device to capacitors inside, thereby powering the slave device.      
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/72img.png)        

Each slave device on the bus contains a unique 64-bit code stored in ROM. The lowest 8-bit code is used to describe the type of slave device. The next 48 bits contain a unique serial number. The highest 8 bits contain the cyclic redundancy check generated from the previous 56-bit encoding.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/73img.png)        

The 1-Wire bus is a master-slave communication system in which a master device is connected to one or more slave devices via a single data line. The data line requires a resistor pull-up to the power line, typical pull-up resistor values are between 1 kΩ and 4.7 kΩ. According to the power supply method, it can be divided into two wiring methods.   
■ Power supply:     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/74img.png)        

■ Parasitic power supply:    
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/75img.png)        

**3. protocol analysis**     
The single-bus communication protocol defines several signal command types: reset pulse, presence pulse, write 0, write 1, read 0, read 1. Except for the presence pulse, all signal commands are initiated by the bus host.       
Let's take the DS18B20 temperature sensor as an example to explain the protocol.     

■ Initialization sequence - reset and presence pulse   
All communication with the slave starts with the initialization sequence, which consists of a reset pulse from the master followed by a presence pulse from the slave. When a slave sends a presence pulse in response to a reset pulse, it indicates to the host that it is attached to the bus and is ready to run. During the initialization sequence, the host sends a reset pulse by pulling the single bus down for at least 480 µs. The bus host then releases the bus into receive mode. When the bus is released, the 4.7kΩ pull-up resistor pulls the bus higher. When the slave detects this rising edge, it waits 15µs to 60µs and then emits a presence pulse by pulling the single bus down 60µs to 240µs.      
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/76img.png)        

■ Write time slot    
There are two kinds of write time slots: "write 1" time slots and "write 0" time slots. The bus host writes a logical 1 to the slave device through the write 1 time slot and a logical 0 to the slave device through the write 0 slot. All write time slots must last at least 60µs and there must be a recovery time of at least 1µs between two write time slots. Both write slots are initiated by the host pulling the single-bus low. To generate a write 1 time slot, the bus host must release the single bus within 15µs after pulling it low. When the bus is released, the 4.7kΩ pull-up resistor pulls the bus higher. To generate a write 0 slot, after pulling a single bus low, the bus host must keep the bus low for the entire time slot (at least 60µs). The slave device will sample a single bus in a time window of at least 15µs to 60µs after the host initiates a write time slot. If at this sampling time the window bus is high, a 1 is written to the slave device. If the bus is low, a 0 is written to the slave device.     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/77img.png)        

■ Read the time slot      
The slave device can transmit data to the host only during the read slot issued by the host. All read time slots must last at least 60µs and the recovery time between two write time slots must be no less than 1µs. Read time slots are generated by the host pulling down the single bus for at least 1µs and then releasing the bus (see Figure 1).    
After the host initiates a read slot, the slave starts transferring 1's or 0's on the bus. The slave sends a 1 by holding the bus high and a 0 by pulling the bus low. When 0 is transmitted, the slave device releases the bus at the end of the time slot, after which the bus is pulled back to the high idle state by the pull-up resistor. The output data of the slave device is valid for 15µs after the falling edge of the start time slot, so the host must release the bus and sample the bus state within 15µs after the start of the time slot. Figure 2 illustrates that the sum of TINIT, TRC and TSAMPL E must be less than 15µs in a read time slot. Figure 3 shows that the timing margin of the system can be maximized by keeping tINT and tRC as short as possible, and by putting the host sampling time at the end of the 15µs period of the read time slot.    
Figure 1     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/78img.png)        

Figure 2     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/79img.png)        

Figure 3     
![Img](../../../_static/common_product/C1K0000_4in1_basic_learning_kit/Pico_tutorial/Advanced_tutorial/80img.png)        

Datasheet of EEPROM: [EEPROM](../../../_static/pdf/A1E0000_basic_learing_shield/DS2431.pdf)   

**End!**      
