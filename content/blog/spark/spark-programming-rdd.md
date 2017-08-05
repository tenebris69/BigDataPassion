+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-08-03T02:00:34+02:00"
description = "Wprowadzenie do programowania Apache Spark z wykorzystaniem RDD"
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Spark programming - Resilient Distributed Datasets"
type = "post"

+++

# Wprowadzenie

TODO


~~~Java
val lines = sc.textFile("README.md")
lines.count()
lines.first()
~~~

~~~Python
lines = sc.textFile("README.md")
lines.count()
lines.first()
~~~

Aplikacja składa się z programu sterownika i klastra obliczeniowego, tam mamy węzły wykonawcze

SparkContext reprezentuje połączenie z klastrem, od niego wszystko się zaczyna




RDD ofertują transformacje i akcje

Filtracja:
~~~Java
val lines = sc.parallelize(List("pizza", "hamburger", "lasagne"))
lines.count()
lines.first()
~~~

~~~Python
lines = sc.parallelize(["pizza", "hamburger", "lasagne"])
lines.count()
lines.first()
~~~

# Tworzenie RDD

TODO

# Transformacje

## Mapowanie wartości

Jedną z najpopularnieszych tranformacji jest *map* pozwalająca na zmianę obiektów jednego RDD na zupełnie inne obiekty w wynikowym RDD. Typ obiektu wejściowego i wynikowego mogą się różnić, np. liczby możemy zmienić na stałe napisowe.

Poniżej przykład mapowania RDD zawierającego liczby na nowe RDD z ich kwadratami.

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,3,4,5))
val squared = nums.map(x => x * x)
val result = squared.collect()
println(result.mkString("\n"))
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,3,4,5])
squared = nums.map(lambda x: x * x)
results = squared.collect()
for result in results:
    print result
~~~

UWAGA
Do wyświetlenia wynikowego RDD użyliśmy funkcji *collect* która zwróciła tablicę wszystkich elementów w RDD. Przy bardzo dużych RDD ściągnięcie wszystkich wartości do programu sterownika może wygenerować błąd braku wystarczającej ilości pamięci RAM.

Jeśli chcemy by nowe RDD miało więcej obiektów, możemy zwrócić RDD zawierające kolekcje (jeden obiekt wejściowy mapujemy w oddzielną kolekcję) lub użyć metody *flatMap* zwracającą nowe RDD mające wszystkie nasze nowe obiekty.

Scala:
~~~Java
val lines = sc.parallelize(List("Ala ma kota", "Witaj świecie", "Dwadzieścia tysięcy mil podmorskiej żeglugi"))
val words = lines.flatMap(line => line.split(" "))
val result = words.collect()
println(result.mkString("\n"))
~~~

Python:
~~~Python
lines = sc.parallelize(["Ala ma kota", "Witaj świecie", "Dwadzieścia tysięcy mil podmorskiej żeglugi"])
words = lines.flatMap(lambda line: line.split(" "))
results = words.collect()
for result in results:
    print result
~~~

## Filtrowanie wartośći

Drugą bardzo popularną transformacją jest funkcja *filter* zwracająca nowe RDD z elementami które spełniły wskazany warunek.

Scala:
~~~Java
val lines = sc.textFile("README.md")
val sparkLines = lines.filter(line => line.contains("Spark"))
sparkLines.count()
sparkLines.first()
~~~

~~~Python
lines = sc.textFile("README.md")
sparkLines = lines.filter(lambda line: "Spark" in line)
sparkLines.count()
sparkLines.first()
~~~

## Operacje na wielu zbiorach

Transformacje nie muszą pracować na jednym RDD ale mogą także operować na wielu zbiorach. Podstawowymi transformacjami działającymi na wielu RDD są:
* union - łączenie dwóch RDD
* intersection - zwraca część wspólną obydwu RDD (bez duplikatów)
* subtract - różnica dwóch zbiorów
* cartesian - iloczyn kartezjański, zwraca połączenie każdego elementu pierwszego zbioru z każdym elementem drugiego zbioru

Scala:
~~~Java
val rdd1 = sc.parallelize(List(1,2,3,4,5))
val rdd2 = sc.parallelize(List(4,5,6,7))

val union = rdd1.union(rdd2)
println(union.collect().mkString(", "))

val intersection = rdd1.intersection(rdd2)
println(intersection.collect().mkString(", "))

val subtract = rdd1.subtract(rdd2)
println(subtract.collect().mkString(", "))

val cartesian = rdd1.cartesian(rdd2)
println(cartesian.collect().mkString(", "))
~~~

Python:
~~~Python
rdd1 = sc.parallelize([1,2,3,4,5])
rdd2 = sc.parallelize([4,5,6,7])

union = rdd1.union(rdd2)
print(union.collect())

intersection = rdd1.intersection(rdd2)
print(intersection.collect())

subtract = rdd1.subtract(rdd2)
print(subtract.collect())

cartesian = rdd1.cartesian(rdd2)
print(cartesian.collect())
~~~

Trzeba jednak pamiętać, że w projektach Big Data takie operacje na dużych zbiorach bywają niezwykle kosztowne obliczeniowo a czasami nawet nie możliwe do przeprowadzenia, np. iloczyn kartezjański dwóch bardzo licznych zbiorów może wygenerować tak duży zbiór wynikowy, którego nie będziemy w stanie zapisać ani w pamięci RAM ani także na naszym dysku. Dodatkowo operacje na zbiorach rozproszonych często mogą spowodować bardzo dużo połączeń i przesunięc danych z jednego serwera na drugi, co także znacząco spowolni nasze obliczenia (nie bazujemy tutaj na tak zwanej lokalności danych).





Scala:
~~~Java
~~~

Python:
~~~Python
~~~