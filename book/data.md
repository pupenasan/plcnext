## Робота з даними

### Типи даних

У PLCnext Engineer чітко розрізняються коди, пов’язані з безпекою, і стандартні (не пов’язані з безпекою). Тому також розрізняють змінні, або, точніше, типи даних пов'язані з безпекою, і стандартні.

- Стандартні (не пов'язані з безпекою) типи даних, сумісні з IEC 61131-3. Це включає елементарні (elementary) та визначені користувачем типи даних (user-defined data types).
- Елементарні типи даних, пов'язані з безпекою.

Наприклад, неможливо підключити змінну зі стандартним типом даних до формального параметра, який очікує змінну, пов’язану з безпекою. У аркушах коду, пов’язаного з безпекою, пов’язані з безпекою та стандартні змінні можуть бути змішані та напряму пов’язані одна з одною. Це відповідає неявному перетворенню типу даних, яке вимагає дотримання деяких правил. Типи даних по'вязані з безпекою розглядаються в іншому розділі. 

На відміну від типів даних, визначених користувачем, [elementary](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.3/en/Help/elementarydatatypes.htm) і [generic data types](file:// /C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.3/en/Help/genericdatatypes.htm) не потрібно оголошувати, оскільки вони попередньо визначені в системі, і тому одразу доступні в кожному POU.

Визначені користувачем типи даних, такі як ARRAYs, STRUCTs і STRINGs, повинні бути оголошені в аркуші типів даних за допомогою текстового редактора. Керування аркушами типів даних здійснюється в категорії «Типи даних» в області COMPONENTS праворуч. Тут ви додаєте аркуші нових типів даних, відкриваєте наявні аркуші для редагування або структуруєте категорію, вставляючи папки. Кожен блок оголошення типу даних починається ключовим словом TYPE і закінчується ключовим словом END_TYPE. Блок оголошення може містити одне або більше оголошень типу даних.

Елементарні типи даних показані в таблиці:

| **Data type**        |                            | **Size in bits** | **Range**                                                    | **Default initial value** |
| -------------------- | -------------------------- | ---------------- | ------------------------------------------------------------ | ------------------------- |
| BOOL                 | Boolean                    | 1                | TRUE / FALSE                                                 | FALSE                     |
| SINT                 | Short integer              | 8                | -128 to 127                                                  | 0                         |
| INT                  | Integer                    | 16               | -32,768 to 32,767                                            | 0                         |
| DINT                 | Double integer             | 32               | -2,147,483,648 to 2,147,483,647                              | 0                         |
| LINT                 | Long integer               | 64               | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807      | 0                         |
| USINT                | Unsigned short integer     | 8                | 0 to 255                                                     | 0                         |
| UINT                 | Unsigned integer           | 16               | 0 to 65,535                                                  | 0                         |
| UDINT                | Unsigned double integer    | 32               | 0 to 4,294,967,295                                           | 0                         |
| ULINT                | Unsigned long integer      | 64               | 0 to 18,446,744,073,709,551,615                              | 0                         |
| REAL                 | Real numbers               | 32               | -3.402823466 E+38 (approx. 7 digits) 								  to  -1.175494351 E-38 (approx. 7 digits) 								 and +1.175494351 E-38 (approx. 7 digits) 								  to  +3.402823466 E+38 								(approx. 7 digits) 								See the  note below the table concerning REAL and LREAL processing. | 0.0                       |
| LREAL                | Long real numbers          | 64               | ~ -1.798 E+308 (approx. 15 digits)  to  ~ -2.225 E-308 (approx. 15 digits) and ~ +2.225 E-308 (approx. 15 digits)  to  ~ +1.798 E+308 (approx. 15 digits)See the  note below the table concerning REAL and LREAL processing. | 0.0                       |
| TIME                 | Duration                   | 32               | -24d20h31m23s648ms up to 24d20h31m23s647ms                   | t#0s                      |
| LTIME                | Long duration              | 64               | - 106751d23h47m16s854ms775us808ns up to  +106751d23h47m16s854ms775us807ns | LTIME#0ns                 |
| BYTE                 | Bit string of length 8     | 8                | 0  to 255  (16#00...16#FF)                                   | 0                         |
| WORD                 | Bit string of length 16    | 16               | 0  to 65,535  (16#00...16#FFFF)                              | 0                         |
| DWORD                | Bit string of length 32    | 32               | 0 to 4,294,967,295  (16#00....16#FFFFFFFF)                   | 0                         |
| LWORD                | Bit string of length 64    | 64               | 0 to 18,446,744,073,709,551,615  (16#00....16#FFFFFFFFFFFFFFFF) | 0                         |
| LDATE / LD           | Long date                  | 64               | min. : 1677-09 -22max. :  2262-04-11                         | LDATE#1970-01-01          |
| LTIME_OF_DAY / LTOD  | Long time of day           | 64               | min. : 00:00:00.0max. : 23:59:59.999999999See the  note below the table concerning LTOD processing. | LTOD#00:00:00             |
| LDATE_AND_TIME / LDT | Long date with time of day | 64               | min. : 1677-09-21-00:12:43.145224192max. : 2262-04-11-23:47:16.854775807 | LDT#1970-01-01-00:00:00   |

Текстові типи даних показані в таблиці:

| Byte                                                         | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **STRING data type**                                         |                                                              |
| 0...1                                                        | Capacity - Maximum number of characters that the string can hold (from 0 to UINT16.MAX). |
| 2...3                                                        | Length - Current number of characters in the string (from 0 to Capacity). |
| 4...84  (for an 80 characters string)4...124  (for  a user-defined string of 120 characters) | String of ANSI characters followed by a trailing zero character. Each character is represented by one byte. |
| **WSTRING data type**                                        |                                                              |
| 0-1                                                          | Capacity - Maximum number of characters that the string can hold (from 0 to UINT16.MAX). |
| 2-3                                                          | Length - Current number of characters in the string (from 0 to Capacity). |
| 4...165 (for an 80 characters string)4...245 (for  a user-defined string of 120 characters) | String of UTF16 characters followed by a trailling zero character. Each character is represent |

Загальні типи даних (Generic data types) — це типи даних, які включають ієрархічні групи елементарних типів даних. Наприклад, ANY_INT включає елементарні типи даних SINT, DINT, INT, LINT, USINT, UINT, UDINT і ULINT. Якщо функція може бути пов'язана з ANY_INT, це означає, що змінні цих типів даних можна обробляти.

Загальні типи даних організовані таким чином:

![img](media/GenericDataTypes.PNG)

### Користувацькі типи даних 

IEC 61131-3 надає можливість декларувати власні, визначені користувачем типи даних на основі стандартизованих елементарних типів даних IEC 61131-3. Таким чином, прикладні програмісти можуть вказати власну «модель даних» з огляду на вимоги поточної програми. Ці визначені користувачем типи даних також відомі як похідні типи даних або визначення типів.

Визначені користувачем типи даних оголошуються на аркуші типів даних за допомогою блоку декларацій TYPE ... END_TYPE.

PLCnext Engineer підтримує такі визначення типів:

- Arrays - типи даних масиву складаються з кількох елементів одного типу даних.
- Structures - структуровані типи даних включають кілька елементів одного або різних типів даних
- Enumerations - змінна може зберігати одне з кількох імен, указаних у визначенні типу
- User-defined strings - Визначені користувачем рядки складаються з певної кількості символів, указаних у визначенні типу.
- Комбіновані - Масиви, структури, перерахування та рядки можна комбінувати, утворюючи, наприклад, структуру з переліками або масиви структур

Використання та формат (пам’яті) типів даних, визначених користувачем, залежать від типу контролера. Тип даних, визначений користувачем, має бути визначений лише один раз на аркушах типів даних проекту. Однак у включених бібліотеках допускається ідентичне визначення типу даних (те саме ім’я). Це означає, що, беручи до уваги бібліотеки, можливе кілька визначень одного типу даних.

#### Масиви

Масиви оголошуються на аркуші типу даних за допомогою блоку оголошення TYPE...END_TYPE. Масив може бути багатовимірним. Типи даних масиву складаються з кількох елементів одного типу даних. До кожного елемента можна отримати доступ через його унікальний індекс елемента.

Довжину масиву (кількість елементів або значень, які може містити масив) можна визначити за допомогою літералів (фіксованих значень) або постійних змінних. Постійні змінні мають бути оголошені як VAR CONSTANT на аркуші типу даних за допомогою блоку оголошення VAR CONSTANT...END_VAR (див. приклад нижче).

Приклад 1. 1-вимірний масив з 10 цілих чисел, де довжина масиву, визначена за допомогою літералів матиме вигляд:

![img](media/Array_TypeDeclaration.gif)

Якщо необхідно об'являти масив через константну змінну:

![img](media/Array_TypeDeclaration_VarConst.png)

Створення змінної матиме вигляд:

![img](media/Array_VarDeclaration.png)

Звернення до 1-го елементу масиву матиме вигляд:

![img](media/Array_STcode.gif)

Приклад 2. 2-вимірний масив. Об'явлення матиме вигляд як масив масивів

![img](media/MultiArray_TypeDeclaration.gif)

Звернення до елементу масиву:

![img](media/MultiArray_STcode.gif)



Масиви, або, точніше, окремі елементи, що містяться в масиві, можна ініціалізувати наступним чином:

![img](media/Array_Init_dataTypeWS.gif)

Для ініціалізації масивів з великою кількістю значень можна використовувати так звані *числа повторення*.
Приклад ініціалізації в аркуші типів даних:

![img](media/Array_Init_dataTypeWS_repNumbers.gif)

Ця ініціалізація означає те саме, що:

![img](media/Array_Init_dataTypeWS_repNumbers_explain.gif)

У редакторі «[Налаштування початкового значення](file:///C:/Program Files/PHOENIX CONTACT/PLCnext Engineer 2022.3/en/Help/InitValueConfigurationEditor.htm)» ці числа повторення можна ввести як оператор ініціалізації для ініціалізації масивів .

#### Структури

Структуровані типи даних включають кілька елементів одного або різних типів даних. Таким чином, структури особливо підходять для відображення та обробки кількох різних властивостей об’єкта, наприклад, характеристик машини. У наступному прикладі тип структурованих даних "machine" складається з компонентів `x_pos`, `y_pos`, `depth` і `rpm`. Усі компоненти описують характеристики машини. Приклад опису типів даних показаний на рис.

![img](media/Struct_TypeDeclaration.gif)

Створення змінної матиме вигляд:

![img](media/Struct_VarDeclaration.png)

Приклад використання:

![img](media/Struct_STcode.gif)

Структури, або, точніше, окремі елементи, що містяться в структурі, можна ініціалізувати наступним чином:

![img](media/Struct_Init_dataTypeWS.gif)

#### Перечислення (Enumerations, enums)

Змінна визначеного користувачем типу даних `enum` може мати значення, вибране з попередньо визначеного списку. Інші значення змінних неможливі.

Приклад: перелік «PossibleColors» включає значення Red, Green і Blue, кожне з яких є типом даних INteger, тобто Red = 0, Green = 1 і Blue = 2. У ST для змінної «Color», яка має тип даних enum, встановлено значення INT = 1, тобто зелений, якщо Start = TRUE. 

Означення типу:

![img](media/Enum_TypeDeclaration.GIF)

Створення змінної типу:

![img](media/Enum_VarDeclaration.png)

Використання змінної:

![img](media/Enum_STcode.gif)

Якщо два або більше перерахувань містять елемент з однаковою назвою, ім’я ENUM за бажанням може бути додано перед ім’ям елемента, а потім символ решетки. Приклад: два перерахування містять елемент Green.

```pascal
TYPE
   EnumColor : (Red := 0, Green := 10, Blue := 20) OF INT := Green;
   EnumBackgound : (Red := 255, Green := 128, Blue := 0) OF INT := Green;
END_TYPE
```

У коді оператор `eColor := EnumBackgound#Green;` є унікальним.

Перерахування можна ініціалізувати двома способами:

- Значення можна призначити кожному елементу, зазначеному в списках імен (Blue := 5 у прикладі нижче). Без ініціалізації цих елементів списку вони постійно нумеруються, починаючи з 0 для першого елемента.

- Один елемент списку можна попередньо вибрати або на аркуші типу даних, або в таблиці змінних, як показано в наступному прикладі (initial value = Green).

   Ви також можете використовувати редактор «Init Value Configuration» для визначення попередньо вибраного елемента переліку.

![img](media/Enum_Init_dataTypeWS.gif)

![img](media/Enum_InitValue.png)

Змінна типу даних "PossibleColors" буде ініціалізована значенням Green (INT#1).

#### Strings

User-defined strings (STRINGs) are composed of a specified or a  variable number of characters. If a number is specified in brackets at  the string type definition, this is considered as fixed string length.  If no length is specified, the string can contain any number of  characters within the valid range.

Визначені користувачем рядки (STRING) складаються з певної або змінної кількості символів. Якщо число вказано в дужках у визначенні типу рядка, це вважається фіксованою довжиною рядка. Якщо довжина не вказана, рядок може містити будь-яку кількість символів у допустимому діапазоні від 1 до 32766.

Означення типу даних:

![img](media/String_TypeDeclaration.GIF)

Означення змінної:

![img](media/String_VarDeclaration.png)

Приклад використання змінної:

![img](media/String_STcode.gif)





Наступне стосується віддаленого доступу до змінних STRING, наприклад, з OPC UA, PLCnext Engineer HMI та онлайн-функцій PLCnext Engineer:

 **Обмеження віддаленого доступу**: доступ для читання та запису до змінних STRING поза програмою контролера обмежено 511 байтами. Причиною цього є обмеження у Remote Service Calls (RSC) IDataAccessService та ISubScriptionService. RSC — це служби для зв’язку між процесами та пристроями. Вони надають API для доступу до всіх основних компонентів мікропрограми PLCnext Technology.

Окрім визначених користувачем рядків, визначених у таблиці типу даних, стандарт IEC 61131-3 визначає рядки за замовчуванням. На відміну від рядків, визначених користувачем, STRING за замовчуванням можна безпосередньо оголошувати в таблицях змінних, вибравши тип STRING. Визначення типу даних не потрібне. Стандартний рядок має довжину 80 символів.

Початкове значення встановлюється для рядка в таблиці змінних шляхом введення рядка ініціалізації в стовпець «Ініціалізація» в одинарних лапках.

![img](media/String_InitValue.png)

#### Комбінації типів даних, визначених користувачем

Визначені користувачем типи даних можуть бути комбінованими але не можуть бути вкладеними (тобто без рекурсивних викликів).

Використовуйте редактор «Init Value Configuration» для ініціалізації структур і масивів.

Використання масиву структур, тобто структур, організованих як масиви, показано в наступному прикладі.

![img](media/ArrayOfStruct.gif)

Тут відображається виробнича лінія (масив «line») із 6 свердлильними верстатами (структура 'machine'). За допомогою індексу масиву можна отримати доступ до кожної конкретної станції. За допомогою компонентів конструкції можна призначати різні значення для свердління.

У структурах можна використовувати масиви, перерахування та рядки, тобто один або кілька оголошених типів даних, визначених користувачем, містяться як компоненти в структурі. У наступному прикладі масив ('seq') міститься в структурі, а також перелік 'material', якому можна встановити одне зі значень 'wood', 'aluminum' або 'stone'.

![img](media/StructWithArrayAndEnum.gif)

Комбінації структур і масивів, а якщо бути точніше, їх окремі члени, також можна ініціалізувати за допомогою так званих *repetition numbers (чисел повторення)*.

**Example 1: Structure with array**

У наступному прикладі структура MyStruct містить серед інших елементів масив Integers.

![img](media/StructWithArray_Init_dataTypeWS_repNumbers.png)

У редакторі 'Init Value Configuration' оператор ініціалізації може мати такий вигляд:

![img](media/StructWithArray_InitValueConfigurationEditor.png)

**Example 2: Array of structures**

У наступному прикладі масив MyStruct оголошено на аркуші типів даних:

![img](media/ArrayOfStructs_Init_dataTypeWS_repNumbers.png)

У редакторі Init Value Configuration рядок ініціалізації містить repetition numbers не лише для масиву в MyStruct, але й для масиву структури:

![img](media/ArrayOfStructWithArray_InitValueConfigurationEditor.png)

Пояснення оператора ініціалізації:

```pascal
[4((Field1 := 1, Field2 := TRUE, Field3 := [10(4)])),//4 elements are initialized
  2( ),                        //next 2 elements with default init value
  4((Field1 := 9, Field2 := TRUE, Field3 := [5(3), 5 (6)])) //last 4 elements are initialized
]
```

#### Ports of user-defined Data Types

Порти можуть мати визначені користувачем типи даних, наприклад масиви або структури. Ви можете створити такий визначений користувачем порт так само, як і змінну типу даних, визначеного користувачем:

1. Введіть визначення типу в аркуш типу даних, використовуючи ключові слова оголошення `TYPE ... END_TYPE`.
2. Оголошуйте порт із визначеним користувачем типом даних: встановіть для параметра «Usage» значення «IN Port» або «OUT port», коли оголошуєте порт у програмному POU.

Тепер ви можете використовувати елементи, що містяться у визначених користувачем типах у програмному коді.

Також зовнішні (не IEC 61131-3) програми можуть містити порти складних типів даних.

У списку портів (вузла «PLCnext» або вузла екземпляра програми в PLANT) ви можете призначити окремих членів такого порту іншому порту. Коли відкривається інструмент вибору ролей для призначення, для вибору пропонуються відповідні члени порту.

**Example**

Порт OUT MyPortArray — це визначений користувачем тип даних ARRAY:
`MyArray : ARRAY[0..9] OF BOOL; `

Під час відкриття засобу вибору ролі для логічного порту в списку портів для вибору пропонуються окремі члени порту.

![img](media/PortList_AssignPortMember.png)

The first bit of the array has been mapped to  Boolean IN port:

![img](media/PortList_PortMemberAssigned.png)



After disconnecting a port member, it remains unconnected in the Port List. It can then be deleted with the toolbar command 'Delete  unconnected port member':

### Порядок створення користувацьких типів даних в средовищі


