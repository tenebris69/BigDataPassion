+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-08-07T16:10:38+02:00"
description = ""
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Zaawansowane aspekty wykorzystania RDD w Apache Spark"
type = "post"

+++

W tym poście powiemy sobie o pewnych aspektach działania Sparka w kontekście przetwarzania RDD.

# Opóżnione wykonywanie i przechowywanie RDD

Spark podobnie jak wiele innych narzędzia Big Data takich jak Apache Pig, nie wykonuje naszego programu krok po kroku, tylko odkłada sobie polecenia w pamięci i wykonuje dopiero wtedy gdy są potrzebne. Zaletą takiego podejścia jest wykonywanie tylko tych operacji które były niezbędne oraz możliwość ich optymalizacja przez połączenie ich w prostszą operację (np. kilka operacji filtrowania).

Niestety gdy na tym samym zbiorze chcemy wykonać kilka akcji Spark za każdym razem wykona całą listę poleceń potrzebnych do jego uzyskania. Możemy tego uniknąć przez nakazanie Sparkowi zapisania danych zgromadzonych w pośrednich i tymczasowych RDD tak by można było je reużywać. Jest to niezwykle potrzebne w przypadku algorytmów iteracyjnych.

Zapisu możemy dokonać za pomocą funkcji *persist(storageLevel)* która przyjmuje sposób w jaki dane mają być zapisane w postaci jednej z poniższych wartości:
 
Poziom  | Opis
------------- | -------------
MEMORY_ONLY | Domyślny poziom, zapisuje RDD tylko w pamięci RAM jako obiekty maszyny wirtualnej Java (JVM). Jeśli RDD nie zmieści się do pamięci, pewne jego fragmenty (partycje) będą wyliczone w locie kiedy będą potrzebne.
MEMORY_AND_DISK | Przechowuje RDD w pamięci RAM jako obiekty JVM, jednakże jeśli zabraknie pamięci pozostałe fragmenty (partycje) są zapisywane na dysku i stamtąd wczytywane w razie potrzeby
MEMORY_ONLY_SER | Zapisuje dane RDD w pamięci RAM ale w postaci zserializowanych obiektów przez co zajmuje mniej miejsca ale zużywa więcej mocy procesora na proces serializacji i deserializacji (dostępny tylko dla języka Java i Scala)
MEMORY_AND_DISK_SER | Dane trzymane są analogicznie jak w MEMORY_ONLY_SER z tym wyjątkiem, że gdy zabraknie pamięci RAM pozostałe partycje RDD są zapisywane na dysku jako zserializowane obiekty (dostępny tylko dla języka Java i Scala)
DISK_ONLY | Zapisuje całe RDD tylko na dysku, dużo wolniejszy zapis i odczyt ale nie zajmuje pamięci RAM
MEMORY_ONLY_2, MEMORY_AND_DISK_2, etc. | Analogicznie do innch poziomów, jednak każda partycja zapisywana jest na dwóch węzłach klastra (replikacja)
OFF_HEAP (eksperymentalna) | Podobnie do MEMORY_ONLY_SER z tym, że używa pamięci off-heap maszyny wirtualnej (jest to część pamięci maszyny wirtualnej która nie jest zarządzana przez Garbage Collector co zwiększa jej wydajność)

UWAGA W przypadku języka Python dostępne są jedynie poziomy: MEMORY_ONLY, MEMORY_ONLY_2, MEMORY_AND_DISK, MEMORY_AND_DISK_2, DISK_ONLY, and DISK_ONLY_2 zaś wszystkie obiekty są zawsze serializowane za pomocą biblioteki *Pickle*
 
Gdy wywołamy funkcję *persist* bez parametrów zostanie użyty domyślny poziom zapisu, czyli MEMORY_ONLY. Na RDD mamy jeszcze funkcję *cache* która tak naprawdę po wywołaniu wykorzystuje funkcję *persist* bez parametrów, co oznacza że powinniśmy ją traktować jedynie jako alias dla metody *persist* bez parametrów.

Gdy zaczyna brakować pamięci RAM, Spark czyści swój cache zgodnie z algorytmem LRU (least-recently-used), czyli usuwane są dane najdłużej ostatnio nieużywane. Istnieje także możliwość ręcznego zwolnienia pamięci za pomocą metody *unpersist*.
