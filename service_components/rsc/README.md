# RSC (Remote Service Calls)

Розширення функцій (Function extensions) можуть спілкуватися з основними компонентами PLCnext Technology через інтерфейс RSC. Ви можете отримати доступ до різноманітних функцій і елементів даних через інтерфейси.

Наприклад, ви можете отримати доступ для читання та запису до даних глобального простору даних ([GDS](https://www.plcnext.help/te/PLCnext_Runtime/GDS_Global_Data_Space.htm)) за допомогою служби RSC `IDataAccessService` .

![PLCnext_RSC.png](https://www.plcnext.help/assets/images/Firmware/PLCnext_RSC.png)

 

У вас є можливість використовувати вже зареєстровані служби RSC SDK через `ServiceManager`.

`ServiceManager` діє як RSC API і використовується для запиту послуг.