# Raporlama altyapısı

### Veritabanı yapısı

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

### Rapor Şablonları ile İlgili İşlemler

### Şablonları Listeleme:

* Request: http://localhost:8181/lider/template/list
* Response:

    ```json
{
	"status": "OK",
	"messages": ["Records listed."],
	"resultMap": {
		"templates": "[{\"id\":52701,\"name\":\"Çalıştırılan Görevler\",\"description\":\"Anlık Olarak, Sisteme Bağlı Bulunan Tüm Kullanıcılara Ait Bilgiler İçeren Rapor.\",\"query\":\"SELECT a.id, a.jid, us.username, us.createDate, a.ipAddresses, a.dn   FROM UserSessionImpl us INNER JOIN us.agent a WHERE us.sessionEvent = 1 \\tAND NOT EXISTS (select 1 from UserSessionImpl logout where logout.sessionEvent = 2 and logout.agent = us.agent  \\t\\t\\tand logout.username = us.username and logout.createDate > us.createDate) ORDER BY us.createDate, us.username\",\"templateParams\":[],\"templateColumns\":[{\"id\":118453,\"name\":\"Sıra No.\",\"columnOrder\":1,\"createDate\":1468853068000},{\"id\":55803,\"name\":\"Görev tarihi\",\"columnOrder\":4,\"createDate\":1466521725000},{\"id\":55801,\"name\":\"Başarılı olanlar\",\"columnOrder\":5,\"createDate\":1466521725000},{\"id\":118452,\"name\":\"IP Adresleri\",\"columnOrder\":6,\"createDate\":1468853068000},{\"id\":70901,\"name\":\"Başarısız olanlar\",\"columnOrder\":7,\"createDate\":1467115539000}],\"createDate\":1466406569000,\"modifyDate\":1468853068000},{\"id\":67201,\"name\":\"Wake-on-LAN\",\"description\":\"Uyandırılan ya da Kapatılan Bilgisayarlar Hakkında Detaylı Rapor\",\"query\":\"SELECT cer.responseMessage, t.createDate, p.name FROM CommandImpl c LEFT JOIN c.commandExecutions ce INNER JOIN ce.commandExecutionResults cer INNER JOIN c.task t INNER JOIN t.plugin p WHERE p.name = 'wol' AND (t.commandClsId = 'WAKE-MACHINE' OR t.commandClsId = 'SHUT-DOWN-MACHINE') AND t.createDate BETWEEN :startDate AND :endDate\",\"templateParams\":[{\"id\":69401,\"key\":\"startDate\",\"label\":\"start\",\"type\":\"DATE\",\"defaultValue\":null,\"mandatory\":true,\"createDate\":1467103363000},{\"id\":69402,\"key\":\"endDate\",\"label\":\"end\",\"type\":\"DATE\",\"defaultValue\":null,\"mandatory\":true,\"createDate\":1467103363000}],\"templateColumns\":[{\"id\":69051,\"name\":\"Sonuç\",\"columnOrder\":1,\"createDate\":1467097434000}],\"createDate\":1467035294000,\"modifyDate\":1468565343000},{\"id\":69851,\"name\":\"Çevrimiçi Kullanıcılar\",\"description\":\"Anlık olarak sisteme bağlı olan tüm kullanıcılara ait bilgiler içerir.\",\"query\":\"SELECT a.id, a.jid, us.username, us.createDate, a.ipAddresses, a.dn   FROM UserSessionImpl us INNER JOIN us.agent a WHERE us.sessionEvent = 1 \\tAND NOT EXISTS (select 1 from UserSessionImpl logout where logout.sessionEvent = 2 and logout.agent = us.agent and logout.username = us.username and logout.createDate > us.createDate) ORDER BY us.createDate, us.username\",\"templateParams\":[],\"templateColumns\":[{\"id\":69901,\"name\":\"Kullanıcı Adı\",\"columnOrder\":3,\"createDate\":1467106380000},{\"id\":69902,\"name\":\"Sisteme Giriş Tarihi\",\"columnOrder\":4,\"createDate\":1467106380000},{\"id\":69903,\"name\":\"IP Adresleri\",\"columnOrder\":5,\"createDate\":1467106380000},{\"id\":69904,\"name\":\"DN\",\"columnOrder\":6,\"createDate\":1467106380000}],\"createDate\":1467106380000,\"modifyDate\":null},{\"id\":78951,\"name\":\"Ahenk Log Kayıtları\",\"description\":\"Ahenk kurulu bilgisayarlardan toplanan log kayıtları\",\"query\":\"SELECT s.fromHost as fromhost,s.eventUser as eventuser, s.eventSource as eventsource,s.eventLogType as eventlogtype , s.genericFileName as genericfilename, s.message as message, s.receivedAt as receivedat, s.sysLogTag as syslogtag FROM SystemEventsImpl s WHERE s.fromHost LIKE :fromhostparam ORDER BY s.deviceReportedTime DESC\",\"templateParams\":[{\"id\":79001,\"key\":\"fromhostparam\",\"label\":\"Makina İsmi\",\"type\":\"STRING\",\"defaultValue\":null,\"mandatory\":false,\"createDate\":1467277994000}],\"templateColumns\":[{\"id\":79051,\"name\":\"From Host\",\"columnOrder\":1,\"createDate\":1467277994000},{\"id\":79052,\"name\":\"Event User\",\"columnOrder\":2,\"createDate\":1467277994000},{\"id\":79053,\"name\":\"Event Source\",\"columnOrder\":3,\"createDate\":1467277994000},{\"id\":79054,\"name\":\"Event Log Type\",\"columnOrder\":4,\"createDate\":1467277994000},{\"id\":79055,\"name\":\"Generic File Name\",\"columnOrder\":5,\"createDate\":1467277994000},{\"id\":79056,\"name\":\"Message\",\"columnOrder\":6,\"createDate\":1467277994000},{\"id\":79057,\"name\":\"Received At\",\"columnOrder\":7,\"createDate\":1467277994000},{\"id\":79058,\"name\":\"Sys Log Tag\",\"columnOrder\":8,\"createDate\":1467277994000}],\"createDate\":1467277994000,\"modifyDate\":null}]"
	}
}
    ```

