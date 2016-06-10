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
| COLUMN_ID          | bigint(20)   | NO   | PRI | NULL    |       |
| COLUMN_ORDER       | int(11)      | NO   |     | NULL    |       |
| NAME               | varchar(255) | NO   |     | NULL    |       |
| VISIBLE            | bit(1)       | YES  |     | NULL    |       |
| WIDTH              | int(11)      | YES  |     | NULL    |       |
| REPORT_TEMPLATE_ID | bigint(20)   | NO   | MUL | NULL    |       |
+--------------------+--------------+------+-----+---------+-------+
```

##### R_REPORT_TEMPLATE_PARAMETER:

```
+--------------------+--------------+------+-----+---------+-------+
| Field              | Type         | Null | Key | Default | Extra |
+--------------------+--------------+------+-----+---------+-------+
| PARAMETER_ID       | bigint(20)   | NO   | PRI | NULL    |       |
| PARAMETER_KEY      | varchar(255) | NO   |     | NULL    |       |
| LABEL              | varchar(255) | NO   |     | NULL    |       |
| PARAMETER_TYPE     | int(11)      | NO   |     | NULL    |       |
| REPORT_TEMPLATE_ID | bigint(20)   | NO   | MUL | NULL    |       |
+--------------------+--------------+------+-----+---------+-------+
```

### 2. Rapor şablonu sorgulama

* **URL**: http://localhost:8181/lider/report/{id}/get
* **Method**: GET
* Örnek:

    * Request: http://localhost:8181/lider/report/5951/get
    * Response:

    ```json
{
"status": "OK"
"messages": [1]
0:  "Record retrieved."
-
"resultMap": {
"template": "{"id":5951,"name":"Çalıştırılan Görevler","description":"Çalıştırılan görevler hakkında detay rapor","query":"SELECT p.name, p.version, t.commandClsId, t.createDate, SUM(CASE WHEN cer.responseCode = :resp_success then 1 ELSE 0 END) as success, SUM(CASE WHEN cer.responseCode = :resp_received THEN 1 ELSE 0 END) as received, SUM(CASE WHEN cer.responseCode = :resp_error then 1 ELSE 0 END) as error FROM CommandImpl c LEFT JOIN c.commandExecutions ce LEFT JOIN ce.commandExecutionResults cer INNER JOIN c.task t INNER JOIN t.plugin p WHERE p.name LIKE :pluginName AND p.version LIKE :pluginVersion GROUP BY p.name, p.version, t.commandClsId, t.createDate","templateParams":[{"id":11002,"key":"pluginVersion","label":"Eklenti sürümü","type":"STRING"},{"id":11001,"key":"pluginName","label":"Eklenti adı","type":"STRING"}],"templateColumns":[],"reportHeader":"","reportFooter":"","createDate":1464594928000,"modifyDate":1465544594000}"
}-
}
    ```

* Örnek 2:

    * Request: http://localhost:8181/lider/report/14951/get
    * Response:

    ```json
{
"status": "OK"
"messages": [1]
0:  "Record retrieved."
-
"resultMap": {
"template": "{"id":14951,"name":"Rsyslog Konfigurasyonu","description":"Log dosyalarının konfigurasyonu hakkında detay rapor","query":"SELECT s.fromHost as fromhost,s.eventUser as eventuser, s.eventSource as eventsource,s.eventLogType as eventlogtype , s.genericFileName as genericfilename, s.message as message, s.receivedAt as receivedat, s.sysLogTag as syslogtag FROM SystemEventsImpl s WHERE s.fromHost LIKE :fromhostparam","templateParams":[{"id":15651,"key":"fromhostparam","label":"Makina İsmi","type":"STRING"}],"templateColumns":[{"id":16401,"name":"From Host","visible":true,"width":null,"columnOrder":1},{"id":16402,"name":"Event User","visible":true,"width":null,"columnOrder":2},{"id":16403,"name":"Event Source","visible":true,"width":null,"columnOrder":3},{"id":16404,"name":"Event Log Type","visible":true,"width":null,"columnOrder":4},{"id":16405,"name":"Generic File Name","visible":true,"width":null,"columnOrder":5},{"id":16351,"name":"Message","visible":true,"width":null,"columnOrder":6},{"id":16406,"name":"Received At","visible":true,"width":null,"columnOrder":7},{"id":16407,"name":"Sys Log Tag","visible":true,"width":null,"columnOrder":8}],"reportHeader":"","reportFooter":"","createDate":1464867135000,"modifyDate":1464954819000}"
}-
}
    ```

> **Not** Diğer CRUD işlemleri için örnek URL'ler aşağıdaki gibidir:
> **URL**: http://localhost:8181/lider/report/{id}/delete **Method**: GET
> **URL**: http://localhost:8181/lider/report/list **Method**: GET
> **URL**: http://localhost:8181/lider/report/update **Method**: POST
> **URL**: http://localhost:8181/lider/report/add **Method**: POST
> **URL**: http://localhost:8181/lider/report/validate **Method**: POST

### 3. Rapor şablonundan rapor üretimi

* **URL**: http://localhost:8181/lider/report/generate
* **Method**: POST

