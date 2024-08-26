PS C:\Users\Ярослав> yc compute zone list
+---------------+--------+
|      ID       | STATUS |
+---------------+--------+
| ru-central1-a | UP     |
| ru-central1-b | UP     |
| ru-central1-c | DOWN   |
| ru-central1-d | UP     |
+---------------+--------+

PS C:\Users\Ярослав> yc config set compute-default-zone ru-central1-a
PS C:\Users\Ярослав> yc config get compute-default-zone
ru-central1-a

PS C:\Users\Ярослав> yc compute disk-type list
+---------------------------+--------------------------------+
|            ID             |          DESCRIPTION           |
+---------------------------+--------------------------------+
| network-hdd               | Network storage with HDD       |
|                           | backend                        |
| network-ssd               | Network storage with SSD       |
|                           | backend                        |
| network-ssd-io-m3         | Fast network storage with      |
|                           | three replicas                 |
| network-ssd-nonreplicated | Non-replicated network storage |
|                           | with SSD backend               |
+---------------------------+--------------------------------+
