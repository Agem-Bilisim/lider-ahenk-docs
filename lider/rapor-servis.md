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

#### Tanımları Listeleme:

    * Request: http://localhost:8181/lider/view/list
    * Response:

### Tanımdan Rapor Çıktısı Oluşturma:

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
