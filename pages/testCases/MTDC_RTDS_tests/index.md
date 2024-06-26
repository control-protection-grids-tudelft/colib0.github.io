---
layout: page
title: MTDC benchmark models and test simulations
---

# MTDC benchmark models for the real-time simulation

## Use case purpose: ​
This 100% power electronics small test system combines VSC and wind turbines technologies, grid following and grid forming controls. 
It aims at showing the limitations of the phasor approximation while being small and easily tractable but realistic. Hence, an EMT and RMS version of this system exist for the benchmark.

This case is typical of the converter driven stability slow interactions problem. 

## References:
This benchmark has been designed by the Control of HVDC/AC electrical grids group from TU Delft. 

## Network ​description:
The network is described by the following figure:
The general structure of the HVDC VSC standard model is this one [[5]](#5):
<img src="{{ '/pages/testCases/MTDC_RTDS_tests/MTDC_network.png' | relative_url }}"
     alt="MTDC topologies"
     style="float: left; margin-right: 10px;" />
This figure shouws four different HVDC configurations that are available in the repository.

## Dynamic models​
This test case includes: 
- two generic [HVDC VSC lines](/models/HVDC/VSC/HVDCVSCPhasor)
- two wind turbines generators (equivalent for a Wind park)
- two sets of 6 cables in parallel (225kV)
- seven 400kV overhead lines
- six two windings transformers
- two shunt reactors 
- one RL load 


## Data 

* Parameters of the converters

| Parameter   | Onshore converter station per MMC |  Offshore converter station per MMC  | 
| ----------- | --------------- | --------------- |
| Rated Power (MVA)     | 2000      |    2000   |  
| Line frequency (Hz)   | 50        |    50     |   
| AC grid voltage (kV)  | 400       |    220    |   
| AC converter bus voltage (kV)     | 275             |    275   |   
| DC link voltage (kV)  | 525       |    525    |   
| Transformer reactance (p.u.)  | 0.18      |    0.15    |   
| MMC equivalent arm inductance (mH)  | 0.025       |    0.0497    |   
| MMC equivalent arm resistance ($\Omega$)  | 0.0785       |    0.0785   |   
| Capacitor energy in each submodule (MJ)  | 30       |    30    |   
| Number of submodules per valve  | 240       |    200    |   
| Rated voltage and current of each sub-module   | 2.5 kV /2kA       |    2.5 kV /2kA    |   
| Conduction resistance of each IGBT/diode   | $5.44 \times 10^{-4}$       |    $5.44 \times 10^{-4}$    |   

* Relevant Geometrical and Material Data of Generic 525 kV HVDC Land Cable

| Main layers | Properties (unit) |  Parameter value    | 
| ----------- | --------------- | --------- |
| Core conductor        | Metallic cross-sectional area (mm$^2$)    |    3000      |


* 6 transformers in parallel 400MVA each 

| Converter   | Snom (MVA) |  Pnom (MVA)  | 
| ----------- | ---------- | ------ | 
| WP1         | 2400       |   2300 | 
| WP2         | 2400       |   2300 |
| HVDC1       | 1200       |   1150 |    
| HVDC2       | 1700       |   1630 |  


## Scenarios

### Scenario No. 1: Unscheduled Events

**Operating point No. 1**
<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_operating_point1.png' | relative_url }}"
     alt="Four VSC system"
     style="float: left; margin-right: 10px;" />
 
In this scenario, the power injected by the wind power plants WP1 and WP2 and by the HVDC links HVDC1 and HVDC2 is exported to the external system. The network is heavily loaded, and the active power of the load is equal to zero. 

**Control modes**
WP1, WP2 and HVDC1 are in grid following mode. WP1 in reactive power control, WP2 and HVDC1 are in voltage control mode, and the dynamic voltage support is disabled.
HVDC2 is in grid forming mode.

**Event** : A solid three phase fault is applied on the first of the two lines A-C, next to bus A at 100ms. The fault is cleared after 150ms by opening the line.  

### Scenario No. 2: No export to transmission grid and mild disturbance


**Operating point No. 2**
<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_operating_point2.png' | relative_url }}"
     alt="Four VSC system"
     style="float: left; margin-right: 10px;" />

In this scenario, the power injected by the wind power plants WP1 and WP2 are exported by HVDC links, there is no export to the external network and the network is lightly loaded.
The load at bus C is withdrawing active power, and the shunt reactors are withdrawing reactive power.

**Variant A:** A first variant of this case consists in unloading the C bus and export the full power to external equivalent.

**Control modes**
WP1, WP2, HVDC1 and HVDC2 are all in grid following mode. WP1 in reactive power control, WP2 and HVDC1 are in voltage control mode, and the dynamic voltage support is disabled.

**Variant B:** A variant of this case is to switch the grid following control of the WP2 by a following forming control.


**Event** : A circuit breaker opens the line between A and B after 1 second.
Several simulations with various values of the short-circuit power Ssc of the external equivalent grid [from 5500 MVA up to 20 000 MVA] are performed.


​
## Simulation parameters
The system is an hybrid stiff DAE, the solver should be compatible with this type of problem. 
A fixed time step is applied for both phasor and EMT simualtions: 50 ms for EMT, 
The duration of simulation is of 1 second in the first scenario (event at 150ms), and of 3 seconds in the second scenario (event at 1 second).

## Outputs: ​

### First scenario:  Full export to transmission grid and large disturbance
The disturbance is seen at bus A, B and C with highest impact on bus A voltage as expected.
<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario1_bus_voltages.png' | relative_url }}"
     alt="Scenario 1 bus voltages"
     style="float: left; margin-right: 10px;" />

On the grid following converters' responses, we can see the grid voltage support from WP2 and HVDC1 due to their voltage control support, whereas WP1 is in reactive power control.

<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario1_converter_response_Gf.png' | relative_url }}"
     alt="Scenario 1 grid following converters' responses: active powers and currents"
     style="float: left; margin-right: 10px;" />
<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario1_converter_response_Gfollowing_Q_iq.png' | relative_url }}"
     alt="Scenario 1 grid following converters' responses : reactive powers and currents"
     style="float: left; margin-right: 10px;" />

On the grid forming converter's response (HVDC2), the injected current increases after fault up to its maximum value. When the fault is cleared (250ms), the injected current falls down quickly before slowly returning to a steady state value.

<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario1_converter_response_GForming.png' | relative_url }}"
     alt="Scenario 1 grid forming converter's response: injected total current and voltages"
     style="float: left; margin-right: 10px;" />

<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario1_converter_response_GForming2.png' | relative_url }}"
     alt="Scenario 1 grid forming converter's response: injected active power (P) and angle's difference (deltam)"
     style="float: left; margin-right: 10px;" />

### Scenario No. 2: No export to transmission grid and mild disturbance

In this second scenario, we can see that the response of the system to a mild disturbance very much depends on the short-circuit power of the connected equivalent grid. More importantly, this response is different if the system is simulated with a phasor simulator or an EMT one.

On the EMT simulation (left figure) we can see that the system becomes unstable if the short-circuit power is lower than 16 GVA. On the phasor simulation (right figure), the system is stable even if the short-circuit power is lower than 16 GVA (5.5 GVA). It seems that the phasor is underestimating the stability of the system in this very specific case.
<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario2_converter_response_Gfollowing.png' | relative_url }}"
     alt="Scenario 2 grid following converter's response for various short circuit powers, EMT and Phasor WP2's voltage response"
     style="float: left; margin-right: 10px;" />


**Variant A:** When the C bus is unload and full power is taken by the external network, the difference between voltages curves is significant with the EMT simulation, with the phasor approximation, the difference is bearly noticable. 
The damping effect of the constant admittance load isn't properly captured. 
<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario2A_converter_response_Gfollowing_voltage_WP2.png' | relative_url }}"
     alt="Scenario 2 grid following converter's response for various short circuit powers, EMT and Phasor WP2's voltage response"
     style="float: left; margin-right: 10px;" />


**Variant B:** When WP2 grid following control is changed by a grid forming one, the grid-forming control has such a strong stabilizing effect that both response remains stable even if the short-circuit power is as low as 300 MVA. Both simulation has comparable results.
<img src="{{ '/pages/testCases/converterDrivenStability/4VSCSystem/4VSCSystem_results_scenario2B_converter_response_Gforming_voltage_WP2.png' | relative_url }}"
     alt="Scenario 2 variant A:  grid following converter's response for various short circuit powers, EMT and Phasor WP2's voltage responses"
     style="float: left; margin-right: 10px;" />

## Open source implementations

| Software      | [Link](https://www.rtds.com/) |  
| -----------------  | --- | 
| Models        | [Link](https://github.com/control-protection-grids-tudelft/HVDC-RTDS-models) |  