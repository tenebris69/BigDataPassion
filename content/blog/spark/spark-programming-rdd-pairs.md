+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-08-07T16:08:07+02:00"
description = ""
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Przetwarzanie RDD par w Apache Spark"
type = "post"

+++

W RDD nie musimy przechowywać pojedynczych obiektów, ale możemy tam przekazywać pary obiektów, czyli tak zwane krotki lub z angielskiego tuple. Na takim RDD mamy dostęp do nowych metod usprawniających nam pracę z krotkami.

# TWorzenie RDD par w programie sterownika

Scala:
~~~Java
val lines = Map((1,"Ala ma kota"), (2,"Witaj świecie"), (3,"Dwadzieścia tysięcy mil podmorskiej żeglugi"))
val rddPair = sc.parallelize(lines.toSeq)
rddPair.first
~~~

Python:
~~~Python
lines = [(1,"Ala ma kota"), (2,"Witaj świecie"), (3,"Dwadzieścia tysięcy mil podmorskiej żeglugi")]
rddPair = sc.parallelize(lines)
rddPair.first()
~~~

# Transformacje na RDD par

## Mapowanie wartości

Podobnie jak dla RDD z pojedynczym oboektem, RDD par udostępnia metodę map pozwalającą zmapować jedno RDD w drugie. Możemy także tej metody z powodzeniem użyć do stworzenia RDD par z zwykłego RDD jak w poniższym przykładzie gdzie mapujemy linie tekstu na krotki zawierające linię i jej długość:

Scala:
~~~Java
val lines = sc.textFile("README.md")
val pairs = lines.map(line => (line, line.split(" ").size))
pairs.take(10).foreach(println)
~~~

Python:
~~~Python
lines = sc.textFile("README.md")
pairs = lines.map(lambda line : (line, len(line.split(" "))))
pairs.take(10)
~~~

W przypadku RDD par możemy także skorzystać z metody *map* mapując całą krotkę na inny obiekt lub inną krotkę, ale możemy także skorzystać z funkcji *mapValues* oraz *flatMapValues* które to mapują nam same wartości z naszej krotki bez zmiany klucza, oraz w drugim przypadku dokonują także "spłaszczenia" listy tablic na jedną listę wszystkich obiektów, w tym przypadku RDD.

Scala:
~~~Java
val values = List(("Ala",3), ("Tomek", 4), ("Kasia", 5), ("Ala",5), ("Tomek", 3), ("Kasia", 4))
val valueRdd = sc.parallelize(values)
val countRdd = valueRdd.mapValues( v => (v, 1))
countRdd.take(10).foreach(println)
~~~

Python:
~~~Python
values = [("Ala",3), ("Tomek", 4), ("Kasia", 5), ("Ala",5), ("Tomek", 3), ("Kasia", 4)]
valueRdd = sc.parallelize(values)
countRdd = valueRdd.mapValues(lambda v : (v, 1))
countRdd.collect()
~~~

## Redukcja wartości

Oprócz mapowania drugą bardzo popularną operacją jest agregacja której możemy dokonać za pomocą funkcji *reduceByKey*, *combineByKey* oraz *groupByKey*.

Funkcja *groupByKey* umożliwia nam zgrupowaniu wszystkich wartości występujących pod wspólnym kluczem i zwraca nowe RDD.

Funkcja *reduceByKey* pozwala dodatkowo wskazać funkcję która zredukuje zgrupowane wartości do pojedynczego elementu:






Scala:
~~~Java
~~~

Python:
~~~Python
~~~