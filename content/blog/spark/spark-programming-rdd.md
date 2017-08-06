+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-08-03T02:00:34+02:00"
description = "Wprowadzenie do programowania Apache Spark z wykorzystaniem RDD"
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Spark programming - Resilient Distributed Datasets"
type = "post"

+++

# Wprowadzenie

TODO


~~~Java
val lines = sc.textFile("README.md")
lines.count()
lines.first()
~~~

~~~Python
lines = sc.textFile("README.md")
lines.count()
lines.first()
~~~

Aplikacja składa się z programu sterownika i klastra obliczeniowego, tam mamy węzły wykonawcze

SparkContext reprezentuje połączenie z klastrem, od niego wszystko się zaczyna




RDD ofertują transformacje i akcje

Filtracja:
~~~Java
val lines = sc.parallelize(List("pizza", "hamburger", "lasagne"))
lines.count()
lines.first()
~~~

~~~Python
lines = sc.parallelize(["pizza", "hamburger", "lasagne"])
lines.count()
lines.first()
~~~

# Tworzenie RDD

W realnych zastosowaniach RDD tworzy się poprzez wczytanie danych z rozproszonych zasobów sieciowych takich jak na przykład HDFS, Cassandra, MongoDB, Amazon S3 i wielu innych. Mając doczynienia z dużo mniejszymi zbiorami, możemy je zaczytać w programie sterownika i przekształcić programowo w rozproszony zbiór RDD. Pomoże nam w tym funkcja *parallelize* na obiekcie SparkContext (domyślnie dostępny w Shell dla języka Scala i Python). Poniżej przykłady utworzenia takiego zbioru RDD z utworzonej kolekcji w programie sterownika.

Scala:
~~~Java
val lines = sc.parallelize(List("pizza", "hamburger", "lasagne"))
~~~

Python:
~~~Python
lines = sc.parallelize(["pizza", "hamburger", "lasagne"])
~~~

# Transformacje

## Mapowanie wartości

Jedną z najpopularnieszych tranformacji jest *map* pozwalająca na zmianę obiektów jednego RDD na zupełnie inne obiekty w wynikowym RDD. Typ obiektu wejściowego i wynikowego mogą się różnić, np. liczby możemy zmienić na stałe napisowe.

Poniżej przykład mapowania RDD zawierającego liczby na nowe RDD z ich kwadratami.

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,3,4,5))
val squared = nums.map(x => x * x)
val result = squared.collect()
println(result.mkString("\n"))
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,3,4,5])
squared = nums.map(lambda x: x * x)
results = squared.collect()
for result in results:
    print result
~~~

UWAGA
Do wyświetlenia wynikowego RDD użyliśmy funkcji *collect* która zwróciła tablicę wszystkich elementów w RDD. Przy bardzo dużych RDD ściągnięcie wszystkich wartości do programu sterownika może wygenerować błąd braku wystarczającej ilości pamięci RAM.

Jeśli chcemy by nowe RDD miało więcej obiektów, możemy zwrócić RDD zawierające kolekcje (jeden obiekt wejściowy mapujemy w oddzielną kolekcję) lub użyć metody *flatMap* zwracającą nowe RDD mające wszystkie nasze nowe obiekty.

Scala:
~~~Java
val lines = sc.parallelize(List("Ala ma kota", "Witaj świecie", "Dwadzieścia tysięcy mil podmorskiej żeglugi"))
val words = lines.flatMap(line => line.split(" "))
val result = words.collect()
println(result.mkString("\n"))
~~~

Python:
~~~Python
lines = sc.parallelize(["Ala ma kota", "Witaj świecie", "Dwadzieścia tysięcy mil podmorskiej żeglugi"])
words = lines.flatMap(lambda line: line.split(" "))
results = words.collect()
for result in results:
    print result
~~~

## Filtrowanie wartośći

Drugą bardzo popularną transformacją jest funkcja *filter* zwracająca nowe RDD z elementami które spełniły wskazany warunek.

Scala:
~~~Java
val lines = sc.textFile("README.md")
val sparkLines = lines.filter(line => line.contains("Spark"))
sparkLines.count()
sparkLines.first()
~~~

~~~Python
lines = sc.textFile("README.md")
sparkLines = lines.filter(lambda line: "Spark" in line)
sparkLines.count()
sparkLines.first()
~~~

## Pomijanie duplikatów

Jeśli chcemy pominąć duplikaty w naszym rozproszonym RDD możemy skorzystać z funkcji *distinct*.

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,2,3,4,4,5))
val distinct = nums.distinct()
println(distinct.collect().mkString(", "))
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,2,3,4,4,5])
distinct = nums.distinct()
print(distinct.collect())
~~~

## Próbkowanie

Czasami chcemy przetestować nasz algorytm na mniejszej, czy nawet znacznie mniejszej próbce danych. Idealnie nadaje się do tego funkcja *sample* która pozwala zwrócić nam tylko część naszego RDD.

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,3,4,5))
val sample = nums.sample(false, 0.5)
println(sample.collect().mkString(", "))
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,3,4,5])
sample = nums.sample(False, 0.5)
print(sample.collect())
~~~

Trzeba pamiętać, że algorytm jest niederteministyczny, co oznacza, że każde wywołanie programu zwraca nam zupełnie inny wynik.

## Operacje na wielu zbiorach

Transformacje nie muszą pracować na jednym RDD ale mogą także operować na wielu zbiorach. Podstawowymi transformacjami działającymi na wielu RDD są:
* union - łączenie dwóch RDD
* intersection - zwraca część wspólną obydwu RDD (bez duplikatów)
* subtract - różnica dwóch zbiorów
* cartesian - iloczyn kartezjański, zwraca połączenie każdego elementu pierwszego zbioru z każdym elementem drugiego zbioru

