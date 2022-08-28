---

layout: col-document
title: WSTG - Latest
tags: WSTG

---

{% include breadcrumb.html %}
# Тестирование учётных данных по умолчанию

|ID          |
|------------|
|WSTG-ATHN-02|

## Обзор

Многие web-приложения и аппаратные устройства имеют пароли по умолчанию для встроенной учётной записи администратора. Хотя в некоторых случаях они и могут быть сгенерированы случайным образом, чаще они статичны, что означает, что их можно легко угадать или выяснить злоумышленнику.

Кроме того, когда в приложениях создаются новые пользователи, для них могут устанавливаться предопределённые пароли. Они могут быть либо сгенерированы автоматически приложением, либо созданы персоналом вручную. В обоих случаях, если они не генерируются безопасным способом, злоумышленник может их угадать.

## Задачи тестирования

- Определить, есть ли в приложении какие-либо учётные записи пользователей с паролями по умолчанию.
- Проанализировать, не создаются ли новые учётные записи пользователей со слабыми или предсказуемыми паролями.

## Как тестировать

### Тестирование на учётные данные по умолчанию от поставщика

Первым шагом к выявлению паролей по умолчанию является определение используемого программного обеспечения. Это подробно описано в разделе [Сбор информации](../01-Information_Gathering/README.md) данного Руководства.

Как только программное обеспечение будет идентифицировано, попробуйте выяснить, использует ли оно пароли по умолчанию, и если да, то какие. Включая:

- Поиск по "[название приложения] default password".
- Просмотр руководства или документации поставщика.
- Проверка распространённых баз данных паролей по умолчанию, таких как [CIRT.net](https://cirt.net/passwords), [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials) или [DefaultCreds](https://github.com/ihebski/DefaultCreds-cheat-sheet/blob/main/DefaultCreds-Cheat-Sheet.csv).
- Анализ исходного кода приложения (если таковой имеется).
- Установка приложения на виртуальную машину и его изучение.
- Осмотр оборудования на наличие наклеек (часто присутствующих на сетевых устройствах).

Если пароль по умолчанию не нашёлся, попробуйте наиболее распространённые варианты, например:

- admin, password, 12345, или другие [распространённые пароли по умолчанию](https://github.com/nixawk/fuzzdb/blob/master/bruteforce/passwds/default_devices_users%2Bpasswords.txt);
- без пароля;
- серийный номер или MAC-адрес устройства.

Если имя пользователя неизвестно, существуют различные варианты перебора имён, описанные в разделе [Перебор и угадывание учётных записей пользователей](../03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account.md). В качестве альтернативы попробуйте распространённые варианты, такие как Администратор, admin, root, или system.

### Тестирование на пароли по умолчанию для организации

Когда работники организации вручную создают пароли для новых учётных записей, они могут делать это предсказуемым образом. Это часто может быть:

- Один общий пароль, например, Пароль1.
- Сведения, относящиеся к этой организации, такие как её название или адрес.
- Пароли, которые следуют простому шаблону, например, Понедельник123, если учётная запись создана в понедельник.

Эти типы паролей часто трудно найти методом чёрного ящика, если только они не угаданы или взломаны методом перебора. Тем не менее, их легко выявить при тестировании методом серого или белого ящика.

### Тестирование паролей по умолчанию, сгенерированных приложением

Если приложение автоматически генерирует пароли для новых учётных записей пользователей, они также могут быть предсказуемыми. Чтобы протестировать их, создайте в приложении сразу несколько учётных записей с одинаковыми данными и сравните пароли, которые для них сформированы.

Пароли могут быть основаны на:

- одной статичной строке, общей для всех учётных записей;
- хэшированной или обфусцированной части данных учётной записи, например, `md5($username)`;
- алгоритме с использованием времени;
- некриптостойком генераторе псевдослучайных чисел (PRNG).

Такого рода проблемы часто трудно идентифицировать с точки зрения чёрного ящика.

## Инструменты

- [Burp Intruder](https://portswigger.net/burp/documentation/desktop/tools/intruder)
- [THC Hydra](https://github.com/vanhauser-thc/thc-hydra)
- [Nikto 2](https://www.cirt.net/nikto2)

## Ссылки

- [CIRT](https://cirt.net/passwords)
- [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials)
- [DefaultCreds](https://github.com/ihebski/DefaultCreds-cheat-sheet/blob/main/DefaultCreds-Cheat-Sheet.csv)