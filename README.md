# BigAss3DPronter
*__Big 3D printer made with industrial parts from injection molding machine__*

### Introduction

This is a 3D printer made with an extruder from an injection moulding machine and Motors and axis from several old ATM CNC robots. It will be using an material dryer and a mini vacuum conveyor for supplying the extruder with material. It will be build such that the extruder only moves on one axis because of its weight (approximately 50kg). The extruder will be moved on the x-axis and the bottom plate will be moved on the z- and y-axis. The software will be Marlin which is going to run on a Teensy 4.0. Then octoprint will be used as an Marlin host to control. Here octoprint will run on a Raspberry Pi 4.0 with an 15" industrial grade touch screen.



### Motor control

The motors are controlled by lenze servo controllers. All of the servo drives and the power management are already packed in a nice cabinet. This will be reused. Here the teensy will be sitting inside this cabinet. The model of the servo controllers is Lenze 9300. Here the X9 and X10 connectors will be used as seen here:

![Lenze X9-X10](/md_attachments/Lenze X9-X10.png)
Format: ![Alt Text](url)

*Figure 1: Page 47 of lenze 9300 servodrive manual*

*__Lenze manual:  [BA_9300_Register_control_EN.pdf](/md_attachments/BA_9300_Register_control_EN.pdf)__*

Here the X9 connector is digital frequency input. This input requires a TTL encoder signal. Then the servo drive will mimic this encoder signal. This way the drive can be controlled. So the Teensy 4.0 will input the Marlin stepper signals to this input. The plan is to have 4 outputs on the teensy that, so the X,Y,Z,W axisses can be controlled. Here the W axis is the motor that turns the extruder. Another nice feature this allows is that the servo drives can easily be daisy chained if multiple motors is needed per axis by connecting the X10 and X9. 

To make connecting the inputs to the Teensy 4.0, a circuitboard is going to be made with 4 d-sub connectors. Here is the current state of the board drawing and schematic:

![Current board state](/md_attachments/Current board state.png)
Format: ![Alt Text](url)

*Figure 2: Board*

![Current schematic state](/md_attachments/Current schematic state.png)
Format: ![Alt Text](url)

*Figure 3: Here is seen a piece of the schematic*

As seen in figure 2, non-inverting- and inverting bus-trancievers are used to unload the teensy's output GPIO pins. The reason the inverting bus-trancievers are used is because the input signal is differential as seen in figure 1. 

The reason why the stepper output of Marlin can be connected directly to the TTL encoder signal of the X9 input is because the signals are identical as seen by comparing Figure 1 and Figure 4 below:

![Stepper output](/md_attachments/Stepper output.png)
Format: ![Alt Text](url)

*Figure 4: Here is seen the stepper output marlin*

Here it should be noted that the Z signal is not nescessary to use, so it will not be used. The z signal just signifies when the encoder has turned one revolution. This will not be relevant though since all stepping amounts will be calibrated in marlin at last.



### Octoprint with Raspberry Pi 4.0

Here as mentioned in the introduction Octoprint will be running on the raspberry pi. Here the raspberry pi will be connected to the Teensy via serial and a 15" touch screen will be connected to the Raspberry. The screen used will be this: https://www.beetronics.dk/15-tommer-touchskaerm-metal-4-3. The reason this HMI is used is because the ease of use that octoprint offers and future expansion of functionality is easy.

#### Camera in octoprint

Here a Sony QX1 will be mounted on the printer to monitor the printing process. The QX1 already outputs a mjpeg stream over its own network. So here the raspberry pi will be connected to the QX1's network. and input the stream into octoprint. In the future this functionality will be expanded to sending trigger commands to the QX1 to take pictures so octolapse can be integrated. Here a problem arises if the raspberry pi is to be connected to wifi and controlled remotely. To solve this problem either a WiFi dongle will be connected or the ethernet port on the raspberry will be used. 

##### Camera commands so far to get live-view

*Going to insert commands here*



### Mechanical composition

Here as said before the extruder is going to be moved in the X-axis and the bottom plate is going to be moved in the Y- and Z-axis. This is seen in Figure 5 below:

![3D-Printer-Axis](/md_attachments/3D-Printer-Axis.png)
Format: ![Alt Text](url)

*Figure 5: Here is seen an example of how the axis will look*

Here it should be noted that although the axis placements are correct on the image. The way the axisses are moved are not. This image is just to show axis placement. The Z-axis is not moving the extruder it should here be moving the bottom plate instead. The reason why the extruder is placed on the x-axis is because of its sheer size and weight. Here it will be advantagous only to move it from side to side to minimize movement of mass. To make the x-axis move the weight of the extruder, the long axis of the ATM CNC robot is going to be used. This is because this has several support and can therefore support the extruder with ease. It also has place for motor mounting, gear and gearrack for moving the axis, and a cable carrier. This i going to be very advantageus for running the material supply line for the extruder, motor cables for x and w axisses, and heating element wires for extruder. 



### Material Supply and Processing

Here as said in the introduction a mini vacuum conveyer will be mounted on the extruder. Its material line, will then be run through the cable carrier and down to a material dryer which will be drying the material, so it has minimal moisture for fine prints. The mini vacuum conveyer in question is here the Conradt by Andertech as seen in this attachment: [CONRADT.pdf](md_attachments/CONRADT.pdf). The dryier in question is not found yet. The heating elements of the extruder will be heated by some sort of heat control elements preferably ones that can be controlled by software, so the temp control can be situated in the octoprint interface. But if this is out of the budget or simply not possible conventional heat control elements will be used.









