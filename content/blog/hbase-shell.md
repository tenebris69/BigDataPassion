+++
author = "Radosław Szmit"
type = "post"
featuredpath = ""
description = ""
categories = ["HBase"]
featuredalt = ""
featured = ""
date = "2017-04-07T17:08:29+02:00"
title = "Hbase Shell"
linktitle = ""

+++


# Podstawowy HBase Shell #

Uruchomienie
~~~
[sages@sandbox ~]$ hbase shell
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/hdp/2.4.0.0-169/hadoop/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/hdp/2.4.0.0-169/zookeeper/lib/slf4j-log4j12-1.6.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version 1.1.2.2.4.0.0-169, r61dfb2b344f424a11f93b3f086eab815c1eb0b6a, Wed Feb 10 07:08:51 UTC 2016

hbase(main):001:0> 
~~~


Pomoc dla Shell'a
~~~ruby
hbase(main):053:0* help
HBase Shell, version 1.1.2.2.4.0.0-169, r61dfb2b344f424a11f93b3f086eab815c1eb0b6a, Wed Feb 10 07:08:51 UTC 2016
Type 'help "COMMAND"', (e.g. 'help "get"' -- the quotes are necessary) for help on a specific command.
Commands are grouped. Type 'help "COMMAND_GROUP"', (e.g. 'help "general"') for help on a command group.

COMMAND GROUPS:
  Group name: general
  Commands: status, table_help, version, whoami

  Group name: ddl
  Commands: alter, alter_async, alter_status, create, describe, disable, disable_all, drop, drop_all, enable, enable_all, exists, get_table, is_disabled, is_enabled, list, show_filters

  Group name: namespace
  Commands: alter_namespace, create_namespace, describe_namespace, drop_namespace, list_namespace, list_namespace_tables

  Group name: dml
  Commands: append, count, delete, deleteall, get, get_counter, get_splits, incr, put, scan, truncate, truncate_preserve

  Group name: tools
  Commands: assign, balance_switch, balancer, balancer_enabled, catalogjanitor_enabled, catalogjanitor_run, catalogjanitor_switch, close_region, compact, compact_rs, flush, major_compact, merge_region, move, normalize, normalizer_enabled, normalizer_switch, split, trace, unassign, wal_roll, zk_dump

  Group name: replication
  Commands: add_peer, append_peer_tableCFs, disable_peer, disable_table_replication, enable_peer, enable_table_replication, list_peers, list_replicated_tables, remove_peer, remove_peer_tableCFs, set_peer_tableCFs, show_peer_tableCFs

  Group name: snapshots
  Commands: clone_snapshot, delete_all_snapshot, delete_snapshot, list_snapshots, restore_snapshot, snapshot, snapshot_all, snapshot_restore

  Group name: configuration
  Commands: update_all_config, update_config

  Group name: quotas
  Commands: list_quotas, set_quota

  Group name: security
  Commands: grant, revoke, user_permission

  Group name: procedures
  Commands: abort_procedure, list_procedures

  Group name: visibility labels
  Commands: add_labels, clear_auths, get_auths, list_labels, set_auths, set_visibility

SHELL USAGE:
Quote all names in HBase Shell such as table and column names.  Commas delimit
command parameters.  Type <RETURN> after entering a command to run it.
Dictionaries of configuration used in the creation and alteration of tables are
Ruby Hashes. They look like this:

  {'key1' => 'value1', 'key2' => 'value2', ...}

and are opened and closed with curley-braces.  Key/values are delimited by the
'=>' character combination.  Usually keys are predefined constants such as
NAME, VERSIONS, COMPRESSION, etc.  Constants do not need to be quoted.  Type
'Object.constants' to see a (messy) list of all constants in the environment.

If you are using binary keys or values and need to enter them in the shell, use
double-quote'd hexadecimal representation. For example:

  hbase> get 't1', "key\x03\x3f\xcd"
  hbase> get 't1', "key\003\023\011"
  hbase> put 't1', "test\xef\xff", 'f1:', "\x01\x33\x40"

The HBase shell is the (J)Ruby IRB with the above HBase-specific commands added.
For more on the HBase Shell, see http://hbase.apache.org/book.html
hbase(main):054:0> 
~~~






# Podstawowe polecenia #

Sprawdzenie stanu bazy danych
~~~
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
Jak widać status może być typu:

* simple
* summary
* detailed
* replication
* replication source
* replication sink

W powyższym przykładzie widać, że baza nie jest uruchomiona (dokładnie rzecz biorąc ZooKeeper bez którego HBase nie potrafi działać)

Uruchomienie bazy (Sandbox Hortonworks)
~~~shell
[root@sandbox ~]# /etc/init.d/hbase-starter start
Starting HBase...
Starting Postgre SQL                                      [  OK  ]
Starting name node                                        [  OK  ]
Starting zookeeper nodes                                  [  OK  ]
Starting hbase master                                     [  OK  ]
Starting hbase thrift                                     [  OK  ]
Starting hbase stargate                                   [  OK  ]
Starting hbase regionservers                              [  OK  ]
[root@sandbox ~]# 
~~~


