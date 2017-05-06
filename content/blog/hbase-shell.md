+++
author = "Radosław Szmit"
categories = ["HBase"]
date = "2017-04-07T17:08:29+02:00"
description = "Korzystanie z HBase za pomocą linii komend"
featured = "killer_whale1.jpg"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Hbase shell"
type = "post"

+++

Poniżej podstawy korzystania z HBase za pomocą linii komend (tak zwany Shell) oraz przegląd większośći dostępnych poleceń.

# Podstawowy HBase Shell #

Uruchomienie
~~~shell
[hbase@hadoop1 ~]$ hbase shell
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version 1.1.2.2.5.3.0-37, rcb8c969d1089f1a34e9df11b6eeb96e69bcf878d, Tue Nov 29 18:48:22 UTC 2016
hbase(main):001:0>  
~~~

Wyjście z konsoli
~~~ruby
exit
~~~
lub Ctrl+C / Ctrl+D

Pomoc dla Shell'a możemy wywołać poleceniem
~~~ruby
help
~~~

w wyniku otrzymamy całą listę dostępnych poleceń pogrupowanych ze względu na przeznaczenie

* general - status, table_help, version, whoami
* ddl - alter, alter_async, alter_status, create, describe, disable, disable_all, drop, drop_all, enable, enable_all, exists, get_table, is_disabled, is_enabled, list, locate_region, show_filters
* namespace - alter_namespace, create_namespace, describe_namespace, drop_namespace, list_namespace, list_namespace_tables
* dml - append, count, delete, deleteall, get, get_counter, get_splits, incr, put, scan, truncate, truncate_preserve
* tools - assign, balance_switch, balancer, balancer_enabled, catalogjanitor_enabled, catalogjanitor_run, catalogjanitor_switch, close_region, compact, compact_rs, flush, major_compact, merge_region, move, normalize, normalizer_enabled, normalizer_switch, split, splitormerge_enabled, splitormerge_switch, trace, unassign, wal_roll, zk_dump
* replication - add_peer, append_peer_tableCFs, disable_peer, disable_table_replication, enable_peer, enable_table_replication, list_peers, list_replicated_tables, remove_peer, remove_peer_tableCFs, set_peer_tableCFs, show_peer_tableCFs
* snapshots - clone_snapshot, delete_all_snapshot, delete_snapshot, list_snapshots, restore_snapshot, snapshot, snapshot_all, snapshot_restore
* configuration - update_all_config, update_config
* quotas - list_quotas, set_quota
* security - grant, revoke, user_permission
* procedures - abort_procedure, list_procedures
* visibility labels - add_labels, clear_auths, get_auths, list_labels, set_auths, set_visibility
* rsgroup - add_rsgroup, balance_rsgroup, get_rsgroup, get_server_rsgroup, get_table_rsgroup, list_rsgroups, move_rsgroup_servers, move_rsgroup_tables, remove_rsgroup

Jeśli chcemy uzyskać pomoc dla jakiejś grupy lub polecenia wystarczy wpisać ich nazwę po słowie _help_

~~~ruby
help "ddl"
help "alter"
~~~

Dodatkowo jesteśmy informowania o pracy z językiem _Ruby_ w którym _HBase Shell_ został stworzony i podstawach jego składni.





# Podstawowe polecenia #

Sprawdzenie stanu bazy danych
~~~ruby
status
~~~

wywołanie
~~~shell
hbase(main):001:0> status
1 active master, 0 backup masters, 3 servers, 0 dead, 1.0000 average load
hbase(main):002:0> 
~~~
w wyniku dostajemy informację o liczbie działających usług sterujących i roboczych (master & slave), w naszym przypadku 1 usługa sterująca i 3 robocze. Ostatnia pozycja _average load_ mówi nam o średniej liczbie regionów udostępnianych (host) przez region serwer, wyliczana w momencie uruchomienia polecenia.

W razie problemów z bazą możemy dostać wyjątek mówiący np. braku połączenia z ZooKeeper'em
~~~shell
hbase(main):001:0> status

ERROR: Can't get master address from ZooKeeper; znode data == null

Here is some help for this command:
Show cluster status. Can be 'summary', 'simple', 'detailed', or 'replication'. The
default is 'summary'. Examples:

  hbase> status
  hbase> status 'simple'
  hbase> status 'summary'
  hbase> status 'detailed'
  hbase> status 'replication'
  hbase> status 'replication', 'source'
  hbase> status 'replication', 'sink'


