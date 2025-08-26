## Waveshare 
default slave ID 1

write 0-4 to holdingregister 4096+(n-1) to set output type as current, voltage, or raw
4096 for input 1

0 for 0–5 V
1 for 1–5 V
2 for 0–20 mA
3 for 4–20 mA
4 for raw 0–4096 code

read input registers 0-7 for inputs 1-8 


## Witmotion inclinometer

default slave ID 80

Read holding register 
0x3D for X roll angle
0x3E for Y roll angle
0x3F for Z roll angle



roll angles = value/32768*180

XREFROLL addr 121 (0x79), YREFPITCH addr 122 (0x7A). 
Write the offset corresponding to the angle to subtract

BANDWIDTH addr 31 (0x1F) — choose 20 Hz or 10 Hz to smooth motion

FILTK addr 37 (0x25)

Unlock → write → save flow:
	1.	KEY addr 105 (0x69) write 0xB588 (unlock),
	2.	write your settings,
	3.	SAVE addr 0 (0x00): write 0x0000 to save, or 0x00FF to reboot.

