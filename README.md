# Embedded Systems Class Project
## INTERFACING TEMPERATURE SENSOR WITH LPC1768 AND DISPLAY ON MULTIPLEXED SEVEN SEGMENT

Temperature sensor: LM35 Basic Centigrade Temperature Sensor (2°C to 150°C)

### WORKING:

When using the Basic configuration we get an output swing of ~0V(20mV) to 1.5V.
So, if we use a VREF of 3.3 Volts we get a resolution of 3.3V/4096 = 0.0008056V or 0.8mV since LPC1768 has a 12-bit ADC.
With Vref in Volts and Scale-factor in V/°C, we can compute the temperature(T) from 12-bit ADC Result as:


##### Temperature ( in °C ) = ( ADC_Result * 330.0 ) / 4096  

The controller calculates this result very fast and the sensor almost outputs a voltage almost the whole time. So to give
stable readings, we used a timer that gives a delay of 3s. Hence, the temperature changes (if there is any change) every
3s.

### HARDWARE:

+ P<sub>0.4</sub>  to P<sub>0.11</sub> ( Connector A ) as seven segment data lines.

+ P<sub>1.23</sub> to P<sub>1.26</sub> ( Connector B ) as decoder lines.

+ P<sub>0.24</sub> ( Connector D, 1st bottom right pin ) as ADC input-ADD<sub>0.1</sub>

+ Timer used in the project is TIM<sub>***0***</sub>.

### CODE:

+ The function _timer_init()_ is used to intialize and run a timer to give a delay of about 3s.

+ The function _display()_ is used to display the calculated temperature on seven segment.

+ Pins P<sub>0.4</sub>  to P<sub>0.11</sub> and P<sub>1.23</sub> to P<sub>1.26</sub> are configured as GPIO outputs for the seven segment display.

+ Pin P<sub>0.24</sub> ( ADD<sub>0.1</sub> ) as ADC input ( function 1 ).

+ ADC interrupts on channel 1 ( ADD<sub>0.1</sub> ) is enabled.So whenever the controller detects a voltage on P<sub>0.24</sub>, it passes the 
control to the _ADC_IRQHandler()_.

+ In the handler, the calculations are done according to the formula given in the WORKING section above. This temperature is then displayed for 3s and the temperature is recalculated after 3s and displayed again.
