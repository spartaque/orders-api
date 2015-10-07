## ->createOrderWithCustomData
 (Создание заказа с добавлением пользовательских данных)

#### Пример URL для обращения:
 Данне можно передавать методом GET или POST

 http://rocketprofit.ru/crm/api/createOrderWithCustomData?flow_id=1&phone=82223334455&fio=Теркин%20Василий%20Василиевич&key=bc29b36f623ba82aaf6724fd3b16718&custom_data[my_shit]=mysupershit&custom_data[my_other_shit]=mysupershit2

#### Возможные данные для передачи:
 (Обязательные элементы помечены звездочкой)

     {
        'phone' : '8913154884', // * число или строка, содержащая только цифры от 10 до 13 знаков
        'fio' : 'Петров Василий Юрьевич', // * любая сторка
        'login' : 'bestPartner', // * логин в системе rocketprofit.ru
        'flow_id' : '123', // * число или строка, содержащая только цифры  - ID потока в системе rocketprofit.ru
        'key' : '1bc29b36f623ba82aaf6724fd3b16718', // * строка, которая формируется по алгоритму md5($api_key.$flow_id.$phone); Где точка - конкатенация строк;
        'ip' : '195.123.234.23', // строка, IP адрес покупателя
        'date' : '2015-05-06 22:18:12', // Дата создания заказа в формате TIMESTAMP
        'custom_data' : {'myData' : 123}, // произвольный массив данных, который сохранятся в нашей системе и жет быть запрошен позже
        'subids' : {'adv' : 'adv123', 'src' : 'vk'}, // массив данных, который сохранятся в нашей системе и будет интерпретирован как SubID

    }

#### JSON ответ:
    {
        'code': $code, // код ответа (200 - если успех и любой другой, если ошибка)
        'message': $message, // сообщение об успешно выполненной операции, либо об ошибке (зависит от кода)
        'order_id': $order_id // номер заказа (возвращается только в случае успешно завершенной операции)
    }

 Варианты:
 * 200 - Заказ успешно создан! Номер заказа: $order_id
 * 481 - Заполните телефон!
 * 482 - Заполните ФИО!
 * 483 - Невозможно определить поток! ( не передан flow_id)
 * 484 - Поток № $flow_id не зарегистрирован в системе!
 * 485 - Неверный токен!
 * 500 - Ошибка валидации поля (Если какое-либо переданное поле не прошло валидацию)
 







### ->GetOrdersStatuses
 Запрос данных по закаам за определенный период
 
#### Пример URL для обращения
 Данне можно передавать методом GET или POST
 
 http://rocketprofit.ru/crm/api/getOrdersStatuses?login=BestPartner&from=2015-02-08+00:00:00&to=2015-02-09+23:59:59&key=0a153b13d8a443915f832424b8f7949a

#### Данные для передачи
 (все обязательные)

     {
        'login' : 'bestPartner', // логин в системе rocketprofit.ru
        'key' : '1bc29b36f623ba82aaf6724fd3b16718', // строка, которая формируется по алгоритму md5($api_key.$from.$to); Где точка - конкатенация строк;
        'from' : '2015-05-06 22:18:12', // начало выборки по периоду
        'to' : '2015-05-07 22:18:12', // конец выборки по периоду
    }


#### Формат ответа

     {
        "status": $status,
        "orders" : {
        {
            "id" : 999, // Ид в нашей системе
            "status_id" : 22, // Ид статуса в нашей системе
            "date" : "2015-02-02 12:12:21", // Дата создания заказа
            "ip" : "115.201.2.34", // IP адрес заказчика
            "geo" : "RU", // Код страны заказчика
            "money" : 550, // Вознаграждение за заказ в рублях
            "money_paid" : 550, // Начисленное вознаграждение за заказ в рублях
            "custom_data" : {"mySysId" : 7809}, // Ваши данные по заказы переданные ранее при создании заказа
            "subids" : {'adv' : 'adv123', 'src' : 'vk'}, // SubID по этому заказу
        },
        {...}
        }
     }

     // или
     {
          "status" : $status, // статус ответа (200 - если успех, любой другой - ошибка)
          "message" : $message // сообщение об ошибке
     }

#### Варианты ответов
 * 492 - $param is required (Если вы забыли передать один из обязательных параметров)
 * 493 - Wrong login (Если вы передали незарегистрированный в нашей системе логин или вы заблокированы)
 * 494 - Wrong key: $key (Если вы передали неверный ключ $key - ключ, который вы передали)
 * 200 - Массив с данными заказов

#### Перечень возможных сатусов для заказов по доставке:
 * 1 - Новый
 * 8 - Недозвон
 * 2 - В работе
 * 3 - Доставляется
 * 4 - Доставлен
 * 5 - Доставлен (вознаграждение начислено)
 * 6 - Отменен

 #### Перечень возможных сатусов для заказов по подтвержденке:
 * 0  - Не найден (если заказ с переданным вами идентификатором не был найден в нашей системе)
 * 1 - В работе
 * 2 - Подтверждено
 * 3 - Отмена
 * 4 - Невалид
