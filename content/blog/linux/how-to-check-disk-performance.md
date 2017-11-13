+++
author = "Radosław Szmit"
categories = ["Linux"]
date = "2017-10-19T11:17:44+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Jak sprawdzić wydajność dysków twardych"
type = "post"

draft = true
+++

# iotop

Pierwszym narzędziem jest *iotop* który jest odpowiednikiem systemowego *top* tylko przeznaczonym do monitorowania zużycia pamięci trwałej.

Program działa w trybie interaktywnym obsługując kilka przydatnych skrótów:

* a - widok bieżący/chwilowy lub cała sesja (accumulated)
* o - tylko aktywne procesy
* P - pokazuj procesy a nie wątki
* strzałki w lewo i w prawo - wybranie kolumny do sortowania
* r - odwrócenie sortowania
* q - wyjście

możemy też przekazać je podczas startu:
~~~shell
iotop -aoP 
~~~