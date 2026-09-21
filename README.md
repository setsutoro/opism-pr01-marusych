# Практична робота № 1

**Дисципліна:** Основи побудови інформаційних систем та мереж

**Тема:** Спостереження за процесом звернення до вебресурсу. Побудова власної моделі рівнів взаємодії

| | |
|---|---|
| **Прізвище, ім'я** | Марусич Артем |
| **Група** | ІПЗ-2.01 |
| **Номер варіанта** | 19 |
| **Домен варіанта** |  cmake.org |
| **Середовище виконання** |  Windows  |
| **Версія curl** | curl 8.13.0 (Windows) libcurl/8.13.0 Schannel zlib/1.3.2 WinIDN WinLDAP |
| **Дата виконання** |21.09.2026 |

---

## Частина A. Збір експериментальних даних

### A.1. Запит із діагностичним виводом

**Команда:**

```
curl.exe -v https://cmake.org
```

**Вивід:**

```
* Host cmake.org:443 was resolved.
* IPv6: (none)
* IPv4: 50.58.123.185
*   Trying 50.58.123.185:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Connected to cmake.org (50.58.123.185) port 443
* using HTTP/1.x
> GET / HTTP/1.1
> Host: cmake.org
> User-Agent: curl/8.13.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 200 OK
< Date: Sun, 20 Sep 2026 23:04:42 GMT
< Server: Apache
< Strict-Transport-Security: max-age=31536000; includeSubDomains
< Strict-Transport-Security: max-age=15552000; includeSubDomains
< Last-Modified: Sun, 20 Sep 2026 17:02:17 GMT
< Vary: Accept-Encoding
< X-Frame-Options: sameorigin
< Content-Type: text/html; charset=UTF-8
< X-Content-Type-Options: nosniff
< Transfer-Encoding: chunked

<!doctype html>
<html lang="en-US">
<head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1" />

    <link rel='dns-prefetch' href='//www.google.com' />
```


---

### A.2. Запит без захисту з'єднання

**Команда:**

```
curl.exe -v http://cmake.org
```

**Вивід:**

```
* Host cmake.org:80 was resolved.
* IPv6: (none)
* IPv4: 50.58.123.185
*   Trying 50.58.123.185:80...
* Connected to cmake.org (50.58.123.185) port 80
* using HTTP/1.x
> GET / HTTP/1.1
> Host: cmake.org
> User-Agent: curl/8.13.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 301 Moved Permanently
< Date: Sun, 20 Sep 2026 23:16:51 GMT
< Server: Apache
< Location: https://cmake.org/
< Content-Length: 226
< Content-Type: text/html; charset=iso-8859-1
<
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://cmake.org/">here</a>.</p>
</body></html>
* Connection #0 to host cmake.org left intact
```
---

### A.3. Запит до служби доменних імен

*Windows: `Resolve-DnsName ВАШ_ДОМЕН`*

**Команда (перше виконання):**

```
Resolve-DnsName cmake.org
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
cmake.org                                      A      436   Answer     50.58.123.185
```
**Команда (повторне виконання через 5–7 хвилин):**

```
Resolve-DnsName cmake.org
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
cmake.org                                      A      80    Answer     50.58.123.185
```

**Зафіксовані значення:**

| Параметр | Перше виконання | Повторне виконання |
|---|---|---|
| Час виконання (год:хв) | 2:26| 2:33|
| IP-адреса |50.58.123.185 | 50.58.123.185|
| Значення TTL | 436 | 80|

---

### A.4. Контрольний ресурс

**Команда:**

```
curl.exe -v https://google.com
```

**Вивід:**

```
* Host google.com:443 was resolved.
* IPv6: (none)
* IPv4: 142.250.109.100, 142.250.109.101, 142.250.109.139, 142.250.109.138, 142.250.109.102, 142.250.109.113
*   Trying 142.250.109.100:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Connected to google.com (142.250.109.100) port 443
* using HTTP/1.x
> GET / HTTP/1.1
> Host: google.com
> User-Agent: curl/8.13.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 301 Moved Permanently
< Location: https://www.google.com/
< Content-Type: text/html; charset=UTF-8
< Content-Security-Policy-Report-Only: object-src 'none';base-uri 'self';script-src 'nonce-V7_yaPuN7HRHaN2vnNdmWQ' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp
< Date: Sun, 20 Sep 2026 23:36:29 GMT
< Expires: Tue, 20 Oct 2026 23:36:29 GMT
< Cache-Control: public, max-age=2592000
< Server: gws
< Content-Length: 220
< X-XSS-Protection: 0
< X-Frame-Options: SAMEORIGIN
< Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
<
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="https://www.google.com/">here</A>.
</BODY></HTML>
* Connection #0 to host google.com left intact
```


---

### A.5. Ресурси з некоректною конфігурацією сертифіката

**Випадок 1**

**Команда:**

```
curl.exe -v https://expired.badssl.com
```

**Вивід:**

