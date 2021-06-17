What you will find in this article:
* Description of [IPCAD](https://lionet.info/ipcad/) daemon that pretends as a Cisco router and provides aggregated network statistic
* Example of materialized views in ClickHouse database
* [ipcad2ch](https://github.com/inkuber/ipcad2ch) utility that will help you to organize accounting of network traffic

# How I saved network statistics from IPCAD to ClickHouse 
In the era of broadband Internet there are not so much sense to account network traffic. May be just a small ISP by some reason not taken by the big players. Ifrastracture of this kind of ISP I proudly supporting. Technological stack is following:

* FreeBSD server as router that aggregate network statistic in IPCAD daemon
* FreeBSD server with self made billing on PHP and PostgreSQL database. Aggregates are saved in DB, detail statistic in zipped ipcad text files

This configuration was enoght for narrow network channels of 2000th but now in the epoch of gigabites become very bad and everything start to shamelessly slow down. It's time to do something...

If something start to slow down, what we need? Right, something that could not be slow. As everybody knows ClickHouse is never slow then we install it.

<details>
  <summary>ClickHouse  - это ...</summary>
  ClickHouse is a database, created in the bowels of russian company Yandex. Most interesting thing in this database is that it is column db, i.e. store datum of tables not by rows how it doing relation databases but by columns. Roughly saying, values of one column stored in one file. This gives advantages in processing data from several columns not reading everything from disk by rows.
</details>

Main features of application are reports about service usage:

* Who and how much traffic consumed by customers in report repiod and how much money we must ask from them.
* Take a look how much service consumed by customer in particular day, hour and minute with beautiful charts.
* Output detail information by customer request where where he was in internet by requested period.

It comes from requirements that we need store received data from IPCAD format as is and calculate some aggregates by time. Looks not so difficult...

# IPCAD format

IPCAD (Cisco IP accounting simulator) - daemon that accounts passing through router traffic and calculating aggregates of packet count and traffic volume in RAM by tuple:

* source IP
* source Port
* destination IP
* destination Port
* Protocol
* Interface

Data could be fetched by requesting it using RSH

```bash
#!/bin/sh
RSH=`which rsh`
IP=<router IP address>
FILE=<saving path>

# flush statistic to control point
$RSH -l root $IP clear ip accounting > /dev/null

# show statistic in control point
$RSH -l root $IP show ip accounting checkpoint > $FILE
```
Output is:

```
   Source           Destination    Packets        Bytes  SrcPt DstPt Proto   IF
 188.218.189.188  188.138.119.98         1           88     83 28088    18  em1
 188.218.189.188  188.138.119.98         1           88     83 12038    18  em1
 108.232.38.113   188.218.189.198        1           80    883 28818     8  em1
```

This data we need to store not in old fasion way in zip archive but in brand new ClickHouse database.

# Table structure in ClickHouse

One of the most power features of ClickHouse is materialized views. It is tables that could be updated automaticaly from data saved in other tables i.e we could insert into detal traffic table and aggregates will be calculated automaticaly by giving rules.

<details>
  <summary>Табличка детального трафика</summary>
  
```sql
CREATE TABLE IF NOT EXISTS details
(
    collected DateTime,
    user_id String,
    dir Enum8('unknown' = 0, 'in' = 1, 'out' = 2),
    class Enum8('unknown' = 0, 'local' = 1, 'peering' = 2, 'internet' = 3, 'multicast' = 4),
    src_ip UInt32,
    src_port UInt16,
    dst_ip UInt32,
    dst_port UInt16,
    packets UInt16,
    bytes UInt32,
    proto UInt8
)
ENGINE = MergeTree
PARTITION BY toYYYYMMDD(collected)
ORDER BY (collected, user_id, dir, class, src_ip, dst_ip, proto)
SETTINGS index_granularity = 8192
```
</details>

In the table of detail traffic we store IPCAD data as is also adding timestamp and several classification fields as customer id, traffic direction and traffic class. Lets make materialized views:

<details>
  <summary>Табличка агрегатов по дням</summary>
  
```sql
CREATE MATERIALIZED VIEW IF NOT EXISTS daily
(
    date Date,
    user_id String,
    class Enum8('unknown' = 0, 'local' = 1, 'peering' = 2, 'internet' = 3, 'multicast' = 4),
    dir Enum8('unknown' = 0, 'in' = 1, 'out' = 2),
    bytes AggregateFunction(sum, UInt32)
)
ENGINE = AggregatingMergeTree()
PARTITION BY toYYYYMM(date)
ORDER BY (date, user_id, class, dir)
SETTINGS index_granularity = 8192 AS
SELECT
    toDate(collected) AS date,
    user_id,
    class,
    dir,
    sumState(bytes) AS bytes
FROM details
GROUP BY
    toDate(collected),
    user_id,
    class,
    dir
```
</details>

<details>
  <summary>Таблицы агрегатов по часам и минутам</summary>
  
```sql
CREATE MATERIALIZED VIEW IF NOT EXISTS hourly
(
    date DateTime,
    user_id String,
    class Enum8('unknown' = 0, 'local' = 1, 'peering' = 2, 'internet' = 3, 'multicast' = 4),
    dir Enum8('unknown' = 0, 'in' = 1, 'out' = 2),
    bytes AggregateFunction(sum, UInt32)
)
ENGINE = AggregatingMergeTree()
PARTITION BY toYYYYMM(date)
ORDER BY (date, user_id, class, dir)
SETTINGS index_granularity = 8192 AS
SELECT
    toStartOfHour(collected) AS date,
    user_id,
    class,
    dir,
    sumState(bytes) AS bytes
FROM details
GROUP BY
    toStartOfHour(collected),
    user_id,
    class,
    dir;

CREATE MATERIALIZED VIEW IF NOT EXISTS minutely
(
    date DateTime,
    user_id String,
    class Enum8('unknown' = 0, 'local' = 1, 'peering' = 2, 'internet' = 3, 'multicast' = 4),
    dir Enum8('unknown' = 0, 'in' = 1, 'out' = 2),
    bytes AggregateFunction(sum, UInt32)
)
ENGINE = AggregatingMergeTree()
PARTITION BY toYYYYMM(date)
ORDER BY (date, user_id, class, dir)
SETTINGS index_granularity = 8192 AS
SELECT
    toStartOfMinute(collected) AS date,
    user_id,
    class,
    dir,
    sumState(bytes) AS bytes
FROM details
GROUP BY
    toStartOfMinute(collected),
    user_id,
    class,
    dir;
```
</details>

Now aggregates calculating automaticaly when data inserted into detail table. It's the magic!

# Storing data into ClickHouse
Now there is a little left to make a tool for that. Drawing two ovals and than finishing rest of the owl. I took my favorit Golang, parsed, classified and saved. I wrote a little simple application that opensourced on github.

* [ipcad2ch](https://github.com/inkuber/ipcad2ch)

## Usage
Remember in the beginning of the artical was a script for fetching statistic from IPCAD, let's change it:

```bash
#!/bin/sh
RSH=`which rsh`
IP=<router IP address>
FILE=<saving path>

# flush statistic to control point
$RSH -l root $IP clear ip accounting > /dev/null

# show statistic in control point
$RSH -l root $IP show ip accounting checkpoint | ipcad2ch > $FILE
```

Insert tool with pipe after data fetch but before writing it to file. Now proggram reads ipcad data and saves it into ClickHouse then it prints it untouched to standart output. Thereby we could save data immediatly even not changing legacy code. Now we need to collect enought of statistic and smoothly transit to new data source. As I said, old statistic keeps in zip archives in ipcad format and could be also processed with this tool.
