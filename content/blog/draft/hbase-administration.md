+++
author = "Radosław Szmit"
categories = ["HBase"]
date = "2017-05-06T15:35:32+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "hbase administration"
type = "post"

draft = true
+++

# Polecenia zaawansowane #

Tworzenie tabeli z narzuconym wstępnym podziałem regionów
~~~ruby
create 't1','f',SPLITS => ['10','20',30']
~~~