```
* Host expired.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - Получен сертификат с истекшим сроком действия.
* closing connection #0
curl: (35) schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - Получен сертификат с истекшим сроком действия.
```

**Випадок 2**

**Команда:**

```
curl.exe -v https://wrong.host.badssl.com
```

**Вивід:**

```
* Host wrong.host.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - Главное конечное имя неверно.
* closing connection #0
curl: (60) schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - Главное конечное имя неверно.More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

**Випадок 3**

**Команда:**

```
curl.exe -v https://self-signed.badssl.com
```

**Вивід:**

```
* Host self-signed.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - Цепочка сертификатов выпущена центром сертификации, не имеющим доверия.
* closing connection #0
curl: (60) schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - Цепочка сертификатов выпущена центром сертификации, не имеющим доверия.
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

---

## Частина B. Власна модель рівнів

**Кількість виділених груп:** 4

| № | Назва групи (власне формулювання) | Рядки виводу, віднесені до групи | Обґрунтування |
|---:|---|---|---|
| 1 | **Передача даних** | GET / HTTP/1.1<br>&gt; Host: cmake.org<br>&gt; User-Agent: curl/8.13.0<br>&gt; Accept: */*<br>&lt; HTTP/1.1 200 OK<br>&lt; HTTP/1.1 301 Moved Permanently<br>&lt; Location: https://cmake.org/ | Відповідає за обмін HTTP-повідомленнями між клієнтом і сервером: формування запиту, його надсилання та отримання відповіді.. |
| 2 | **Захист з'єднання** | schannel: disabled automatic use of client certificate<br>ALPN: curl offers http/1.1<br>ALPN: server accepted http/1.1<br>SEC_E_CERT_EXPIRED<br>SEC_E_WRONG_PRINCIPAL<br>SEC_E_UNTRUSTED_ROOT | Охоплює механізми встановлення захищеного HTTPS-сеансу та перевірки відповідності й надійності сертифіката сервера. |
| 3 | **Пошук сервера** | Host cmake.org:443 was resolved.<br>Host cmake.org:80 was resolved.<br>Host google.com:443 was resolved.<br>IPv4: 50.58.123.185<br>IPv4: 142.250.109.100, ...<br>Name: cmake.org<br>Type: A<br>TTL: 436<br>TTL: 80<br>IPAddress: 50.58.123.185 | Містить етап визначення мережевої адреси за доменним ім'ям, що дозволяє клієнту знайти потрібний сервер у мережі. |
| 4 | **Мережеве підключення** | Trying 50.58.123.185:443...<br>Trying 50.58.123.185:80...<br>Connected to cmake.org (50.58.123.185) port 443<br>Connected to cmake.org (50.58.123.185) port 80<br>using HTTP/1.x<br>Request completely sent off<br>Connection #0 to host cmake.org left intact | Описує встановлення та підтримання мережевого каналу між клієнтом і сервером, через який надалі відбувається обмін інформацією. |


**Рядки, які не вдалося віднести до жодної групи:**

| Рядок виводу | Причина утруднення |
|---|---|
| | |
| | |
| | |

---

## Контрольні питання

**1. Скільки рядків діагностичного виводу передує отриманню даних сторінки (завдання A.1)?**

```
**33 рядки діагностичного виводу.**
```

**2. Які рядки наявні у виводі A.1 і відсутні у виводі A.2? Чим це зумовлено?**

```
**У виводі A.1 наявні рядки:**
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Connected to cmake.org (50.58.123.185) port 443

Це зумовлено тим, що в A.1 використовується HTTPS, яке перед передаванням HTTP-запиту встановлює захищене TLS з’єднання.
```

**3. Звідки у виводі з'явилося значення `443`, якщо його не було вказано в адресі?**

```
Значення 443 з’явилося автоматично, оскільки це стандартний порт для HTTPS-з’єднань.
```

**4. Як змінилося значення TTL між двома запитами (A.3)? Що означає це число?**

```
Значення TTL зменшилося з 436 до 80 секунд. TTL показує, скільки часу запис DNS може залишатися в кеші до його оновлення.
```

**5. Чим відрізняються між собою три причини помилок із завдання A.5? Сформулювати кожну однією фразою.**


| Випадок | Причина помилки |
|---|---|
| `expired` | Був вичерпаний термін дії цього сертифікату. |
| `wrong.host` | Ім'я хоста не відповідає імені, зазначеному в SSL-сертефікаті.|
| `self-signed` | Цей сертифікат підписанний недовіренним центром сертифікації.|

**6. Три рядки з власних виводів, про які не йшлося на лекції 1:**

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | schannel: disabled automatic use of client certificate |А.1|
| 2 | ALPN: server accepted http/1.1 |А.1|
| 3 | < Strict-Transport-Security: max-age=31536000; includeSubDomains |A.1|

---

## Висновки

*150–300 слів. Спиратися на власні спостереження, а не на матеріал лекції.*

**D.1. Що виявилося неочевидним або несподіваним**

*Назвати конкретно, з посиланням на рядок виводу.*

> У пункті A3 я звернув увагу на TTL: за кілька хвилин він змінився з 436 до 80, хоча IP-адреса залишилася тією самою. За межами роботи, після повторних запитів вже був 0. Значення після цього ніяк не змінювалося.

**D.2. Чому саме така кількість груп у частині B**

*На якій підставі ухвалено рішення. Що змусило б його змінити.*

> Я виділив 4 групи, тому що у виводах чітко простежуються чотири різні типи операцій: пошук сервера за доменним ім'ям, встановлення мережевого з'єднання, захист HTTPS та обмін HTTP-даними.


**D.3. Питання, яке залишилося без відповіді.**

> Залишилося питання, чому саме для виділенних доменів у різні моменти часу може використовуватися той самий або інший IP, і як саме DNS визначає, яку адресу повернути користувачу, якщо для домену доступно кілька адрес


---

## Використання штучного інтелекту

*Розділ обов'язковий. Заповнюється незалежно від того, чи використовувався ШІ. Детальні вимоги — у документі «Політика використання технологій штучного інтелекту».*

**Факт використання:** використано / не використано *(потрібне залишити)*

**Установлений рівень для цієї роботи:** Р3 — ШІ як співвиконавець

**Фактичний рівень використання:** Р-3 

### Використані системи

| Система | Версія або модель | Період використання |
|---|---|---|
| ChatGPT | GPT-5.6 Luna | 21.09.2026, 1:26-3:57 |

### Промпти

*Наводити дослівно, у тому вигляді, у якому запит було надано системі. Переказ не приймається.*

| № | Розділ роботи | Текст промпта |
|---|---|---|
|1| A1| Привет! Сейчас делаю практическую работу по "Основи побудови інформаційних систем та мереж" По заданию, через PowerShell я должен ввести команду, которая даст мне информацию о ссылке У других оно выдаёт по команде (пример) "curl -v https://lua.org" следующее: * Host lua.org:443 was resolved. * IPv6: (none) * IPv4: 46.175.8.47 * Trying 46.175.8.47:443... * schannel: disabled automatic use of client certificate * ALPN: curl offers http/1.1 * ALPN: server accepted http/1.1 ... И там дальше ещё куча информации Я, когда ввожу свою ссылку (curl -v https://cmake.org ), получаю это: ПОДРОБНО: GET https://cmake.org/ with 0-byte payload ПОДРОБНО: received -1-byte response of content type text/html; charset=UTF-8 Я может что то делаю не так? Можешь помочь? |
|2| A2| Короче, тогда мне в описании моей работы просто заменить версию curl с 8.21.0 на 8.13.0 , и записать кусок кода который я тебе скинул?
|3| B1| Смотри, следующее задание у меня такое: Частина B. Побудова власної моделі рівнів Використовуючи виключно власні виводи, отримані в частині A: B.1. Виділити в них окремі етапи взаємодії клієнта та сервера. B.2. Згрупувати етапи, які, на вашу думку, логічно належать разом. B.3. Упорядкувати групи від найближчої до користувача до найближчої до апаратного забезпечення. B.4. Пронумерувати групи. Кількість груп визначає студент самостійно. Єдиної правильної кількості не існує. B.5. Оформити результат за формою Додатка Б. На скрине показываю пример одногруппника. Ниже скину свои данные, которые я получил за задания от А1 до А5. Можешь оформить так же? |

### Дії з отриманим результатом

| № промпта | Що перевірено | Що змінено | Що відхилено і чому |
|---|---|---|---|
| 1 | В завданні я не одразу зрозумів що мені потрібно писати не команду curl -v https://cmake.org , а curl.exe -v https://cmake.org  | Виправив свою помилку.| Нічого не було відхилено. |
| 2 | Отримані дані у порівнянні з іншими роботами відрізнялися декількома строками, з-за різниці версій PowerShell. | Змінив у своїй роботі використану версію PowerShell.| Нічого не було відхилено, ШІ цілком виправив мою помилку.|
| 3 | Не розумів як мені правильно оформити таблицю B1-B5, бо не знайшов форму додатка Б. | Записав таблицю у вірному форматі. | Так як потрібно було описати своїми словами, а ШІ переписав слово в слово (окрім рядків вводу, віднесенних до групи), замінив дані своїми словами і з власного розуміння. Переробив назви груп, та обгрунтування.|

### Підтвердження

Підтверджую, що всі наведені в цьому звіті виводи команд отримано мною особисто внаслідок фактичного виконання відповідних дій, а відомості цього розділу є повними та достовірними.

> Виводи `curl`, `dig` та інші артефакти не можуть бути згенеровані. Це стосується будь-якого рівня використання ШІ.

---

## Примітки виконавця

*(необов'язковий розділ: що не спрацювало, які команди довелося змінити, які виникли труднощі)*
