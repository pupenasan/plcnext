[На основну](../README.md)

# Емулятор контролеру

**Note**    The controller simulation is available for PLCnext Technology targets with a firmware version 2022.0 or newer. For some controller types the simulation must be unlocked by license.For any other controller type than the AXC F 1152, activate the simulation  license as usual in the Phoenix Contact Activation Wizard. For particular controller types, the simulation may not be supported.



This topic contains the following sections:

- [General information on the Controller Simulation](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Simulation.htm#N2D298)
- [When working with the controller simulation ...](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Simulation.htm#N2D2C4)
- [Setting the controller simulation as target system](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Simulation.htm#N2D2F7)
- [Simulation credentials](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Simulation.htm#SimulationCredentials)
- [Possible operations with the simulation ](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Simulation.htm#N2D357)



**Note**  This topic only applies to the standard (non-safety-related)  controller. The steps for simulating the Safety PLC are described in the help chapter "Safety PLC Commissioning: From Compiling to Debugging ![img](file:///C:/Program%20Files/PHOENIX%20CONTACT/PLCnext%20Engineer%202022.6/en/Help/arrow-linkcontainer.png)                   ".



## General information on the Controller Simulation

PLCnext Engineer includes a controller simulation which you can use to simulate the execution of the application logic. This is useful, 

- if no hardware is available, or
- if a simulated function test is recommended prior to starting up the "real" network.



**Note**  The simulation of the application may not replace the proper  function test using I/O devices/sensors/actuators under any  circumstances. The simulated test may only be performed in addition to  the standard function test, as a preliminary test, for example.



The PLCnext controller simulation is a separate application which  actually emulates the CPU of the controller set as target your project.

It is able to simulate the entire controller behavior and it  processes the identical machine code as generated for the controller  involved. Therefore, no rebuild is necessary when switching between real hardware and simulation in the Cockpit (see below).

## When working with the controller simulation ...

- The PLANT shows the controller node as  simulation node. The shield icon beside the node is different from the  shield icon which indicates a controller connection: 

  ![img](file:///C:/Program%20Files/PHOENIX%20CONTACT/PLCnext%20Engineer%202022.6/en/Help/images/SimulationNode_Plant.png)

- The process data are available as ports on the  simulation and can therefore be read or written by the controller  application. However, if the network is connected, these process data is never updated by the field bus devices. This means, physical device  inputs will not be read and device outputs will not be written while  simulating.

- Online editors and all debug tools are identical in simulation mode and are to be used in the same way. Even the  controller status LEDs and information are available in the [Cockpit ('Overview' category)](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/ControllerDiagnostics.htm).

- You perform the same steps in PLCnext Engineer  as you would if you were working with the physical network, although you must ensure that the 'Simulation' entry is selected in the 'Cockpit'  editor of the controller node in the PLANT. While simulating the  application, you can force variables or display online values in the  editors as usual. 

## Setting the controller simulation as target system

1. In the PLANT, double-click the controller node and open the Cockpit.
2. On the 'Cockpit' toolbar, select 'Simulation' from the drop-down list.
   If the 'Simulation' entry cannot be selected, no simulation is available for the controller type used.

![img](media/Simulation_SetInCockpit.png)

The simulation may request a user name and password when establishing a communication connection from  PLCnext Engineer (see  section [below](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Simulation.htm#SimulationCredentials)). This can be done, for example by selecting the 'Connect...' command in the Cockpit or by writing and starting the project:

![img](media/Simulation_Cockpit_EstablishConnection.png)

While simulation is started and connected, a dialog with a progress  bar is visible. Clicking 'Cancel' in this dialog, terminates the  simulation and the connection establishment. 

After having connected the simulation, the PLANT shows the controller node as simulation node. The shield icon beside the node is different  from the shield icon which indicates a controller connection: 

![img](media/SimulationNode_Plant.png)

All commands and debug operations now relate to the simulation. This  includes the commands in the context menu of the controller in the PLANT and in the ONLINE STATE window (which is located in the Cross Functions Area at the screen bottom).

## Simulation credentials

The logon data for the simulation is as follows:

- User name: admin
- Password: plcnext

## Possible operations with the simulation 

- Performing a simulated function test.
- Displaying online values in [debug mode](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/OnlineMode.htm) which are read cyclically from the controller and shown in the editors.
- Using debug commands, such as [forcing/overwriting of variables](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Debug_ForceOverwrite.htm) and [breakpoints](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Debug_Breakpoints.htm) in  						debug mode.
- Using the [WATCHES window](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Watches.htm) for collecting variables from different worksheets, displaying their online values and execute debug commands.
- [Running the HMI application](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/HMI_Visualization_Run.htm) on the web server of the simulation.