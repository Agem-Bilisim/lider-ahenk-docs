# Lider Rapor Servis

Lider raporlama altyapısı ile ilgili wiki.

> **İpucu**: Tüm örneklerde, request header'ında **username:emre** ve **password:12345** değerlerinin gönderildiği varsayılacaktır.

### 1. Veritabanı yapısı

> **Not**: Veritabanı sunucusu bağlantı bilgileri aşağıdaki gibidir:
> `mysql -h 192.168.1.41 -uroot -pqwert123`
> Bağlantı kurulduktan sonra, `use liderdb;` diyerek Lider ahenk veritabanı seçilebilir.

##### R_REPORT_TEMPLATE:

```
+--------------------+---------------+------+-----+---------+-------+
| Field              | Type          | Null | Key | Default | Extra |
+--------------------+---------------+------+-----+---------+-------+
| REPORT_TEMPLATE_ID | bigint(20)    | NO   | PRI | NULL    |       |
| CREATE_DATE        | datetime      | NO   |     | NULL    |       |
| DESCRIPTION        | varchar(500)  | YES  |     | NULL    |       |
| MODIFY_DATE        | datetime      | YES  |     | NULL    |       |
| NAME               | varchar(255)  | NO   | UNI | NULL    |       |
| QUERY              | varchar(4000) | NO   |     | NULL    |       |
| REPORT_FOOTER      | varchar(500)  | YES  |     | NULL    |       |
| REPORT_HEADER      | varchar(500)  | YES  |     | NULL    |       |
+--------------------+---------------+------+-----+---------+-------+
```

##### R_REPORT_TEMPLATE_COLUMN:

```
+--------------------+--------------+------+-----+---------+-------+
| Field              | Type         | Null | Key | Default | Extra |
+--------------------+--------------+------+-----+---------+-------+
| TEMPLATE_COLUMN_ID | bigint(20)   | NO   | PRI | NULL    |       |
| COLUMN_ORDER       | int(11)      | NO   |     | NULL    |       |
| CREATE_DATE        | datetime     | NO   |     | NULL    |       |
| NAME               | varchar(255) | NO   |     | NULL    |       |
| REPORT_TEMPLATE_ID | bigint(20)   | NO   | MUL | NULL    |       |
| COLUMN_ID          | bigint(20)   | YES  |     | NULL    |       |
| VISIBLE            | bit(1)       | YES  |     | NULL    |       |
| WIDTH              | int(11)      | YES  |     | NULL    |       |
+--------------------+--------------+------+-----+---------+-------+
```

##### R_REPORT_TEMPLATE_PARAMETER:

```
+-----------------------+---------------+------+-----+---------+-------+
| Field                 | Type          | Null | Key | Default | Extra |
+-----------------------+---------------+------+-----+---------+-------+
| TEMPLATE_PARAMETER_ID | bigint(20)    | NO   | PRI | NULL    |       |
| CREATE_DATE           | datetime      | NO   |     | NULL    |       |
| DEFAULT_VALUE         | varchar(4000) | YES  |     | NULL    |       |
| PARAMETER_KEY         | varchar(255)  | NO   |     | NULL    |       |
| LABEL                 | varchar(250)  | NO   |     | NULL    |       |
| MANDATORY             | bit(1)        | YES  |     | NULL    |       |
| PARAMETER_TYPE        | int(11)       | NO   |     | NULL    |       |
| REPORT_TEMPLATE_ID    | bigint(20)    | NO   | MUL | NULL    |       |
| PARAMETER_ID          | bigint(20)    | YES  |     | NULL    |       |
+-----------------------+---------------+------+-----+---------+-------+
```

##### R_REPORT_VIEW:

