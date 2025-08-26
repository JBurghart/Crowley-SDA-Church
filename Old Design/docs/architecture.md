# System Architecture
## Block Diagram
- Add pdf

## Main Components

|Component | Function |
| -- | -- |
|STM32F469 + Riverdi Display| touchscreen UI, Modbus Master|
|STM32F446RE| hardware control, PWM generation, sensor polling|
|RS-485 Network| inter-device communication|
| Load cell | middle Winch tension feedback|
|Inclinometers | Left and Right winch level feedback |
|rotary encoder| position feedback | 
|limit switches | top limit safety cutoff|

## Communication Protocols
- RS-485 with Modbus RTU
- Analog sensor IO
- SPI

## Hardware Details
### Controller Boards
- Nucleo-F446RE
- Riverdi Eval Board + RVT50HQBNWC00-B

### Sensors / Actuators
|Component|Function|Notes|
|--|--|--|
|200kg S-type Load Cell|Weight sensing|~2 mV/V output|
|WitMotion Inclinometer|Angle measurement|RS-485|
|Rotary Encoder|Position tracking|Interrupt-driven|
|DC Winch Motors|Actuation|120V, H-bridge using MOSFETs|


### Communication
- RS-485 over CAT5e (12V, 24V, GND, RS-485)
- Gravity: Active Isolated RS485 to UART modules

## Software Structure
Code Repos / Layout \
Core/: STM32Cube-generated files \
Src/: your FreeRTOS threads, motor logic, PID, etc. \
Modbus/: Modbus stack files, registers, interface code \

### Tasks and Scheduling -F446RE (FreeRTOS)
|Task|Purpose|Priority|Notes|
|--|--|--|--|
|Sensor Poller|Reads RS-485 and analog sensors|Med|10Hz|
|Motor Controller|PID, direction & speed updates|High|100Hz|
|Safety Monitor|Limit switch, overcurrent checks|High|
|Modbus Server|Responds to master UI board|Low|

### Tasks and Scheduling -STM32F469 + Riverdi Display (FreeRTOS)


## Wiring and Pin Assignments

| Function | STM32 Pin | Peripheral | Notes |
|--|--|--|--|
| RS-485 TX | PA2 | USART2_TX | |
| PWM Out | PB0 | Motor Driver | TIM3_CH3 |
| Encoder A | PA8 | EXTI8 | Interrupt |

## Modbus Register Map


| Address | Name | Type | Description | 
|--|--|--|--|
| 0x0001 | Motor 1 Speed | Holding Reg | 0–1000 = 0–100% | 
| 0x0002 | Load Cell 1 | Input Reg | Weight in N | 
|...|...|...|...|

## Hardware Wiring
- F469 UART 
- TBD on board 
- breakout boards 
- design mosfets and passive cooling PCB

## Setup Instructions
- Flashing Firmware
- Connecting Display
- Starting the System
    - design cold boot seqence

## UI Features
### Main screen
- up button
- down button
- settings button

### Settings Screen
- static 
    - 3x motor speed 
    - scale load 
    - encoder position
    - 3x limit switch status

- buttons
    - set home position
    - inclinometer zero
    - calibrate load cell
    - zero inclinometer


## Testing & Debugging