hbase(main):002:0> 
~~~

Jak widać wyżej, status może być typu:

* simple
* summary
* detailed
* replication
* replication source
* replication sink

Gdy nie wybierzemy żadnego z powyższych, domyślnie jest brany status _summary_. Opcje _simple_ i _detailed_ zwracają nam więcej informacji o stanie poszczególnych usług (serwerów), zaś ostatnie 3 dotyczą replikacji bazy z inną jej instancją czemu poświęcony będzie oddzielny wpis.

~~~shell
hbase(main):065:0* status 'simple'
active master:  hadoop1:16000 1494057920582
0 backup masters
3 live servers
    hadoop2:16020 1494057949122
        requestsPerSecond=0.0, numberOfOnlineRegions=2, usedHeapMB=149, maxHeapMB=2007, numberOfStores=3, numberOfStorefiles=1, storefileUncompressedSizeMB=0, storefileSizeMB=0, memstoreSizeMB=0, storefileIndexSizeMB=0, readRequestsCount=4, writeRequestsCount=0, rootIndexSizeKB=0, totalStaticIndexSizeKB=0, totalStaticBloomSizeKB=0, totalCompactingKVs=0, currentCompactedKVs=0, compactionProgressPct=NaN, coprocessors=[SecureBulkLoadEndpoint]
    hadoop1:16020 1494057947164
        requestsPerSecond=0.0, numberOfOnlineRegions=1, usedHeapMB=181, maxHeapMB=2007, numberOfStores=1, numberOfStorefiles=1, storefileUncompressedSizeMB=0, storefileSizeMB=0, memstoreSizeMB=0, storefileIndexSizeMB=0, readRequestsCount=48, writeRequestsCount=2, rootIndexSizeKB=0, totalStaticIndexSizeKB=0, totalStaticBloomSizeKB=0, totalCompactingKVs=32, currentCompactedKVs=32, compactionProgressPct=1.0, coprocessors=[MultiRowMutationEndpoint, SecureBulkLoadEndpoint]
    hadoop3:16020 1494057948195
        requestsPerSecond=0.0, numberOfOnlineRegions=0, usedHeapMB=201, maxHeapMB=2007, numberOfStores=0, numberOfStorefiles=0, storefileUncompressedSizeMB=0, storefileSizeMB=0, memstoreSizeMB=0, storefileIndexSizeMB=0, readRequestsCount=0, writeRequestsCount=0, rootIndexSizeKB=0, totalStaticIndexSizeKB=0, totalStaticBloomSizeKB=0, totalCompactingKVs=0, currentCompactedKVs=0, compactionProgressPct=NaN, coprocessors=[]
0 dead servers
Aggregate load: 0, regions: 3

hbase(main):066:0>
~~~
W powyższym przykładzie wywołania polecenia _status 'simple'_ widzimy status dla każdej z maszyn używanych przez bazę, wraz z statystykami takimi jak liczba żądań (requestsPerSecond), liczbę dostępnych regionów (numberOfOnlineRegions), konfigurację pamięci (usedHeapMB, maxHeapMB), liczbę przestrzeni _Store_ i plików składujących _StoreFile_ (numberOfStores, numberOfStorefiles), wielkość tych plików wraz z indeksem (storefileUncompressedSizeMB, storefileSizeMB, storefileIndexSizeMB), wielkość sprzestrzeni _MemStore_ (memstoreSizeMB), liczbę żądań całkowitą (readRequestsCount, writeRequestsCount), wielkość głównego indeksu (rootIndexSizeKB), wielkość wszystkich indeksów (totalStaticIndexSizeKB), całkowita wielkość fitru Bloom (totalStaticBloomSizeKB), stan kompatkowania (totalCompactingKVs, currentCompactedKVs, compactionProgressPct) oraz zarejestrowane koprocesory (coprocessors). Parametr _Aggregate load_ jest wyznaczany z _requestsPerSecond_ ze wszystkich maszyn.


Sprawdzenie wersji na jakiej pracujemy
~~~shell
hbase(main):001:0> version
1.1.2.2.5.3.0-37, rcb8c969d1089f1a34e9df11b6eeb96e69bcf878d, Tue Nov 29 18:48:22 UTC 2016

