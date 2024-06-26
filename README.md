# csa_lab3

 - Ляшенко Никита Андреевич, P3209
 - Вариант: `asm | acc | harv | mc -> hw | tick -> instr | struct | stream | mem | pstr | prob1 | cache`
 - Базовый вариант

## Язык программирования
- ```[ ... ]``` -- вхождение 0 или 1 раз
- ```{ ... }``` -- вхождение 0 или несколько раз
- ```{ ... }+``` -- вхождение 1 или несколько раз
  
```
program ::= { line }

line ::= [data_part] [code_part]

data_part ::= ".data:" [comment] "\n"
               {data_line} 

data_line ::= name_of_data data [comment] "\n"

data ::= "NUMBER" number 
       | "STRING" string

code_part ::= ".code:" [comment] "\n"
              {code_part}

code_part ::= label [comment] "\n"
            | instr [comment] "\n"
            | [comment] "\n"
            
label ::= name_оf_label ":" "\n"

instr ::= op1 | op2 | op3 | op4
        
op1 ::= "NOP" 
      | "HLT" 
      | "JMP" name_of_label
      | "JZ" name_of_label
      | "CALL" name_of_label
      | "RET" 

op2 ::= "INC"
      | "DEC"
      | "CLA"
      | "NOT"

op3 ::=  "ADD" addr; Адресные
      | "SUB" addr
      | "DIV" addr
      | "MUL" addr
      | "MOD" addr
      | "LOAD" addr ; Думаю
      | "CMP" addr ; Думаю

op4 ::= "READ" ; Адресные
      | "PRINT"
        
comment ::= ";" {<any except "\n">}

name_of_label ::= { <any of "a-z A-Z 0-9 _"> }+

name_of_data ::= { <any of "a-z A-Z 0-9 _"> }+

number ::= ["-"] { <any of "0-9"> }+

string ::= {<any except "\n" ";">}

comment ::= ";" {<any except "\n">}
```
_Операции потока программы_
- `NOP` - пустышка 
- `HLT` - стоп программы
- `JMP` - перейти к метке (адресу памяти команд)
- `JZ` - перейти к метке (адресу памяти команд), если в аккумуляторе 0
- `CALL` - вызвать функцию по адресу памяти команд и записать адрес в стек возврата
- `RET` - вернуться по адресу из стека возврата

_Арифметические безадресные операции_
- `INC` - увеличить на один значение в AC
- `DEC` - уменьшить на один значение в АС
- `CLA` - очистить аккумулятор
- `NOT` - инвертировать аккумулятор

_Арифметические адресные операции_
- `ADD` - сложить число из аккумулятора с числом из адреса переданного в команде
- `SUB` - вычесть число из аккумулятора с числом из адреса переданного в команде
- `DIV` - поделить число из аккумулятора с числом из адреса переданного в команде
- `MUL` - умножить число из аккумулятора с числом из адреса переданного в команде
- `MOD` - остаток от деления число из аккумулятора с числом из адреса переданного в команде

_Операции работы с I\O_
- `READ` - ввести значение извне в AC
- `PRINT` - вывести строку из AC

_Типы данных_
- `STRING` - Length-prefixed (Pascal string)
- `NUMBER` - целое число

Особенности: 
- Код выполняется последовательно
- При трансляции все метки в коде заменяются на соответствующие адреса
- Память выделяется и заполняется начальными значениями статически при запуске модели
- Видимость данных - глобальная
- Литералы:
  - Целое число
  - Строка

## Организация памяти
Модель памяти процессора:
- Гарвардская архитектура - память данных и память команд
- Память команд:
  - Машинное слово - 32 бита
  - Линейное адресное пространство
  - Реализуется списком целых чисел
  - Одно число - одна инструкция или аргумент
- Память данных:
  - Машинное слово - 32 бита, знаковое
  - Линейное адресное пространство
  - Реализуется списком целых чисел
  - Одно число - одно значение
- Память микрокоманд: 
  - Машинное слово - количество видов сигналов
  - Линейное адресное пространство
  - Реализуется списком списков активных сигналов(Множество бит)
- Адресация абсолютная (JMP JZ CALL)
- Для реализации подпрограмм присутствует стек возврата, в котором хранятся адреса следующих инструкций после возврата из подпрограммы
- Управление процессором осуществляется с помощью микрокода

```
           Registers
+------------------------------+
| acc                          |
+------------------------------+
 
       Instruction memory
+------------------------------+
| 00  : instruction            |
| 01  : instruction            |
|             ...              |
| n   : program start          |
|             ...              |
| i   : instruction            |
| i+1 : instruction            |
|             ...              |
+------------------------------+

          Data memory
+------------------------------+
| 00  : variable/int_vector    |
| 01  : variable               |
|    ...                       |
| i-1 : variable               |
| i   : variable               |
+------------------------------+

       Microprogram memory
+------------------------------+
| 00  : Signals                |
| 01  : Signals                |
|    ...                       |                
+------------------------------+

        Returns controller
+------------------------------+
| stack_of_returns: stack      |
+------------------------------+
``` 
#### Адресация
Прямая загрузка операнда
- add 10
- cmp 10
Абсолютная адресация
- add ^10
- cmp ^10  
P.S. адрессация начинается с 0, с первого блока .data 
## Система команд 

### Набор инструкций

## Транслятор

## Модель процессора

### DataPath

### ControlUnit

### MPC

## Тестирование

### CI

### Для сбора аналитики

