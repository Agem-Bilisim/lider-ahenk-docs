#Lider - Ahenk Mesaj Tipleri ve JSON Yapıları

##Mesaj Tipleri

**Ahenk** ile **Lider** birbirlerine Ejabberd üzerinden mesaj göndererek haberleşir. Bu mesaj **JSON** formatındadır. Ve mesajlar farklı tiplerde olsa da **type** ve **timestamp** alan isimleri her mesajda bulunur.

**Ahenk** kurulu olduğu bilgisayarda ilk kez çalışırken kendini **Ejabberd server**, **veri tabanı**na ve **ldap**'a kaydetmesi üzere bir takım bilgiyi Lider'e gönderir. Bu işlem **Registration**'dır ve mesaj tipi **REGISTER**'dır. Registration mesajına Lider **REGISTRATION_RESPONSE** tipinde bir json mesajı döndürür.

Ahenk kurulu bilgisayarlarda kullanıcılar **LDAP** üzerinden authenticate olurlar. Oturum açma işlemi sırasında Ahenk, Lider'e 2 adet mesaj gönderir. Bunlar **LOGIN** ve **GET_POLICIES** mesajlarıdır. **LOGIN** mesajında Lider'e hangi kullanıcının  oturum açtığını bildirilir. **GET_POLICIES** mesajında (varsa önceden gönderilmiş politikanın bilgisi gönderilir) giriş yapan kullanıcı ile çalışmakta olan ajan bilgisayar üzerine önceden tanımlanmış politikaları Lider'den ister.

**GET_POLICIES** tipindeki mesaja Lider, **EXECUTE_POLICY** tipinde mesaj gönderir. Bu mesaj varsa kullanıcı ve ajan bilgisayar üzerinde tanımlı olan politikaların içeriği hakkında cevap döndürür. Burada **dikkat edilmesi gereken nokta**; Ahenk **GET_POLICIES** mesajıyla hali hazırda kayıtlı kullanıcı ve ajan politikalarının versiyonlarını Lidere gönderir. Lider bu kullanıcı-ajan üzerinde bu versiyondan daha yeni bir versiyon politika olmadığını görürse profili null olarak döndürür fakat geçerli politika versiyonunu yazar. Ahenk, Liderden dönen **EXECUTE_POLICY** mesajı ile gelen politikalara bakıp, yeni politika geldi ise veritabanını günceller. Ahenk güncel politikaları çalıştırmaya başlar. Eğer politika versiyonları null ise bu kullanıcı ya da ajan için politika tanımlanmamış demektir.

**EXECUTE_TASK** tipindeki mesajlar ise o anda çalıştırılması gereken görev için kullanılan gereklili parametreleri barındıran mesajdır.

Çalıştırılan her görev ya da Politika Profili sonunda Ahenk, Lidere **RESPONSE** gönderir. Response mesajı ile çalıştırılan görev/politikanın başarı durumu, varsa döndürülen data bu mesaj ile iletilir.

Ahenk bir görev ya da politika çalıştırmayı denediğinde eğer ilgili eklentiyi bulamazsa **MISSING_PLUGIN** tipinde bir mesaj gönderir.

Lider'e **MISSING_PLUGIN** tipinde mesaj eriştiğinde eğer ilgili eklentinin Lider tarafındaki yapılandırma dosyasında bu eklentinin ahenk tarafının deb versiyonuna nasıl erişilebileceği hakkında distro bilgileri tanımlandıysa **INSTALL_PLUGIN** tipinde bir mesaj döndürür.


_ _ _

###REGISTER (A>>>L)

**from:** alınmak istenen ejabberd jid.

**password:** alınacak ejabberd hesabının şifresi.

**hostname,ipAddresses,macAddresses** makine temel bilgileri..

**data:** çalışan işletim sistemi, donanım, oturumlar, vs... hakkında genişleyebilir veriler. Bu verilerin miktarı değişebilir ve Lider tarafında gruplandırma vs.. gibi işlemlerde gerekli olabilir. 

