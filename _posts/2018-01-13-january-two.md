---
layout: serenya_post
title:  "2019, вторая неделя января"
categories: [postgresql, achievements-diary, django, vuejs]
---

1. <a href='#django'>Django code: core.serializers; core.cache;</a>
2. <a href='#xor'>Заметки про xor</a>

### <span name='django'>Django code: core.serializers; core.cache;</span>
Отмечу общий принцип организации кода в base.py описывается базовый класс (частично реализованный, но большинство методов NotImplemented) исключения и некоторые функции
django.core.serializers.DjangoJSONEncoder добавляет типы данных (Date, Time, DateTime, timedelta, UUID, Decimal, Promise, CallableBool)

В base.Serializer реализован метод serializer внутри которого start_obj/end_obj. Эти вызовы в цикле оборачивают работу с объектом.
Основной принцип, что start_obj - добавляет _current_obj переменную, end_obj - делает её None.
В core.case.base есть `DEFAULT_TIMEOUT = object()` - используется для передачи по умолчанию и сравнения с ним. Называется stub класс - заглушка.

`django.utils:` strconv; datastructs (`ImmutableList, MultiDict, OrderedSet`); `cache` (Vary, add_headers, max_cache_time); encoding (`force_bytes, smart_bytes` нужно еще читать);

### <span name='xor'>Заметки про xor</span>
XOR - шифрование самое простое - побитовое сложение 
1) Попытался сделать xor для ascii символов в python, получаются не abcdef... Алфавит должен быть только из букв

2) Ключ должен выбираться по максимальному расстоянию между буквами
3) Просто взламывается, нужно научиться взламывать его.