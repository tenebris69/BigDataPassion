+++
author = "Radosław Szmit"
categories = ["HBase"]
date = "2017-05-06T11:27:00+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "hbase architecture"
type = "post"

draft = true
+++

Table       (HBase table)
    Region       (Regions for the table)
         Store          (Store per ColumnFamily for each Region for the table)
              MemStore           (MemStore for each Store for each Region for the table)
              StoreFile          (StoreFiles for each Store for each Region for the table)
                    Block             (Blocks within a StoreFile within a Store for each Region for the table)