+++
author = "Radosław Szmit"
categories = ["Java", "Wyzwania"]
date = "2018-04-20"
description = ""
featured = "java-kodolamacz.jpg"
featuredalt = ""
featuredpath = "/img/java"
linktitle = ""
title = "Wprowadzenie do świata języka Java"
type = "post"

+++

Wiele początkujących osób zadaje głównie dwa pytania:
1. Jaki język programowania wybrać na początek i dlaczego
1. Jak się uczyć programować

W tym poście spróbuję odpowiedzieć dość szeroko na obydwa z tych pytań :)

# Języki programowania

Programowanie to tak naprawdę rozmowa z komputerem, przekazywanie mu naszych poleceń, które on będzie wykonywać. Jednak do tego potrzebny jest odpowiedni język, który maszyna, czytaj komputer, zrozumie. Takich języków powstało naprawdę wiele. Niektóre są proste i łatwe, inne znowu wyrafinowane i zarazem skomplikowane. Każdy z języków ma swoje zalety i wady, stąd jest ich tak wiele - by można było wybrać taki, który najbardziej będzie się nadawał do naszego zadania lub taki, który będzie dla nas bardziej przyjazny.

Trzeba też pamiętać, że nie każdy język programowania służy do tego samego. Języki takie jak HTML, CSS czy JavaScript (nie mylić z Javą!) służą przede wszystkim do tworzenia stron internetowych i są zrozumiałe dla przeglądarek internetowych takich jak Firefox, Chrome czy Edge. Z drugiej strony bardzo popularny język SQL (ang. Structured Query Language) jest językiem służącym do komunikacji z bazami danych, czyli narzędziami służącymi do przechowywania informacji zgodnie z określonymi regułami. Język PHP jest bardzo popularny w zastosowaniach serwerowych służących do generowania dynamicznie naszych stron internetowych (w przeciwieństwie do HTML, CSS i Javascript nie działa po stronie przeglądarki internetowej tylko serwera). Języki takie jak C, C++ czy C# bardzo często używane są do tworzenia aplikacji okienkowych, najprawdopodobniej większość waszych aplikacji zainstalowanych w komputerze jest stworzona w oparciu o te języki programowania. Oczywiście to tylko przykłady, języków programowania jest dużo więcej oraz ich zastosowania bywają szersze niż wskazane powyżej.

# Język Java

My zachęcamy Was do rozpoczęcia swojej przygody w językiem Java. Jest to obecnie jeden z popularniejszych języków programowania, bardzo wszechstronny i zarazem wydajny.

## Historia

