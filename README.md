# Print Bot 
## v.1.0


Перед запуском в файле `.env` вставить свои значения:
```
API_TOKEN = 'sample:text'
ADMIN_ID = 'sample:text'
PAYMENT = 'sample:text'
```
Установить библиотеки через `pip`:
```
pip install -r requirements.txt
```
# Первоначальная идея

### О боте
Данный чат-бот преследует цель автоматизировать печать. Он имеет весьма узкое применение, но достаточно актуальное для студентов с принтерами. В университете как известно нужно много печатать, но не у всех есть принтер, поэтому когда выясняется кто счастливый обладатель данного аппарата, этот самый человек становится на расхват и время от времени печатает по просьбе товарищей. Со временем, либо сразу, можно прийти к тому, что не плохо бы это монетизировать, разумеется чтобы отбивать цену листов, чернил и теоретически даже тех обслуживания. Так я и поступал 3 курса обучения и на 4 курсе пришла идея автоматизировать сей процесс, бонусом это частично решит проблему неловкости обсуждения печати в переписке.

### Пользователь
Таким образом, со стороны пользователя при первом запуске его встретит три кнопки:

1. Пополнить баланс
2. Отправить файлы на печать
3. Проверка статуса печати

Телеграм позволяет подключить платежные системы, что очень автоматизирует процесс, но на первых этапах реализуем проще, при нажатии, будет отправлятсья сообщение с инструкцией, затем же администратору (владельцу принтера), придётся самостоятельно пополнить пользователю баланс через этого же бота.

Когда пользователь нажмёт кнопку “Отправить файлы на печать”, ему предложит прикрепить файлы формата .pdf или .doc(x), бот попросит выбирать файлы внимательно для избежания ошибок в подсчёте конечной цены. Если файлы подходят по формату, бот начинает выполнять алгоритм для вычисления цены печати.

Для удобства, файлы объединяются и конвертируются в один .pdf, затем в .jpg

Формирование цены происходит по следующим параметрам:

1. Количество листов
2. Количество не белых пикселей

Таким образом для файла будет известно количество листов и процент их заполненности, из чего не сложно вычислить цену печати. Дополнительно можно добавить опцию скрепления степлером, скрепкой или комплектацией в файлик. И наконец суммировать итоговую стоимость. Когда цена формируется динамически, это позволяет более просто принять решение о заказе печати, т.к. очевидно из чего формируется оплата.

Если цена устраивает, пользователь нажимает “далее”, бот предупреждает что не несёт ответственность за ошибки в файлах и при нажатии “оплатить” отмена печати и возврат средств можно совершить, только при статусе “ожидает печати”. Файл моментально отправляется оператору.

Собственно наконец статус печати. При нажатии на кнопку “Проверка статуса печати” перед пользователем, будет список всех файлов с различными статусами. Так например, после подтверждения оплаты, файл попадает в очередь и автоматически получает статус “ожидает печати”, в этот момент его еще можно отменить. Далее, если оператор запустил печать, то статус изменяется на “напечатан” и с этого момента, возврат невозможен. Статус “отменён” также будет отображаться в этом разделе и формально он будет выполнять функцию журнала или истории, который или которую разумеется можно будет очистить. По каждому из статусов, пользователь получит соответствующее сообщение.

### Владелец-Оператор-Админ
Теперь перейдём к виду бота для владельца принтера, в его распоряжении будут кнопки:

1. Баланс пользователей
2. Файлы в очереди
3. Статус принтера

В первом пункте можно будет посмотреть баланс каждого из пользователей и при надобности изменить баланс в ручном режиме, полезно если нужно вернуть средства за ошибку со стороны бота или оператора.

Во втором пункте открывается список всех файлов в очереди на печать со статусом “ожидает печати”, можно вызвать каждый файл по отдельности и проставить новый статус. Все изменения моментально отправляются пользователям-владельцам файлов.

Последняя кнопка позволяет выставить статус принтера, нужна при форс-мажорных обстоятельствах, например когда кончились чернила, оператор выставляет статус “Закончились чернила” и каждому пользователю выводится соответсвующее сообщение и запрещается отправка новых файлов на печать.

Помимо всего прочего, оператору сразу же приходят сообщения с заявками на печать, он тут же может поменять статус не прибегая к кнопке “Файлы в очереди”.
