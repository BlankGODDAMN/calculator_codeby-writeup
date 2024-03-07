# calculator_codeby-writeup
После перехода по данному нам адресу, мы видим страницу сайта-калькулятора 

![izobrageniee](https://github.com/BlankGODDAMN/calculator_codeby-writeup/assets/162637113/3438cb9c-aaed-4102-bd66-d53510850c4b)

Нас интерсует, то, что написанно жирным шрифтом
>***Мы также подключили WAF, чтобы обеспечит полную безопасность.***

Что ж, первое что можем сделать, это коннечно проверить что нам выдаст калькулянтор, если мы введём задачку

![image2](https://github.com/BlankGODDAMN/calculator_codeby-writeup/assets/162637113/00bc3f9b-a63c-4ecb-a13e-51fe918fb386)

![image3](https://github.com/BlankGODDAMN/calculator_codeby-writeup/assets/162637113/1ab38b3e-c405-42f3-a6e4-a5b37f8d1408)

Мы посто получили ответ. 

Давайте вернёмся к надписи сверху, и посмотрим подсказку. 

>***Если ты сейчас пытаешься обойти фильтры, то ты на правильном пути :D***

Несложно догадаться, что нужно кооперация в народности WAF. Обращаем к Нашему Драгу Краузеру и Спрашивакем.
Почитав об обходе WAF, мы выяассняем, что нужно использовать служебные символы

со списком таких символов можно ознакомиться [здесь](https://vds-admin.ru/shell-scripting/sluzhebnye-simvoly#singlekav)

Для решения данного таска нам понадобиться символ `. Заключив в него команду, сервер исполнит её и выдаст нам ответ. Попробуем исполнить команду *ls*

![izobrageniee](https://github.com/BlankGODDAMN/calculator_codeby-writeup/assets/162637113/e7df46ae-d1fa-41c2-a77c-b25c94964a2c)

![izobrageniee](https://github.com/BlankGODDAMN/calculator_codeby-writeup/assets/162637113/ce2dcdef-5277-4af9-a7c0-983f5fa13669)

Отлично, мы нашли файл ***index.php***

Ну, попробуем прочитать его с помощью *cat* 

![izobrageniee](https://github.com/BlankGODDAMN/calculator_codeby-writeup/assets/162637113/faefeec3-d463-4668-9e37-d4e2ac9ccd4b)

![izobrageniee](https://github.com/BlankGODDAMN/calculator_codeby-writeup/assets/162637113/296957e1-ce55-4ec4-8aa4-619c6eae9c40)

Среди всего полученного текста находития наш флаг, думаю найти его не сложно.


