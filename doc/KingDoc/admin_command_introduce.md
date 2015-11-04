# 管理端命令

kingshard的管理端口复用了工作端口，通过特定的关键字来标示，目前支持对后端DB常用的管理操作。

## 平滑上（下）线后端DB

```
#添加一个新的slave到node1
admin node(opt,node,k,v) values(‘add’,’node1’,’slave’,’127.0.0.1:3306’)

#删除node1上的一个slave。注意：只能删除slave，不能删除master
admin node(opt,node,k,v) values(‘del’,’node1’,’slave’,’127.0.0.1:3306’)

#将一个slave设置为下线状态
admin node(opt,node,k,v) values(‘down’,’node1’,’slave’,’127.0.0.1:3306’)

#将一个slave设置为上线状态
admin node(opt,node,k,v) values(‘up’,’node1’,’slave’,’127.0.0.1:3306’)

#将master设置为下线状态
admin node(opt,node,k,v) values(‘down’,’node1’,’master’,’127.0.0.1:3306’)

#将master设置为上线状态
admin node(opt,node,k,v) values(‘up’,’node1’,’master’,’127.0.0.1:3306’)

```

## 查看kingshard配置

```
#查看kingshard全局配置
mysql> admin server(opt,k,v) values('show','proxy','config');
+-------------+----------------+
| Key         | Value          |
+-------------+----------------+
| Addr        | 127.0.0.1:9696 |
| User        | kingshard      |
| Password    | kingshard      |
| LogLevel    | debug          |
| Nodes_Count | 2              |
| Nodes_List  | node1,node2    |
+-------------+----------------+
6 rows in set (0.00 sec)

#查看node状态
mysql> admin server(opt,k,v) values('show','node','config');
+-------+---------------------+--------+-------+-------------------------------+-------------+----------+
| Node  | Address             | Type   | State | LastPing                      | MaxIdleConn | IdleConn |
+-------+---------------------+--------+-------+-------------------------------+-------------+----------+
| node1 | 127.0.0.1:3306      | master | up    | 2015-08-07 15:54:44 +0800 CST | 16          | 1        |
| node2 | 192.168.59.103:3307 | master | up    | 2015-08-07 15:54:44 +0800 CST | 16          | 1        |
+-------+---------------------+--------+-------+-------------------------------+-------------+----------+
2 rows in set (0.00 sec)

#查看schema配置

mysql> admin server(opt,k,v) values('show','schema','config');
+-----------+------------------+---------+------+--------------+-----------+---------------+
| DB        | Table            | Type    | Key  | Nodes_List   | Locations | TableRowLimit |
+-----------+------------------+---------+------+--------------+-----------+---------------+
| kingshard |                  | default |      | node1        |           | 0             |
| kingshard | test_shard_hash  | hash    | id   | node1, node2 | 4, 4      | 0             |
| kingshard | test_shard_range | range   | id   | node1, node2 | 4, 4      | 10000         |
+-----------+------------------+---------+------+--------------+-----------+---------------+
3 rows in set (0.00 sec)

```
