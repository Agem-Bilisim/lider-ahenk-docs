## Modüller

Lider projesi içerisinde yer alan OSGI modüllerinin (bundle) kısa tanımları ve işlevleri aşağıdaki gibidir:

### lider-authorization-impl

Görev çalıştırma ve rapor oluşturma/görüntüleme gibi 'action'lara ait yetkilendirme/doğrulama işlemlerini yapmakla yükümlü 
servisi sunmaktadır.

### lider-cache-impl

Diğer Lider modülleri tarafından kullanılabilecek önbellekleme mekanizmalarını sunmaktadır.

### lider-config

Lider tarafından kullanılan çoğu yapılandırma ayarına ait yapılandırma dosyası (tr.org.liderahenk.cfg) ve bu dosyayı takip
ederek dinamik bir şekilde diğer modüllerin hizmetine sunan bir yapılandırma servisinden oluşmaktadır.

### lider-core

Tüm Lider modülleri ve eklentilerinin geliştirilmesi için gerekli olan Java arayüz sınıflarından oluşan iskelet modüldür. OSGI
servisleri arayüz tanımlamaları üzerinden gerçekleştirildiğinden, her modül tarafından erişilebilir bir iskelet modül ihtiyacını
karşılamak için ortaya çıkmıştır.

### lider-datasource-mariadb

MariaDB veritabanı bağlantısının yönetilmesi için gerekli yapılandırma dosyalarını içeren modüldür. Bağlantı havuzu bu modüldeki
yapılandırmaya göre oluşturulur ve otomatik olarak yönetilir.

### lider-deployer

Lider Ahenk eklentilerinin tek bir merkezden Lider, Lider Arayüz ve Ahenk bileşenlerine dağıtılması için geliştirilmekte olan
modüldür. 

### lider-itest

Lider entegrasyon testlerinin yer aldığı modüldür. Şu an için test ortamına altyapı sağlayan ve birkaç test senaryosu içeren modüle
gerekli test senaryoları zamanla eklenerek sistemin tamamının test edilebilir hale gelmesi hedeflenmektedir.

### lider-karaf

Lider modüllerinin Karaf OSGI konteynırına başarılı bir şekilde yüklenebilmesi için gerekli 'feature' tanımının yer aldığı modül

### lider-ldap-impl

Lider modülleri tarafından LDAP ile ilgili kullanılabilecek tüm işlevleri sunan servisi içermektedir.

### lider-log-impl

Lider modülleri tarafından kullanılabilecek ve Lider log tablosuna kayıt eklemeye yarayan log modülüdür.

### lider-mail-notification

Lider modülleri ve eklentileri tarafından e-posta gönderme amacıyla kullanılabilecek e-posta servisini sunan modüldür.

### lider-messaging-xmpp

Lider'in XMPP sunucusu ile olan iletişimini sağlayan, mesaj gönderme ve gelen mesajları dinleme gibi yeteneklere sahip
XMPP istemcisini içeren modül

### lider-persistence-mariadb

Veritabanı CRUD işlemleriyle ilgili servisleri sunan modüldür. Eklentiler için kullanılabilecek PluginDbService ile birlikte
çekirdek katmanı oluşturan görev, politika, profil gibi tablolar için ayrı ayrı servisler içermektedir.

### lider-pluginmanager-impl

Lider'e yüklenen eklenti modüllerinin sisteme kaydını gerçekleştiren modüldür. Kayıt işlemi ile birlikte eklenti adı, sürümü,
eklentinin görev yada politika olarak çalıştığı, dosya transferi kullanıp kullanmadığı gibi nitelikler buradan kaydedilmiş olur.

### lider-policymanager-impl

Ahenk yüklü bilgisayarda bir kullanıcının giriş yapması ile birlikte Lider'den sorgulanan makine ve kullanıcı politikalarını
bulup Ahenk'e döndüren politika yöneticisi bu modülde yer almaktadır.

### lider-report-impl

Lider'de varsayılan olarak tanımlı rapor şablonları bu modül üzerinden sunulmaktadır. Bununla birlikte modülün bir diğer görevi, eklentilerden servis olarak sunulan diğer rapor şablonlarını izlemek ve değişiklikleri veritabanına yansıtmaktır.

### lider-taskmanager-impl

Ahenk üzerinde çalıştırılacak görevleri ilgili Ahenk'lere göndermek ve Ahenk'te çalıştırılan görevlere ait sonuç mesajları
dinleyip veritabanındaki ilgili kayıtları oluşturmak bu modüldeki görev yöneticisinin sorumluluğundadır. Buna ilaveten ileri tarihli görevleri de veritabanını belirli aralıklarla kontrol ederek tespit edip, çalıştırılma zamanı gelmiş olanları
çalıştırır.

### lider-web

RESTful web servisi oluşan kontrolör sınıfları içermektedir. Lider uygulamasına HTTP üzerinden giriş noktası burasıdır. Lider Arayüz'den gelen HTTP istemleri bu modül tarafından karşılanıp ilgili diğer modüllere iletilir ve ilgili modül istemi
işlettikten sonra cevabı da bu web modülü üzerinden JSON formatında Lider Arayüz'e döndürülür.

### lider-rest-impl

Lider Arayüz'den gelen HTTP istemlerinin ele alındığı sınıfları (request processor) içerir. Söz konusu sınıflar kapsamına göre (task, profile, report, policy gibi) birbirlerinden ayrılmıştır. Genellikle bulundukları kapsama göre veritabanı işlemlerini gerçekleştirmek, LDAP veya XMPP istemcisini kullanmak gibi çeşitli işlevlere sahiplerdir.

### lider-service-impl

HTTP istemlerine ait (request, response, context gibi) her türlü Bean sınıfı ve bu sınıfları oluşturmakta kullanılan Factory sınıfları bu modülde tanımlanmıştır. Diğer modüller burada tanımlı servisleri kullanarak gelen istemleri işleyip sonuç döndürebilir.

### lider-router-impl

Lider modüllerinde ve eklentilerde tanımlı görev (task) işleme amacıyla geliştirilmiş servisleri izleyen ve yeni servis tanımlandıkça yada varolan kaldırıldığında üzerindeki servis listesini güncelleyen modüldür. Görev işletimi sırasında, göreve ait sınıfın tetiklenmesi için bu listeden faydalanılır. Yine, benzer şekilde, Ahenk'ten gelen görev sonucuna göre 
ilgili sınıfın tetiklenmesi için de aynı liste kullanılır.