### 3. Rapor Tanımları ile İlgili İşlemler

#### Tanımları Listeleme:

* Request: http://localhost:8181/lider/view/list
* Response:

    ```json
{
	"status": "OK",
	"messages": ["Records listed."],
	"resultMap": {
		"views": "[{\"id\":79151,\"template\":{\"id\":78951,\"name\":\"Ahenk Log Kayıtları\",\"description\":\"Ahenk kurulu bilgisayarlardan toplanan log kayıtları\",\"query\":\"SELECT s.fromHost as fromhost,s.eventUser as eventuser, s.eventSource as eventsource,s.eventLogType as eventlogtype , s.genericFileName as genericfilename, s.message as message, s.receivedAt as receivedat, s.sysLogTag as syslogtag FROM SystemEventsImpl s WHERE s.fromHost LIKE :fromhostparam ORDER BY s.deviceReportedTime DESC\",\"templateParams\":[{\"id\":79001,\"key\":\"fromhostparam\",\"label\":\"Makina İsmi\",\"type\":\"STRING\",\"defaultValue\":null,\"mandatory\":false,\"createDate\":1467277994000}],\"templateColumns\":[{\"id\":79051,\"name\":\"From Host\",\"columnOrder\":1,\"createDate\":1467277994000},{\"id\":79052,\"name\":\"Event User\",\"columnOrder\":2,\"createDate\":1467277994000},{\"id\":79053,\"name\":\"Event Source\",\"columnOrder\":3,\"createDate\":1467277994000},{\"id\":79054,\"name\":\"Event Log Type\",\"columnOrder\":4,\"createDate\":1467277994000},{\"id\":79055,\"name\":\"Generic File Name\",\"columnOrder\":5,\"createDate\":1467277994000},{\"id\":79056,\"name\":\"Message\",\"columnOrder\":6,\"createDate\":1467277994000},{\"id\":79057,\"name\":\"Received At\",\"columnOrder\":7,\"createDate\":1467277994000},{\"id\":79058,\"name\":\"Sys Log Tag\",\"columnOrder\":8,\"createDate\":1467277994000}],\"createDate\":1467277994000,\"modifyDate\":null},\"name\":\"Ahenk Rsyslog\",\"description\":\"emre-test\",\"type\":\"TABLE\",\"viewParams\":[{\"id\":79201,\"referencedParam\":{\"id\":79001,\"key\":\"fromhostparam\",\"label\":\"Makina İsmi\",\"type\":\"STRING\",\"defaultValue\":null,\"mandatory\":false,\"createDate\":1467277994000},\"label\":\"Makina İsmi\",\"value\":\"Cemre\",\"createDate\":1467278229000}],\"viewColumns\":[{\"id\":79251,\"referencedCol\":{\"id\":79056,\"name\":\"Message\",\"columnOrder\":6,\"createDate\":1467277994000},\"type\":\"VALUE_FIELD\",\"legend\":\"Mesaj\",\"width\":100,\"createDate\":1467278229000}],\"createDate\":1467278229000,\"modifyDate\":null}]"
	}
}
    ```

