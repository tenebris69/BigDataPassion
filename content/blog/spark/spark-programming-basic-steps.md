+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-07-30T14:49:14+02:00"
description = "Wprowadzenie do programowania Apache Spark dla programistów Java, Scala i Python"
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Podstawy programowania Apache Spark"
type = "post"

+++

# Programowanie Apache Spark

Apache Spark został napisany w języku Scala, przez co domyślnym językiem programowania jak i tym które oferuje najwięcej możliwości jest właśnie Scala. Dzięki wyjątkowo dobrej kompatybilności Scali z językiem Java, drugim językiem programowania jaki jest dostępny dla tej platformy jest oczywiście język Java oferujący od wersji 8 wiele funkcjonalności zbliżonych do języka Scala (dosłownie rzecz biorąc, twórcy języka Java w wersji 8 wzorowali się między innymi na języku Scala wprowadzając nowe fukcjonalności). Z racji dużej popularności języka Python wśród analityków i inżynierów Big Data, twórcy także dodali wsparcie dla tego języka. Osoby nie mający doświadczenia z programowaniem w powyższych językach ale znający pakiet R mogą także skorzystać z API stworzonego dla tego języka (o tym w innych postach). Ostatnim językiem który mogą wykorzystać użytkownicy Apache Spark jest obecnie najpopularniejszy język świata jakim jest język SQL. Można tutaj skorzystać zarówno z modułu zwanego Spark SQL ale także użytkownicy Apache Hive mogą skonfigurować go do wykorzystania Apache Spark jako silnika przetwarzającego dane (obok Apache Hadoop MapReduce czy Apache Tez).

W tym poście skupimy się na podstawach programowania w języku Scala, Java i Python.

# Podstawowe operacje Apache Spark czyli zliczanie słów

Jednymi z podstawowych funkcji Apache Spark są funkcje *map* i *reduce*, dobrze znane programistom języków funkcyjnych czy użytkownikom Apache Hadoop oraz innych narzędzi jak MongoDB, Cassandra implementujących paradygmat MapReduce.

Funkcja *map* służy do transformacji jednego bytu w coś innego, np możemy zmienić linijki w liczbę oznaczającą liczbę słów w tej linii:
~~~Java
val textFile = spark.read.textFile("README.md")
val lineSizeDataset = textFile.map(line => line.split(" ").size)
lineSizeDataset.show(10)
~~~

w wyniku czego otrzymamy w konsoli następujący rezultat:
~~~shell
scala> val textFile = spark.read.textFile("README.md")
textFile: org.apache.spark.sql.Dataset[String] = [value: string]

scala> val lineSizeDataset = textFile.map(line => line.split(" ").size)
lineSizeDataset: org.apache.spark.sql.Dataset[Int] = [value: int]

scala> lineSizeDataset.show(10)
+-----+
|value|
+-----+
|    3|
|    1|
|   14|
|   13|
|   11|
|   12|
|    8|
|    6|
|    1|
|    1|
+-----+
only showing top 10 rows
~~~
Jak widać funkcja *map* utworzyła nam nowy Dataset zawierający tyle samo elementów, jednak jako wartości są ilości słów w linii. Dodatkowo na koniec dodałem funkcję  *show* która wyświetla przekazaną ilość parametrów w konsoli z kolekcji typu Dataset.

Funkcja *reduce* służy do "redukcji" naszego zbioru, czyli jego "uproszczeniu". W tym przypadku chcemy zsumować liczbę słów w każdej linijce tekstu by otrzymać łączną liczbę słów w całym tekście.
~~~Java
val textFile = spark.read.textFile("README.md")
val lineSizeDataset = textFile.map(line => line.split(" ").size)
val textFileWordCount = lineSizeDataset.reduce((a, b) => a + b)
~~~
Po uruchomieniu programu otrzymaliśmy wynik w postaci:
~~~shell
scala> val textFile = spark.read.textFile("README.md")
textFile: org.apache.spark.sql.Dataset[String] = [value: string]

scala> val lineSizeDataset = textFile.map(line => line.split(" ").size)
lineSizeDataset: org.apache.spark.sql.Dataset[Int] = [value: int]

scala> val textFileWordCount = lineSizeDataset.reduce((a, b) => a + b)
textFileWordCount: Int = 566
~~~
czyli cały tekst ma 566 słów.

Ten sam przykład w języku Python:
~~~Python
textFile = spark.read.text("README.md")
from pyspark.sql.functions import *

lineSizeDataset = textFile.select(size(split(textFile.value, "\s")).name("numWords"))
lineSizeDataset.show(10)

textFileWordCount = lineSizeDataset.agg(sum(col("numWords")).name("result")).collect()[0][0]
textFileWordCount
~~~
wynik:
~~~shell
>>> textFile = spark.read.text("README.md")
>>> from pyspark.sql.functions import *
>>> 
>>> lineSizeDataset = textFile.select(size(split(textFile.value, "\s")).name("numWords"))
>>> lineSizeDataset.show(10)
+--------+
|numWords|
+--------+
|       3|
|       1|
|      14|
|      13|
|      11|
|      12|
|       8|
|       6|
|       1|
|       1|
+--------+
only showing top 10 rows

>>> 
>>> textFileWordCount = lineSizeDataset.agg(sum(col("numWords")).name("result")).collect()[0][0]
>>> textFileWordCount
~~~

Api dla DataFrame jest trochę inne niż w języku Scala, tutaj nie użyliśmy funkcji *map* tylko *select*, zaś zamiast *reduce* użyliśmy funkcji *agg*. Dodatkowo skorzystaliśmy z wbudowanych w język Python fukcji.



