+++
author = "Radosław Szmit"
categories = ["Administracja", "Linux", "CentOS"]
date = "2017-11-17T19:09:57+01:00"
description = ""
featured = "hortonworks-logo.png"
featuredalt = ""
featuredpath = "/img/administration"
linktitle = ""
title = "Ambari i biblioteka libtirpc-devel"
type = "post"

+++

Podczas instalacji Ambari na najnowszych platformach CentOS / Red Hat możemy dostać błąd podobny do poniższego:

~~~
2017-11-17 13:04:44,147 - Execution of '/usr/bin/yum -d 0 -e 0 -y install hadoop_2_6_3_0_235-client' returned 1. Error: Package: hadoop_2_6_3_0_235-hdfs-2.7.3.2.6.3.0-235.x86_64 (HDP-2.6)
           Requires: libtirpc-devel
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
2017-11-17 13:04:44,147 - Failed to install package hadoop_2_6_3_0_235-client. Executing '/usr/bin/yum clean metadata'
~~~

W celu dokończenia instalacji wystarczy zainstalować brakującą zależność:

Dla Red Hat 7.x
~~~shell
sudo yum install ftp://mirror.switch.ch/pool/4/mirror/centos/7/os/x86_64/Packages/libtirpc-devel-0.2.4-0.10.el7.x86_64.rpm
~~~

Ścieżka do powyższego pliku może się zmieniać (zwiększa się numer wersji), gdy dostaniemy błąd "nie ma pliku" bądź "nie ma nic do zrobienia" należy sprawdzić czy ścieżka jest poprawna.