```json
{  
   "type":"REGISTER",
   "timestamp":"07-06-2016 09:58",
   "from":"2559305d-a415-38e7-8498-2dbc458662a7",
   "password":"41b6eeb5-1927-459a-b596-3115a40dfade",
   "hostname":"volkansahin",
   "ipAddresses":"'192.168.1.121', '192.168.56.1'",
   "macAddresses":"'74:d4:35:0c:74:2c', '0a:00:27:00:00:00'",
   "data":{  
      "hardware.disk.used":86574,
      "hardware.cpu.architecture":"x86_64",
      "hardware.disk.partitions":[  
         ["/dev/sda2","/","ext4","rw,errors=remount-ro"],
         ["/dev/sda1","/boot/efi","vfat","rw"]],
      "os.name":"Linux",
      "hostname":"volkansahin",
      "ipAddresses":"'192.168.1.121', '192.168.56.1'",
      "sessions.userNames":["volkan"],
      "hardware.cpu.physicalCoreCount":2,
      "os.distributionVersion":"17",
      "macAddresses":"'74:d4:35:0c:74:2c', '0a:00:27:00:00:00'",
      "hardware.memory.total":7112,
      "hardware.disk.total":104790,
      "os.kernel":"3.13.0-24-generic",
      "hardware.disk.free":12869,
      "os.architecture":"64bit",
      "os.version":"#47-Ubuntu SMP Fri May 2 23:30:00 UTC 2014",
      "os.distributionName":"LinuxMint",
      "hardware.network.ipAddresses":[  
         "192.168.1.121",
         "192.168.56.1"],
      "hardware.cpu.logicalCoreCount":4,
      "os.distributionId":"qiana"
   }
}
```

_ _ _

###REGISTRATION_RESPONSE (L>>>A)
**status:** Kayıt işleminin sonucu.  (status **REGISTERED, REGISTERED_WITHOUT_LDAP, ALREADY_EXISTS, REGISTRATION_ERROR** değerlerinden biri olabilir)

**message:** Kayıt işleminin sonucu (text alan)

**agentDn:** LDAP kayıtı gerçekleştirdiyse LDAP kaydının DN değeri.

**REGISTRATION_ERROR** mesajı dönerse Ahenk belli miktarda tekrar REGISTRATION isteği gönderir. Deneme sayısı biterse hata mesajı oluşturup kendisini kapatır.

```json
{"type":"REGISTRATION_RESPONSE",
"status":"ALREADY_EXISTS",
"message":"cn=2559305d-a415-38e7-8498-2dbc458662a7,ou=Uncategorized,dc=mys,dc=pardus,dc=org already exists! Updated its password and database properties with the values submitted.",
"agentDn":"cn=2559305d-a415-38e7-8498-2dbc458662a7,ou=Uncategorized,dc=mys,dc=pardus,dc=org",
"timestamp":1465282735658}
```


_ _ _

###LOGIN (A>>>L)
**username:** Giriş yapan kullanıcının adı.

```json 
{"type": "LOGIN", 
"username": "volkan", 
"timestamp": "07-06-2016 10:01"}
```

_ _ _

###LOGOUT (A>>>L)
**username:** Çıkış yapan kullanıcının adı.
```json
{  
   "type":"LOGOUT",
   "username":"volkan",
   "timestamp":"07-06-2016 11:11"
}
```

_ _ _

###GET_POLICIES (A>>>L)

**userPolicyVersion:** Kayıtlı kullanıcı politikası versiyonu

**agentPolicyVersion:** Kayıtlı ajan politikası versiyonu

**username:** İstenilen politikanın sahibi.
(aşağıdaki örnekte kayıtlı bir versiyon bulunmadığı için değerler null atanmış)

