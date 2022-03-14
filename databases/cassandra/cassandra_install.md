# Cassandraのインストール

## 内容

* cqlsh設置とjava環境を設置

* 前準備
  * add user
  * ssh no password設置(全部のnode)

    ```code
        ssh-keygen -t rsa
        at ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
        at ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    ```

  * jdk環境を設置
  * CASSANDRA_HOME環境変数を設置

## download cassandra

>wget http://archive.apache.org/dist/cassandra/{version}/apache-cassandra-{version}-bin.tar.gz
>tar zxvf apache-cassandra-{version}-bin.tar.gz
>ln -s apache-cassandra-{version} cassandra

* configure Cassandra

  * cassandra/conf/cassandra.yaml

    ```code
    data_file_directories: one or more directories where data files are located.
    commitlog_directory: the directory where commitlog files are located.
    saved_caches_directory: the directory where saved caches are located.
    hints_directory: the directory where hints are located.
    For performance reasons, if you have multiple disks, consider putting commitlog and data files on different disks.
    ```

## 詳細
>http://cassandra.apache.org/doc/latest/getting_started/configuring.html
>http://cassandra.apache.org/doc/latest/configuration/cassandra_config_file.html

## フォルダー権限を付与

```code
chmod 777 /var/lib/cassandra（data_file_directories）
chmod 777 /var/log/cassandra
```

## 起動

```code
cd $CASSANDRA_HOME
./bin/cassandra -f（プロセスはconsoleで実行）
```
