+++
author = "Radosław Szmit"
categories = ["AWS","CentOS","Administracja Big Data"]
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

# Tworzenie klastra maszyn na AWS

Poniżej kolejne kroki w celu uzyskania kompletnego klastra obliczeniowego na Amazon Web Services.

Całość zaczynamy od wejścia na stronę: https://console.aws.amazon.com/console/ a następnie z menu usług wybieramy "EC2". Powinniśmy zobaczyć ekran jak niżej:
![](/img/aws-installing-cluster-with-centos/aws-ec2.png)

Na górze widzimy podsumowanie statusu w ramach aktualnie wybranej lokalizacji którą możemy zmienić w prawym górnych rogu (w moim przypadku jest to Virginia w USA).

Jeśli klikniemy menu "Running Instances" zobaczymy listę maszyn dostępnych w tej lokalizacji (w tym przypadku pustą).
![](/img/aws-installing-cluster-with-centos/aws-before-start-oregon.png)

Aby stworzyć nowe maszyny klikamy "Launch Instances". W wyniku dostajemy listę obrazów systemów do załadowania.
![](/img/aws-installing-cluster-with-centos/aws-machine-image.png)

W prezentowanej liście brakuje systemu CentOS którego chcemy użyć, dlatego skorzystamy z marketu (AWS Marketplace w menu po lewej) i wbudowanej wyszukiwarki. Wybieramy interesującą nas wersję i klikamy "Select"
![](/img/aws-installing-cluster-with-centos/aws-marketplace.png)
![](/img/aws-installing-cluster-with-centos/aws-marketplace2.png)

Po wybraniu odpowiedniego obrazu otrzymujemy jego podsumowanie. Warto przejrzeć zawarte tam informacje, gdyż możemy się dowiedzieć na przykład jak będzie się nazywał domyślny użytkownik na którego mamy się połączyć po SSH oraz czy obraz jest płatny
![](/img/aws-installing-cluster-with-centos/aws-centos.png)

Następnie przechodzimy do wybrania typu maszyny. Do naszego klastra testowego skorzystamy z maszyn typu "m4.xlarge" (4 CPU, 16GB RAM)
![](/img/aws-installing-cluster-with-centos/aws-instance-type.png)

W kolejnym oknie możemy zdefiniować liczbę maszyn wybranego typu oraz ustawić dodatkowe opcj. Ustawiam 7 maszyn
![](/img/aws-installing-cluster-with-centos/aws-instance-details.png)

W następnym oknie definiujemy przestrzeń dyskową dla naszych maszyn. W poniższym przykładzie wybrałem dyski magnetyczne o pojemności 100 GB
![](/img/aws-installing-cluster-with-centos/aws-storage.png)
![](/img/aws-installing-cluster-with-centos/aws-storage2.png)

Kolejny krok możemy pominąć, umożliwia on dodanie dodatkowych tagów do maszyn
![](/img/aws-installing-cluster-with-centos/aws-tags.png)

AWS promuje wykorzystanie droższych dysków SSD, więc musimy potwierdzić swój wybór dysków HDD (lub wybrać jednak dyski SSD)
![](/img/aws-installing-cluster-with-centos/aws-magnetic.png)

W kolejnym kroku definiujemy konfigurację zabezpieczeń (firewall) poprzez stworzenie nowej grupy. Z racji tworzenia klastra testowego otworzę cały ruch do tych maszyn (uwaga, w żadnym wypadku nie należy tego robić produkcyjnie, gdyż każdy będzie miał dostęp do naszych maszyn i usług na wszystkich portach)
![](/img/aws-installing-cluster-with-centos/aws-security.png)

W ostatnim kroku dostajemy podsumowanie naszej stworzonej konfiguracji wraz z ostrzeżeniem że nasza konfiguracja będzie płatna (przez pierwszy rok korzystania z AWS dostajemy tak zwany "free tier" czyli pewien limit użycia za który nie musimy płacić). Klikamy "Launch" by potwierdzić naszą konfigurację.
![](/img/aws-installing-cluster-with-centos/aws-review.png)

Domyślny użytkownik systemów na AWS nie ma możliwości logowania się hasłem, dlatego w kolejnym kroku jesteśmy proszeni o stworzenie odpowiedniego klucza który posłuży nam do zalogowania się na niego. Ten klucz musimy zapisać na swoim komputerze oraz nikomu go nie udostępniać.
![](/img/aws-installing-cluster-with-centos/aws-keypair.png)

Ostatecznie tworzą się nasze maszyny
![](/img/aws-installing-cluster-with-centos/aws-launch.png)

i na koniec dostajemy podsumowanie
![](/img/aws-installing-cluster-with-centos/aws-after-launch.png)

Klikając "View Instances" możemy zobaczyć listę naszych maszyn
![](/img/aws-installing-cluster-with-centos/aws-instances.png)


# Legenda
* https://alestic.com/2014/01/ec2-ssh-username/
* https://aws.amazon.com/marketplace/pp/B00O7WM7QW
* https://docs.hortonworks.com/HDPDocuments/Ambari-2.5.1.0/bk_ambari-installation/content/download_the_ambari_repo_lnx7.html

