Ahenk

Ahenk; Lider'den gelen görevleri/politikaları bulunduğu bilgisayar üzerinde çalıştırıp sonuçlarını yine Lider'e döndüren bir servistir. Yetenekleri eklentilerle genişletilebilir. Sistem üzerindeki olaylardan veya Liderden gelen mesajlar ile iç süreçleri tetiklenir. 
Bu süreçleri şöyle listeleyebiliriz:

##Ahenk Servisinin Çalışmaya Başlaması

Ahenk base scripti olan ahenkd.py, python Daemon olarak çalışmaya başlar. İlk olarak bir scope oluşturur. Scope, oluşturulacak servislerin tutulduğu global bir sepet olarak düşünülebilir. Ardından Ahenk/ Ahenk eklentilerinin kullanabileceği ve Scope'a atılacak servisler oluşturulur. Bu servisler şunlardır:

**Config Manager:** Yapılandırma dosyasının okunması, değiştirilmesi, yazılmasını sağlar.


**Logger:** Farklı seviyelerde log dosyasına kayıt düşmek için kullanılır. Kaydedilen loglar /var/ahenk/log/ahenk.log dosyasına kaydedilir. Ahenk'in baştan başlatılması ile kayıtlar silinmez. Genel kayıt mesajı standardı şöyledir: logger.debug('[ExecutionManager] Politika işlemeye başlandı'), logger.error('[PLUGINA-INIT] A işlemi gerçekleştirilirken hata ile karşılaşıldı. Hata Mesajı: {0}'.format(str(e)))


**Event Manager:** Event-Function eşleştirilmesini sağlar. Böylece uygulamanın herhangi bir yerinden fırlatılan event ile önceden tanımlanmış event-actionlar sayesinde fonksiyon tetiklenir.
Ahenk Db Service: Ahenk'in kullandığı sqlite için temel veritabanı işlemlerini gerçekleştirmek için kullanılır.


**Message Manager:** Temel işleyişleri gerçekleştirmek için kullanılan json mesajlarını oluşturmak için kullanılır.Örneğin message = scope.getMessageManager().policy_request_msg('user_name') 


**Plugin Manager:** Eklentilerin Ahenk sistemine yüklenip kendi threadlerinin başlatılmasını sağlar. Böylece eklentiye gelen bir görev ya da profil bu thread içinde işlevini gerçekleştirebilir. Ayrıca eklentiyi Ahenk'ten kaldırıp, yeniden yüklemeye de izin verir. Eklentiler yüklendikten sonra yüklü eklentilerin init.py betikleri çalıştırılır.


**Scheduler:** Zamanlı görevlerin kontrolünü ve çalıştırılmasını sağlar. Kendi custom cron mekanizmasını barındırır.


**Task Manager:** Görev ve politikalar üzerinde kaydetmek, eklemek, silmek gibi temel işlemleri gerçekleştirir. Görevi kaydettikten sonra çalıştırılmasını sağlar.


**Registration:** Ahenk uygulaması çalışmaya başladığında lider tarafından doğrulanmasını sağlar. Doğrulanmamışsa ya da ilk defa çalıştırılıyorsa kendisini doğrulaması için lider ile gerekli protokolü başlatır.


**ExecutionManager:** Ahenk ve Lider çekirdeği arasında belirlenen protokolleri ve iletişim şablonlarını tanımlar ve EventManager kullanarak bu mesaj şablonlarının doğrulanmasını gerçekleştirir.


**Messager:** İletişim yöntemlerini tanımlar ve gerçekleştirir. XMPP bağlantısı açıp kapatılabilir. Bir çeşit XMPP Client'ıdır. Gelen mesajın tipinden Even Manager üzerinden Event'i tetikler



##Ahenk Servisinin İlk Defa Çalışmaya Başlaması

Ahenk'in çalışmasından farklı olarak ilk defa çalışmada registration işlemi gerçekleştirilir. Ahenk kendisini kaydetmesi için, Lider'e içinde üzerinde çalıştığı makinenin bilgileri ile birlikte bir bilgi mesajını Anonim olarak gönderir ve kayıt işleminin gerçekleştirildiğine dair bir cevap bekler. Bu cevap yapılandırılma dosyasında belirlenmiş bekleme süresi içinde gelmezse Ahenk servisi kendini kapatır. Eğer olumlu bir cevap dönerse Anonim bağlantı kapatılıp Lider tarafından onaylanan kalıcı hesap üzerinden iletişime devam eder. Kayıt için olumlu cevap dönmezse, makinenin sahip olduğu network adresinin 3 katı kadar daha farklı jid bilgileriyle registration denemesi yapılır. Bunların hiçbirinde başarılı olunmazsa Ahenk servisi kapatılır.


