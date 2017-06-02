+++
author = ""
categories = []
date = "2017-06-02T13:29:58+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "aws hdp"
type = "post"

+++

~~~shell
# ping
for i in {1..7..1}; do ping -c 1 hdp${i}; done

# uptime
for i in {1..7..1}; do ssh hdp${i} uptime; done

cssh centos@hdp-{1..7}

~~~