#### Tanımdan Rapor Çıktısı Oluşturma:

* Request: http://localhost:8181/lider/view/generate

    ```json
{
  "viewId": 75102,
  "paramValues": {
      "startDate":"2016-06-30 15:16:17", "endDate":"2016-07-01 15:16:17"
   }
}
    ```
    
* Response:

    ```json
{
	"status": "OK",
	"messages": ["Record retrieved."],
	"resultMap": {
		"data": "[[null,\"30-06-2016 17:11\",\"wol\"],[null,\"30-06-2016 17:14\",\"wol\"],[null,\"30-06-2016 17:16\",\"wol\"],[null,\"30-06-2016 17:17\",\"wol\"],[null,\"30-06-2016 17:20\",\"wol\"],[null,\"30-06-2016 17:33\",\"wol\"],[null,\"30-06-2016 17:39\",\"wol\"],[null,\"01-07-2016 11:03\",\"wol\"],[null,\"01-07-2016 11:29\",\"wol\"],[null,\"01-07-2016 11:34\",\"wol\"],[null,\"01-07-2016 11:36\",\"wol\"],[null,\"01-07-2016 11:39\",\"wol\"],[null,\"01-07-2016 11:42\",\"wol\"],[null,\"01-07-2016 11:44\",\"wol\"],[null,\"01-07-2016 11:45\",\"wol\"],[null,\"01-07-2016 11:47\",\"wol\"],[null,\"01-07-2016 11:51\",\"wol\"],[\"A problem occured while handling WOL task: string index out of range\",\"01-07-2016 11:52\",\"wol\"],[\"A problem occured while handling WOL task: string index out of range\",\"01-07-2016 11:52\",\"wol\"],[\"[Wol - Wake Machine] Machine is awake. Mac Address(es): 1c:b7:2c:a7:d8:ec, Host: 192.168.1.122, Port: 80\",\"01-07-2016 11:54\",\"wol\"],[\"[Wol - Wake Machine] Machine is awake. Mac Address(es): 1c:b7:2c:a7:d8:ec, Host: 192.168.1.122, Port: 80\",\"01-07-2016 11:54\",\"wol\"]]",
		"columns": [{
			"id": 87353,
			"referencedCol": {
				"id": 69051,
				"name": "Sonuç",
				"columnOrder": 1,
				"createDate": 1467097434000
			},
			"type": "VALUE_FIELD",
			"legend": "Sonuç",
			"width": 100,
			"createDate": 1468488539000
		}],
		"type": "TABLE"
	}
}
    ```

#### Kaynak Kod

- [Rapor servis için Controller sınıfı](https://github.com/Pardus-Kurumsal/lider/blob/master/lider-web/src/main/java/tr/org/liderahenk/web/controller/ReportController.java)
