[Сервісні компоненти](../README.md)

# REST data interface

https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Introduction.htm

## Концепція

У цій темі ви дізнаєтеся про підхід на основі REST технології PLCnext, який використовується для обміну даними через HTTP із вбудованим програмним забезпеченням PLCnext Control і з даними в глобальному просторі даних (GDS).

REST, або **RE**presentational **S**tate **T**transfer, — це стиль архітектури програмного забезпечення, що складається з рекомендацій і найкращих практик для створення масштабованих веб-служб. Про REST можна прочитати наприклад в [лекції по REST](https://pupenasan.github.io/ProgIngContrSystems/%D0%9B%D0%B5%D0%BA%D1%86/6_httpapi.html) а перед тим варто познайомитися з HTTP, якщо для Вас це нова тема, можна почитати [тут](https://pupenasan.github.io/ProgIngContrSystems/%D0%9B%D0%B5%D0%BA%D1%86/4_http.html).

Ключовими особливостями інтерфейсу даних PLCnext REST є:

- Доступ до даних через порти GDS або змінні PLC. Через REST API можна отримати доступ лише до портів і змінних GDS, які позначені атрибутом HMI.
- Авторизація на різних рівнях користувачів, на основі керування користувачами PLCnext Engineer HMI
- Зашифрована передача даних через https

Цей інтерфейс даних REST має версії, і кожна версія зберігається стабільною та підтримуватиметься. Як загальний інтерфейс для обміну даними, він також доступний для використання з іншими клієнтами REST.

Phoenix Contact залишає за собою право вносити **зміни та доповнення** в подальші версії, які можуть призвести до несумісності. Тому настійно рекомендується викликати цей інтерфейс даних REST із вашої програми **явно за допомогою версії**, яка використовувалася під час розробки, а **не** за допомогою ключа `latest` .

Інтерфейс даних REST надається компонентом HMI PLCnext для використання клієнтом HMI PLCnext Engineer, який зазвичай є програмою на основі браузера, що працює на вбудованому веб-сервері `nginx` на будь-якому контролері PLCnext.

Цей схематичний рисунок показує, де знаходиться інтерфейс даних REST у контексті PLCnext Runtime System:

![img](media/REST_data_interface_architecture.svg)

Обмін даним налаштовується в такому порядку:

1. Перш за все потрібен проект PLCnext Engineer, який має містити принаймні одну сторінку HMI (спочатку може бути порожньою), щоб ініціалізувати компонент HMI PLCnext. 
2. У проекті PLCnext Engineer усі порти GDS або змінні ПЛК, до яких потрібно отримати доступ за допомогою інтерфейсу даних REST, мають бути затверджені прапором `HMI`.
3. Потім вам потрібно отримати авторизований доступ до портів GDS і змінних PLC через інтерфейс REST data . Прочитайте тему автентифікація, щоб дізнатися, як це робиться.
4. Після відкриття Sesame ви можете читати або писати змінні, налаштовувати змінні в групах, отримати звіти сеансу і зробити це за надсилання всіх видів запитів.

Зверніть увагу що для перевірці на імітаторі порті повинен бути таким, як налаштовано в конфігурації, за замовченням 5050.

## Автентифікація 

Автентифікація в інтерфейсі REST data – це двоетапний процес:

1. Запит маркера автентифікації
2. Запит на автентифікацію користувача, якщо тіло запиту містить наведений вище маркер автентифікації

Після цього за допомогою результуючого маркера доступу ви отримаєте доступ до портів і змінних GDS. Реалізація перетворення інтерфейсу даних REST у захищений ресурс базується *вільно* на OAuth 2.0, як описано в [RFC 6749](https://tools.ietf.org/html/rfc6749) (зверніть увагу: є деякі відмінності!) . 

На рисунку нижче показано процедуру автентифікації в контексті PLCnext Technology:

![img](media/HMI_webserver_auth.png)

- `(A)` Клієнт запитує авторизацію для інтерфейсу REST data . Це надає клієнту дозвіл на автентифікацію в `pxc_api` (API Phoenix Contact). Запит містить секрет, відомий клієнту, який має бути повторений у відповіді від сервера.
- `(B)` Сервер відповідає маркером авторизації та секретом, наданим клієнтом.
- `(C)` Коли користувач намагається увійти, клієнт надсилає ім’я користувача та пароль відкритим текстом через зашифрований канал (TLS).
- `(D)` Якщо підсистема безпеки PLCnext Runtime System приймає користувача та пароль, то маркер доступу та ролі для цього користувача повертаються у відповідь. 

Коли клієнт отримує маркер доступу, для кожного запиту даних відбувається наступний обмін:

- `(E)` Клієнт включає маркер доступу в HTTP-заголовок запиту даних `pxc_api`, використовуючи заголовок `Authorization` і маркер `Bearer`.
- `(F)`  Після успішної перевірки маркера `Bearer` у заголовку запиту повертаються дані запиту.

Помилки автентифікації повертаються в полі `WWW-Authenticate Response Header` за допомогою інфраструктури, описаної в HTTP/1.1 [RFC 2617](https://tools.ietf.org/html/rfc2617).

- `invalid_token `у відповіді про помилку вказує, що **токен відсутній або недійсний**.
- `invalid_grant ` вказує на те, що **користувач і пароль не збігаються** з обліковими даними на сервері.

**Запит маркера автентифікації**. 

Надішліть запит із таким тілом до URI запиту:

| Частина запиту/відповіді         | Значення                                               |
| -------------------------------- | ------------------------------------------------------ |
| Request URI (HTTP method `POST`) | `https://192.168.225.78/_pxc_api/v1.2/auth/auth-token` |
| Request body (.json)             | `{"scope":"variables"}`                                |
| Response (.json)                 | `{"code":"yourAuthToken","expires_in":600}`            |

Маркер автентифікації, переданий у відповідь, буде прив’язаний до запитуючої IP-адреси до закінчення терміну дії.

**Запит на автентифікацію користувача**

Вставте маркер автентифікації (`"yourAuthToken"`), дійсне ім’я користувача та відповідний пароль у нове тіло запиту. Надішліть запит із підготовленим тілом на URI запиту:

| Частина запиту/відповіді          | Значення                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| Request URI (HTTP method `POST`): | `https://192.168.225.78/_pxc_api/v1.2/auth/access-token`     |
| Request body (.json):             | `{"code": "yourAuthToken", "grant_type": "authorization_code", "username": "admin", "password": "PLCpassword"}` |
| Response (.json):                 | e.g. `{"token_type":"Bearer","access_token":"df3b895e160a92f1","roles":[]}` |

Маркер доступу, який передається у відповідь, надає доступ до портів і змінних GDS, якщо його помістити в заголовок HTTP. У запиті даних передаються ролі користувача, пов’язані з іменем користувача. Ролі користувачів відповідають `EHmiLevels`, встановленим у  Web-based Management

**Автентифікація при запиту даних**

Приклад запиту даних міститиме в заголовку таку пару `key-value`:

```
{"Authorization": "Bearer xyz"}
```

**Відключення автентифікації**

Із налаштуваннями за замовчуванням для доступу до інтерфейсу даних REST потрібна автентифікація користувача через керування користувачами PLCnext. Phoenix Contact рекомендує ці налаштування за замовчуванням з міркувань безпеки. Якщо вам потрібен прямий доступ без автентифікації, автентифікацію можна вимкнути в налаштуваннях безпеки веб-сервера HMI, як показано нижче:

![img](media/PLCnext_HMI_User_Management.png)



## Колекції та словники 

У цій темі показано, як отримати словники для інтерфейсу даних REST, і наведено приклади колекцій листонош для команд, які докладно описано у відповідних темах.  Щоб завантажити колекцію запитів у цьому інтерфейсі даних REST, просто натисніть посилання та розпакуйте:

- [Browse Items](https://www.plcnext.help/assets/files/PLCnext_DA_REST_BrowseItems.postman_collection.json.zip) collection v1.4
- [Data Session](https://www.plcnext.help/assets/files/PLCnext_DA_REST_DataSession.postman_collection.json.zip) collection v1.4

Щоб отримати словник, ви можете використовувати такі виклики:

**Отримання словника даних**

Наступний запит повертає JSON з усіма **доступними** змінними та інформацією про їх тип.

```http
GET https://%PlcAddress%/ehmi/data.dictionary.json
```

де `%PlcAddress%` - адреса PLC

Приклад відповіді:

```json
{
    "$schema":"http:\/\/json-schema.org\/draft-0\/schema#",
    "title":"DataDictionary",
    "HmiVariables2":
      {
        "Arp.Plc.Eclr/ESM_DATA.ESM_INFOS[1].TICK_COUNT":
          { 
            "Type": "DINT",
            "InitValue": "0",
            "ReadOnly": true
          },
        "Arp.Plc.Eclr/Test_Global":
          {
            "Type": "BOOL",
            "InitValue": "FALSE"
          },
        "Arp.Plc.Eclr/Test_Global_GroupVar1":
          {
            "Type": "BOOL",
            "InitValue": "FALSE"
          },
        "Arp.Plc.Eclr/Test_Global_GroupVar2":
          { 
            "Type": "BYTE",
            "InitValue": "BYTE#16#0"
          },
        "Arp.Plc.Eclr/Test_Global_GroupVar3":
          {
            "Type": "WORD",
            "InitValue": "WORD#16#0"
          },
        "Arp.Plc.Eclr/Test_Global_GrouptVar4":
          {
            "Type": "DWORD",
            "InitValue": "DWORD#16#0"
          },
        "Arp.Plc.Eclr/TestArray":
          {
            "Type": "arrByte_Stream",
            "InitValue": ""
          }
      }
  }
```

**Отримання словника типів**

Наступний запит повертає JSON з усіма **використаними типами даних**.

```
GET https://%PlcAddress%/ehmi/type.dictionary.json
```

де `%PlcAddress%` це адреса ПЛК

Приклад відповіді:

```json
{
  "$schema":"http:\/\/json-schema.org\/draft-0\/schema#",
  "title":"TypeDictionary",
  "Types": 
  [
    {
      "Name": "DINT",
      "RuntimeType": "System.Int32",
      "RuntimeSize": 4,
      "RuntimeAlignment": 4,
      "Kind": "Elementary"
    },
    {
      "Name": "BOOL",
      "RuntimeType": "System.Boolean",
      "RuntimeSize": 1,
      "RuntimeAlignment": 1,
      "Kind": "Elementary"
    },
    {
      "Name": "BYTE",
      "RuntimeType": "System.Byte",
      "RuntimeSize": 1,
      "RuntimeAlignment": 1,
      "Kind": "Elementary"
    },
    {
      "Name": "WORD",
      "RuntimeType": "System.UInt16",
      "RuntimeSize": 2,
      "RuntimeAlignment": 2,
      "Kind": "Elementary"
    },
    {
      "Name": "DWORD",
      "RuntimeType": "System.UInt32",
      "RuntimeSize": 4,
      "RuntimeAlignment": 4,
      "Kind": "Elementary"
    },
    {
      "Name": "arrByte_Stream",
      "RuntimeType": "ProConOS_eCLR.arrByte_Stream",
      "RuntimeSize": 28,
      "RuntimeAlignment": 1,
      "Kind": "Array",
      "ElementType": "BYTE",
      "Dimensions": 
      [
        {
          "StartIndex": 0,
          "Length": 28
        }
      ]
    }
  ]
}
```

## Запити

У наведеній нижче таблиці підсумовано запити та відповіді на них, детально описані в окремих розділах або темах.

| Command                                                      | HTTP method | Relative URI for PLCnext Control                         | Query Parameters                                             | Response  |
| ------------------------------------------------------------ | ----------- | -------------------------------------------------------- | ------------------------------------------------------------ | --------- |
| [Service Description](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Request.htm#Service_description) | `GET`       | `/_pxc_api/api`             `/_pxc_api/v%Major%.%Minor%` |                                                              | JSON data |
| [Sessions](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Sessions.htm) |             |                                                          |                                                              |           |
| Report Sessions                                              | `GET`       | `/_pxc_api/api/sessions`                                 |                                                              | JSON data |
| Create Session                                               | `POST`      | `/_pxc_api/api/sessions`                                 | with request body `stationID=%StationID%&timeout=%OptionalTimeout%` | JSON data |
| Maintain Session                                             | `POST`      | `/_pxc_api/api/sessions/%SessionID%`                     |                                                              | JSON data |
| Reassign Session                                             | `POST`      | `/_pxc_api/api/sessions/%SessionID%`                     | with request body `stationID=%StationID%&timeout=%OptionalTimeout%` | JSON data |
| Delete Session                                               | `DELETE`    | `/_pxc_api/api/sessions/%SessionID%`                     |                                                              | –         |
| [Groups](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Groups.htm) |             |                                                          |                                                              |           |
| Report Groups                                                | `GET`       | `/_pxc_api/api/groups`                                   |                                                              | JSON data |
| Register Group                                               | `POST`      | `/_pxc_api/api/groups/%GroupID%`                         | with request body `pathPrefix=%OptionalVariablePathPrefix%&paths= 		%VariablePath1%,...,%VariablePathN%` | JSON data |
| Read Group                                                   | `GET`       | `/_pxc_api/api/groups/%GroupID%`                         | `summary=%OptionalSummarySetting%`                           | JSON data |
| Unregister Group                                             | `DELETE`    | `/_pxc_api/api/groups/%GroupID%`                         |                                                              | –         |
| [Variable](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Variables.htm) |             |                                                          |                                                              |           |
| Read Variable(s)            via `GET`                        | `GET`       | `/_pxc_api/api/variables`                                | `pathPrefix=%OptionalVariablePathPrefix%&paths= 		%VariablePath1%,...,%VariablePathN%` | JSON data |
| Read Variable(s)            via `POST`                       | `POST`      | `/_pxc_api/api/variables`                                | with request body `pathPrefix=%OptionalVariablePathPrefix%&paths= 		%VariablePath1%,...,%VariablePathN%` | JSON data |
| Write Variable(s) with constant values                       | `PUT`       | `/_pxc_api/api/variables`                                | JSON Data with `%ConstantValue1%` to `%ConstantValueN%`      | JSON data |
| Write Variable(s)with variable values                        | `PUT`       | `/_pxc_api/api/variables`                                | JSON Data with `%ReadVariablePath1%` to `%ReadVariablePathN%` | JSON data |

де `_pxc_api`, `api`, `v1.0`, `sessions`, `groups`, `variables` віртуальні директорії

Примітка: щоб працювати з вищевказаними функціями на вашому PLCnext Control, спочатку зробіть інтерфейс даних REST **захищеним ресурсом**, як описано в розділі Автентифікація.

### Service Description

Документ з описом служби доступний у корені сервісу. Він надає версію сервісу у формі `v{Major}.{Minor}`. Шлях `api` можна використовувати для отримання останньої версії сервісу. Шлях поточної версії – `v1.1`. 

Це робиться за допомогою однієї з наступних команд HTTP GET:

```http
GET https://%plcaddress%/api
```

або

```http
GET https://%plcaddress%/_pxc_api/v%Major%.%Minor%
```

де `%PlcAddress%` - це адреса ПЛК, `%Major%` — це основна версія API 1, і `%Minor%` — це проміжна версія API (0–1).

Відповіддю є код статусу HTTP `200` (OK) для успішного отримання опису сервісу разом із отриманими даними JSON. Інакше для невдалої спроби повідомляється відповідний код статусу HTTP (наприклад, `400` = неправильний запит, `500` = внутрішня помилка сервера).

Детальніше можна прочитати [за посиланням](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Request.htm#Service_description).

### Sessions (сеанси)

При бажанні веб-сервер може відстежувати набір активних клієнтських станцій на основі відповідної інформації про сеанс. В іншому випадку веб-сервер автоматично вважатиме кожну станцію анонімною. Дані сервіси надають можливості керувати сеансами. Доступні наступні дії з сеансами через REST:

- Звітування про сеанси - повідомляє про колекцію поточних активних сеансів на веб-сервері (якщо такі є). Отриманий звіт – це сторінка з нульом або більше ідентифікаторами та посиланнями сеансу разом із посиланням на наступну сторінку (якщо така є), доки не буде отримано повний список поточних активних сеансів.
- Створення сеансу - створює контрольований за часом сеанс для певної станції. Отриманою відповіддю є ідентифікатор сеансу та час очікування сеансу.
- Підтримання сеансу - підтримує попередньо створений сеанс для активної клієнтської станції, щоб уникнути тайм-ауту сеансу.
- Перепризначення сеансу - перепризначає раніше створений сеанс іншій станції, а отримана відповідь є ідентифікатором нового сеансу.
- Видалення сеансу - видаляє попередньо створений сеанс 

Детальніше можна прочитати [за посиланням](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Sessions.htm)

### Variables

Інтерфейс надає можливість читання/запису змінних. Глобальні змінні повинні мати тег HMI, щоб бути доступними для REST API! Читання можна робити через GET або POST. 

#### Читання

Приклад читання через GET:

```http
GET https://%PlcAddress%/_pxc_api/api/variables?sessionID=%OptionalSessionID%&pathPrefix=%OptionalVariablesPathPrefix%&paths=%VariablePath1%,...,%VariablesPathN%
```

| Замінники                      | Призначення                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| `%PlcAddress%`                 | адреса ПЛК                                                   |
| `%OptionalSessionID%`          | необов’язково призначений ідентифікатор для поточного сеансу, який потрібно підтримувати, щоб неявно уникнути тайм-ауту сеансу або `""`, якщо не використовується |
| `%OptionalVariablePathPrefix%` | необов’язковий бажаний префікс шляху для кожної зі змінних (наприклад, `Arp.Plc.Eclr/`) |
| `%VariablePath1%`              | відносний або повний шлях першої змінної (наприклад, `Go`, `Arp.Plc.Eclr/Go` відповідно), а також додатковий індекс, індекси та/або діапазон індексів у випадку змінної масиву (наприклад, `PartArray[2]`, `PartArray[2; 4]`, `PartArray[2; 4; 6-8]` відповідно) |
| `%VariablePathN%`              | відносний або повний шлях останньої змінної (наприклад, `CycleCount`, `Arp.Plc.Eclr/CycleCount` відповідно), а також необов’язковий індекс, індекси, -та/або- діапазон індексів у випадку змінної масиву (наприклад, `ProductArray[2]`, `ProductArray[2; 4]`, `ProductArray[2; 4; 6-8]` відповідно) |

Ця команда зчитує поточне(і) значення(а) вказаної(их) змінної(ї) у тому самому порядку, у якому було надано спочатку. Результуюча відповідь є значеннями змінної або змінних. 

Відповіддю є код статусу HTTP `200` (ОК) для успішного читання змінної разом із отриманими даними JSON, показаними в наступному блоці коду; інакше для невдалої змінної прочитаєте відповідний код стану HTTP (наприклад, `400` = неправильний запит, `408` = час очікування запиту, `500` = внутрішня помилка сервера).

```json
{
  "apiVersion": "%ApiVersion%",
  "projectCRC": %ProjectCrc%,
  "userAuthenticationRequired": %UserAuthenticationRequired%,
  "variables":
  [
    {
    "path": "%VariablePath1%",
    "value": %VariableValue1%
    },
    …
    {
    "path": "%VariablePathN%",
    "value": %VariableValueN%
    }
  ]
}
```

Наприклад, запит:

```http
GET https://127.0.0.1:5050/_pxc_api/api/variables?pathPrefix=Arp.Plc.Eclr/MyProgram1.Machine&paths=1,2
```

При вдалому виконанні може повернути наступне корисне навантаження:

```json
{
	"apiVersion": "1.8.0.0",
	"projectCRC": 4197131709,
	"userAuthenticationRequired": true,
	"variables": [
		{
			"path": "Arp.Plc.Eclr/MyProgram1.Machine1",
			"value": {
				"x_pos": 10,
				"y_pos": -10,
				"depth": 25,
				"rpm": 800,
				"ready": false,
				"temper": 2.412008762359619
			}
		},
		{
			"path": "Arp.Plc.Eclr/MyProgram1.Machine2",
			"value": {
				"x_pos": 10,
				"y_pos": -10,
				"z_pos": -10,
				"depth": 25,
				"rpm": 800,
				"ready": false,
				"temper": 2.612008810043335
			}
		}
	]
}
```

Аналогічну операцію можна зробити через метод `POST` , про що можна прочитати за [цим посиланням](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Variables.htm).

#### Запис константи

Записування передбачає два варіанти:

- записування в змінну значення константи
- записування в змінну значення іншої змінної

Формат записування константи наступний:

```http
PUT https://%PlcAddress%/_pxc_api/api/variables
```

Корисне навантаження має мати наступний формат:

```json
{
  "sessionID": "%OptionalSessionID%",
  "pathPrefix": "%OptionalVariablePathPrefix%",
  "variables":
  [
    {
      "path": "%VariablePath1%",
      "value": %ConstantValue1%,
      "valueType": "Constant"
    },
    …
    {
      "path": "%VariablePathN%",
      "value": %ConstantValueN%,
      "valueType": "Constant"
    }
  ]
}
```

Де `%ConstantValueN%` - значення, яке записується, усі інші замінники в синтаксисі наведено в таблиці вище.

Відповіддю є код статусу HTTP 200 (ОК) для успішного запису змінної разом із отриманими даними JSON. Інакше для невдалої змінної видасть відповідний код статусу HTTP (наприклад, 400 = Bad Request, 408 = Request Timeout, 500 = Internal Server Error).

Наприклад, URL запиту:

```http
PUT https://127.0.0.1:5050/_pxc_api/api/variables
```

При цьому корисне навантаження:

```json
{
    "pathPrefix": "Arp.Plc.Eclr/MyProgram1.Machine",
    "variables": [
        {
            "path": "1.x_pos",
            "value": 1000,
            "valueType": "Constant"
        },
        {
            "path": "2.temper",
            "value": 1.1,
            "valueType": "Constant"
        }
    ]
}
```

Корисне навантаження у відповіді:

```json
{
	"apiVersion": "1.8.0.0",
	"projectCRC": 4197131709,
	"userAuthenticationRequired": true,
	"variables": [
		{
			"path": "Arp.Plc.Eclr/MyProgram1.Machine1.x_pos",
			"value": 1000,
			"uri": "/_pxc_api/api/variables/Arp.Plc.Eclr/MyProgram1.Machine1.x_pos"
		},
		{
			"path": "Arp.Plc.Eclr/MyProgram1.Machine2.temper",
			"value": 1.100000023841858,
			"uri": "/_pxc_api/api/variables/Arp.Plc.Eclr/MyProgram1.Machine2.temper"
		}
	]
}
```

#### Запис значення з іншої змінної

Нас команда записує поточні значення (значення) запитуваної (джерельних) змінної (змінних) до вказаної (цільової) змінної (змінних). Це робиться за допомогою такої ж команди HTTP `PUT` як в попередньому випадку:

```http
PUT https://%PlcAddress%/_plc_api/api/variables
```

але в корисному навантаженню у "value" вказується посилання на іншу змінну: 

```json
{
  "sessionID": "%OptionalSessionID%",
  "pathPrefix": "%OptionalVariablePathPrefix%",
  "variables":
  [
    {
      "path": "%WriteVariablePath1%",
      "value": "%ReadVariablePath1%",
      "valueType": "Variable"
    },
    …
    {
      "path": "%WriteVariablePathN%",
      "value": "%ReadVariablePathN%",
      "valueType": "Variable"
    }
  ]
}
```

### Groups

Якщо потрібно, веб-сервер може керувати зареєстрованими групами, щоб полегшити читання попередньо означеного списку змінних. В іншому випадку можна просто використовувати веб-сервер для безпосереднього читання змінних. 

- Report Groups - Ця команда повідомляє про колекцію поточних зареєстрованих груп (якщо такі є). Отриманий звіт – це сторінка з нулем або більше назвами груп і посиланнями разом із посиланням на наступну сторінку (якщо така є), доки не буде отримано повний список зареєстрованих на даний момент груп.
- Register Group -  Ця команда реєструє групу для наданого списку змінних, які пізніше можна прочитати разом. Отриманою відповіддю буде назва групи та типи змінних.
- Read Group - Ця команда зчитує значення попередньо зареєстрованого списку змінних групи в тому ж порядку, в якому вони були надані спочатку. Отримана відповідь є тією самою відповіддю на реєстрацію групи разом зі значеннями змінних.
- Unregister Group - Ця команда скасовує реєстрацію попередньо зареєстрованої групи, а отримана відповідь просто означає, чи було це успішно.

Детальніше можна прочитати [за посиланням](https://www.plcnext.help/te/Service_Components/REST_data_interface/REST_data_interface_Groups.htm)

## Приклад клієнта в Node-RED

Для демонстрації роботи REST використаємо Node-RED. Ніяких спеціальних вузлів не потрібно, скористаємося вбудованими вузлами Node-RED.  

Для початку 