```
+--------------------+--------------+------+-----+---------+-------+
| Field              | Type         | Null | Key | Default | Extra |
+--------------------+--------------+------+-----+---------+-------+
| REPORT_VIEW_ID     | bigint(20)   | NO   | PRI | NULL    |       |
| CREATE_DATE        | datetime     | NO   |     | NULL    |       |
| DESCRIPTION        | varchar(500) | YES  |     | NULL    |       |
| MODIFY_DATE        | datetime     | YES  |     | NULL    |       |
| NAME               | varchar(255) | NO   | UNI | NULL    |       |
| REPORT_TYPE        | int(11)      | NO   |     | NULL    |       |
| REPORT_TEMPLATE_ID | bigint(20)   | NO   | MUL | NULL    |       |
+--------------------+--------------+------+-----+---------+-------+
```

##### R_REPORT_VIEW_COLUMN:

```
+--------------------+--------------+------+-----+---------+-------+
| Field              | Type         | Null | Key | Default | Extra |
+--------------------+--------------+------+-----+---------+-------+
| VIEW_COLUMN_ID     | bigint(20)   | NO   | PRI | NULL    |       |
| CREATE_DATE        | datetime     | NO   |     | NULL    |       |
| LEGEND             | varchar(255) | YES  |     | NULL    |       |
| COLUMN_TYPE        | int(11)      | NO   |     | NULL    |       |
| WIDTH              | int(11)      | YES  |     | NULL    |       |
| REPORT_VIEW_ID     | bigint(20)   | NO   | MUL | NULL    |       |
| TEMPLATE_COLUMN_ID | bigint(20)   | NO   | MUL | NULL    |       |
+--------------------+--------------+------+-----+---------+-------+
```

##### R_REPORT_VIEW_PARAMETER:

```
+-----------------------+---------------+------+-----+---------+-------+
| Field                 | Type          | Null | Key | Default | Extra |
+-----------------------+---------------+------+-----+---------+-------+
| TEMPLATE_PARAMETER_ID | bigint(20)    | NO   | PRI | NULL    |       |
| CREATE_DATE           | datetime      | NO   |     | NULL    |       |
| DEFAULT_VALUE         | varchar(4000) | YES  |     | NULL    |       |
| PARAMETER_KEY         | varchar(255)  | NO   |     | NULL    |       |
| LABEL                 | varchar(250)  | NO   |     | NULL    |       |
| MANDATORY             | bit(1)        | YES  |     | NULL    |       |
| PARAMETER_TYPE        | int(11)       | NO   |     | NULL    |       |
| REPORT_TEMPLATE_ID    | bigint(20)    | NO   | MUL | NULL    |       |
| PARAMETER_ID          | bigint(20)    | YES  |     | NULL    |       |
+-----------------------+---------------+------+-----+---------+-------+
```

### 2. Rapor Şablonları ile İlgili İşlemler

TODO

### 3. Rapor Tanımları ile İlgili İşlemler

