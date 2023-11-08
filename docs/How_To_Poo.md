#Быдлоперевод

Переводим английский текст в  **item** и **desc** сохраняя синтаксис.


#Адвансед перевод:

##Проки для перевода:

- **to_chat**(userref, “ENGLISH_TEXT”) - то, что выводится пользователю. Пример:

```html
to_chat(user, "<span class='notice'>You slot \the **[TC]** into **[parent]** and charge its internal uplink.</span>")
```

То, что закрыто в квадратные скобки - **[ПЕРЕМЕННАЯ]**, является переменной и не требует перевода.\
Они могут быть переставлены или заменены при надобности, если это может улучшить перевод. Всё остальное можно смело переводить (кроме HTML-тегов, естественно).

 Также, макросы по типу **\the** или **\a** должны ПОЛНОСТЬЮ отсутствовать в конечном переводе. Они не используются.

**Пример перевода:**

```html
 to_chat(user, "<span class='notice'>Вставляю [TC] в [parent] заряжая его внутренний передатчик.</span>")
```

- **visible_message**(“СООБЩЕНИЕ”, “СООБЩЕНИЕ ПОЛЬЗОВАТЕЛЮ (ЕСЛИ ТОТ УКАЗАН)”, “СООБЩЕНИЕ ДЛЯ СЛЕПЫХ”, …) - то, что выводится всем пользователям в радиусе. Пример:


```html
target_turf.visible_message("<span class='warning'>[target_turf] collapses under its own weight into a puddle of goop and undigested debris!</span>")
```

В некоторых случаях придётся использовать процедуру **capitalize(msg)**. Она заставляет текст внутри себя писаться с большой буквы.

**Пример перевода:**
```html
target_turf.visible_message("<span class='warning'><b>[capitalize(target_turf)]</b> рушится под собственным весом в лужу из слизи и рыхлого мусора!</span>")
```


Тут стоит заметить, что был добавлен тег ```</b> </b>```. Его стоит использовать тоже по возможности, так как он делает часть текста более заметной для акцентирования внимания пользователя на важном.

Сообщения из проков **examine(mob/user)**.
То, что пользователь видит при осмотре той или иной штуки. Пример:
```cpp
/obj/item/pinpointer/wayfindin/gexamine(mob/user)
	. = ..()
	. += "<hr>"
	var/msg = "Its tracking indicator reads "
	if(target)
		var/obj/machinery/navbeacon/wayfinding/B  = target
		msg += "\"[B.codes["wayfinding"]]\"."
	else
		msg = "Its tracking indicator is blank."
	if(owner)
		msg += " It belongs to [owner]."
	. += msg
```
Переменная есть объект наследования. Если проще, то это дополняет часть вывода основной процедуры **examine()**

 **Пример перевода:**
```cpp
/obj/item/pinpointer/wayfinding/examine(mob/user)
	. = ..()
	. += "<hr>"
	var/msg = "Отслеживающий индикатор сообщает "
	if(target)
		var/obj/machinery/navbeacon/wayfinding/B  = target
		msg += "\"[B.codes["wayfinding"]]\"."
	else
		msg = "Отслеживающий индикатор пуст."
	if(owner)
		msg += " Принадлежит [owner]."
	. += msg.
```

Тут есть особенности.
Макросы **\n** не стоит удалять из текста, так как они отвечают за перенос строки. Если их не будет, то текст станет выглядеть как каша.

Ещё существует прок **examine_more()** и подразумевает собой более детальный осмотр той или иной вещи. Он переводится аналогично.

- Прок **say(“СООБЩЕНИЕ”)**. Пример:

```html
say("<span class='robot'>Thank you for recycling, [user.first_name()]! Here is [rfnd_amt ? "[rfnd_amt] credits." : "a freshly synthesized costume!"]</span>")
```


Здесь можно наблюдать особенную переменную с двумя вариантами:\
> [rfnd_amt ? **"[rfnd_amt] credits."** : **"a freshly synthesized costume!"**].

Она работает очень просто. Сначала идёт переменная, которая проверяется, это у нас **[rfnd_amt]**, если она не **0**, то выводится первый вариант сообщения **"[rfnd_amt] credits."**, иначе же второй **"a freshly synthesized costume!".**

**Пример перевода:**

```html
say("<span class='robot'>Спасибо за переработку, [user.first_name()]! Получи [rfnd_amt ? "[rfnd_amt] кредитов." : "свежий синтезированный костюмчик!"]</span>")
```

**Важно**, что в процедурах **say()** не должно быть **HTML-тегов** (возможны исключения как в этом примере, учтите).

Новые вводные.

Подобные вещи -
```cpp
to_chat(creator,"<span class="warning">No choice ... </span>")
```
исправляем на

```cpp
to_chat(creator, span_warning("No choice selected. The area remains undefined."))
```


