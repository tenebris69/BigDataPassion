+++
author = "Radosław Szmit"
categories = ["AWS","CentOS","Administracja"]
date = "2017-06-20T12:59:11+02:00"
description = "Stworzenie klastra kilku maszyn w oparciu o Amazon Web Services i system CentOS 7"
featured = "AmazonWebservices_Logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Testowy klaster obliczeniowy - Amazon Web Services i CentOS 7"
type = "post"

+++

Publiczne chmury obliczeniowe cieszą się coraz większym zainteresowaniem. Jedną z najbardziej popularnych rozwiązań tego typu jest zdecydowanie Amazon Web Services. W poniższym krótkim tutorialu pokażę jak szybko zbudować mini klaster testowy do zastosowań Big Data, na którym będziemy mogli wdrożyć na przykład platformę HDP (instalacji poświęcony został inny tutorial).

# Założenie konta

Nasz proces musimy zacząć od założenia konta w serwisie (https://aws.amazon.com/console/) wybierając opcję "Sign in to the Console" a następnie wybranie opcji "I am a new user" i wypełnienie formularza.

Rejestracja jest dość prosta, jedynym utrudnieniem może się okazać konieczność podania karty kredytowej z której będą dokonywane płatności za comiesięczny rachunek około trzeciego dnia następnego miesiąca. Dodatkowo w momencie rejestracji, w celu potwierdzenia karty kredytowej, zostanie pobrana opłata w wysokości jednego dolara.

# Planowanie kosztów

Amazon udostępnia możliwość tworzenia maszyn na życzenie (on demand) za które płacimy zgodnie z wykorzystaniem (płatność za godzinę użycia). Aktualny cennik możemy znaleźć pod adresem: https://aws.amazon.com/ec2/pricing/on-demand/. Należy zwrócić uwagę, że ceny zależą od lokalizacji w której będziemy tworzyć maszyny (najtaniej jest w USA, np. Oregon lub Virginia) oraz dostępność wersji maszyn także zależy od kolokacji. Dla przykładu za maszynę m4.xlarge (4 CPU, 16 GB RAM) w Oregon zapłacimy 0.2 $ za godzinę użycia (włącozna maszyna). Dodatkowo naliczane są ceny za wykorzystane powierzchnie dyskowe. Całość naszego wdrożenia, najlepiej sprawdzić za pomocą kalkulatora: https://calculator.s3.amazonaws.com/index.html wybierając w zakładce EC2 odpowiednie maszyny, ich ilość i planowane wykorzystanie przestrzeni dyskowej.

# Tworzenie maszyn

![](/img/aws-installing-cluster-with-centos/aws-after-launch.png)
![](/img/aws-installing-cluster-with-centos/aws-before-start-oregon.png)
![](/img/aws-installing-cluster-with-centos/aws-centos.png)
![](/img/aws-installing-cluster-with-centos/aws-clustername.png)
![](/img/aws-installing-cluster-with-centos/aws-confirm-hosts.png)
![](/img/aws-installing-cluster-with-centos/aws-instance-details.png)
![](/img/aws-installing-cluster-with-centos/aws-instances.png)
![](/img/aws-installing-cluster-with-centos/aws-instance-type.png)
![](/img/aws-installing-cluster-with-centos/aws-keypair.png)
![](/img/aws-installing-cluster-with-centos/aws-launch.png)
![](/img/aws-installing-cluster-with-centos/aws-machine-image.png)
![](/img/aws-installing-cluster-with-centos/aws-magnetic.png)
![](/img/aws-installing-cluster-with-centos/aws-marketplace.png)
![](/img/aws-installing-cluster-with-centos/aws-marketplace2.png)
![](/img/aws-installing-cluster-with-centos/aws-review.png)
![](/img/aws-installing-cluster-with-centos/aws-security.png)
![](/img/aws-installing-cluster-with-centos/aws-selectversion.png)
![](/img/aws-installing-cluster-with-centos/aws-storage.png)
![](/img/aws-installing-cluster-with-centos/aws-storage2.png)
![](/img/aws-installing-cluster-with-centos/aws-tags.png)




~~~shell
# ping
for i in {1..7..1}; do ping -c 1 hdp${i}; done

for i in {1..5..1}; do ssh-keygen -f "/home/radek/.ssh/known_hosts" -R sages${i}; done

# uptime
for i in {1..7..1}; do ssh hdp${i} uptime; done
for i in {1..5..1}; do ssh sages${i} uptime; done


cssh centos@hdp{1..7}
cssh ec2-user@sages{1..5}

~~~

~~~
# Sages Hadoop
Host sages*
  User ec2-user
  IdentityFile /home/radek/Sages/pem/awshdpvirginia.pem

Host hdp*
  User centos
  IdentityFile /home/radek/Sages/pem/awshdp.pem
~~~


/etc/hosts:
~~~
54.149.229.39	ip-172-31-13-225.us-west-2.compute.internal	hdp1
54.149.72.85	ip-172-31-8-246.us-west-2.compute.internal	hdp2
54.244.161.97	ip-172-31-11-52.us-west-2.compute.internal	hdp3
34.209.25.141	ip-172-31-12-137.us-west-2.compute.internal	hdp4
54.201.192.34	ip-172-31-14-19.us-west-2.compute.internal	hdp5
54.200.237.19	ip-172-31-3-44.us-west-2.compute.internal	hdp6
54.191.200.16	ip-172-31-10-121.us-west-2.compute.internal	hdp7

52.90.240.16    ip-172-31-18-75.ec2.internal    sages1
54.86.15.248    ip-172-31-22-254.ec2.internal   sages2
54.211.248.36   ip-172-31-17-129.ec2.internal   sages3
184.73.113.151  ip-172-31-16-231.ec2.internal   sages4
52.90.25.164    ip-172-31-23-198.ec2.internal   sages5
~~~

~~~
172.31.13.225	hdp1
172.31.8.246	hdp2
172.31.11.52	hdp3
172.31.12.137	hdp4
172.31.14.19	hdp5
172.31.3.44     hdp6
172.31.10.121	hdp7
~~~

~~~
ip-172-31-13-225.us-west-2.compute.internal	hdp1
ip-172-31-8-246.us-west-2.compute.internal	hdp2
ip-172-31-11-52.us-west-2.compute.internal	hdp3
ip-172-31-12-137.us-west-2.compute.internal	hdp4
ip-172-31-14-19.us-west-2.compute.internal	hdp5
ip-172-31-3-44.us-west-2.compute.internal	hdp6
ip-172-31-10-121.us-west-2.compute.internal	hdp7
~~~

~~~shell
for i in {1..7..1}; do ssh-copy-id root@hdp{i}; done
~~~




Żeby się zalogować bez hasła z konta root na inne należy wkleić od plików ~/.ssh/authorized_hosts zawartość pliku ~/.ssh/id_rsa.pub która w naszym przypadku wygląda tak:

~~~
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5U26cKr+/F+e88/GFs+hryyafOTzch7Sx1WVp2h+vee9/OWpySbaAZkyqT5/iNUmlAyoZ+87JD03PR7u0I4tgX8Lj6/KkSETVtAVL5ufeT1TWrJ7XuI/gcCt6/pLjc5p7gyWKbg6LKgbyaf+aey+Pkz5iI8FbDPnt5RrR8wIGY4LOzikkIwR0ZLrJUdbr0o4Mv96yvC+mkzcdIiuZ4VfH81lWMlpMbUv+3udM6qiQyjUbRNrSw7Ej0fWxU94/IoHz5P/ozuE2QNCDPLc+Ej2i9hMJKE3i2/Cqx2/JOJnAY3AHVz97MpfuYnoOmXMFhhrP7okvi+nmugKYIintbFbx root@ip-172-31-13-225.us-west-2.compute.internal
~~~


~~~
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAuVNunCq/vxfnvPPxhbPoa8smnzk83Ie0sdVladofr3nvfzlq
ckm2gGZMqk+f4jVJpQMqGfvOyQ9Nz0e7tCOLYF/C4+vypEhE1bQFS+bn3k9U1qye
17iP4HArev6S43Oae4Mlim4OiyoG8mn/mnsvj5M+YiPBWwz57eUa0fMCBmOCzs4p
JCMEdGS6yVHW69KODL/esrwvppM3HSIrmeFXx/NZVjJaTG1L/t7nTOqokMo1G0Ta
0sOxI9H1sVPePyKB8+T/6M7hNkDQgzy3PhI9ovYTCShN4tvwqsdvyTiZwGNwB1c/
ezKX7mJ6DplzBYYaz+6JL4vp5roCmCIp7WxW8QIDAQABAoIBAHeed42LNjqktmpK
1oDjT8iL1sD9E+CQIzyJray2Iq+Dt+dZavCbzZVw8lkXje5XUNKWiU0+MAmsvB9n
gKtUKfkptkShHfuVvgBl8uk8ADuI3wy1XM2Gji9il14LBUqUaokAbHG+edqvZM6B
Rn8ytc8pTiHQTFR1urgnobkT7iGqbNi6eRSajfJOlQcB0nVVw4Q089ijdPdAY0Y6
gk1l+p34FldV+FKAs7uoukrPiNxr9P/DVsKVWb35tNXdegp/9rtFf0PhL8fuKyo+
gkblEj+nkSNyHPVWmLgrmbAFo0tt+0lxggVN6nqBZEGtaYHeAvQKfldFLr2mSIK1
F0vmUgECgYEA8AXFqLxBO8uTXDS8LFfVXIf5B58d0TcChZBB+W5LRM5SjLjfWxu2
drPYdExOwhbYZVEuq74PQKg/QTIBGNLlzhmHaA/jZAcAiB0BJ2MJtrT2Mqi/hP4C
CHVmxRqczLLGIgQ4Q9jdf6qblBgH4tvw3l0BCHdqRqu/83a1+ec5ZiECgYEAxamS
3EpyqYRy8K5WE5hc+isgqWAbOKZTj3sJQr8zn0zQFKQsTBanmvHg35yp2IVgregd
LTxLpK8jaAXDfmqY1z+6F7ogEEldfp2Sqc22BvPYw7cCj2qpIPCrOHSTRaFSGZOF
Ea6kbIjRQrILmpzAvO+QL9Az2ooWLmBbvqFlNtECgYBYXresbUtTOZuSqjPR27DJ
daKBZNr0iV1bUYaI0EvUFGaeOv65K5XdVE/QWbvxh7m6a85UGxDAjHFljoSK4DMN
06Zf8OGWlWFju9IN70/HPg8bDbgdvet/s8HXtfme//8kzQruJ+09MNJBDyvwIWwo
YnOb62Nsi5WLjNxpGvGuIQKBgANgwoHBQ/RhrxUt5YqwL+aWlhhO7Cgrr4HkOGRL
oDY4udWgeKFUQckEGTO5Ga35mY1fSiBbx28pDxHYB19Bsxr6m9OL+sBMgKyJRNhi
C5pS0IGHvyN0Ty+g7UwpsdqexyhovP1wXp78N5dMM6aQxzpzXaNzi29QrNBeFTNM
zs4BAoGBAIy371Md/55HGaP0KS+T9mhvZwMA/AeQfYv14MACA/aQH+D6ClzgP+Xa
DZxFileAw+8DEA0Y3PPXZAb05af70khvxm/ab2xizvJC3fu4pcU/HgHPkSFR6Aub
3Cf6zvHO5SQYfCV1/Gmf6eKeW8wxeV+NR1NLQGRWlft/B31JCCUa
-----END RSA PRIVATE KEY-----
~~~


~~~
vim machines.txt
for i in `cat machines.txt`; do ssh ${i} uptime; done
~~~

Hosty:
~~~
ip-172-31-13-225.us-west-2.compute.internal
ip-172-31-8-246.us-west-2.compute.internal
ip-172-31-11-52.us-west-2.compute.internal
ip-172-31-12-137.us-west-2.compute.internal
ip-172-31-14-19.us-west-2.compute.internal
ip-172-31-3-44.us-west-2.compute.internal
ip-172-31-10-121.us-west-2.compute.internal
~~~

cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'


vim /etc/ssh/sshd_config
PasswordAuthentication yes
service sshd restart


adduser username
echo password | passwd username --stdin



for i in `cat users.txt`; do adduser ${i} ; done
for i in `cat users.txt`; do echo ${i} | passwd ${i} --stdin ; done



# Legenda
* https://alestic.com/2014/01/ec2-ssh-username/
* https://aws.amazon.com/marketplace/pp/B00O7WM7QW
* https://docs.hortonworks.com/HDPDocuments/Ambari-2.5.1.0/bk_ambari-installation/content/download_the_ambari_repo_lnx7.html