```json
{  
   "type":"GET_POLICIES",
   "userPolicyVersion":null,
   "agentPolicyVersion":null,
   "username":"volkan",
   "timestamp":"07-06-2016 10:01"
}
```

_ _ _

###EXECUTE_POLICY (L>>>A)
**username**: Kullanıcı politikasının sahibinin kullanıcı adı.

**userPolicyProfiles**: Kullanıcı politikasının barındırdığı profillerin içeriği. Json array formatındadır.

**userPolicyVersion**: Kullanıcı politikasının versiyonu

**userCommandExecutionId**: Kullanıcı politikasının çalıştırma id'si

**agentPolicyVersion**: Ajan politikasının versiyonu

**agentPolicyProfiles**: Ajan politikasının barındırdığı profillerin içeriği. Json array formatındadır.

**agentCommandExecutionId**: Ajan politikasının çalıştırma id'si

Aşağıdaki örnekte  **userPolicyProfiles** ve **userPolicyVersion** değerleri null atanmış; çünkü gelen **GET_POLICIES** mesajındakinden daha gündel bir versiyon bulunmamakta. 

**userPolicyProfiles** ve **agentPolicyProfiles** aynı formattadırlar ve  barındırdıkları profillerde id,çalışacakları plugin hakkında bilgileri, profil adı, tanımı, console'da oluşturulan parametreler (**profileData**)bulunur.

```json
{  
   "type":"EXECUTE_POLICY",
   "username":"volkan",
   "userPolicyProfiles":null,
   "userPolicyVersion":null,
   "userCommandExecutionId":null,
   "agentPolicyVersion":"2701-1",
   "agentCommandExecutionId":2454,
   "agentPolicyProfiles":[  
      {  
         "id":2651,
         "plugin":{  
            "id":1801,
            "name":"browser",
            "version":"1.0.0",
            "description":"Lider Browser Plugin",
            "active":true,
            "deleted":true,
            "machineOriented":false,
            "userOriented":true,
            "policyPlugin":true,
            "xBased":false,
            "createDate":1464254239000,
            "modifyDate":1465203376000
         },
         "label":"volkan_t",
         "description":"anasayfa",
         "overridable":false,
         "active":true,
         "deleted":false,
         "profileData":{  
            "preferences":[  
               {  
                  "preferenceName":"extensions.BlockSite.enabled",
                  "value":"false"
               },
               {  
                  "preferenceName":"extensions.BlockSite.showWarning",
                  "value":"false"
               },
               {  
                  "preferenceName":"extensions.BlockSite.removeLinks",
                  "value":"false"
               },
               ....
            ]
         },
         "createDate":1464263525000,
         "modifyDate":null
      }
   ],
   "timestamp":1465282879781
}
```

_ _ _

###EXECUTE_TASK (L>>>A)

**task**: Çalıştırılacak görev hakkında bilgiler bulunur. Yapısı **(user/agent)PolicyProfiles**' e benzerdir. Konsoldan gelen parametreler **parameterMap**'tedir.

```json
{  
   "type":"EXECUTE_TASK",
   "task":{  
      "id":21856,
      "plugin":{  
         "id":1951,
         "name":"network-inventory",
         "version":"1.0.0",
         "description":"Lider Network Inventory Plugin",
         "active":true,
         "deleted":false,
         "machineOriented":true,
         "userOriented":false,
         "policyPlugin":false,
         "xBased":false,
         "createDate":1464254876000,
         "modifyDate":1465283994000
      },
      "commandClsId":"SCANNETWORK",
      "parameterMap":{  
         "timingTemplate":"3",
         "ipRange":"192.168.1.100-101",
         "executeOnAgent":true
      },
      "deleted":false,
      "cronExpression":null,
      "createDate":1465285670481,
      "modifyDate":null
   },
   "timestamp":"07-06-2016 10:47"
}
```

_ _ _

###POLICY RESPONSE (A>>>L)


