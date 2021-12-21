---
title: "Digital Input to Microcontroller"
tags: ["microcontroller", "digital", "trigger", "gate"]
--- 
Most microcontrollers run on either 3.3V or 5V logic, whereas Eurorack gates and triggers are usually between 5-8V.  
Thus, if you would like to 'read' a digital input such as a gate or external clock source from your microcontroller, you will need some method of conditioning the incoming voltage to prevent it from damaging the chip.  
  
One very basic method could be used, which is to simply 'clip' the input voltage using diodes to the input range of the microcontroller, for example using this circuit found in the [Doepfer DIY page](https://doepfer.de/DIY/a100_diy.htm)  
  
![image](https://user-images.githubusercontent.com/79809962/146977585-e8bdfe69-e71a-4a7e-a47d-d230c48882c6.png)
  
This method however has its limitations. For one, if the incoming voltage is less than the microcontroller's input requirement for reading 'high', then no input will be registered.  
This limitation would also be present if we simply scaled down the input voltage to make 12V into 5V or 3.3V; if the input was *less* than 12V, then the microcontroller wouldn't read an input as being high even when it was.
  
What we would really like is a circuit that uses the incoming voltage to 'allow' a second voltage, supplied by the microcontroller itself, to be connected to the input pin.  
And, wouldn't you know, that's just what an NPN transistor can be used for!  
The transistor has three terminals, called the 'collector', the 'base', and the 'emitter'. When a voltage is received at the base, current is able to flow from the collector to the emitter.  

Thus, if we use a circuit where the collector is connected to the microcontroller input pin, the emitter is connected to ground, and the base is connected to the incoming signal, the incoming signal will 'allow' the pin to be connected to ground (triggering the circuit) when it is high.  
However, this circuit would also have some big problems, the largest being that with no input present, the pin would be 'floating' which means it is not connected to anything and may fluctuate randomly between voltages.  
What we need to add is what's called a 'pull-up resistor'. This is essentially a default path for the pin which goes to the microcontroller's positive voltage rail, but which is overridden by the lower impedance path to ground when the transistor conducts.
  
The circuit, including a 10k pull-up resistor, looks as follows:  
  
![image](https://user-images.githubusercontent.com/79809962/146978668-4944a99c-a10c-451e-89d4-ba2b81d9b353.png)  
There is also an input resistor of 100k, which will 'set' the input impedance of the circuit. This prevents the circuit from drawing a huge amount of current from the input signal. The transistor used is a 2N2222, but any common NPN transistor will work, such as a 2N3904.
The only extra part is a diode that only takes effect if a negative voltage is passed to the input, and will provide a path to ground to prevent damage to the transistor.
