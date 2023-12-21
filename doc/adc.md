# adc

## module diagram

![](E:\IC_design\Verilog\FPGA_S6\dig_volt\doc\adc_module.png)

## signals

|    name     | width(bit) |   type   |                         description                          |
| :---------: | :--------: | :------: | :----------------------------------------------------------: |
|   sys_clk   |     1      |  input   |                            50Mhz                             |
|  sys_rst_n  |     1      |  input   |                            reset                             |
|   ad_data   |     8      |  input   | the output of a AD/DA converter. Its value is in the range of 0000_0000~1111_1111 (0~255). |
|   ad_clk    |     1      |  output  |          12.5Mhz. It's a clock for AD/DA converter.          |
|    volt     |     16     |  output  |       unsigned voltage corresponding to ad_data: 0v~5v       |
|    sign     |     1      |  output  | It is decided whether volt is positiv or negativ(-5v~0v, 0v~5v) |
| cnt_sys_clk |     1      | internal |             count 0~1 to generate 12.5Mhz clock              |
| clk_sample  |     1      | internal | 12.5Mhz clock for adc module. 1/4 clock cycle later than ad_clk to ensure sample the steady ad_data |
| cnt_median  |     11     | internal | caculate and define data_median from 1 to 1024 clk cycles of clk_smaple |
| data_sum_m  |     19     | internal |       sample ad_data 1024 times and sum them together        |
| data_median |     8      | internal |            median value corresponds to 0v of volt            |
|  median_en  |     1      | internal | Only when data_median is defined, we can begin to measure voltage. Otherwise volt = 0v |
|   data_n    |     28     | internal |       the resolution of the range of volt from -5v~0v        |
|   data_p    |     28     | internal |        the resolution of the range of volt from 0v~5v        |
|  volt_reg   |     28     | internal |      1 clk cycle later, caculate digital voltage value       |
|  sign_reg   |     1      | internal | determine whether digital voltage value is positiv or negativ |

## waveform

- define a clk cycle of sys_clk = 20ns
- define a clk cycle of clk_sample = 80ns
- define ad_data = 125

![](E:\IC_design\Verilog\FPGA_S6\dig_volt\doc\adc_waveform1.png)

![](E:\IC_design\Verilog\FPGA_S6\dig_volt\doc\adc_waveform2.png)