hbase(main):002:0>
~~~

Sprawdzanie jakim jesteśmy użytkownikiem
~~~shell
hbase(main):002:0> whoami
hbase (auth:SIMPLE)
    groups: hadoop

hbase(main):003:0> 
~~~





# Zarządzanie schematem (Data Definition Language, DDL)


Nową tabelę możemy stworzyć za pomocą polecenia _create_
~~~ruby
create 'person', 'cf'
~~~
Musimy podać minimum dwa parametry, nazwę tabeli i nazwę rodziny kolumn (column family).

Możemy też od razu dodać więcej niż jedną rodzinę
~~~ruby
create 'person', 'cf1', 'cf2', 'cf3'
~~~

lub w ten sposób
~~~ruby
create 'person', {NAME => 'cf1'}, {NAME => 'cf2'}, {NAME => 'cf3'}
~~~

Każda rodzina może mieć przypisany zestaw parametrów które możemy zdefiniować już na poziomie polecenia _create_

* NAME - nazwa rodziny
* DATA_BLOCK_ENCODING - algorytm enkodowania kluczy głównych, do wyboru [NONE, PREFIX, DIFF, FAST_DIFF, PREFIX_TREE]
* BLOOMFILTER - Filtr Blooma [NONE, ROW (default), ROWCOL]
* REPLICATION_SCOPE - Mechanizm replikacji pomiędzy dwoma klastrami HBase zwiększające HA na poziomie dwóch różnych klastrów [0 - brak, 1 - aktywna replikacja]
* VERSIONS - maksymalna liczba wersji do przechowywania, domyślnie 1
* COMPRESSION - algorytm kompresji bloków [NONE, Snappy, LZO, LZ4, GZ]
* MIN_VERSIONS - minimalna liczba wersji, domyślnie 0, używać w połączeniu z TTL
* TTL - czas (sekundy) po który wersja jest usuwana (dotyczy także najnowszych)
* KEEP_DELETED_CELLS - Polecenia "delete" nie usuwają fizycznie danych lecz tylko oznaczają je jako usunięte, wciąż jest możliwość pobrania ich za pomocą poleceń Scan lub Get.
* BLOCKSIZE - wielkość bloku, domyślnie 64k, większe komórki wymagają większych bloków, mniejsze bloki to szybszy dostęp ale większe użycie pamięci RAM dla indeksów
* IN_MEMORY - sugestia dla HBase by silniej korzystał z cache'a dla tej rodziny (nie ma gwarancji że całość będzie w pamięci operacyjnej)
* BLOCKCACHE - cache blokowy przyśpieszający odczyty popularnych (często uzywanych) danych


Poniżej kilka przykładów stworzenia tabeli z różnymi parametrami
~~~ruby
create 'person', {NAME => 'cf', VERSIONS => 5}
create 'person', {NAME => 'f1', VERSIONS => 1, TTL => 2592000, BLOCKCACHE => true}
create 'person', {NAME => 'f1', CONFIGURATION => {'hbase.hstore.blockingStoreFiles' => '10'}}
~~~

Lista wszystkich istniejących tabel w bazie (pomija tabele systemowe bazy)
~~~ruby
list
~~~

Opis tabeli
~~~ruby
describe 'person'
~~~

przykład użycia
~~~shell
hbase(main):095:0* describe 'person'
Table person is ENABLED
person
COLUMN FAMILIES DESCRIPTION
{NAME => 'cf1', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}  
{NAME => 'cf2', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}  
{NAME => 'cf3', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}  
3 row(s) in 0.0370 seconds
hbase(main):096:0> 
~~~
Shell zwraca informację że tabela jest aktywna (ENABLED) oraz zwraca listę rodzin kolumn wraz z ich parametrami, każda w oddzielnym nawiasie klamrowym {}

Domyślnie każda tabela jest aktywna, co oznacza, że jest gotowa do operowania na niej. Wiele poleceń administracyjnych dotyczących tabel wymaga jednak by tabela była nieaktywna (disabled). Administrator może także w ten sposób zablokować dostęp do tej tabeli użytkownikom, np. na czas trwania backupu. Żeby zmienić stan tabeli na disabled należy użyć polecenia

~~~ruby
disable 'person'
~~~

Aby ponownie zrobić tabelę dostępną należy użyć odwrotnego polecenia

