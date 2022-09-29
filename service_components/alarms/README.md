# Alarms in PLCnext Technology

https://www.plcnext.help/te/Service_Components/Alarms/Alarms.htm

Alarms Dispatcher у PLCnext Technology дає змогу надсилати повідомлення про тривоги іншим програмам, що працюють у тому самому PLCnext Control, будь то C++, Simulink®, IEC 61131-3 або вбудована OPC UA Server. Модель тривог PLCnext орієнтована на OPC UA Alarms and Conditions і доступна безпосередньо для будь-якого сумісного клієнта OPC UA. Тривоги можуть бути параметризовані для пріоритету та запиту на підтвердження користувачем.  Тривоги в PLCnext можуть реєструвати як програми [які постачаються з PLCnext Runtime](https://www.plcnext.help/te/Service_Components/Notifications/Notifications_of_PLCnext_Runtime.htm ) так і програми на C++, IEC 61131-3 і Simulink®  



![img](https://www.plcnext.help/assets/images/Firmware/Alarms_Concept.png)

Якщо стан тривоги змінюється, надсилається сповіщення; його `NotificationName `завжди починається з "Arp.Services.Alarms.Log...". (див. [Повідомлення про тривоги](https://www.plcnext.help/te/Service_Components/Notifications/Notifications_of_PLCnext_Runtime.htm#Alarms)). Ці сповіщення також відображатимуться в [реєстраторі сповіщень](https://www.plcnext.help/te/Service_Components/Notifications/Notification_Logger.htm). Для цього кожному сигналу потрібен `AlarmID`, який є унікальним у контролері.
