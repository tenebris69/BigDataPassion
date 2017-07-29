+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-07-29T13:18:41+02:00"
description = "Zacznij swoją przygodę z Apache Spark"
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Apache Spark - Szybki start"
type = "post"

+++

# Pobranie Apache Spark

Sparka najlepiej pobrać ze strony projektu: http://spark.apache.org/downloads.html. Ja skorzystam z wersji 2.2.0 przystosowanej do pracy z Apache Hadoop w wersji 2.7 lub wyższej.
Po ściągnięciu należy wypakować w dowolnym katalogu na dysku.

Spark działa na systemach z rodziny Windows oraz Unix (Linux, MacOS, etc.). Ja skorzystam z systemu Linux Mint.

Do działa Sparka będzie wymagana Java 8+, Python 2.7+/3.4+ i R 3.1+ oraz Scala 2.11 (ze względu na kompatybilność musimy użyć dokładnie tej wersji 2.11.x).

# Uruchomienie

Po rozpakowaniu ściągniętej paczki, wchodzi do utworzonego katalogu i w konsoli wykonujemy polecenie:

~~~shell
cd spark-2.2.0-bin-hadoop2.7/
./bin/run-example SparkPi 10
~~~

Jeśli wszystko poszło tak jak trzeba w wyniku otrzymany stos logów gdzie pod koniec powinniśmy zobaczyć taką wiadomość:

> Pi is roughly 3.1416831416831417

Uruchomiliśmy jeden z przykładowych algorytmów zaimplementowanych dla Apache Spark wyliczający liczbę PI.

Gdybyśmy chcieli uruchomić powyższy przykład wykorzystując zamiast języka *Scala* język *Python* musimy w konsoli wpisać:

~~~shell
./bin/spark-submit examples/src/main/python/pi.py 10
~~~

otrzymując w wyniku:

> Pi is roughly 3.144064

Podobnie można postąpić w przypadku języka R:

~~~ shell
./bin/spark-submit examples/src/main/r/dataframe.R
~~~

(Uwaga, Api dla języka R jest wciąż oznaczone jako eksperymentalne i zawiera tylko podzbiór możliwości Sparka, ale o różnicach w API dla różnych języków w innym poście.)

# Uruchomienie w trybie interaktywnym

W poprzednim przykładzie zadanie zostało uruchomienie w sparku za pomocą skryptu *spark-submit* (https://spark.apache.org/docs/latest/submitting-applications.html), czyli nasze zadanie (w tym przypadku zadanie z przykładów) zostało w całości wysłane do Sparka i przez niego uruchomione.

Istnieje jednak możliwość uruchomienia Sparka lokalnie w trybie interaktywnym, gdzie Spark linijka po linijce będzie wykonywał nasze polecenia. Tryb ten jest tak naprawdę modyfikacją trybu interaktywnego języka Scala. 

Aby włączyć tryb interaktywny dla języka *Scala* należy w konsoli wykonać polecenia:
~~~shell
./bin/spark-shell --master local[2]
~~~

dla języka Python:
~~~shell
./bin/pyspark --master local[2]
~~~

zaś dla języka R:
~~~shell
./bin/sparkR --master local[2]
~~~

(Uwaga, język *Java* dopiero od wersji 9 udostępnia tryb interaktywny, dlatego nie jest on jeszcze dostępny w Sparku, niemniej język Scala jest kompatybilny z językiem Java, więc z powodzeniem można użyć tego trybu do pracy z językiem Java).

Parametr "master" pozwala nam zdefiniować w ilu wątkach będzie uruchomiony Spark (local[2] oznacza dwa wątki) a także możliwość podłączenia się do uruchomionego wcześniej Sparka (https://spark.apache.org/docs/latest/submitting-applications.html#master-urls).


# Panel WWW Spark'a

Po uruchomieniu Spark'a (także w trybie interaktywnym) możemy wejść na jego interfejs graficzny dostępny pod adresem http://localhost:4040.










# Legenda
* http://spark.apache.org
* http://spark.apache.org/downloads.html
* https://spark.apache.org/docs/latest/submitting-applications.html
* https://spark.apache.org/docs/latest/submitting-applications.html#master-urls

