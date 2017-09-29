+++
author = "Radosław Szmit"
categories = ["HBase"]
date = "2017-04-08T17:08:29+02:00"
description = "Przykłady korzystania HBase za pomocą linii komend"
featured = "Killer_whale_in_water.jpg"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Hbase shell - przykłady"
type = "post"

draft = true
+++

Kilka przykładów wyjaśniających zasadę działania bazy HBase


# stworzenie i usunięcie tabeli, lista tabel
~~~ruby
create 'person', 'cf'
exists 'person'
list 'per.*'
disable 'person'
drop 'person'
~~~

# Dodawanie i pobieranie wiersza
~~~ruby
create 'person', 'cf'
put 'person', '1', 'cf:forename', 'Jan'
put 'person', '1', 'cf:surname', 'Nowak'
put 'person', '1', 'cf:city', 'Warszawa'
get 'person', '1'
disable 'person'
drop 'person'
~~~

# Zwiększanie i wyświetlanie licznika
~~~ruby
create 'person', 'cf'
put 'person', '1', 'cf:forename', 'Jan'
put 'person', '1', 'cf:surname', 'Nowak'
put 'person', '1', 'cf:city', 'Warszawa'
incr 'person', '1', 'cf:score', 1
incr 'person', '1', 'cf:score', 10
get 'person', '1'
get_counter 'person', '1', 'cf:score' #powinno być 11
disable 'person'
drop 'person'
~~~

# Dodawanie i przeglądanie wielu wierszy
~~~ruby
create 'person', 'cf'
put 'person', '1', 'cf:forename', 'Jan'
put 'person', '1', 'cf:surname', 'Nowak'
put 'person', '1', 'cf:city', 'Warszawa'
put 'person', '2', 'cf:forename', 'Anna'
put 'person', '2', 'cf:surname', 'Kozłowska'
put 'person', '2', 'cf:city', 'Gdańsk'
put 'person', '3', 'cf:forename', 'Tomasz'
put 'person', '3', 'cf:surname', 'Kowalski'
put 'person', '3', 'cf:city', 'Kraków'
scan 'person'
disable 'person'
drop 'person'
~~~

# Wyszukiwanie w Scan
~~~ruby
create 'person', 'cf'
for i in 1..10
   put 'person', i, 'cf:forename', "Imie #{i}"
   put 'person', i, 'cf:surname', "Nazwisko #{i}"
   put 'person', i, 'cf:city', "Miasto #{i}", i
end
scan 'person', {COLUMNS => ['cf:forename', 'cf:surname'], CACHE_BLOCKS => false}
scan 'person', {COLUMNS => ['cf:forename', 'cf:surname'], LIMIT => 2, STARTROW => '2'}
scan 'person', {COLUMNS => 'cf:city', TIMERANGE => [1, 5]} # z prawej otwarty
scan 'person', {REVERSED => true}
show_filters
scan 'person', {FILTER => org.apache.hadoop.hbase.filter.ColumnPaginationFilter.new(1, 0)} #uwaga kolumny są sortowane!!!, limit i offset kolumn
scan 'person', {LIMIT => 10, COLUMNS => 'cf:forename', FILTER => org.apache.hadoop.hbase.filter.RandomRowFilter.new(0.5)}
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Wersja #{i}"
end
scan 'person'
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10} #zwraca usunięte wiersze niewidoczne normalnie
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami (flush)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Wersja #{i}"
end
scan 'person'
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10}
flush 'person'
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~



# Usuwanie danych
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
for i in 1..5 # wiersze
   for j in 1..3 # wersje
      put 'person', i, 'cf:forename', "Imie #{i}.#{j}", j
      put 'person', i, 'cf:surname', "Nazwisko #{i}.#{j}", j
   end
end

deleteall 'person', 1 # usuwa cały wiersz (rodzinę)
deleteall 'person', 2, 'cf:forename' # usuwa całą kolumnę
deleteall 'person', 3, 'cf:forename', 4 # usuwa starsze wersje w kolumnie

delete 'person', 4, 'cf:forename' # usuwa całą kolumnę
delete 'person', 5, 'cf:forename', 4 # usuwa starsze wersje w kolumnie

scan 'person'
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10}

disable 'person'
drop 'person'
~~~








# Zabawa z wersjami (delete version)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Imie #{i}", i
   put 'person', 1, 'cf:surname', "Nazwisko #{i}", i
end
delete 'person', 1, 'cf:forename', 5
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami (delete whole cell)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Imie #{i}", i
   put 'person', 1, 'cf:surname', "Nazwisko #{i}", i
end
delete 'person', 1, 'cf:forename'
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami (delete whole row)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Imie #{i}", i
   put 'person', 1, 'cf:surname', "Nazwisko #{i}", i
end
deleteall 'person', 1
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami (delete i raw)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Wersja #{i}", i
end
delete 'person', 1, 'cf:forename', 5
scan 'person'
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10}
flush 'person'
scan 'person', {RAW => true, VERSIONS => 10}
major_compact 'person'
scan 'person', {RAW => true, VERSIONS => 10}
sleep 5
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami (delete i keep deleted cells)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5, KEEP_DELETED_CELLS => true}
for i in 1..7
   put 'person', 1, 'cf:forename', "Wersja #{i}", i
end
delete 'person', 1, 'cf:forename', 5
scan 'person'
scan 'person', {VERSIONS => 10}
scan 'person', {RAW => true, VERSIONS => 10}
flush 'person'
scan 'person', {RAW => true, VERSIONS => 10}
major_compact 'person'
sleep 5
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami (TTL)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5, TTL => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Wersja #{i}"
end
scan 'person', {RAW => true, VERSIONS => 10}
sleep 5
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~

# Zabawa z wersjami (TTL i MIN_VERSIONS)
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5, MIN_VERSIONS => 2, TTL => 5}
for i in 1..7
   put 'person', 1, 'cf:forename', "Wersja #{i}"
end
scan 'person', {RAW => true, VERSIONS => 10}
sleep 5
scan 'person', {RAW => true, VERSIONS => 10}
disable 'person'
drop 'person'
~~~

# Praca na zmiennych
~~~ruby
person = create 'person', {NAME => 'cf', VERSIONS => 3}
for i in 1..3
   for j in 1..3
      person.put i, 'cf:forename', "Imie #{j}"
      person.put i, 'cf:surname', "Nazwisko #{j}"
      person.put i, 'cf:city', "Miasto #{j}"
   end
end
person.scan
person.disable
person.drop
~~~

# Tworzenie tabel ze zdefiniowanym splitem (podziałem regionów)
~~~ruby
create 'person', 'cf', SPLITS=> ['a', 'b', 'c']
get_splits 'person'
scan 'hbase:meta',{FILTER=>"PrefixFilter('person')"} # alternatywny sposób sprawdzenia regionów
disable 'person'; drop 'person'
~~~