Sprawdzenie stanu bazy po włączeniu
~~~ruby
hbase(main):001:0> status
1 servers, 0 dead, 280.0000 average load
hbase(main):002:0> 
~~~


Sprawdzenie wersji na jakiej pracujemy
~~~ruby
hbase(main):062:0> version
1.1.2.2.4.0.0-169, r61dfb2b344f424a11f93b3f086eab815c1eb0b6a, Wed Feb 10 07:08:51 UTC 2016
hbase(main):063:0> 
~~~

Sprawdzanie jakim jesteśmy użytkownikiem
~~~ruby
hbase(main):025:0* whoami
xrszmit (auth:SIMPLE)
    groups: users, wheel

hbase(main):026:0> 
~~~

Informacja na temat używania zmiennych w Shell'u (dostępne od wersji 0.96+)
~~~ruby
hbase(main):058:0> table_help
~~~







# Zarządzanie schematem (DDL) #


Tworzenie tabeli
~~~ruby
hbase(main):036:0* create 'person', 'cf'
0 row(s) in 1.2750 seconds

=> Hbase::Table - person
hbase(main):037:0>
~~~

Stworzenie tabeli z jedną rodziną kolumn i liczbą wersji równą 5
~~~ruby
hbase> create 'person', {NAME => 'cf', VERSIONS => 5}
hbase> create 't1', {NAME => 'f1', VERSIONS => 1, TTL => 2592000, BLOCKCACHE => true}
hbase> create 't1', {NAME => 'f1', CONFIGURATION => {'hbase.hstore.blockingStoreFiles' => '10'}}
~~~

Stworzenie tabeli z kilkoma rodzinami
~~~ruby
hbase> create 't1', {NAME => 'f1'}, {NAME => 'f2'}, {NAME => 'f3'}
~~~


Lista istniejących tabel
~~~ruby
hbase(main):038:0* list
person
1 row(s) in 0.0290 seconds

=> ["SYSTEM.CATALOG", "SYSTEM.FUNCTION", "SYSTEM.SEQUENCE", "SYSTEM.STATS", "ibm:ibmtable", "iemployee", "inventory", "java", "movies", "movies_comedy", "movies_data", "nam1:tab5", "person", "product", "ratings", "ratings_average", "sages:tabelkav2", "tabelkav", "tabv5", "tags", "test", "test:tablename", "users"]
hbase(main):039:0> 
~~~


Opis tabeli
~~~
hbase(main):040:0* describe 'person'
Table person is ENABLED 
person
COLUMN FAMILIES DESCRIPTION
{NAME => 'cf', DATA_BLOCK_ENCODING => 'NONE', BLOOMFILTER => 'ROW', REPLICATION_SCOPE => '0', VERSIONS => '1', COMPRESSION => 'NONE', MIN_VERSIONS => '0', TTL => 'FOREVER', KEEP_DELETED_CELLS => 'FALSE', BLOCKSIZE => '65536', IN_MEMORY => 'false', BLOCKCACHE => 'true'} 
1 row(s) in 0.0970 seconds
hbase(main):041:0> 
~~~

Shell zwraca informację że tabela jest aktywna (ENABLED) oraz zwraca listę rodzin kolumn wraz z ich parametrami, każda w oddzielnym nawiasie klamrowym {}

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
















Wyjście z konsoli
~~~ruby
hbase(main):002:0> exit
~~~
lub Ctrl+C / Ctrl+C





# Polecenia zaawansowane #

Tworzenie tabeli z narzuconym wstępnym podziałem regionów
~~~ruby
create 't1','f',SPLITS => ['10','20',30']
~~~





# Skrypty w Ruby #

Uruchamianie z pliku ruby i pliku txt
Sztuczki w ruby




# Konfiguracja Shella za pomocą zmiennej systemowej #

Jeśli chcemy by shell używałe więcej pamięci RAM lub przekazać mu inne parametry, możemy to zrobić za pomocą zmiennej systemowej HBASE_SHELL_OPTS

~~~shell
export HBASE_SHELL_OPTS="-verbose:gc -XX:+PrintGCApplicationStoppedTime -XX:+PrintGCDateStamps -XX:+PrintGCDetails -Xloggc:$HBASE_HOME/logs/gc-hbase.log"
~~~




# Przydatne linki #

* http://hbase.apache.org/1.2/book.html#shell
* http://archive.cloudera.com/cdh5/cdh/5/hbase-0.98.6-cdh5.2.0/book/shell.html
* https://community.hortonworks.com/articles/54761/compression-in-hbase.html
* https://www.ruby-lang.org/pl/documentation/quickstart/
* http://hadoop-hbase.blogspot.com/2011/12/deletion-in-hbase.html
* https://www.cloudera.com/documentation/enterprise/5-6-x/topics/admin_hbase_blockcache_configure.html
* https://www.cloudera.com/documentation/enterprise/5-5-x/topics/admin_configure_blocksize.html








