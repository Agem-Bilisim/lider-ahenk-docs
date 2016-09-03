## Veritabanı

Lider Ahenk veritabanı olarak MariaDB 10.x kullanılmakta ve veritabanı katmanı olarak Lider içerisinde OpenJPA kullanılmaktadır.
Veritabanına ait EER diagramına [bu adresten](http://agem.com.tr/liderahenk/liderdb.mwb) erişebilirsiniz.

Lider Ahenk (görev işletimi, politika uygulama, Ahenk kaydı gibi) süreçlerinde sıklıkla kullanılan bazı veritabanı tabloları
ve açıklamaları aşağıdaki gibidir:

### C_COMMAND

```
+-------------------+--------------+------+-----+---------+-------+
| Field             | Type         | Null | Key | Default | Extra |
+-------------------+--------------+------+-----+---------+-------+
| COMMAND_ID        | bigint(20)   | NO   | PRI | NULL    |       |
| ACTIVATION_DATE   | date         | YES  |     | NULL    |       |
| COMMAND_OWNER_UID | varchar(255) | YES  |     | NULL    |       |
| CREATE_DATE       | datetime     | NO   |     | NULL    |       |
| DN_LIST           | text         | YES  |     | NULL    |       |
| DN_TYPE           | int(11)      | YES  |     | NULL    |       |
| POLICY_ID         | bigint(20)   | YES  | MUL | NULL    |       |
| TASK_ID           | bigint(20)   | YES  | MUL | NULL    |       |
+-------------------+--------------+------+-----+---------+-------+
```

İşletilen her bir görev veya uygulanan her bir politikaya karşılık C_COMMAND tablosunda yeni bir kayıt oluşturulur. Yapılan 
işlemin (görev yada politika) LDAP üzerinde hangi ögeler (DN_LIST) üzerinde çalıştırıldığı, bu ögelerden hangilerinin dikkate
alınması gerektiği (DN_TYPE) ve işlemin kim tarafından yapıldığını kayıt altına alır. Söz konusu her bir kayıt C_TASK veya C_POLICY
tablosuna referans vermektedir.

### C_TASK

```
+-----------------+--------------+------+-----+---------+-------+
| Field           | Type         | Null | Key | Default | Extra |
+-----------------+--------------+------+-----+---------+-------+
| TASK_ID         | bigint(20)   | NO   | PRI | NULL    |       |
| COMMAND_CLS_ID  | varchar(255) | YES  |     | NULL    |       |
| CREATE_DATE     | datetime     | NO   |     | NULL    |       |
| CRON_EXPRESSION | varchar(255) | YES  |     | NULL    |       |
| DELETED         | bit(1)       | YES  |     | NULL    |       |
| MODIFY_DATE     | datetime     | YES  |     | NULL    |       |
| PARAMETER_MAP   | longblob     | YES  |     | NULL    |       |
| PLUGIN_ID       | bigint(20)   | NO   | MUL | NULL    |       |
+-----------------+--------------+------+-----+---------+-------+
```

İşletilen her bir görev için C_TASK tablosunda yeni bir kayıt oluşturulmaktadır ve görev işletimi sırasında Lider Arayüz üzerinden
gönderilen görev parametrelerini içermektedir.

### C_POLICY 

```
+----------------+--------------+------+-----+---------+-------+
| Field          | Type         | Null | Key | Default | Extra |
+----------------+--------------+------+-----+---------+-------+
| POLICY_ID      | bigint(20)   | NO   | PRI | NULL    |       |
| ACTIVE         | bit(1)       | YES  |     | NULL    |       |
| CREATE_DATE    | datetime     | NO   |     | NULL    |       |
| DELETED        | bit(1)       | YES  |     | NULL    |       |
| DESCRIPTION    | varchar(255) | YES  |     | NULL    |       |
| LABEL          | varchar(255) | NO   |     | NULL    |       |
| MODIFY_DATE    | datetime     | YES  |     | NULL    |       |
| POLICY_VERSION | varchar(255) | YES  |     | NULL    |       |
+----------------+--------------+------+-----+---------+-------+
```

Kaydedilen her bir politika için C_POLICY tablosunda bir kayıt oluşturulmakta ve her bir kayıt ilgili politikaya
dair politika adı, açıklaması, oluşturulma tarihi ve sürümü gibi bilgileri tutmaktadır. Diğer belgelerde açıklandığı gibi bir
politika bir veya birden fazla profil kaydından oluşmaktadır ve politikaya ait herhangi bir profilin değiştirilmesi durumunda
politika üzerindeki sürüm değeri bir arttırılmaktadır. Bu sürüm değeri Ahenk'in politika sorguladığı durumlarda bir politikanın
değişip değişmediğinin anlaşılması için kullanılmaktadır.

### C_COMMAND_EXECUTION

```
+----------------------+--------------+------+-----+---------+-------+
| Field                | Type         | Null | Key | Default | Extra |
+----------------------+--------------+------+-----+---------+-------+
| COMMAND_EXECUTION_ID | bigint(20)   | NO   | PRI | NULL    |       |
| CREATE_DATE          | datetime     | NO   |     | NULL    |       |
| DN                   | varchar(255) | YES  |     | NULL    |       |
| DN_TYPE              | int(11)      | YES  |     | NULL    |       |
| COMMAND_ID           | bigint(20)   | NO   | MUL | NULL    |       |
+----------------------+--------------+------+-----+---------+-------+
```

İşletilen her bir görev ve uygulanan her bir politika için C_COMMAND tablosunda bir kayıt oluşturulduğundan bahsetmiştik. Yapılan
bu işlemde kaç tane LDAP ögesi var ise, her biri için C_COMMAND_EXECUTION tablosunda birer kayıt oluşturulmaktadır. Örneğin; LDAP
ağacında 3 adet Ahenk ögesi seçilerek 'ekran görüntüsü al' görevi işletildiğinde, 1 adet C_COMMAND kaydı altında (her bir Ahenk 
DN değerine karşılık) 3 adet C_COMMAND_EXECUTION kaydı oluşturulur.

### C_COMMAND_EXECUTION_RESULT

```
+-----------------------------+------------+------+-----+---------+-------+
| Field                       | Type       | Null | Key | Default | Extra |
+-----------------------------+------------+------+-----+---------+-------+
| COMMAND_EXECUTION_RESULT_ID | bigint(20) | NO   | PRI | NULL    |       |
| AGENT_ID                    | bigint(20) | YES  |     | NULL    |       |
| CONTENT_TYPE                | int(11)    | YES  |     | NULL    |       |
| CREATE_DATE                 | datetime   | NO   |     | NULL    |       |
| RESPONSE_CODE               | int(11)    | NO   |     | NULL    |       |
| RESPONSE_DATA               | longblob   | YES  |     | NULL    |       |
| RESPONSE_MESSAGE            | text       | YES  |     | NULL    |       |
| COMMAND_EXECUTION_ID        | bigint(20) | NO   | MUL | NULL    |       |
+-----------------------------+------------+------+-----+---------+-------+
```

Detay tablo olarak görev yapan C_COMMAND_EXECUTION_RESULT tablosu işletilen her bir görev veya uygulanan her bir politika
ait sonuç mesajların saklanması için kullanılmaktadır. Buna göre daha önceki örnekten devam edersek, LDAP ağacında 3 adet 
Ahenk ögesi seçilerek 'ekran görüntüsü al' görevi işletildiğinde, 1 adet C_COMMAND kaydı altında 3 adet C_COMMAND_EXECUTION 
kaydı ve her bir C_COMMAND_EXECUTION kaydı altında 1 adet C_COMMAND_EXECUTION_RESULT kaydı oluşturulur.

Bu tablonun detay tablo olarak yaratılması, görev veya politika sonucu olarak birden fazla sonucun döndürülebilmesine olanak sağlar.
Buna göre; örneğin (CPU, disk, RAM bellek gibi) sistem kaynaklarının belirli aralıklarla dinlendiği ve eşik değerin aşılmasıyla 
uyarı mesajı döndürüldüğü bir görev işletilirken, eşik değer her geçildiğinde gönderilen görev sonucu bir C_COMMAND_EXECUTION_RESULT
kaydı olarak veritabanında tutulmaktadır.