Prace nad stworzeniem języka Java rozpoczęły się już w 1991 roku. Autorami projektu byli James Gosling, Mike Sheridan, i Patrick Naughton. Początkowo język nazywał się *Oak* po drzewie o tej samej nazwie, co w języku polskim oznacza Dąb. Inspiracja pochodziła od drzewa, które rosło obok gabinetu James'a Gosling'a uznawanego obecnie za ojca języka Java. Sam projekt, w ramach którego powstał ten język, nosił kryptonim *Green*. Następnie z powodu zastrzeżenia nazwy przez Oak Technologies, język zyskał nazwę **Java** pochodzącą od kawy z wyspy Jawa w Indonezji (ang. Java coffee, https://www.javaworld.com/article/2077265/core-java/so-why-did-they-decide-to-call-it-java-.html).

Informacja o stworzeniu języka Java została podana publicznie w 23 maja 1995 roku, zaś pierwsza wersja do ściągnięcia ukazała się 23 stycznia 1996 roku (http://oracle.com.edgesuite.net/timeline/java/). Obecnie najnowsza wersja języka Java opatrzona jest numerem 10 i została wydana 20 marca 2018 roku, zaś kolejne mają ukazywać się co pół roku.

Za powstaniem języka Java stoi firma Sun Microsystems (skrót od Stanford University Network), producent sprzętu i oprogramowania komputerowego oraz rozwiązań sieciowych. 27 stycznia 2010 roku Sun został przejęty przez firmę Oracle, która jest drugim na świecie, po firmie Microsoft, sprzedawcą oprogramowania (https://www.pwc.com/gx/en/industries/technology/publications/global-100-software-leaders/explore-the-data.html).

## Cechy języka Java

Składnia języka Java bazuje na jednym z najpopularniejszych w tamtych czasach języku programowania jakim był C++, dzięki czemu była bardzo łatwa do nauki dla dużego grona programistów oraz była od razu językiem obiektowym. Jednak w przeciwieństwie do języka C++, Java miała być dużo łatwiejsza do tworzenia oprogramowania, przez co miała skrócić czas niezbędny na napisanie programu. Osiągnięto to przede wszystkim przez wprowadzenie prostszego i automatycznego zarządzania pamięcią. Programiści nie muszą już samodzielnie zwalniać pamięci dzięki wprowadzeniu narzędzia zwanego *Garbage Collector* oraz nie ma możliwości korzystania ze wskaźników do pamięci operacyjnej, który sprzyjał nadpisywaniu trzymanych tam informacji, a tym samym awariom programu czy utratom danych. W języku Java wprowadzono także wiele uproszczeń w składni względem języka C++. Celem twórców było stworzenie języka, który miał być prostszą wersją języka C++ z zachowaniem wszystkich zalet języka takich jak np. bardzo duża wydajność.

Początkowo miała być wykorzystywana do tworzenia oprogramowania dla urządzeń telewizyjnych, gdzie wcześniej używany był język C++. Niestety korzystanie z tego języka wymagało często tworzenia dedykowanego kodu na każde urządzenie, dlatego główną ideą która przyświecała twórcom Javy była "Write Once, Run Anywhere", czyli napisz raz swój program i uruchamiaj gdziekolwiek. Tak powstała maszyna wirtualna Java umożliwiająca uruchamianie wszędzie tego samego kodu napisanego w języku Java. Możliwość uruchamiania programów napisanych w Javie na wielu systemach i urządzeniach nazywamy także multiplatformowością.

Trzecią ważną ideą, która przyświecała twórcom języka Java, poza automatycznym czyszczeniem pamięci oraz uruchamianiem tego samego programu na wielu platformach, miała być uniwersalność języka. Java miała się stać językiem wszechstronnym, w którym można pisać aplikacje okienkowe, serwerowe, internetowe, mobilne oraz w dalszym ciągu umożliwiać tworzenie oprogramowania dla urządzeń, tak zwanych systemów wbudowanych (https://pl.wikipedia.org/wiki/System_wbudowany). Java też jest językiem, który sprzyja wykorzystaniu go w warunkach sieciowych dzięki dużemu bezpieczeństwu i wbudowanym mechanizmom obsługi komunikacji sieciowej.

## Popularność

W internecie bardzo łatwo znaleźć serwisy, które za pomocą wielu różnych metod próbują oszacować popularność języków programowania. Do najpopularniejszych należą:

* https://www.tiobe.com/tiobe-index/
* https://insights.stackoverflow.com/survey/2018/#technology-programming-scripting-and-markup-languages
* https://octoverse.github.com/
* http://pypl.github.io/PYPL.html

Jak widać z powyższych statystyk, Java jest czołowym językiem programowania wykorzystywanym od lat.

## Zastosowania

O popularności języka Javy zdecydowała w dużej mierze jego wszechstronność. Możemy z jego pomocą tworzyć aplikacje okienkowe, serwerowe, mobilne (np. na systemie Android), a nawet spotkać programy napisane w tym języku także w urządzeniach takich jak telewizory, odtwarzacze Blu-Ray czy karty chipowe.

## Wydajność

Język Java dzięki wielu optymalizacjom jest także jednym z wydajniejszych języków programowania o czym możemy się przekonać analizując wyniki testów zamieszczonych na stronie: https://benchmarksgame.alioth.debian.org/.

## Możliwości

Język Java oferuje także wiele możliwości swoim użytkownikom. Jest przede wszystkim językiem obiektowym, ale od wersji 8 języka Java możemy także programować w nim funkcyjnie. O obiektowości i funkcyjności języka Java będziemy mówić w oddzielnych wyzwaniach. Java jest też językiem wielowątkowym, czyli takim który umożliwia wykonywanie wielu poleceń na raz (równolegle na wielu rdzeniach procesora).

## Wirtualna maszyna Java

Java to język programowania. To już wiemy. Ale to nie wszystko. Java to także *wirtualna maszyna Java* (ang. Java Virtual Machine, w skrócie JVM) na której możemy uruchamiać programy napisane w języku Java i nie tylko w tym języku! Obecnie na JVM można uruchamiać wiele innych języków. Do najpopularniejszych z nich należą: Scala, Kotlin, Groovy, Clojure oraz języki, które nie powstały z myślą o pracy w wirtualnej maszynie Java, lecz zostały później zaadaptowane jak np. Ruby (JRuby), Python (Jython).

## Platforma Java

Oprócz języków programowania i maszyny wirtualnej, często mówimy także o "platformach" dostępnych dla środowiska Java. Do tych platform należą:
* Java Standard Edition (Java SE) - aplikacje uruchamiane w konsoli lub aplikacje okienkowe
* Java Enterprise Edition (Java EE) - aplikacje serwerowe
* Java Micro Edition (Java ME) - aplikacje mobilne lub działające na urządzeniach o niskiej wydajności
* Java FX - aplikacje RIA (rich internet applications) oraz aplikacje okienkowe
* Java Card - mikro aplikacje uruchamiane na kartach elektronicznych

Platforma Java to tak naprawdę zbiór bibliotek i narzędzi umożliwiających lub ułatwiających tworzenie oprogramowania dla konkretnych zastosowań, np. aplikacje serwerowe w Java EE.

Java oraz Kotlin są także językami używanymi do pisania aplikacji mobilnych na systemie Android tworzonego przez firmę Google (https://developer.android.com/samples/index.html).

## Społeczność

Java to także, a może przede wszystkim, olbrzymia społeczność programistów z całego świata, którzy nie tylko pomagają sobie wzajemnie, ale także dzielą się swoim oprogramowaniem na zasadach Open Source, czyli otwartego kodu. Najpopularniejsze biblioteki i narzędzia tworzone są przez fundację Apache (http://www.apache.org/). Wiele też firm tworzy swoje oprogramowanie i udostępnia je innym firmom i programistom za darmo lub na podstawie płatnych licencji. Jednym z najpopularniejszych portali gdzie możemy znaleźć ogromną ilość oprogramowania i bibliotek napisanych w języku Java (oraz innych używanych na JVM), dostępnych na zasadach otwartego kodu źródłowego, jest portal GitHub (https://github.com/).

Język Java, maszyna wirtualna Java oraz platformy Java, także są tworzone przez społeczność. Właścicielem praw do języka Java jest firma Oracle, mimo to, sam proces wprowadzania nowych zmian i funkcjonalności realizowany jest przez społeczność programistów i firm z całego świata w ramach procesu zwanego *Java Community Process, JCP* (https://jcp.org). Listę zaangażowanych firm można znaleźć pod adresem: https://jcp.org/en/participation/members/. Wśród nich możemy odnaleźć takie firmy jak Google, Hewlett Packard Enterprise, Intel Corp, Twitter, Red Hat, IBM, Fujitsu Limited oraz wiele, wiele innych. Zmiany wprowadzane są w postaci tak zwanych *Java Specification Requests, JSR*, które podlegają ocenie i głosowaniu. Cały proces opisany jest pod adresem: https://jcp.org/en/procedures/overview.

## Oracle Java vs Microsoft .NET

Niewiarygodny sukces platformy Java TODO





# A na koniec coś na poprawę humoru :)

{{< youtube id="kLO1djacsfg" >}}


Legenda:

* https://www.javaworld.com/article/2077265/core-java/so-why-did-they-decide-to-call-it-java-.html
* https://pl.wikipedia.org/wiki/Java_(kawa)
* https://en.wikipedia.org/wiki/Coffee_production_in_Indonesia#Java
* http://www.oracle.com/technetwork/java/javase/overview/javahistory-index-198355.html
* http://oracle.com.edgesuite.net/timeline/java/
* https://www.pwc.com/gx/en/industries/technology/publications/global-100-software-leaders/explore-the-data.html
* https://benchmarksgame.alioth.debian.org/
* https://docs.oracle.com/cd/E19798-01/821-1770/gcrkk/index.html
* http://www.apache.org/
* https://jcp.org/en/procedures/overview
* https://jcp.org/en/participation/members/