#### Tanımları Listeleme

    * Request: http://localhost:8181/lider/view/list
    * Response:

    ```json
{
   "status":"OK",
   "messages":[
      "Records listed."
   ],
   "resultMap":{
      "views":"[{\"id\":55601,\"template\":{\"id\":52701,\"name\":\"Çalıştırılan Görevler\",\"description\":\"Anlık Olarak, Sisteme Bağlı Bulunan Tüm Kullanıcılara Ait Bilgiler İçeren Rapor.\",\"query\":\"SELECT a.id, a.jid, us.username, us.createDate, a.ipAddresses, a.dn   FROM UserSessionImpl us INNER JOIN us.agent a WHERE us.sessionEvent = 1 \\tAND NOT EXISTS (select 1 from UserSessionImpl logout where logout.sessionEvent = 2 and logout.agent = us.agent  \\t\\t\\tand logout.username = us.username and logout.createDate > us.createDate) ORDER BY us.createDate, us.username\",\"templateParams\":[],\"templateColumns\":[{\"id\":117702,\"name\":\"Sıra No.\",\"columnOrder\":1,\"createDate\":1468849388000},{\"id\":55803,\"name\":\"Görev tarihi\",\"columnOrder\":4,\"createDate\":1466521725000},{\"id\":55801,\"name\":\"Başarılı olanlar\",\"columnOrder\":5,\"createDate\":1466521725000},{\"id\":117703,\"name\":\"IP Adresleri\",\"columnOrder\":6,\"createDate\":1468849388000},{\"id\":70901,\"name\":\"Başarısız olanlar\",\"columnOrder\":7,\"createDate\":1467115539000}],\"createDate\":1466406569000,\"modifyDate\":1468849388000},\"name\":\"emre\",\"description\":\"emre\",\"type\":\"TABLE\",\"viewParams\":[{\"id\":55652,\"referencedParam\":null,\"label\":\"qwer\",\"value\":\"qwer\",\"createDate\":1466519272000},{\"id\":55651,\"referencedParam\":null,\"label\":\"erty\",\"value\":\"erty\",\"createDate\":1466519272000}],\"viewColumns\":[{\"id\":55701,\"referencedCol\":null,\"type\":\"VALUE_FIELD\",\"legend\":\"dfgdfg\",\"width\":100,\"createDate\":1466519272000}],\"createDate\":1466519272000,\"modifyDate\":null},{\"id\":55901,\"template\":{\"id\":52701,\"name\":\"Çalıştırılan Görevler\",\"description\":\"Anlık Olarak, Sisteme Bağlı Bulunan Tüm Kullanıcılara Ait Bilgiler İçeren Rapor.\",\"query\":\"SELECT a.id, a.jid, us.username, us.createDate, a.ipAddresses, a.dn   FROM UserSessionImpl us INNER JOIN us.agent a WHERE us.sessionEvent = 1 \\tAND NOT EXISTS (select 1 from UserSessionImpl logout where logout.sessionEvent = 2 and logout.agent = us.agent  \\t\\t\\tand logout.username = us.username and logout.createDate > us.createDate) ORDER BY us.createDate, us.username\",\"templateParams\":[],\"templateColumns\":[{\"id\":117702,\"name\":\"Sıra No.\",\"columnOrder\":1,\"createDate\":1468849388000},{\"id\":55803,\"name\":\"Görev tarihi\",\"columnOrder\":4,\"createDate\":1466521725000},{\"id\":55801,\"name\":\"Başarılı olanlar\",\"columnOrder\":5,\"createDate\":1466521725000},{\"id\":117703,\"name\":\"IP Adresleri\",\"columnOrder\":6,\"createDate\":1468849388000},{\"id\":70901,\"name\":\"Başarısız olanlar\",\"columnOrder\":7,\"createDate\":1467115539000}],\"createDate\":1466406569000,\"modifyDate\":1468849388000},\"name\":\"1\",\"description\":\"1\",\"type\":\"PIE_CHART\",\"viewParams\":[{\"id\":55952,\"referencedParam\":null,\"label\":\"1\",\"value\":\"1\",\"createDate\":1466527345000},{\"id\":55951,\"referencedParam\":null,\"label\":\"2\",\"value\":\"2\",\"createDate\":1466527345000}],\"viewColumns\":[{\"id\":56001,\"referencedCol\":{\"id\":55801,\"name\":\"Başarılı olanlar\",\"columnOrder\":5,\"createDate\":1466521725000},\"type\":\"VALUE_FIELD\",\"legend\":\"1\",\"width\":100,\"createDate\":1466527345000}],\"createDate\":1466527345000,\"modifyDate\":null},{\"id\":70251,\"template\":{\"id\":69851,\"name\":\"Çevrimiçi Kullanıcılar\",\"description\":\"Anlık olarak sisteme bağlı olan tüm kullanıcılara ait bilgiler içerir.\",\"query\":\"SELECT a.id, a.jid, us.username, us.createDate, a.ipAddresses, a.dn   FROM UserSessionImpl us INNER JOIN us.agent a WHERE us.sessionEvent = 1 \\tAND NOT EXISTS (select 1 from UserSessionImpl logout where logout.sessionEvent = 2 and logout.agent = us.agent and logout.username = us.username and logout.createDate > us.createDate) ORDER BY us.createDate, us.username\",\"templateParams\":[],\"templateColumns\":[{\"id\":69901,\"name\":\"Kullanıcı Adı\",\"columnOrder\":3,\"createDate\":1467106380000},{\"id\":69902,\"name\":\"Sisteme Giriş Tarihi\",\"columnOrder\":4,\"createDate\":1467106380000},{\"id\":69903,\"name\":\"IP Adresleri\",\"columnOrder\":5,\"createDate\":1467106380000},{\"id\":69904,\"name\":\"DN\",\"columnOrder\":6,\"createDate\":1467106380000}],\"createDate\":1467106380000,\"modifyDate\":null},\"name\":\"User Sessions\",\"description\":\"seren\",\"type\":\"TABLE\",\"viewParams\":[],\"viewColumns\":[{\"id\":70302,\"referencedCol\":{\"id\":69902,\"name\":\"Sisteme Giriş Tarihi\",\"columnOrder\":4,\"createDate\":1467106380000},\"type\":\"LABEL_FIELD\",\"legend\":\"login date\",\"width\":100,\"createDate\":1467114696000},{\"id\":70301,\"referencedCol\":{\"id\":69901,\"name\":\"Kullanıcı Adı\",\"columnOrder\":3,\"createDate\":1467106380000},\"type\":\"LABEL_FIELD\",\"legend\":\"username\",\"width\":100,\"createDate\":1467114696000},{\"id\":70303,\"referencedCol\":{\"id\":69904,\"name\":\"DN\",\"columnOrder\":6,\"createDate\":1467106380000},\"type\":\"LABEL_FIELD\",\"legend\":\"dn\",\"width\":100,\"createDate\":1467114696000},{\"id\":70304,\"referencedCol\":{\"id\":69903,\"name\":\"IP Adresleri\",\"columnOrder\":5,\"createDate\":1467106380000},\"type\":\"VALUE_FIELD\",\"legend\":\"Ips\",\"width\":100,\"createDate\":1467114696000}],\"createDate\":1467113320000,\"modifyDate\":1467114696000},{\"id\":75102,\"template\":{\"id\":67201,\"name\":\"Wake-on-LAN\",\"description\":\"Uyandırılan ya da Kapatılan Bilgisayarlar Hakkında Detaylı Rapor\",\"query\":\"SELECT cer.responseMessage, t.createDate, p.name FROM CommandImpl c LEFT JOIN c.commandExecutions ce INNER JOIN ce.commandExecutionResults cer INNER JOIN c.task t INNER JOIN t.plugin p WHERE p.name = 'wol' AND (t.commandClsId = 'WAKE-MACHINE' OR t.commandClsId = 'SHUT-DOWN-MACHINE') AND t.createDate BETWEEN :startDate AND :endDate\",\"templateParams\":[{\"id\":69401,\"key\":\"startDate\",\"label\":\"start\",\"type\":\"DATE\",\"defaultValue\":null,\"mandatory\":true,\"createDate\":1467103363000},{\"id\":69402,\"key\":\"endDate\",\"label\":\"end\",\"type\":\"DATE\",\"defaultValue\":null,\"mandatory\":true,\"createDate\":1467103363000}],\"templateColumns\":[{\"id\":69051,\"name\":\"Sonuç\",\"columnOrder\":1,\"createDate\":1467097434000}],\"createDate\":1467035294000,\"modifyDate\":1468565343000},\"name\":\"Wake-on-LAN\",\"description\":\"test\",\"type\":\"TABLE\",\"viewParams\":[{\"id\":75154,\"referencedParam\":{\"id\":69401,\"key\":\"startDate\",\"label\":\"start\",\"type\":\"DATE\",\"defaultValue\":null,\"mandatory\":true,\"createDate\":1467103363000},\"label\":\"Başlangıç\",\"value\":\"2016-06-30 15:16:17\",\"createDate\":1468488539000},{\"id\":75153,\"referencedParam\":{\"id\":69402,\"key\":\"endDate\",\"label\":\"end\",\"type\":\"DATE\",\"defaultValue\":null,\"mandatory\":true,\"createDate\":1467103363000},\"label\":\"Bitiş\",\"value\":\"2016-07-01 15:16:17\",\"createDate\":1468488539000}],\"viewColumns\":[{\"id\":87353,\"referencedCol\":{\"id\":69051,\"name\":\"Sonuç\",\"columnOrder\":1,\"createDate\":1467097434000},\"type\":\"VALUE_FIELD\",\"legend\":\"Sonuç\",\"width\":100,\"createDate\":1468488539000}],\"createDate\":1467210425000,\"modifyDate\":1468488539000},{\"id\":79151,\"template\":{\"id\":78951,\"name\":\"Ahenk Log Kayıtları\",\"description\":\"Ahenk kurulu bilgisayarlardan toplanan log kayıtları\",\"query\":\"SELECT s.fromHost as fromhost,s.eventUser as eventuser, s.eventSource as eventsource,s.eventLogType as eventlogtype , s.genericFileName as genericfilename, s.message as message, s.receivedAt as receivedat, s.sysLogTag as syslogtag FROM SystemEventsImpl s WHERE s.fromHost LIKE :fromhostparam ORDER BY s.deviceReportedTime DESC\",\"templateParams\":[{\"id\":79001,\"key\":\"fromhostparam\",\"label\":\"Makina İsmi\",\"type\":\"STRING\",\"defaultValue\":null,\"mandatory\":false,\"createDate\":1467277994000}],\"templateColumns\":[{\"id\":79051,\"name\":\"From Host\",\"columnOrder\":1,\"createDate\":1467277994000},{\"id\":79052,\"name\":\"Event User\",\"columnOrder\":2,\"createDate\":1467277994000},{\"id\":79053,\"name\":\"Event Source\",\"columnOrder\":3,\"createDate\":1467277994000},{\"id\":79054,\"name\":\"Event Log Type\",\"columnOrder\":4,\"createDate\":1467277994000},{\"id\":79055,\"name\":\"Generic File Name\",\"columnOrder\":5,\"createDate\":1467277994000},{\"id\":79056,\"name\":\"Message\",\"columnOrder\":6,\"createDate\":1467277994000},{\"id\":79057,\"name\":\"Received At\",\"columnOrder\":7,\"createDate\":1467277994000},{\"id\":79058,\"name\":\"Sys Log Tag\",\"columnOrder\":8,\"createDate\":1467277994000}],\"createDate\":1467277994000,\"modifyDate\":null},\"name\":\"Ahenk Rsyslog\",\"description\":\"emre-test\",\"type\":\"TABLE\",\"viewParams\":[{\"id\":79201,\"referencedParam\":{\"id\":79001,\"key\":\"fromhostparam\",\"label\":\"Makina İsmi\",\"type\":\"STRING\",\"defaultValue\":null,\"mandatory\":false,\"createDate\":1467277994000},\"label\":\"Makina İsmi\",\"value\":\"Cemre\",\"createDate\":1467278229000}],\"viewColumns\":[{\"id\":79251,\"referencedCol\":{\"id\":79056,\"name\":\"Message\",\"columnOrder\":6,\"createDate\":1467277994000},\"type\":\"VALUE_FIELD\",\"legend\":\"Mesaj\",\"width\":100,\"createDate\":1467278229000}],\"createDate\":1467278229000,\"modifyDate\":null}]"
   }
}
    ```
