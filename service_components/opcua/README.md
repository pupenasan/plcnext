# Огляд OPC UA з технологією PLCnext

OPC Unified Architecture (UA) — це стандартизований протокол для промислового зв’язку ІТ та ОТ. За запитом сервер OPC UA надає клієнту OPC UA дані процесу та значення змінних із запущеної програми.

Пристрої керування PLCnext Control містять вбудований сервер OPC UA (eUA). На додаток до середовища виконання він також інтегрований у контролер. Він забезпечує доступ до компонентів, програм, функціональних блоків, структур і змінних PLCnext Technology.

## Client-Server communication

### Services 

Сервер eUA надає клієнтським програмам послуги, сумісні з OPC UA (частина 4): 

- [Discovery](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_Discovery.htm)
- [Secure channel](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_Secure_Channel.htm)
- [Session](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_Session.htm) 
- [Monitored item and subscription](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_Monitored_Item_and_Subscription.htm)

### Information models

Окрім цих наборів послуг, сервер eUA надає інформаційні моделі

- [External information models](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_External_information_models.htm)
- [Alarms and conditions](https://www.plcnext.help/te/Service_Components/OPC_UA/Alarms_and_Conditions.htm)
- [Device information](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_Device_Information.htm)
- [Historical data access](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_historical_access.htm)
- [File access](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_File_access.htm)
- [Global Data Space](https://www.plcnext.help/te/Service_Components/OPC_UA/OPCUA_GDS.htm)

## Server-independent communication

- [Publish and Subscribe](https://www.plcnext.help/te/Service_Components/OPC_UA/OPC_UA_PubSub.htm)

# OPC UA PubSub

Починаючи з мікропрограми 2022.0 LTS, мікропрограма PLCnext Technology має реалізацію специфікації публікації та підписки OPC UA (PubSub) як нового протоколу зв’язку. Це дозволяє обмінюватися даними в мережі через UDP (User  Datagram Protocol). Обмін даними можливий між одним або різними типами пристроїв PLCnext Control, а також з пристроями інших виробників, які підтримують OPC UA PubSub. OPC UA PubSub дозволяє обмінюватися даними між пристроями та програмами.

Функція OPC UA PubSub загалом не залежить від звичайної роботи сервера/клієнта OPC UA.

Якщо ви використовуєте комутатори у своїй мережі та плануєте використовувати функцію OPC UA PubSub, переконайтеся, що комутатори підтримують IGMP (Internet Group Management Protocol). IGMP — це протокол зв’язку сімейства протоколів TCP/IP. Це необхідно для організації багатоадресних груп приймачів в мережах IPv4. IGMP дозволяє видавцеві надсилати потоки даних цілим групам одержувачів цільовим способом, оптимально використовуючи транспортні можливості та можливості маршрутизації.

##### Publisher/Видавець

Ви можете налаштувати контролер як видавця, створивши різні WriterGroups, які надсилають певний набір даних у певний інтервал публікації (наприклад, 1000 мс) у мережу. Набори даних для публікації в мережі мають складатися з портів OUT або глобальних змінних ресурсу. Наразі для публікації підтримуються лише елементарні типи даних або одновимірні масиви елементарного типу.

##### Subscriber/Підписник

Ви можете налаштувати один або кілька інших контролерів як абонентів. Для цього ви повинні створити ReaderGroups у конфігурації проекту, які можуть підписатися на дані, надані видавцем. Ці ReaderGroups на стороні підписника мають відображати структуру WriterGroups на стороні видавця. Абонент може отримувати дані з портів IN і глобальних ресурсних змінних. Наразі для підписки підтримуються лише елементарні типи даних або одновимірні масиви елементарного типу.