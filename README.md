# a simple Voltmeter designed with AD/DA converter and FPGA

## Principle

![](E:\IC_design\Verilog\FPGA_S6\dig_volt\doc\a_simple_voltmeter_module.png)

In this experiment, a high-speed AD/DA converter is used. The ADC chip of the converter has 8 bits (i.e., the output ad_data of the converter is 8 bits), and the range of analog voltage input(analog_signal) is from -5v to +5v. Therefore, the voltage drop between the maximum and minimum values is 10v, and the resolution is 10/2^8. 

Before measuring (touching) the object, the FPGA will do calibration in an instant to determine the median value data_median corresponding to 0v.

During the measurement of the object's voltage, the converter samples, quantizes, and encodes the input analog voltage(analog_signal), converting the analog voltage into a digital value (0~255), which is then sent through ad_data to the FPGA. 

At the end, the FPGA builds the relationship between this digital value(ad_data) and the output digital voltage value(volt). After that, it is converted into data, that is suitable to show on the seven-segment display.

## calculation

convert a digital value ad_data(0~255) to a digital voltage value volt(-5v~5v)

- When ad_data in the range of 0~data_median, it means volt is from -5v~0v. The resolution = `10/((data_median+1)*2)`. The volt = `-((10/((data_median+1)*2))*(data_median-ad_data))`
- When ad_data in the range of data_median~255, it means volt is from 0v~5v. The resolution = `10/((255-data_median+1)*2)`. The volt = `((10/((255-data_median+1)*2))*(ad_data-data_median))`