##Kullanıcının İlk Defa Ahenk Çalıştıran Bilgisayarda Oturum Açması

Ahenk çalıştıran bilgisayarda bir kullanıcı ilk defa oturum açtığında kullanıcı sözleşmesini kabul etmesi beklenir. Yapılandırma dosyasında tanımlanmış bekleme süresinde olumlu cevap verilmezse kullanıcı oturumu kapatılır. Kullanıcı sözleşmeyi kabul edene kadar bu süreç devam eder. Eğer Lider yapılandırmasında herhangi bir sözleşme tanımlanmadıysa varsayılan Ahenk Sözleşmesi metni kullanıcıya gösteirilir. Sözleşme metinleri her Ahenk servisi başlatıldığında Lider'den istenilir. Bir öncekinden farklı bir sözleşme Lider'den gönderildiğinde, kullanıcı eski sözleşmeyi kabul etmiş olsa bile yeni sözleme bir sonraki oturum açma sırasında tekrar sorulur.

##Görev Göderilmesi

Görev tipinde bir mesaj messenger servisine geldiğinde, event manager servisi kullanılarak execution servisinde tanımlı task execution kısmına mesaj parametresi ile gönderilir. Burada görevin json mesajı bir nesneye dönüştürülür ve veritabanına kaydedilir.Bu sırada görevin çalıştıracak eklentinin yüklü olup olmadığı kontrol edilir. Eğer yüklü değil ise Lider'e ilgili eklentinin eksik olduğuna dair bir mesaj gönderilir ve eklenti kurulana kadar görev saklanır. Eklenti ile ilgili kurulum bilgileri geldiğinde eklenti paketi uzaktan alınıp kurulur ve Ahenk servisine yüklenir. Saklanan görev aktif hale getirilir. Bu bir zamanlı görev ise scheduler servisine gönderilir; değilse plugin manager servisine gönderilerek çalıştırılır.

##Kullanıcının Oturum Açması

Kullanıcı oturum açtığında son güncel sözleşmeyi kabul edip etmediğinin kontrolü yapılır. Ardından Lider'den bu kullanıcı ve çalışan makineye ait politika istenir. Ardından eklentilerin safe ve login scriptleri varsa çalıştırılır. (Bu scriptlere hangi kullanıcının oturum açtığı bilgisi gönderilir) Yapılandırma dosyasında belirtilen sürede Lider politika bilgilerini Ahenk'e göndermezse Ahenk veritabanından bu kullanıcı ve makine için çalıştırılmış en güncel politikayı çeker ve çalıştırır. Politikaların çalıştırılması görevin çalıştırılması ile aynı mekaniği izlemektedir. Ancak bazı profil tabanlı eklentiler hem kullanıcı hem makine üzerine uygulanmış olabilir. Aynı eklentinin çalıştırabileceği 2 profile geldi ise (hem kullanıcı üzerine atanmış profil hem makine üzerine atanmış profil), makine üzerine atanmış profilin ezilebilir olup olmadığı kontrol edilir. Makine profili ezilebilir ise sadece kullanıcının profili; değilse sadece makine profili çalıştırılır.


##Sonuçların döndürülmesi

Bir görev ya da profil çalıştırıldığında işlemin başarılı ya da başarısız olduğuna dair varsa ek bilgileri ile sonuç dönmesi beklenir. Bu sonuç Response nesnesidir. Eklentinin döndürdüğü response nesnesi belirlenmiş json formatın döndürülür. Varsa data ve content type bilgilerine bakılır. Eğer content type, json değilse ve data da oluşturulmuş bir dosyanın md5 bilgisini barındırıyorsa bu dosya Lider'in gösterdiği uzak makinedeki dizine gönderilir ve sonuç mesajı Lider'e iletilir. Policy Status ile Task Status mesajlarının farkı Task Status'te taskId bulunması, Policy Status'te commandExecutionId ve policyVersion bulunmasıdır.


##Kullanıcının Oturum Kapatması

Bir kullanıcı oturumu kapattığında eklentilerin safe.py ve logout.py betikleri çalıştırılır. Lider'e hangi kullanıcının oturumu kapattığına dair mesaj atılır.

##Ahenk Servisinin Kapanması (Bilgisayarın Kapanması)

Ahenk servisi kapatılırken eklentilerin shutdown.py betikleri çalıştırılır. Eğer herhangi bir eklenti çalışmaya devam ediyorsa işlemini bitirmesi beklenir.
