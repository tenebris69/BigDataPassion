+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-07-29T13:18:32+02:00"
description = "Poznaj jedną z najpopularniejszych technologii Big Data ostatnich lat"
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Wprowadzenie do Apache Spark"
type = "post"

+++

Apache Spark to platforma obliczeniowa stworzona z myślą o przetwarzaniu danych.

Spark powstał jako odpowiedź na ograniczenia paradygmatu MapReduce wykorzystywanym przez projekt Apache Hadoop który z założenia przetwarza dane w trybie wsadowym, to znaczy między innymi, że za każdym razem wczytuje i zapisuje je na dysk (HDFS) przez co jego wydajność w algorytmach iteracyjnych jest dość ograniczona. W Apache Spark została wprowadzona idea *Resilient Distributed Dataset* czyli zbiorów danych które są przechowywane w pamięci dzięki czemu mogą być wykorzystane przez kolejne algorytmy bądź iteracje bez potrzeby ich ponownego wczytywania z dysku. Oczywiście konsekwencją jest znaczne zużycie pamięci RAM w naszym klastrze i jeśli wielkość przetwarzanych danych znacząco przerasta wielkość pamięci operacyjnej na klastrze, Spark potrafi przechowywać je korzystając z dysków twardych podobnie jak to robi Apach Hadoop MapReduce.

Autorem Apache Spark jest Matei Zaharia będący pracownikiem naukowych uniwersytetu Stanforda (https://www.stanford.edu/). Spark powstał jako element pracy naukowej jego autora, dokładnie studiów doktoranckich, na uniwersytecie Berkeley (http://www.berkeley.edu/), zaś idea RDD została po raz pierwszy przez Niego przedstawiona w publikacji naukowej *Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing* (https://cs.stanford.edu/~matei/papers/2012/nsdi_spark.pdf) w 2012 roku. Matei Zaharia jest także współautorem Apache Mesos oraz brał udział przy tworzeniu niektórych algorytmów w narzędziu Apache Hadoop. Obecnie projekt jest rozwijany pod patronatem fundacji Apache głównie przez założoną przez autora firmę Databricks. Pierwszy raz Spark został wydany 30 maja 2014 roku.

Apache Spark można zainstalować w wersji rozproszonej za pomocą natywnej implementacji (opartej o Akka), narzędzia Apache Mesos oraz Hadoop YARN.

Dane można wczytywać z lokalnego dysku lub rozproszonych systemów składowania danych jak Hadoop Distributed File System (HDFS), MapR File System (MapR-FS), Cassandra, OpenStack Swift, Amazon S3, Kudu i innych, a nawet zaimplementować własny sterownik do obsługi zewnętrznych źródeł danych lub znaleźć gotową implementację w sieci.

Apache Spark został napisany w języku Scala, jednak udostępnia wsparcie (tak zwane API) dla języka *Scala*, *Java*, *Python* oraz *R*, jednakże API dla języka Scala udostępnia najwięcej możliwości.

# Artykuły na temat Apache Spark
* [Szybki start z Apache Spark](/blog/spark/spark-quick-start)
* [Podstawy programowania Apache Spark](/blog/spark/spark-programming-basic-steps)

# Legenda:
* Uniwersytet Berkeley http://www.berkeley.edu/
* Uniwersytet Stanforda https://www.stanford.edu/
* Strona domowa Matei Zaharia http://cs.stanford.edu/~matei/
* Publikacja na temat RDD https://cs.stanford.edu/~matei/papers/2012/nsdi_spark.pdf
* Strona domowa projektu http://spark.apache.org