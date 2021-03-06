+++
author = "Radosław Szmit"
categories = ["HBase"]
date = "2017-05-06T15:35:32+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "hbase administration"
type = "post"

draft = true
+++

# Polecenia zaawansowane #

Tworzenie tabeli z narzuconym wstępnym podziałem regionów
~~~ruby
create 't1','f',SPLITS => ['10','20',30']
~~~

~~~
scan 'hbase:meta',{FILTER=>"PrefixFilter('table_name')"}
~~~


# dodawanie koprocesorów

Możemy także dodawać koprocesory (mechanizm rozszerzania możliwości bazy któremu będzie poświęcony inny wpis)
~~~ruby
alter 'table', 'coprocessor'=>'hdfs:///foo.jar|com.foo.FooRegionObserver|1001|arg1=1,arg2=2'
~~~


Since you can have multiple coprocessors configured for a table, a
sequence number will be automatically appended to the attribute name
to uniquely identify it.

The coprocessor attribute must match the pattern below in order for
the framework to understand how to load the coprocessor classes:

  [coprocessor jar file location] | class name | [priority] | [arguments]

You can also set configuration settings specific to this table or column family:

  hbase> alter 't1', CONFIGURATION => {'hbase.hregion.scan.loadColumnFamiliesOnDemand' => 'true'}
  hbase> alter 't1', {NAME => 'f2', CONFIGURATION => {'hbase.hstore.blockingStoreFiles' => '10'}}


  hbase> alter 't1', METHOD => 'table_att_unset', NAME => 'coprocessor$1'







scan 'hbase:meta',{FILTER=>"PrefixFilter('table_name')"}


TODO
~~~ruby
locate_region, 
show_filters
~~~




 hbase> scan 't1', {CONSISTENCY => 'TIMELINE'}
For setting the Operation Attributes 
  hbase> scan 't1', { COLUMNS => ['c1', 'c2'], ATTRIBUTES => {'mykey' => 'myvalue'}}
  hbase> scan 't1', { COLUMNS => ['c1', 'c2'], AUTHORIZATIONS => ['PRIVATE','SECRET']}
For experts, there is an additional option -- CACHE_BLOCKS -- which
switches block caching for the scanner on (true) or off (false).  By
default it is enabled.  Examples:

  hbase> scan 't1', {COLUMNS => ['c1', 'c2'], CACHE_BLOCKS => false}

Also for experts, there is an advanced option -- RAW -- which instructs the
scanner to return all cells (including delete markers and uncollected deleted
cells). This option cannot be combined with requesting specific COLUMNS.
Disabled by default.  Example:

  hbase> scan 't1', {RAW => true, VERSIONS => 10}


# Snapshoty



# Zmiana nazwy tabeli

HBase nie ma pojedyńczego polecenia do zmiany nazwy tabeli, tą funkcjonalnością od wersji 0.94 najprościej można uzyskać za pomocą Snapshotów

~~~ruby
disable 'TableToRename'
snapshot 'TableToRename', 'NewTable'
clone_snapshot 'NewTable', 'newTableToRename'
delete_snapshot 'NewTable'
drop 'TableToRename'
~~~

Tego mechanizmu można także użyć do przesunięcia tabeli pomiędzy przestrzenniami nazw (namespace)


# Spliting

~~~
 split 'radek:ratings'
~~~


# Export i Import

~~~shell
current_date=$(date +"%Y-%m-%d")
table_name='documents'

hdfs dfs -rm -r -skipTrash /tmp/hbase-export-$table_name-$current_date

time hbase org.apache.hadoop.hbase.mapreduce.Export -D mapreduce.output.fileoutputformat.compress=true -D mapreduce.output.fileoutputformat.compress.codec=org.apache.hadoop.io.compress.GzipCodec -D mapreduce.output.fileoutputformat.compress.type=BLOCK -Dhbase.client.scanner.caching=1000 -Dmapreduce.map.speculative=false -Dmapreduce.reduce.speculative=false $table_name /tmp/hbase-export-$table_name-$current_date
 
hdfs dfs -du -h -s /apps/hbase/data/data/default/$table_name
hdfs dfs -du -h -s /tmp/hbase-export-$table_name-$current_date

#hdfs dfs -get /tmp/hbase-export-$table_name-$current_date /tmp/
~~~

~~~shell
current_date=$(date +"%Y-%m-%d")
table_name='documents'

hdfs dfs -rm -r -skipTrash /tmp/hbase-export-$table_name-$current_date
hdfs dfs -put /tmp/hbase-export-$table_name-$current_date /tmp

echo "disable '$table_name'; drop '$table_name'" | hbase shell
echo "create '$table_name', {NAME=>'$table_name',COMPRESSION=>'snappy'}" | hbase shell

time hbase org.apache.hadoop.hbase.mapreduce.Import $table_name /tmp/hbase-export-$table_name-$current_date

hdfs dfs -du -h -s /apps/hbase/data/data/default/$table_name
hdfs dfs -du -h -s /tmp/hbase-export-$table_name-$current_date
~~~


# Legenda

* https://hortonworks.com/blog/apache-hbase-region-splitting-and-merging/
