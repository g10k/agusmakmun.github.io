---
layout: serenya_post
title:  "2019, 4 неделя января"
categories: [postgresql, achievements-diary, django, vuejs]
---

1. <a href='#django'>Функция хэш с 0</a>
2. <a href='#xor'>Заметки про xor</a>

### <span name='django'>Хэш функция</span>

Попытался сделать хэш from scratch



# После Reraise                                                         
# 1) - можем видеть только место reraise
# 2) - можем видеть оригинальный Exception (и не видеть место reraise)
# 3) - можем видеть оба места.      



#### В Python2/Python3

# 2 - только место reraise
# 3 - есть все

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




