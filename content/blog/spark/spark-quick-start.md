+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-07-29T13:18:41+02:00"
description = "Zacznij swoją przygodę z Apache Spark"
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Szybki start z Apache Spark"
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

# Idea Sparka

Spark od początku istnienia opiera się na idei *Resilient Distributed Dataset* (RDD) która została po raz pierwszy przedstawiona w publikacji naukowej *Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing* autorstwa twórcy narzędzia Matei Zaharia w 2012 roku (https://cs.stanford.edu/~matei/papers/2012/nsdi_spark.pdf).

RDD to tak naprawdę niezmienna (brak edycji) rozproszona kolekcja danych, przechowywana przez węzły klastra obliczeniowego. RDD mogą być przetwarzane w sposób rozproszony i wielowątkowy, zaś podstawowymi operacjami na RDD są akcje i transformacje.

Wraz z rozwojem Spark'a oraz wzrostem jego popularności, twórcy chcieli udostępnić narzędzie szerszemu gronu użytkowników poprzez rozszerzenie API. Na wzór "data frames" dostępnych w języku R oraz Python (Pandas) twórcy Apache Spark stworzyli nowy typ zwany *DataFrame*. Podobnie do RDD, DataFrame to rozproszona kolekcja danych, jednakże dane są zorganizowane w nazwane kolumny, analogicznie jak to ma miejsce w bazach danych czy narzędziach typu Apache Hive. Dodatkowo DataFrame posiadają wiele optymalizacji (Catalyst optimizer) i udogodnień w API (domain-specific language). Także podobnie do RDD wszelkie operacje są buforowane i wykonywane dopiero gdy to niezbędne (lazy), przez co Spark może zastosować wiele optymalizacji.

W wersji 1.6 Apache Spark zostało wprowadzone trzecie API zwane Dataset. W przeciwieństwie do DataFrame, nowe Api jest silnie typowane (type-safe) oraz zorientowane obiektowo (object-oriented programming interface). Dodatkowo posiada jeszcze więcej optymalizacji (Catalyst Optimizer i Tungsten project). Użytkowo API jest bardzo zbliżone do RDD, przez co Dataset stał się optymalnym rozwiązaniem łączącym zalety zarówno DataFrame jak i RDD, przy dużo większej wydajności i mniejszym użyciu pamięci RAM niż RDD.

Wraz z pojawieniem się wersji 2.0 Spark'a, API dla DataFrame i Dataset zostało zunifikowane w pewnym stopniu, dzięki czemu korzystanie z nich stało się znacznie prostsze. Niestety względu na specyfikę języków programowania, nie każde API jest dostępne dla każdego języka, co przedstawia tabela:

Język  | Dostępne API
------------- | -------------
Scala | Dataset[T] & DataFrame (alias, DataFrame = Dataset[Row])
Java | Dataset[T]
Python | DataFrame
R | DataFrame

Jak widać silnie typowany język Java obsługuje Datset, zaś przeciwny jemu Python i R powinien korzystać z DataFrame. W języku Scala możemy korzystać z obydwu API. Trzeba zauważyć, że wraz z unifikacją API typ DataFrame to tak naprawdę alias dla typu Dataset[Row].

Podsumowując, jeśli korzystamy ze Sparka, twórcy zalecają korzystanie z Dataset lub DataFrame ze względu na optymalizacje i wygodniejsze API. Jeśli jednak potrzebujemy API niższego poziomu i możemy pominąć optymalizacje wykorzystane w Dataset lub DataFrame, możemy nadal korzystać z klasycznych RDD.

# Pierwsze kroki

Zacznijmy od wczytania jakiegoś pliku z lokalnego dysku:

~~~shell
scala> val textFile = spark.read.textFile("README.md")
textFile: org.apache.spark.sql.Dataset[String] = [value: string]
~~~

w wyniku Shell języka Scala informuje nas o stworzeniu wartości (val) typu Dataset (org.apache.spark.sql.Dataset[String]) i nazwie "textFile".

Na takim obiekcie możemy wykonywać wiele operacji, np:
~~~Java
val textFile = spark.read.textFile("README.md")
textFile.count()
textFile.first()
val linesWithSpark = textFile.filter(line => line.contains("Spark"))
linesWithSpark.count()
linesWithSpark.first()
~~~
W powyższym programie po wczytaniu pliku "README.md" (który znajduje się w bieżącej lokalizacji, czyli w rozpakowanym katalogu Apache Spark) zliczyliśmy liczbę wierszy za pomocą funkcji *count* oraz zwróciliśmy pierwszy element kolekcji, czyli w naszym przypadku pierwszą linijkę. Następnie stworzyliśmy nowy *Dataset* za pomocą funkcji *filter* którą wybraliśmy tylko te wiersze które zawierają słowo "Spark". Z racji że funkcja *filter* zwraca kolejny Dataset, na nim także można wywołać funkcje takie jak *first* oraz *count*

Te same operacje bardzo łatwo możemy wykonać używając języka Python:
~~~Python
textFile = spark.read.text("README.md")
textFile.count()
textFile.first()
linesWithSpark = textFile.filter(textFile.value.contains("Spark"))
linesWithSpark.count()
linesWithSpark.first()
~~~
W przypadku języka Python kod wygląda prawie identycznie, z tym wyjątkiem, że w tym przypadku używany jest oczywiście typ DataFrame. Także sam shell nie zwraca informacji o stworzonych obiektach, jak to się dzieje w przypadku shell'a dla języka Scala.

# Podsumowanie

Jeśli udało Ci się zainstalować i uruchomić Apache Spark na swoim komputerze, jesteś na dobrej drodze do poznania tego narzędzia. W kolejnych postach przedstawię znacznie więcej informacji Apache Spark, w tym informacje o architekturze, zasadzie działania, instalacji na klastrze i programowaniu.

# Legenda
* Strona projektu http://spark.apache.org
* Strona pobierania http://spark.apache.org/downloads.html
* Dokumentacja https://spark.apache.org/docs/latest/
* Wysyłanie zadania https://spark.apache.org/docs/latest/submitting-applications.html
* Parametry dla trybu interaktywnego https://spark.apache.org/docs/latest/submitting-applications.html#master-urls
* API dla języka Scala https://spark.apache.org/docs/latest/api/scala/index.html
* API dla języka Java https://spark.apache.org/docs/latest/api/java/index.html
* API dla języka Python https://spark.apache.org/docs/latest/api/python/index.html
* API dla języka R https://spark.apache.org/docs/latest/api/R/index.html
* DataFrame https://databricks.com/blog/2015/02/17/introducing-dataframes-in-spark-for-large-scale-data-science.html
* Dataset https://databricks.com/blog/2016/01/04/introducing-apache-spark-datasets.html
* RDD vs Dataframe vs Dataset https://databricks.com/blog/2016/07/14/a-tale-of-three-apache-spark-apis-rdds-dataframes-and-datasets.html

