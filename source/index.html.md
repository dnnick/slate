---
title: Техническая документация РФИ БАНК

language_tabs: # must be one of https://git.io/vQNgJ
  - html
  - php
  - python

toc_footers:
  - <a href='https://ecom.rfibank.ru/'>Подключить online оплаты РФИ БАНК</a>
  - <a href='https://ecom.rfibank.ru/connect/'>Интернет-экваринг от РФИ БАНК</a>

includes:
  - errors

search: true
---

# Введение

Данная документация предназначена для клиентов компании АО Банк «Резервные финансы и инвестиции» (АО «РФИ БАНК»). Здесь вы сможете ознакомиться с тем, как организовать взаимодействие с системой Банка РФИ, далее Банка, а также с порядком обмена информацией между серверами клиентов и Банка при оплате через указанный сервис.

Также посетите наш репозиторий на [GitHub](https://github.com/RFIBANK/).

# Cоздание сервиса

Если у вас несколько магазинов размещенных на разных серверах, мы предлагаем вам настроить для каждого сайта отдельный Сервис. Вы сможете гибко управлять настройками каждого Сервиса, при этом учет транзакций будет общий.

![Список сервисов]
(https://confluence.rficb.ru/download/attachments/885237/1.-%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%80%D0%B2%D0%B8%D1%81%D0%B0.jpg?version=1&modificationDate=1474609665697&api=v2)

Для создания сервиса зайдите на сайте [http://home.rficb.ru/](http://home.rficb.ru/) в личный кабинет. Далее выберите раздел «Инструменты», подраздел «Сервисы» и нажмите на кнопку «Добавить сервис».

![Создание нового сервиса]
(https://confluence.rficb.ru/download/attachments/885237/2.-%D0%A0%D0%B5%D0%B4%D0%B0%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%80%D0%B2%D0%B8%D1%81%D0%B0.jpg?version=1&modificationDate=1474609885352&api=v2)

# Как настроить удобный способ оплаты
### Выбор инструмента оплаты
В зависимости от специфики ТСП можно выбрать один или несколько вариантов инициации платежной транзакции. Возможные варианты:

Название | Описание | Типовое использование
---------| -------- | ---------------------
Кнопка | HTML форма с данными необходимыми для проведения оплаты | Использование на сайтах собственной разработки. В поля HTML формы можно передавать собственные параметры платежа (сумму, назначение и др.).
Ссылка (короткая ссылка) | URL или ShortenURL, после перехода по которой отображается форма выбора способа оплаты. | Отправка ссылок на оплату через любые системы обмена сообщениями. В параметрах URL можно динамически менять параметры платежной транзакции.
QR код | Ссылка на динамическое графическое изображение (QR код) | Размещение QR кода online на веб сайтах или offline на любых физических носителях
CRM модуль | Встраиваемый модуль | Если ваш сайт использует какую либо CRM систему, используя готовые модули можно подключить оплату в очень короткие сроки
Всплывающее окно (виджет) | HTML форма + JS | Данное решение полезно, если Вы не хотите, чтобы пользователь уходил с Вашего сайта. Виджет открывается на фоне основного сайта в д.
Внешний API | Вебсервис для инициации платежной операции | Данный способ подходит для ТСП формирующих выбор способа оплаты самостоятельно. При использовании данного механизма можно сформировать платежную транзакцию без отображения страниц платежной системы

## Формат отправки данных в запросе от партнера.

> Переадресация клиента в систему Банка происходит с помощью следующей HTML­формы:

```html
<form method="POST"  class="application"  accept-charset="UTF-8" action="https://partner.rficb.ru/alba/input/">
<input type="hidden" name="key" value="b5/uqup/i/ueWBrRyp9V0n97zyHty5YtV5u/NW27nlk=" />
<input type="hidden" name="cost" value="1" />
<input type="hidden" name="name" value="Название услуги или сервиса"/>
<input type="hidden" name="email" value="a@a.ru" />
<input type="hidden" name="order_id" value="0" />
<input type="image" style="border:0;" src="https://partner.rficb.ru/gui/images/alba_buttons/button_large.png" value="Оплатить" />
</form>
```

> Обратите внимание на параметр "key", его необходимо получить в [личном кабинете в настройках сервиса](https://home.rficb.ru/alba/index/).

Параметры необходимые для инициализации платежа:

Имя параметра	| Версия API | Значение | Примеры/примечания
-------------	| ---------- | -------- | ------------------
key	| 1.0 | ключ (идентификатор сервиса), присваиваемый системой при «создании кнопки» в личном кабинете | “b5/uqup/i/ueWBrRyp9V0n97zyHty5YtV5u/NW27nlk=
cost | 1.0, 2.0 | сумма в рублях, которую клиент должен заплатить | 100
name | 1.0, 2.0 | Описание оплачиваемого товара/услуги. Отображается на странице оплаты. | Не более 128 символов. Пример: Заказ №212
email | 1.0, 2.0 | электронная почта клиента поле обязательно для рекуррентных платежей, для остальных вариантов оплаты необходимость ввода регулируется в настройках сервиса | test@test.com
phone_number | 1.0, 2.0 | телефонный номер плательщика необходимость обязательного ввода регулируется в настройках сервиса и параметрами платежного канала	| 74951234567
order_id | 1.0, 2.0 | Цифровое поле, обязательно Номер заказа в системе партнера, должен быть уникальным. Дважды заказ с одинаковым order_id оплатить не удастся. Если нет необходимости определять каждый заказ, то значение order_id нужно сделать равным 0. Максимальная длина 64 символа. Для рекуррентных платежей длина >=6 символов | 100001
comment | 1.0, 2.0 | Комментарий платежа. Вы можете передавать через него любую свою информацию. Информация переданная в данном параметре не отображается на странице оплаты и может использоваться для внутренних нужд магазина.	| Текстовое поле, не более 512 символов
invoice_data | 1.0, 2.0 | Данные в формате json для фискального чека (см. API для АТОЛ)	| htmlspecialchars JSON
custom_fields | 1.0, 2.0 | Опциональный параметр. Предназначен для передачи дополнительной информации в различные каналы оплаты |	urlencoded словарь JSON
check* | 2.0 | Подпись версии 2.0 – электронная подпись запроса. См. [приложение №1](http://example.com/developers) | Обязательна передача параметра version=’2.0’ и service_id. Параметр key в данном случае не требуется.
service_id | 2.0 | Идентификатор сервиса, обязательно для версии 2.0 | 121233
version | 2.0 | Строка. Обязательно для установки версии API, отличного от 1.0. Если не задано используется версия API 1.0.	2.0 |
check* (устаревшее) | 1.0	| MD5 хеш от параметров: key + cost + name + email + order_id + comment + payment_type + secret_key |

<aside class="notice">
*Принудительная проверка подписи активируется администратором РФИ в настройках сервиса магазина

Для рекуррентных операций необходимо передавать дополнительные поля, см описание [работы рекуррентных платежей](http://dsd).
</aside>

## Кнопка оплаты
## Кнопка оплаты с переменными
## Короткая ссылка (создание заказа)
### Создание ссылки оплаты в личном кабинете

### Создание ссылки оплаты используя API


### Как это работает

С помощью короткой ссылки Вы можете слать своим клиентам счета для оплаты через свой Twitter, Facebook или email-рассылку напрямую. Вот как это может выглядеть:

При переходе по ссылке клиент будет видеть стандартное окно оплаты RFI e­Commerce.

## QR-код

С помощью RFI e­Commerce Вы можете создать специальный QR код для оплаты. Основное достоинство QR­кода — легкое распознавание сканирующим оборудованием (в том числе и фотокамерой мобильного телефона), что дает возможность использования в торговле, производстве, логистике.

Из­за удобства применения QR­коды набирают популярность и их можно видеть в обычной жизни всё чаще. Большинство современных смартфонов по умолчанию или с помощью специальных приложений могут распознать QR­код. 

Вы создаёте QR и можете использовать его в печатной рекламе или на сайте. Пользователь с помощью телефона распознает код и перейдёт по зашитой ссылке на адаптированное для мобильных телефонов окно оплаты RFI e­Commerce.

Зайдите на вкладку “Сервисы” в Личном кабинете RFI e­Commerce. Нажмите на кнопку «Создать кнопку» напротив уже действующего сервиса.Нажмите ссылку «Создать QR­код». Теперь введите название товара и стоимость (дополнительные параметры скрыты значком “+”) и нажмите кнопку «Создать код». После этого сформируется HTML код картинки с QR кодом.

Ссылку для изображения QR­кода также можно получить через API: `https://partner.rficb.ru/alba/build_link/input_qr/` следует передать все параметры, которые пойдут в URL инициации платежа (`/alba/input`). Ответ будет в формате JSON. Если все данные указаны верно, то будет status=”ok”, а в “url” будет URL изображения QR­кода. Описание доступных параметров тут.

## Виджет

```html
<script type="text/javascript" src="https://partner.rficb.ru/gui/js/jquery-1.11.0.min.js"></script>
<script type="text/javascript" src="https://partner.rficb.ru/gui/js/widget_simplebutton.js"></script>
<form method="POST" class="application alba_widget_simplebutton" accept-charset="UTF-8" action="https://partner.rficb.ru/alba/input/">
   <input type="hidden" name="key" value="SpJ81OgVy1tzurrB8OwY44Qb3H0xuXCzt0iZrnrkWVs=" />
   <input type="hidden" name="cost" value="100" />
   <input type="hidden" name="name" value="Тест" />
   <input type="hidden" name="email" value="" />
   <input type="hidden" name="order_id" value="0" />
   <input type="image" id="a1lite_button" style="border: 0;" src="https://partner.rficb.ru/gui/images/a1lite_buttons/button_small.png" value="Оплатить" />
</form>
```

Если Вы хотите, чтобы пользователь совершал оплату без перехода на платёжную форму RFI e­Commerce, Вы можете встроить себе на сайт платёжный виджет.

Для того чтобы создать виджет Вам достаточно включить соответствующий чек-бокс в настройках в процессе создания кнопки оплаты.

Получим код в котором в отличии от стандартного появятся две дополнительные строки, добавляющие java скрипт на страницу:

# Направление пользователя на определенный способ оплаты.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

