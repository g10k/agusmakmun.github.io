---
layout: serenya_post
title:  "2019, 4 неделя января"
categories: [postgresql, achievements-diary, django, vuejs]
---

1. <a href='#django'>Django</a>
2. <a href='#python_exception'>Правильный re-raise</a>
3. <a href='#cipher'>Заметки про шифрование</a>
4. <a href='#cipher'>Заметка про postgres и статью maintenance_work_mem</a>

### <span name='django'>Django массовый update одним запросом</span>
#### Массовый update одним запросом.
Допустим имеем табличку Student(id, count)
И большой список пар [(<student_id>, <count>),] которыми нужно обновить таблицу.
Наиболее часто практикуемый способ сделать это через JOIN c временной таблицей temp_table(id, count)

update Student as S set S.count =TMP.count where S.id= inner join temp_table TMP where S.id =TMP.id;
  
В django 2 - говорят есть bulk_update который делает один запрос.
Но если у нас не django 2, то можем сделать красивую обертку с помощью case, when, value функций.

whens = [When(id=c_id, then=Value(v)) for c_id, v in balance_by_customer.items()]
Customer.objects.filter(
    id__in=balance_by_customer.keys()
).update(
    balance=Case(*whens)
)

Получим такой запрос

UPDATE "customer"
SET "balance" = CASE
                    WHEN "customer"."id" = '1' THEN 600.00
                    WHEN "customer"."id" = '2' THEN 400.00
                    WHEN "customer"."id" = '3' THEN 200.00
                    ELSE NULL
                END
WHERE "customers_customer"."id" IN (1,2,3)
 


### <span name='python_exception'>Правильный re-raise</span>
Наткнулся на практике со случаем, приходит в sentry ошибка - а в трейсбеке нет информации об оригинальной ошибке. 
Случается когда какой-то код оборачивается в try/except и в except блоке еще вызывается raise, но без сохранения traceback первого исключения.

# После Reraise                                                         
# 1) - можем видеть только место reraise
# 2) - можем видеть оригинальный Exception (и не видеть место reraise)
# 3) - можем видеть оба места.      

#### В Python2/Python3

# 2 - только место reraise (1)
# 3 - есть все (3)

class MyException(Exception):
    pass

def some(a,b):
   if a==b:
       raise MyException('fuck, %s=%s' % (a,b))
   elif a==10:
       raise MyException('fuck2, %s=10' % a)

try:
   some(3,4)
   some(5,5)
except MyException as ex:
   print('Myexception handled')
   raise ex



#### В Python2/Python3   
# нет информации о мeсте откуда ReRaise, есть только оригинальный

class MyException(Exception):
    pass

def some(a,b):
   if a==b:
       raise MyException('fuck, %s=%s' % (a,b))
   elif a==10:
       raise MyException('fuck2, %s=10' % a)

try:
   some(3,4)
   some(5,5)
except MyException as ex:

   print('MyException handled')
   raise

################################
#### В Python2/Python3   
# есть и то и другое
import sys, six
class MyException(Exception):
    pass

def some(a,b):
   if a==b:
       raise MyException('fuck, %s=%s' % (a,b))
   elif a==10:
       raise MyException('fuck2, %s=10' % a)

try:
   some(3, 4)
   some(5, 5)
except MyException as ex:
   t,v,tb= sys.exc_info()
   print('MyException handled')
   raise t,v, tb




### <span name='cipher'>Заметки про шифрование</span>

Обзор модулей: 
django.utils.crypto / salted_hmac, pbkdf2, constant_time_compare /  salted_hmac -> django.core.signing; pbkdf2 -> django.contrib.auth.hashers
django.core.signing
    - dumps/loads
    - Signer, TimestampSigner
django.contrib.auth.hashers
    - TO BE CONTINUED
    
#### django.utils.crypto
constant_time_compare - сравнивает две строки всегда за одинаковое время. 
Есть способы взлома, когда по времени проверки находят начальные символы строки.
Пример пароль - имеет длину 40 символов и начинается на 'zae'
Проверяем 'a<39chars>' - неверно за 1мс Перебираем первую букву, все неверные и за 1мс; доходим до z
Проверяем 'z<39chars>' - неверно но за 2мс. -> значит первая буква z


#### django.core.signing
В django.core.signing  - есть 2 функции dumps/loads. Они позволяет закодировать json объект и зашифровать его. А потом читать с проверкой, что он не был изменен.
Используется в django/contrib/sessions/signed_cookies когда данные сессии лежат прямо в ключе сессии и не используется внешнего хранилища.

Основной механизм. <base64(value)><separator><salted_hmac(base64(value))> - примерно так: "54b108db-77f4-480b-b10d-4b94daff3e0a:1gmf2v:8-PAhK2aL1XZFOSIhS_ovcLksC4" 

По умолчанию используется django.core.signing.TimestampSigner - который внутрь данных записывает еще время. При проверке loads можно указать максимальное время жизни, и тогда
если это время уже прошло, то loads неуспешен.

