ESC Design 

Overview: This document is a separate project body for the construction of the Electric Speed Controller. The Controller operates on four key stages: Gate Driver, Microcontroller, Power Supply, Connectors. 

<img width="1317" height="932" alt="image" src="https://github.com/user-attachments/assets/b9701060-ad7f-41b6-aab8-7b124030b821" />

A two pin header connector is used for providing the board with 14.8V power. Attached to the connectors are filtering and decoupling capacitors used for smoothing out high frequency noise and lowering ESR, additionally a TVS DIODE is used as extra added protection for the voltage spikes of the switching power mosfets and the inductive connection between the battery and the board. 

The TVS DIODE acts as a resistive valve for voltage spike and excess current. Ultimately when the power supply voltage goes over voltage limit, then the TVS DIODE acts as a short circuit wire. 

A switch voltage regulator is used for Stepping down the 14.8V (4S x 3.7V) power supply to 5V. 
The IC itself is TI’s LMR33620BDDA switch regulator. The max input voltage rating is 36V and the max output voltage rating is 24V. (Switch regulator operates on Power Conservation Property: Pin = Pout). 


<img width="1237" height="852" alt="image" src="https://github.com/user-attachments/assets/a3e8628b-e13a-46e4-9386-cda396ed20fd" />

The gate driver is the heart of the circuit board. The gate driver itself is TI’s DRV8308 IC. The IC itself is extremely complex, featuring very advanced internal circuitarity features. 


Before analyzing the gate driver IC, I believe the most vexing element of the circuit are the half bridge circuitaries. 

3 N-Channel Pair Mosfets are used for the High and Low side switching. 
In-depth analysis of N-channel Mosfets:

N-channel mosfets are usually used for low side switching, since VGS > 0, source is always connected to ground and load is always placed on drain side. When Mosfet is in linear mode, a voltage drop happens at the load creating a path way to ground.

N-channel mosfets can be used as a high side switch however this only occurs if load is moved to source, the current that travels creates an alternative path to high voltage or VCC. 

However placing the load at source is not sufficient enough since the new incoming high voltage signal at source causes VGS < 0, meaning that a bootstrap circuit is needed to be placed at the gate for adjusting to the automatic change in voltage—ultimately increasing the voltage at the gate.

Back EMF signal is the inductive feedback current that flows through the inductor of the stator that is in the floating phase. The benefit of the signal is that the motor takes in less current, reducing the heat build of excess current—the only resistance of the inductor comes from the actual coil material, the EMF signal provides an extra layer of resistive force

Sensorless motor control algorithms also rely on EMF signal, to control the speed of the BLDC motor and overall the strength of magnetic orthogonality, EMF voltage signal is used for reading the speed of the motor. 

Additionally, the BEMF resistive dividers feature RC filters for reducing noises from PWM signals. 1/2piRC = 159kHz 


<img width="1208" height="812" alt="image" src="https://github.com/user-attachments/assets/c11c4335-c1e5-4c41-8bef-0f58594fa172" />

Key pins of the gate driver are 

Communication between the STM32 and the IC Gate Driver is done via SPI communication. The respected GPIO pins of the STM32 are specified under page 50 (Notes)


Further Firmware specification will be reported after hardware design