~~~ruby
enable 'person'
~~~

Jeśli chcemy zmienić status większej liczbie tabel możemy uruchomić je wiele razy w pętli lub użyć poleceń które przyjmują wyrażenia regularne
~~~ruby
disable_all 'regex' 
enable_all 'regex'
~~~
W tym przypadku shell wyświetli nam listę odnalezionych tabel i poprosi o potwierdzenie operacji
~~~shell
hbase(main):111:0* disable_all 'per.*'
person                                      
Disable the above 1 tables (y/n)?
1 tables successfully disabled
hbase(main):112:0> 
hbase(main):114:0* enable_all 'per.*'
person
Enable the above 1 tables (y/n)?
1 tables successfully enabled
hbase(main):115:0> 
~~~

Jeśli nie wiemy jaki stan ma jakaś tabela, to oprócz używania polecenia _describe_ możemy sprawdzić to poleceniami
~~~ruby
is_disabled 'table'
is_enabled 'table'
~~~

Jednym z poleceń wymagających dezaktywacji tabeli jest usuwanie tabel które także jest dostępne w wersji usuwającej jedną tabelę i takiej które przyjmuje wyrażenie regularne
~~~ruby
drop 'table'
drop_all 'regex' 
~~~

W każdej chwili możemy sprawdzić czy jakaś tabela istnieje poleceniem
~~~ruby
exists 'table'
~~~

Definicję tabel można także zmieniać już po ich stworzeniu za pomocą poleceń
~~~ruby
alter 'table'
alter_async 'table'
~~~
Drugie wywołanie jest asynchroniczne, to znaczy nie czekamy na zmianę na wszystkich regionach.

Za pomocą tych poleceń możemy dodawać, modyfikować i usuwać rodziny kolumn oraz zmieniać ich konfigurację.

**UWAGA** Operacje _alter_ na aktywnych tabelach (enabled) może powodować problemy, zwłaszcza w starszych wersjach dlatego w bazach produkcyjnych bezpieczniej jest robić to na nieaktywnych tabelach lub wcześniej przetestować polecenie na bazie testowej. Administratorzy mogą ustawić parametr "hbase.online.schema.update.enable" na false zmuszając użytkowników do wykonywania polecenia _alter_ tylko na tabelach nieaktywnych.

Przykład użycia wywołania synchronicznego (dodajemy trzecią rodzinę)
~~~ruby
create 'person', {NAME => 'cf1'}, {NAME => 'cf2'}
describe 'person'
alter 'person', {NAME => 'cf3'}
describe 'person'
disable 'person'; drop 'person'
~~~

W przypadku wywołania asynchronicznego status operacji możemy sprawdzić za pomocą polecenia
~~~ruby
alter_status 'table' 
~~~
przykład użycia (dodajemy trzecią rodzinę)
~~~ruby
create 'person', {NAME => 'cf1'}, {NAME => 'cf2'}
describe 'person'
alter_async 'person', {NAME => 'cf3'}
alter_status 'person'
describe 'person'
disable 'person'; drop 'person'
~~~

Jeśli chcemy usunąć rodzinę
~~~ruby
alter 'table', NAME => 'f1', METHOD => 'delete'
alter 'table', 'delete' => 'f1'
~~~

Jeśli modyfikujemy tylko jedną rodzinę, możemy użyć krótszej składni
~~~ruby
alter 't1', NAME => 'f1', VERSIONS => 5
~~~

Przy modyfikacji kilku rodzin trzeba już użyć nawiasów {}
~~~ruby
alter 't1', 'f1', {NAME => 'f2', IN_MEMORY => true}, {NAME => 'f3', VERSIONS => 5}
~~~

Możemy także zmienać parametry tabeli, np. MAX_FILESIZE, READONLY, MEMSTORE_FLUSHSIZE, DURABILITY (w HBase większość parametrów jest definiowanych na poziomie rodziny kolumn), poniżej przykład zmiany domyślnego rozmiaru regionu na 128MB
~~~ruby
alter 'table', MAX_FILESIZE => '134217728'
~~~

Oprócz ustawiania atrybutów tabel, możemy je usuwać
~~~ruby
alter 'table', METHOD => 'table_att_unset', NAME => 'MAX_FILESIZE'
~~~

