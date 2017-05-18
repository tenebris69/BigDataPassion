+++
author = "Radosław Szmit"
categories = ["Elasticsearch"]
date = "2017-05-15T17:36:14+02:00"
description = "Wprowadzenie do technologii wyszukiwania Elasticsearch"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Wprowadzenie do Elasticsearch"
type = "post"

+++


Each Elasticsearch shard is a Lucene index. There is a maximum number of documents you can have in a single Lucene index. As of LUCENE-5843, the limit is 2,147,483,519 (= Integer.MAX_VALUE - 128) documents. You can monitor shard sizes using the _cat/shards api.


