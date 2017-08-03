+++
author = ""
categories = []
date = "2017-08-03T02:00:34+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "spark programming rdd"
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