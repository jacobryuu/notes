replication思想是将数据在集群的多个节点同步、备份,以提高集群数据的可用性(HA)
replication构架通常由一个master和一个或者多个slave构成，master接收应用的write操作，
事务中的read，write操作均有master处理。slave接收read操作。在master上发生的数据变更，都将会复制给slaves。
replication构架解决了
数据多点备份，提高数据可用性
读写分离，提高集群的并发能力。(并非负载均衡)
让一些非实时的数据操作，转移到slaves上。

