---
layout: serenya_post
title:  "2019, первая неделя января"
categories: [postgresql, achievements-diary,nginx, django, vuejs]
---

<!--This is a text with a-->
<!--footnote[^1].-->

<!--[^1]: And here is the definition.-->

1. <a href='#nginx'>Nginx ivp6, новый ssl-сертификат</a>
2. <a href='#django-salted-hmac'>Изучение django</a>
3. <a href='#vue-js-first-steps'>Первые 2 урока Vue.js</a>
4. <a href='#jekyll-markdown'>Jekyll и agusmakmuun шаблон</a>

### <span name='nginx'>Nginx ivp6, новый ssl-сертификат</span>
- не собран с ipv6, не собирается certbot, не может проверить авторизацию
- как я понимаю, nginx не может ответить на запрос ipv6, потому что nginx не поддерживает его [::]:80(443)
- legality.ru 301 редирект на pokovru.ru. Почему так могло?

```
Failed authorization procedure. www.pokovru.ru (http-01): urn:ietf:params:acme:error:connection :: The server could not connect to the client to verify the domain :: Fetching http://www.pokovru.ru/.well-known/acme-challenge/lnkZTB1JAsiZHtiHgstq_W4_YNZAh8ixQQ1kJAi7tSw: Timeout during connect (likely firewall problem)

IMPORTANT NOTES:
 - The following errors were reported by the server:

   Domain: www.pokovru.ru
   Type:   connection
   Detail: Fetching
   http://www.pokovru.ru/.well-known/acme-challenge/lnkZTB1JAsiZHtiHgstq_W4_YNZAh8ixQQ1kJAi7tSw:
   Timeout during connect (likely firewall problem)

   To fix these errors, please make sure that your domain name was
   entered correctly and the DNS A/AAAA record(s) for that domain
   contain(s) the right IP address. Additionally, please check that
   your computer has a publicly routable IP address and that no
   firewalls are preventing the server from communicating with the
   client. If you're using the webroot plugin, you should also verify
   that you are serving files from the webroot path you provided.
```

проблема должна быть распространенной. решил через reg ru которые прислали сертификат.

### <a href='#django-salted-hmac' name='django-salted-hmac'>Изучение django</a>

django.utils: encoding.force_bytes, `utils.salted_hmac`, compare_digest: 
хранится и передается `hash(data):data` для проверки, не было ли заменено содержимое, если будет заменено, хэш изменится.
 
**7 января**

`django.utils:` strconv; datastructs (`ImmutableList, MultiDict, OrderedSet`); `cache` (Vary, add_headers, max_cache_time); encoding (`force_bytes, smart_bytes` нужно еще читать);  


### <a href="#vue-js-first-steps" name='vue-js-first-steps'>Первые шаги vue.js</a>
Хороший [курс](http://js.dmitrylavrik.ru/vue/?utm=site-footer), где Дмитрий Лаврик без академичности и лишней раскачки показывает возможности vue.js

Программа курса:
```
урок 1
Подключение Vue
Идеи реактивности данных
Простой пример с переменной и v-model
Вывод данных в теги и в атрибуты
Упрощённая структура экземпляра Vue
el, data, computed, methods, watch
Основные директивы

урок 2
Жизненный цикл экземпляра Vue и его события
Служебные свойства с $
Шаблоны и свойство template
Работа с классами и стилями
Директива v-for
Подробный разбор событийной модели
Модификаторы событий

урок 3
Переход к компонентному подходу
Глобальная и локальная регистрация компонентов
Передача входных параметров
Композиция компонентов
Неудобства при описании свойства template
Переход к нормальной системе с webPack
Расположение компонентов в отдельных файлах

урок 4
Общение компонентов друг с другом
Однонаправленный поток данных
Отслеживание и порождение событий
Хранение данных в одном объекте
Передача контента слотами
Переключение компонентов
Component и Keep-alive

урок 5
Создание своих директив
Глобальные и локальные директивы
Фильтры, создание своих фильтров
Миксины: глобальные и локальные
Transition и transition-group
Жизненный цикл анимации

урок 6
Управление состоянием приложения
Осознание Flux-архитектуры
Один объект, описывающий состояние
Обновление состояния
Асинхронные действия
Vuex - паттерн управления состоянием

урок 7
Добавление асинхронности в приложение
Vue Resourse
Single Page Application
Анализ серверной части приложения
Vue Router
Роутинг с API History

урок 8
Обобщение изученного материала
Разбор финальных проектов учеников
Сборка кода в production
Vue и валидность кода
Vue в Single Page Application
Vue при встраивании в многостраничный сайт
```

**Плюсы курса**

- простая, не академичная подача
- он имеет опыт в react и может сравнить vue и react
- есть ДЗ
- скачал на комп, можно ускорять, есть исходники

**Что я усвоил:**

 * можно использовать без SPA
 * поля объектов не реактивны, нужно вручную ставить set
 * watch/methods; v-if/v-show; 
 * нужно пользоваться cheatsheet, много полезных фич.
 
Прошел пока 2 урока

    
### <a href="#jekyll-markdown">Jekyll Markdown</a>
Начал пользоваться markdown для ведения этого блога.

Непонятно с семействами пока. В [jekyll](https://jekyllrb.com/docs/configuration/markdown/) по умолчанию используется [kramdown](https://kramdown.gettalong.org/quickref.html) в проекте [agusmakmun](https://github.com/agusmakmun/agusmakmun.github.io) тоже, но с некоторым добавлением css.

- Надо найти описание этого css.
- Надо научиться добавлять кастомизированные штуки, например `max-height` окно, с кнопкой `Раскрыть`
 
