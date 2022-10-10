# HMI Application

[Швидкий старт](hmiquickstart.md)

PLCnext Engineer allows you to create an HMI (Human Machine Interface) application for 

- visualizing and monitoring
- controlling and operating 

the control application running on your controller.

The HMI application consists of one or more HMI pages populated with  standard HMI objects or predefined HMI symbols which can be assigned to  HMI tags in order to animate them or send control commands to the  application.

For visualizing and controlling the automation application this way,  data must be exchanged between the HMI application and the controller.  This data exchange is realized by HMI tags.
An HMI tag can be seen as an HMI-internal variable. An HMI tag is assigned to a global IEC  61131-3 variable or a local variable with set HMI attribute. Via the HMI tag, the HMI application gets read or write access to variables. By  adding a dynamic/action to an HMI object property and linking this  dynamic/action to an HMI tag, the value of the variable can be  visualized/written via the HMI.

**Note**  Regarding their use in HMI pages, the following applies to variables of the Safety PLC: Safety-related IEC 61131-3 variables cannot be  used in an HMI page as no HMI tags can be created and assigned to  variables of a safety-related data type in the Safety PLC Data List. It is possible to create HMI tags for [exchange variables](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.6/en/Help/Variables_RoleMapping.htm), i.e., for Safety PLC variables of a standard data type which are assigned to a global variable of the standard PLC.

The HMI application data is stored automatically as part of the  project. The HMI application is part of the project image that is  written to the controller when executing the 'Write and Start Project'  command. In addition to the application logic and the  configuration/parameterization data of your project, the project image  contains all relevant HMI data. Using the web server on your controller, you can run the HMI application in a standard web browser and then  monitor and control the processes running on the controller.

## Features at a glance

- Powerful HMI editor for designing HMI pages  using HMI objects, symbols and images inserted from the COMPONENTS area. Object properties can be edited in the dockable properties window.
- Animation of HMI object properties such as object position, visibility, etc., by means of HMI tags.
- Actions assigned to HMI objects allow writing values to the automation process (e.g. 'Action on click' for HMI buttons).
- HMI symbol libraries provide predefined symbols.
- Creation and re-use of user-defined HMI symbols.
- Add images and re-use them in your HMI pages as often as required.
- Create released HMI libraries of user-defined HMI images and symbols.
- Flexible navigation to HMI pages by setting the 'Action on Click' dynamic for an HMI object.
- Definition of the navigation structure: the  'Navigation' editor allows you to define how you can navigate between  your HMI pages in the project when running the HMI application.
- Protecting the HMI application against unauthorized use by setting the access rights for HMI pages and HMI objects.
- Create multilingual HMI applications by translating HMI texts in various languages.
- Run the automation application directly from PLCnext Engineer using the web server of the controller.
- Monitor and operate the automation application running on the controller via the HMI application in a standard web browser.
- Optional HMI Generator which allows you to  automatically generate a complete visualization (HMI content) based on  your project with only one command.