

# REST data interface

https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Introduction.htm

## Вступ

У цій темі ви дізнаєтеся про підхід на основі REST технології PLCnext, який використовується для обміну даними через HTTP із вбудованим програмним забезпеченням PLCnext Control і з даними в глобальному просторі даних ([GDS](https://www.plcnext.help/te/PLCnext_Runtime/GDS_Global_Data_Space.htm)).

REST, або **RE**presentational **S**tate **T**transfer, — це стиль архітектури програмного забезпечення, що складається з рекомендацій і найкращих практик для створення масштабованих веб-служб.

Ключовими особливостями інтерфейсу даних PLCnext REST є:

- Доступ до даних через порти GDS або змінні PLC (Примітка: через REST API можна отримати доступ лише до портів і змінних GDS, які позначені атрибутом HMI)
- Авторизація на різних рівнях користувачів, на основі керування користувачами PLCnext Engineer HMI
- Зашифрована передача даних через https

Цей інтерфейс даних REST має версії, і кожна версія зберігається стабільною та підтримуватиметься. Як загальний інтерфейс для обміну даними, він також доступний для використання з іншими клієнтами REST.

Примітка: Phoenix Contact залишає за собою право вносити **зміни та доповнення** в подальші версії, які можуть призвести до несумісності. Тому настійно рекомендується викликати цей інтерфейс даних REST із вашої програми **явно за допомогою версії**, яка використовувалася під час розробки, а **не** за допомогою ключа `latest` .

## Концепція

Інтерфейс даних REST надається компонентом HMI PLCnext для використання клієнтом HMI PLCnext Engineer, який зазвичай є програмою на основі браузера, що працює на вбудованому веб-сервері `nginx` на будь-якому контролері PLCnext.

Цей схематичний рисунок показує, де знаходиться інтерфейс даних REST у контексті PLCnext Runtime System:

![img](https://www.plcnext.help/assets/images/Firmware/REST_data_interface_architecture.svg)

## Як це зробити

Використовувати інтерфейс даних REST не так вже й складно, але з’явиться купа детальної довідкової інформації. Давайте розповімо вам про різні теми, щоб ви не заблукали, не досягнувши успіху.

1. Перш за все, вам знадобиться короткий, але завершений проект PLCnext Engineer, оскільки для використання інтерфейсу даних REST компонент HMI PLCnext має бути запущено з проекту PLCnext Engineer. Проект PLCnext Engineer має містити принаймні одну сторінку HMI (спочатку може бути порожньою), щоб ініціалізувати компонент HMI PLCnext. У проекті PLCnext Engineer усі порти GDS або змінні ПЛК, до яких потрібно отримати доступ за допомогою інтерфейсу даних REST, мають бути затверджені прапором «HMI».
         Примітка. Якщо ви ще не знайомі з PLCnext Engineer та його редактором HMI, спочатку прочитайте тему [PLCnext Engineer HMI](https://www.plcnext.help/te/Programming/IEC_61131-3/Creating_a_PLCnext_Engineer_HMI_application.htm).
2. Потім вам потрібно отримати авторизований доступ до портів GDS і змінних PLC через інтерфейс REST data . Прочитайте тему [автентифікація](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Authentication.htm), щоб дізнатися, як це робиться.
3. Після відкриття Sesame ви можете читати або писати [змінні](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Variables.htm), налаштовувати змінні в [групах](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Groups.htm), отримати звіти [сеансу](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Sessions.htm) і зробити це за надсилання всіх видів [запитів](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Request.htm).