---
layout: serenya_post
title:  "Записи после первого погружения в postgresql"
categories: [postgresql]
---

Занимался используя следующие ресурсы: 
* [Теория и чуть-чуть практики](https://postgrespro.ru/education/courses) Больше всего понравился [https://postgrespro.ru/education/courses/DBA1](dba1)
* Практика. 
    Практиковался на заданиях из курса который проводился в одной большой компании. База данных там используется общедоступная про рейсы самолётов (есть на хабре)

# Вот основные моменты которые бы хотел в виде ключевых слов оставить здесь

Информацию лучше буду усваивать слоями, глубже и глубже. Это пока первый слой.

1. Логический уровень - схемы
	- **pg_class, pg_database, pg_namespace, pg_table_namespace**. 
	    Это основные таблицы на основе них и некоторых других строятся более human-читаемые представления. pg_tables и др.

1. Физический уровень 
    - делается через namespace цель - вынести одни таблицы на быстрые диски, или надежные, с др. файловой системой и проч.

1. Функции
	- set echo_hidden on
	- поиск функция \df *.*<search_name>*
	- уметь посмотреть представление или таблица. Посмотреть запрос представления.
	- txid_current(), pg_typeof()
	- автокомплитв psql - натсроить

1. Транзакции
	- MVCC 
	    - на каждую транзакцию создаётся своя версия данных, они не удаляются после завершения, а ждут очистки
		-  vacuum - очищает уже неиспользуемые MVCC данные. За него отвечают 2 процесса launcher autovacum; worker autovacum;
		   Запуск воркера очистки не по времени, и не попамяти, у него есть свой более умный алгоритм.
    - 4 уровня изоляции: R UNCOM; R COM; REPEAT READ, SERIALIZEABLE;
        4 аномалии. 1) грязное чтение 2) UNREPEATED READ (изменения через update) 3) lost update  4) fantom read (изменения через insert)
        Хорошо представлены [https://postgrespro.ru/docs/postgrespro/9.5/transaction-iso](в таблице), когда какие возможны.ы 

1. Блокировки
	на уровне строк / таблиц (редко, когда меняют таблицу)
	
1. Настройки
    есть два представления pg_settings (установленные значения) , и pg_file_settings (только значения переопределенные через файлы) 
    значения можно задавать в рамках сессии.
     
1. Анализ запросов
    Влияние на план через параметры
        - Query Tuning / Planner Method Configuration
	
1. В качестве дополнения 
    - нужно знать при каких миграциях postgresql блокирует таблицы: хороший доклад на эту тему.


Практика:
    - показать размер базы, таблицы. Показать вся ли таблицы хранится в обычной памяти, может что-то в toast.
    - показать oid таблицы, oid каждой строчки. По oid показать физический файл
    - создать таблицу с миллионом строк, Сделать запрос который использует 1) SeqScan 2) IndexScan 3) Bitmap Scan
        Принудительно очистить кэши. чтобы каждый запрос был с 0 (shared hit=0)
    - создать функциональный индекс, потому что много значений null (и их не нужно хранить)
    - Добиться использования каждого способа в планировщике запроса enable_seqscan и т.д.)(query optimizer, runtime-config-query)