Jeśli chcemy jednocześnie modyfikować rodziny kolumn oraz dodawać dodatkowe parametry możemy to zrobić jak na poniższym przykładzie
~~~ruby
alter 'table', { NAME => 'f1', VERSIONS => 3 }, { MAX_FILESIZE => '134217728' }, { METHOD => 'delete', NAME => 'f2' }, OWNER => 'johndoe', METADATA => { 'mykey' => 'myvalue' }
~~~

Przykład użycia
~~~ruby
create 'person', {NAME => 'cf1'}
alter 'person', NAME => 'cf1', VERSIONS => 5
describe 'person'

alter 'person', {NAME => 'cf2'}
alter 'person', {NAME => 'cf1', VERSIONS => 3}, {NAME => 'cf2', VERSIONS => 3}
describe 'person'

alter 'person', 'delete' => 'cf2'
describe 'person'

alter 'person', MAX_FILESIZE => '134217728'
describe 'person'

alter 'person', METHOD => 'table_att_unset', NAME => 'MAX_FILESIZE'
describe 'person'

alter 'person', { NAME => 'cf3', VERSIONS => 5}, { MAX_FILESIZE => '134217728' }, { METHOD => 'delete', NAME => 'cf1' }, OWNER => 'johndoe', METADATA => { 'mykey1' => 'myvalue1' }
describe 'person'

disable 'person'; drop 'person'
~~~

HBase ma wbudowaną listę filtrów (użyje ich przy okazji polecenia _scan_) którą to listę możemy wyświetlić poleceniem
~~~ruby
show_filters
~~~




# Ułatwienie pracy w HBase shell

Skoro językiem którym poslugujemy się w shellu jest język _Ruby_ to możemy skorzystać także z możliwości jakiej daje nam każdy język programowania, czyli zmiennych.

Instrukcję na temat używania zmiennych w Shell'u możemy dostać używając polecenia
~~~ruby
table_help
~~~

Dzięki temu możemy tworząc tabelę przypisać ją od razu do zmiennej
~~~ruby
t = create 't', 'cf'
~~~
a potem już tylko używać w naszym skrypcie

~~~ruby
t = create 'table', 'cf'
t.put 'r', 'cf:q', 'v'
t.scan
t.flush
t.disable
t.drop
~~~

Jeśli chcemy pracować na wcześniej utworzonej tabeli, możemy ją przypisać do zmiennej za pomocą polecenia

~~~ruby
t = get_table 'table'
~~~





# Manipulowanie danymi (Data Manipulation Language, DML)

Jeśli chcemy dodać nowe wiersze do stworzonej tabeli należy użyć polecenia
~~~ruby
put 'table', 'recordId', 'columnName', 'cellValue'
~~~
lub
~~~ruby
put 'table', 'recordId', 'columnName', 'cellValue', timestamp
~~~
Tym samym poleceniem dokonujemy także edycji, HBase jako baza wersjonowana nie ma tak naprawdę edycji znanej z innych baz, dodajemy do bazy nowe wersje wskazanych wierszy.

Aby pobrać wprowadzony wiersz do bazy należy użyć polecenia
~~~ruby
get 'table', 'r1'
get 'table', 'r1', {TIMERANGE => [ts1, ts2]}
get 'table', 'r1', {COLUMN => 'c1'}
get 'table', 'r1', {COLUMN => ['c1', 'c2', 'c3']}
get 'table', 'r1', {COLUMN => 'c1', TIMESTAMP => ts1}
get 'table', 'r1', {COLUMN => 'c1', TIMERANGE => [ts1, ts2], VERSIONS => 4}
get 'table', 'r1', {COLUMN => 'c1', TIMESTAMP => ts1, VERSIONS => 4}
get 'table', 'r1', {FILTER => "ValueFilter(=, 'binary:abc')"}
get 'table', 'r1', 'c1'
get 'table', 'r1', 'c1', 'c2'
get 'table', 'r1', ['c1', 'c2']
~~~
Jak widać na powyższych przykładach, polecenie get oprocz zwrócenia całego wiersza ma możliwośc szerokiej filtracji zwracanych kolumns ze wzlędu na ich nazwy (COLUMN), dat utworzenia wartości (TIMERANGE, TIMESTAMP), liczby wersji czy nawet możliwośc wskazania dodatkowego filtra.

