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

# Tworzenie RDD par

Scala:
~~~Java
val lines = sc.textFile("README.md")
val pairs = lines.map(line => (line, line.split(" ").size))
pairs.first()
~~~

Python:
~~~Python
lines = sc.textFile("README.md")
pairs = lines.map(lambda line : (line, len(line.split(" "))))
pairs.first()
~~~















Scala:
~~~Java
~~~

Python:
~~~Python
~~~