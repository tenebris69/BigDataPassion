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


Filtracja:
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

Dwie najpopularniejsze tranformacje to filter i map

Map 
- mapujemy coś w coś innego
- może być innego typu
- inna liczność? -> filter (bo lazy)

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,3,4,5))
val squared = nums.map(x => x * x)
val result = squared.collect()
println(result.mkString(","))
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,3,4,5])
squared = nums.map(lambda x: x * x)
result = squared.collect()
for num in result:
    print "%i " % (num)
~~~












Scala:
~~~Java
~~~

Python:
~~~Python
~~~