Jeśli chcemy pobrać więcej niż jeden wiersz, możemy użyć polecenia
~~~ruby
scan 'table',
scan 'table', {COLUMNS => 'info:regioninfo'}
scan 'table',, {COLUMNS => ['c1', 'c2'], LIMIT => 10, STARTROW => 'xyz'}
scan 'table', {COLUMNS => ['c1', 'c2'], LIMIT => 10, STARTROW => 'xyz'}
scan 'table', {COLUMNS => 'c1', TIMERANGE => [1303668804, 1303668904]}
scan 'table', {REVERSED => true}
scan 'table', {ROWPREFIXFILTER => 'row2', FILTER => "(QualifierFilter (>=, 'binary:xyz')) AND (TimestampsFilter ( 123, 456))"}
scan 'table', {FILTER => org.apache.hadoop.hbase.filter.ColumnPaginationFilter.new(1, 0)}
~~~

Zliczanie wierszy
~~~ruby
count 'table'
~~~

Niemniej to polecenie jest jednowątkowe, w przypadku dużych tabel zdecydowanie lepiej użyć do tego MapReduc'a
~~~shell
hbase org.apache.hadoop.hbase.mapreduce.RowCounter <tablename>
~~~

Jeśli chcemy wyczyścić tabelę możemy użyć do tego jednego z dwóch poleceń
~~~ruby
truncate 'table'
truncate_preserve 'table'
~~~
Powyższe polecenia tak naprawdę usuwają i tworzą na nowo tabelę, przy czym drugie zachowuje zakres regionów

Jeśli chcemy usunąć wprowadzony wiersz należy użyć polecenia (fizycznie usuwamy wszystkie komórki w wierszu). Polecenie możemy ograniczyć do wybranych kolumn lub zakresów czasowych
~~~ruby
deleteall 'ns1:t1', 'r1'
deleteall 't1', 'r1'
deleteall 't1', 'r1', 'c1'
deleteall 't1', 'r1', 'c1', ts1
~~~

Jeśli jednak chcemy usunąć tylko wybrane wartości należy skorzystać z innego polecenia (fizycznie dodajemy komórkę oznaczającą usunięcie)
~~~ruby
delete 't1', 'r1', 'c1', ts1
~~~


~~~ruby
~~~


~~~ruby
~~~


~~~ruby
~~~



incr, 
append, 

get_counter, 
get_splits, 
 









# Skrypty w Ruby

HBase Shell'a możemy też używać do wykonywania naszych skryptów zapisanych w postaci pliku tekstowego
~~~shell
cat nasz_skrypt.rb | hbase shell
~~~

lub możemy do niego przekierować pojedyńcze polecenie
~~~shell
echo "status 'detailed'" | hbase shell > /tmp/hbase-status-detailed.txt
~~~
tutaj dodatkowo wynik polecenia przekierowaliśmy do pliku txt.

TODO

Uruchamianie z pliku ruby i pliku txt
Sztuczki w ruby




# Konfiguracja Shella za pomocą zmiennej systemowej

Jeśli chcemy by shell używałe więcej pamięci RAM lub przekazać mu inne parametry, możemy to zrobić za pomocą zmiennej systemowej HBASE_SHELL_OPTS

~~~shell
export HBASE_SHELL_OPTS="-verbose:gc -XX:+PrintGCApplicationStoppedTime -XX:+PrintGCDateStamps -XX:+PrintGCDetails -Xloggc:$HBASE_HOME/logs/gc-hbase.log"
~~~




# Legenda

* http://hbase.apache.org
* http://hbase.apache.org/book.html#shell
* http://hadoop-hbase.blogspot.com/2011/12/deletion-in-hbase.html
* https://community.hortonworks.com/articles/54761/compression-in-hbase.html
* http://archive.cloudera.com/cdh5/cdh/5/hbase-0.98.6-cdh5.2.0/book/shell.html
* https://www.cloudera.com/documentation/enterprise/5-6-x/topics/admin_hbase_blockcache_configure.html
* https://www.cloudera.com/documentation/enterprise/5-5-x/topics/admin_configure_blocksize.html
* https://github.com/apache/hbase/tree/master/hbase-shell/src/main/ruby/shell/commands
* https://github.com/GoogleCloudPlatform/cloud-bigtable-examples/tree/master/quickstart/thirdparty/ruby/shell/commands
* https://www.ruby-lang.org/pl/documentation/quickstart/