# Apache Spark i MapReduce

Osoby które znają paradygmat MapReduce i korzystali z projektu Apache Hadoop, wiedzą, że swoistym "Witaj Świecie" dla aplikacji typu MapReduce jest przypadek zliczania ilości wystąpień każdego ze słów a nie wszystkich razem. Taki przypadek także bardzo łatwo uzyskać za pomocą Apache Spark korzystając z funkcji *flatMap* oraz *groupByKey*:
~~~Java
val textFile = spark.read.textFile("README.md")
val wordCounts = textFile.flatMap(line => line.split(" ")).groupByKey(identity).count()
wordCounts.sort($"count(1)".desc).show(10)
~~~
w wyniku otrzymujemy:
~~~shell
scala> val textFile = spark.read.textFile("README.md")
textFile: org.apache.spark.sql.Dataset[String] = [value: string]

scala> val wordCounts = textFile.flatMap(line => line.split(" ")).groupByKey(identity).count()
wordCounts: org.apache.spark.sql.Dataset[(String, Long)] = [value: string, count(1): bigint]

scala> wordCounts.sort($"count(1)".desc).show(10)
+-----+--------+
|value|count(1)|
+-----+--------+
|     |      71|
|  the|      24|
|   to|      17|
|Spark|      16|
|  for|      12|
|   ##|       9|
|  and|       9|
|    a|       8|
|   on|       7|
|  run|       7|
+-----+--------+
only showing top 10 rows
~~~

Pierwszą różnicą w stosunku do poprzedniego przypadku jest użycie *flatMap* zamiast *map*. Robimy to dlatego że w pierwszym przypadku mapowaliśmy linie tekstu na ilość słów w tej linii, czyli dla każdego elementu wejściowego zbioru produkowaliśmy dokładnie jeden element wynikowy. W drugim przypadku dla jednej linijki tekstu produkujemy listę słów, czyli ostatecznie mamy Dataset złożony z tablicy obiektów co można zauważyć na poniższym fragmencie kodu:
~~~Java
val textFile = spark.read.textFile("README.md")
val wordsDataset = textFile.map(line => line.split(" "))
wordsDataset.show(10)
~~~
w wyniku otrzymujemy:
~~~shell
scala> val textFile = spark.read.textFile("README.md")
textFile: org.apache.spark.sql.Dataset[String] = [value: string]

scala> val wordsDataset = textFile.map(line => line.split(" "))
wordsDataset: org.apache.spark.sql.Dataset[Array[String]] = [value: array<string>]

scala> wordsDataset.show(10)
+--------------------+
|               value|
+--------------------+
|  [#, Apache, Spark]|
|                  []|
|[Spark, is, a, fa...|
|[high-level, APIs...|
|[supports, genera...|
|[rich, set, of, h...|
|[MLlib, for, mach...|
|[and, Spark, Stre...|
|                  []|
|[<http://spark.ap...|
+--------------------+
only showing top 10 rows
~~~
Z racji tego że chcemy ostatecznie mieć Dataset zawierający wszystkie słowa a nie tablice słów, musimy go niejako "spłaszczyć", czyli z angielskiego "to flat".

Podobnie jest w przypadku funkcji *reduce* i *groupByKey*. Pierwsza funkcja pozwala nam zredukować Dataset do pojedynczej wartości, zaś druga użyta w tym przypadku funkcja robi dokładnie to co silnik MapReduce, czyli grupuje nam wszystkie identyczne elementy z wejściowego Dataset w jeden podzbiór, gdzie dla każdego wykonujemy funkcję *count* zwracającą nam jego liczność.

W przypadku języka Python powyższy algorytm MapReduce mógłby wyglądać tak:
~~~Python
textFile = spark.read.text("README.md")
from pyspark.sql.functions import *
wordCounts = textFile.select(explode(split(textFile.value, "\s")).name("word")).groupBy("word").count()
wordCounts.sort("count",ascending=False).show(10)
~~~
wynik:
~~~shell
>>> textFile = spark.read.text("README.md")
>>> from pyspark.sql.functions import *
>>> wordCounts = textFile.select(explode(split(textFile.value, "\s")).name("word")).groupBy("word").count()
>>> wordCounts.sort("count",ascending=False).show(10)
+-----+-----+
| word|count|
+-----+-----+
|     |   71|
|  the|   24|
|   to|   17|
|Spark|   16|
|  for|   12|
|   ##|    9|
|  and|    9|
|    a|    8|
|   on|    7|
|  run|    7|
+-----+-----+
only showing top 10 rows
~~~

Jak widać, nasz rezultat jest identyczny jak w przypadku języka Scala. Tutaj także skorzystaliśmy z funkcji *select* w kombinacji z *explode* zamiast z *flatMap* oraz funkcji *groupBy* zamiast *groupByKey*.

# Legenda
* Strona projektu http://spark.apache.org
* Dokumentacja https://spark.apache.org/docs/latest/
* API dla języka Scala https://spark.apache.org/docs/latest/api/scala/index.html
* API dla języka Java https://spark.apache.org/docs/latest/api/java/index.html
* API dla języka Python https://spark.apache.org/docs/latest/api/python/index.html
* Spark SQL https://spark.apache.org/sql/
* Hive na Spark'u https://cwiki.apache.org/confluence/display/Hive/Hive+on+Spark%3A+Getting+Started
* Hive na Spark'u (JIRA) https://issues.apache.org/jira/browse/HIVE-7292