Scala:
~~~Java
val rdd1 = sc.parallelize(List(1,2,3,4,5))
val rdd2 = sc.parallelize(List(4,5,6,7))

val union = rdd1.union(rdd2)
println(union.collect().mkString(", "))

val intersection = rdd1.intersection(rdd2)
println(intersection.collect().mkString(", "))

val subtract = rdd1.subtract(rdd2)
println(subtract.collect().mkString(", "))

val cartesian = rdd1.cartesian(rdd2)
println(cartesian.collect().mkString(", "))
~~~

Python:
~~~Python
rdd1 = sc.parallelize([1,2,3,4,5])
rdd2 = sc.parallelize([4,5,6,7])

union = rdd1.union(rdd2)
print(union.collect())

intersection = rdd1.intersection(rdd2)
print(intersection.collect())

subtract = rdd1.subtract(rdd2)
print(subtract.collect())

cartesian = rdd1.cartesian(rdd2)
print(cartesian.collect())
~~~

Trzeba jednak pamiętać, że w projektach Big Data takie operacje na dużych zbiorach bywają niezwykle kosztowne obliczeniowo a czasami nawet nie możliwe do przeprowadzenia, np. iloczyn kartezjański dwóch bardzo licznych zbiorów może wygenerować tak duży zbiór wynikowy, którego nie będziemy w stanie zapisać ani w pamięci RAM ani także na naszym dysku. Dodatkowo operacje na zbiorach rozproszonych często mogą spowodować bardzo dużo połączeń i przesunięc danych z jednego serwera na drugi, co także znacząco spowolni nasze obliczenia (nie bazujemy tutaj na tak zwanej lokalności danych).



# Akcje

## Redukcja RDD

Jedną z najpopularniejszych akcji używanych w Apache Spark jest *reduce* pozwalająca na zredukowanie całego RDD do pojedynczego obiektu tego samego typu co elementy RDD.

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,3,4,5))
val sum = nums.reduce( (x, y) => x + y)
println(sum)
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,3,4,5])
sum = nums.reduce(lambda x, y: x + y)
print(sum)
~~~

Podobnie działa funkcja *fold* umożliwiająca redukcję z wartością startową (wartość ta nie powinna wpływać na wynik, np. pusta lista lub wartość zero dla sumy).

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,3,4,5))
val sum = nums.fold(0)((x, y) => x + y)
println(sum)
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,3,4,5])
sum = nums.fold(0, lambda x, y: x + y)
print(sum)
~~~

Jeśli dodatkowo chcemy zmienić typ zwracanej wartości, musimy skorzystać z jeszcze innej funkcji o nazwie *aggregate* która także przyjmuje wartość startową. Do *aggregate* musimy przekazać dwie funkcje, pierwsza służy do redukcji w ramach partycji (danych zgromadzonych fizycznie razem na jednej maszynie) a następnie drugiej funkcji która łączy ze sobą wyniki z poprzedniego kroku.

W poniższym przykładzie zwrócimy krotkę (tuple) zawierającą zarówno sumę jak i liczbę obiektów, co wykorzystamy do wyliczenia średniej.

Scala:
~~~Java
val nums = sc.parallelize(List(1,2,3,4,5,6))
val sumCount = nums.aggregate((0, 0))(
    (acc, value) => (acc._1 + value, acc._2 + 1),
    (acc1, acc2) => (acc1._1 + acc2._1, acc1._2 + acc2._2)
)
println(sumCount)
val avg = sumCount._1.toDouble / sumCount._2.toDouble
println(avg)
~~~

Python:
~~~Python
nums = sc.parallelize([1,2,3,4,5,6])
sumCount = nums.aggregate((0,0),
    (lambda acc, value: (acc[0] + value, acc[1] + 1) ),
    (lambda acc1, acc2: (acc1[0] + acc2[0], acc1[1] + acc2[1]) )
)
print(sumCount)
avg = float(sumCount[0]) / float(sumCount[1])
print(avg)
~~~

## Pobieranie danych do programu sterownika

Jeśli po przetworzeniu nasze RDD jest na tyle małe, że może nam się zmieścić w pamięci RAM maszyny na której uruchomiliśmy nasz program sterownik, możemy skorzystać z możliwości ściągnięcia ich na tą maszynę i tam zapisanie lub dalsze przetworzenie.

Najprostrzym sposobem na to jest używana we wcześniejszych przykładach funkcja *collect* zwracająca RDD jako zwykła kolekcja obiektów.

W sytuacji gdy nie potrzebujemy pobierać całego RDD na lokalną maszynę możemy użyć funkcji *take(n)* zwracająca n dowolnych elementów, *top(n)* zwracająca najwyższe elementy, *takeOrdered* analogiczna do top ale umożliwiająca przekazanie własnej funkcji porównującej oraz *takeSample(withReplacement, n, seed)* zwracająca n elementów w losowy sposób.

Jeszcze inną przydatną funkcją jest *first* zwracająca pierwszy element RDD.

## Zliczanie ilości

Jeśli chcemy sprawdzić ile elementów zawiera nasz RDD możemy skorzystać z funkcji *count* oraz *countByValue* zwracająca ilość unikalnych elementów w postaci mapy element -> liczność.

## Wykonywanie operacji na każdym elemencie

Jeśli zajdzie nam potrzeba wykonania jakiejś operacji na każdym elemencie w sposób rozproszony bez konieczności zwracania jej wyniku, możemy skorzystać z funkcji *foreach*.



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



Scala:
~~~Java
~~~

Python:
~~~Python
~~~




# Legenda
* https://docs.python.org/2/library/pickle.html