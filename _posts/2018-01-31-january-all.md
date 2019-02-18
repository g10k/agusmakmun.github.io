---
layout: serenya_post
title:  "2019, 4 неделя января"
categories: [postgresql, achievements-diary, django, vuejs]
---

1. <a href='#django'>Django</a>
2. <a href='#python_exception'>Правильный re-raise</a>
3. <a href='#cipher'>Заметки про шифрование</a>
4. <a href='#cipher'>Заметка про postgres и статью maintenance_work_mem</a>

### <span name='django'>Django</span>
#### Массовый update одним запросом.
Допустим имеем табличку Student(id, count)
И большой список пар [(<student_id>, <count>),] которыми нужно обновить таблицу.
Наиболее часто практикуемый способ сделать это через JOIN c временной таблицей temp_table(id, count)

update Student as S set S.count =TMP.count where S.id= inner join temp_table TMP where S.id =TMP.id;
  
В django 2 - говорят есть bulk_update который делает один запрос.
Но если у нас не django 2, то можем сделать красивую обертку с помощью case, when, value функций.
 
КАк правильно делать raise

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




