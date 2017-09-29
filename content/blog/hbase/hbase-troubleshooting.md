+++
author = "Radosław Szmit"
categories = ["HBase","Administracja"]
date = "2017-05-01T17:04:17+02:00"
description = "Rozwiązywanie problemów z bazą HBase"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Hbase troubleshooting"
type = "post"

draft = true
+++

HBase jak i sam HDFS na którym bazuje, zapewnia bardzo dobrą odporność na awarię na wielu płaszczyznach (awarie dysków, maszyn, całysz szaf rakowych, etc). Mimo to w specyficznych sytuacjach może się zdarzyć awaria która będzie wymagała od nas konkrtenych działań by umożliwiśc dalszą pracę naszego klastra obliczeniowego.

# Problemy sprzętowe i systemowe

Zanim zaczniemy cokolwiek robić po stronie oprogramowania, neleży najpierw uporać się z problemami sprzętowymi lub systemowymi

# Problemy z HDFS

HBase składuje wszystkie swoje dane w HDFSie. Dlatego do jego poprawnego działania niezbędne jest prawidłowe działanie samego HDFS'a.

Stan naszego rozproszonego dysku (Hadoop Distrubuted File System) możemy łatwo sprawdzić za pomocą polecenia:

~~~shell
hdfs fsck /
~~~

w rezultacie powinniśmy otrzymać podobny wynik w konsoli:
~~~shell
hdfs fsck /
Connecting to namenode via http://hadoop1:50070/fsck?ugi=hdfs&path=%2F
FSCK started by hdfs (auth:SIMPLE) from /192.168.145.16 for path / at Mon May 01 15:13:59 UTC 2017
....................................................................................................
....................................................................................................
....................................................................................................
...........................................................................................Status: HEALTHY
 Total size:    3862908544014 B (Total open files size: 279711 B)
 Total dirs:    2203
 Total files:   3691
 Total symlinks:                0 (Files currently being written: 9)
 Total blocks (validated):      31347 (avg. block size 123230565 B) (Total open file blocks (not validated): 9)
 Minimally replicated blocks:   31347 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    2
 Average block replication:     2.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          8
 Number of racks:               1
FSCK ended at Mon May 01 15:14:00 UTC 2017 in 171 milliseconds

The filesystem under path '/' is HEALTHY
~~~

Jak widać dostajemy komunikat o tym, że nasz system jest zdrowy (HEALTHY) oraz trochę statystyk z których wynika, że nie mamy żadnych brakujących bloków lub replik, żadne też nie są w nadmiarze.

W dalszej części zakładam, że HDFS jest sprawny i rozwiązywanie problemów dotyczy tylko bazy HBase (rozwiązywaniu problemów z HDFS'em będzie poświęcony inny wpis).

# Problemy z HBase

Podstawowym narzędziem od którego możemy zacząć jest sprawdzenie spójności regionów i tabel zwanego **hbck** będącego odpowiednikiem **fsck** dla HDFS'a.

By skorzystać z **hbck** wystarczy w konsoli wywołać polecenie:
~~~shell
hbase hbck
~~~

W wyniku otrzymujemy informację o stanie klastra w postaci napisu "OK" lub liczby odnalezionych niespójności (INCONSISTENCIES).

~~~shell
2017-05-01 17:36:38,049 INFO  [main] util.HBaseFsck: Finishing hbck
Summary:
Table products is okay.
    Number of regions: 31
    Deployed on:  hadoop1,16020,1493654453604 hadoop2,16020,1493654453068 hadoop3,16020,1493654453803 hadoop4,16020,1493654453906 hadoop5,16020,1493654452818 hadoop6,16020,1493654452432 hadoop7,16020,1493654453616
Table hbase:meta is okay.
    Number of regions: 1
    Deployed on:  hadoop2,16020,1493654453068
Table hbase:backup is okay.
    Number of regions: 1
    Deployed on:  hadoop3,16020,1493654453803
Table movies is okay.
    Number of regions: 34
    Deployed on:  hadoop1,16020,1493654453604 hadoop2,16020,1493654453068 hadoop3,16020,1493654453803 hadoop4,16020,1493654453906 hadoop5,16020,1493654452818 hadoop6,16020,1493654452432 hadoop7,16020,1493654453616
Table hbase:namespace is okay.
    Number of regions: 1
    Deployed on:  hadoop7,16020,1493654453616
Table data is okay.
    Number of regions: 381
    Deployed on:  hadoop1,16020,1493654453604 hadoop2,16020,1493654453068 hadoop3,16020,1493654453803 hadoop4,16020,1493654453906 hadoop5,16020,1493654452818 hadoop6,16020,1493654452432 hadoop7,16020,1493654453616
0 inconsistencies detected.
Status: OK
~~~

**UWAGA** w niektórych przypadkach (start klastra, podział regionu) polecenie może wskazać niespójności mimo, że z naszą bazą jest wszystko w porządku. W takiej sytuacji należy ponowić polecenie po jakimś czasie. Dopiero informacja o niespójności przy kilku próbach pod rząd może oznaczać rzeczywisty problem ze spójnością naszych danych.


