**policyVersion**: Çalıştırılan politika versiyonu

**commandExecutionId**: Polikanın çalıştırılma id'si

**responseMessage**: Sonuç mesajı(free text)

**responseCode**:Politikanın çalıştırılma durumuna göre POLICY_ERROR,POLICY_KILLED,POLICY_PROCESSED,POLICY_RECEIVED,POLICY_TIMEOUT,POLICY_WARNING değerlerini alabilir.

**responseData**: Varsa politika sonucu döndürülmek istenen data.

**contentType**: Data'yı tanımlayan tip. Bu tipler şimdilik şunlar olabilir: APPLICATION_MS_WORD,APPLICATION_PDF,APPLICATION_VND_MS_EXCEL,IMAGE_JPEG,IMAGE_PNG,TEXT_HTML,TEXT_PLAIN,APPLICATION_JSON

```json
{  
   "timestamp":"07-06-2016 03:43",
   "type":"POLICY_STATUS",
   "policyVersion":"2551-1",
   "commandExecutionId":"2502",
   "responseMessage":"/opt/thunderbird/thunderbird | Unprivileged | Successful, /opt/firefox/firefox | Privileged | Successful, /usr/bin/kate | Unprivileged | Successful, /usr/bin/gedit | Privileged | Successful, ",
   "responseCode":"POLICY_PROCESSED",
   "responseData":null,
   "contentType":null
}
```


_ _ _

###TASK RESPONSE (A>>>L)

**taskId**: Çalıştırılan görevin id'si
**responseMessage**: Sonuç mesajı(free text)
**responseCode**:Görevin çalıştırılma durumuna göre TASK_ERROR,TASK_KILLED,TASK_PROCESSED,TASK_TIMEOUT,TASK_WARNING,TASK_RECEIVED değerlerini alabilir.
**responseData**: Varsa görev sonucu döndürülmek istenen data.
**contentType**: Data'yı tanımlayan tip. Bu tipler şimdilik şunlar olabilir: APPLICATION_MS_WORD,APPLICATION_PDF,APPLICATION_VND_MS_EXCEL,IMAGE_JPEG,IMAGE_PNG,TEXT_HTML,TEXT_PLAIN,APPLICATION_JSON


```json
{  
   "timestamp":"07-06-2016 03:43",
   "type":"POLICY_STATUS",
   "taskId":"2502",
   "responseMessage":"/opt/thunderbird/thunderbird | Unprivileged | Successful, /opt/firefox/firefox | Privileged | Successful, /usr/bin/kate | Unprivileged | Successful, /usr/bin/gedit | Privileged | Successful, ",
   "responseCode":"POLICY_PROCESSED",
   "responseData":null,
   "contentType":null
}
```

_ _ _


###MISSING_PLUGIN (A>>>L)
**pluginName:** Eksik ahenk eklentisinin adı.
**pluginVersion:** Eksik ahenk eklentisinin versiyonu.

```json
{  
   "pluginVersion":"1.0.0",
   "type":"MISSING_PLUGIN",
   "pluginName":"browser"
}
```

_ _ _

###INSTALL_PLUGIN (L>>>A)
**pluginName**: Kurulacak Ahenk eklentisinin adı.
**pluginVersion**:Kurulacak Ahenk eklentisinin versiyonu.
**protocol**: Kurma yönteminin protokolu. (ssh,htttp,torrent)
**parameterMap**: Seçilen protokole bağlı parametre isterleri

```json
{  
   "type":"INSTALL_PLUGIN",
   "pluginName":"browser",
   "pluginVersion":"1.0.0",
   "protocol":"HTTP",
   "parameterMap":{  
      "url":"http://www.agem.com.tr/msb"
   },
   "timestamp":1465282879968
}
```

_ _ _

**(L>>>A) Liderden Ahenk' e giden mesaj tipleri</br>(A>>>L) Ahenk'ten Lider'e giden mesaj tipleri**

