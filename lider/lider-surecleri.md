## Süreçler

Lider üzerinde işletilen ve sıklıkla kullanılan bazı süreçlerin detaylı açıklaması aşağıdaki gibidir:

### Ahenk kaydı

Ahenk kurulumundan sonra Ahenk servis ayağa ilk kalktığında, Lider'e kayıt olmak için mesaj gönderir. Bu mesajı alan Lider, kayıt
sürecinde Ahenk'in LDAP ağacında nereye yerleşeceğine karar verir ve Ahenk yüklü bilgisayardan toplanan (CPU, disk, BIOS gibi)
donanım bilgilerini saklar. 

Kayıt işleminin başarıyla sonuçlanması durumunda Ahenk DN bilgisi, BAŞARILI etiketiyle birlikte Ahenk'e mesaj olarak geri gönderilir.
Eğer kayıt sırasında hata oluşursa (örneğin oluşturulacak LDAP ögesinin veya ögeye ait UID özniteliğinin daha önceden var olduğu durum gibi)
Ahenk'e HATA etiketiyle birlikte neden hata oluştuğunu açıklayıcı bilgi döndürülür. Tüm bu kayıt süreci Lider/karaf logları üzerinden
takip edilebilir durumdadır.

Varsayılan kayıt sürecinde, Lider yeni Ahenk'ler için LDAP ağacında Uncategorized alt-ağacında yeni öge oluştururken, veritabanına
da IP adresi, MAC adresi ile birlikte disk, bellek, işlemci, işletim sistemi bilgilerini kaydeder. Bununla birlikte söz konusu
kayıt sürecini ele alan servis sadece bir Java arayüzünü gerçekleştiren bir sınıftır dolayısıyla ilgili arayüzü gerçekleştirmiş 
herhanagi bir sınıf Karaf'a yüklenerek kayıt sürecinde farklı bilgilerin toplanması veya LDAP ağacında farklı şekilde yerleştirme
yapılması mümkündür. Bu bağlamda örnek bir de kayıt modülü geliştirilmiş, bu modüle göre kayıt sırasında gelen MAC adresine göre
önceden hazırlanmış bir CSV dosyasından ilgili Ahenk'in ağaçta nereye yerleştirileceğine karar verilmiş ve devamında Ahenk'e gönderilen
bir betik çalıştırılarak fazladan bilgi toplanması sağlanmıştır.

### Görev işletimi

İşletilecek görev dair HTTP isteği Lider Arayüz'den Lider'e gönderildiğinde, söz konusu istek TaskController sınıfı tarafından karşılanır. TaskController bunu TaskRequestProcessor sınıfındaki ilgili metoda yönlendirir. Bu metot içerisinde, eğer Lider yapılandırma dosyasında yetkilendirme özniteliği kurulu ise, görevin işletilmek istendiği LDAP ögeleri kontrol edilierek, kullanıcının bu görevi bu LDAP ögelerinde çalıştırıp çalıştıramayacağına karar verilir. Eğer kullanıcı bu görevi belirtilen hiçbir ögede çalıştıramayacaksa, yetkilendirme hata mesajı Lider Arayüz'e geri döndürülür.

Eğer çalıştırabilecekse, süreç ServiceRouterImpl sınıfından devam eder. Bu sınıf ise, görev ile ilişkili ICommand sınıfını bularak öncelikle bulduğu sınıfın validate() ve execute() metotlarını tetikler. Başarılı bir şekilde bu metotlar çalıştıysa ve ICommand sınıfında bu görevin Ahenk üzerinde çalışması gerektiği belirtildiyse, görev Ahenk'e gönderilmek üzere TaskManagerImpl sınıfına aktarılır.

TaskManagerImpl gelen görev parametrelerine göre veritabanında Task ve Command kayıtlarını oluşturur. Daha sonra bu görevin ileri tarihli olup olmadığını kontrol eder. Eğer ileri tarihli görev ise işletim burada son bulur. (TaskManagerImpl aynı zamanda ileri tarihli görevleri belirli aralıklarla kontrol ederek işletmekle de yükümlüdür) Eğer değilse, görevin işletileceği her Ahenk DN'i için Command Execution kaydını veritabanında oluşturur ve ilişkili Ahenk JID değerini bularak bu Ahenk'e görevi işletmesi için XMPP üzerinden mesaj gönderir.

Bununla birlikte TaskManagerImpl, Ahenk'ten dönen görev sonuçlarını da dinlemektedir. Yeni bir görev sonucu gelmesi durumunda tetiklenecek metot ile Command Execution Result kaydı veritabanında oluşturulur ve bu kayıt içerisinde görev sonucuna dair bütün bilgiler saklanır. Eğer görev sonucu, dosya gönderim türündeyse mesaj içerisinden dosyanın MD5 değeri okunarak dosya sunucusundan dosya transfer edilir ve veritabanında saklanır.

Görev sonucu saklandıktan sonra ilgili eklentilerin ve Lider Arayüz'ün görev sonucundan haberdar edilmesi için bir de Event fırlatılır. Görev sonucunu dinlemek isteyen eklentiler ITaskAwareCommand arayüzünü gerçekleştirerek, bu Event ile birlikte otomatik tetiklenirler.

### Politika uygulama

Lider Arayüz üzerinden politika uygulandığında Lider'e iletilen HTTP isteği PolicyController sınıfı tarafından karşılanır ve PolicyRequestProcessorImpl sınıfındaki ilgili metoda iletilir. Bu metotta tek yapılan politikanın işletileceği LDAP DN değerlerinin bulunması ve tüm parametreleri ile birlikte politikanın veritabanına kaydedilmesidir.

Politikalar Ahenk yüklü bilgisayara kullanıcı girişi esnasında sorgulandığı için, Lider de XMPP üzerinden Ahenk'ten gelen 'politika sorgulama' mesajlarını dinler. Bu türde bir mesaj geldiğinde tetiklenen sınıf PolicySubscriberImpl, gelen mesajdaki kullanıcı adı ile kullanıcı ve makine sürüm değerlerine göre veritabanından son/güncel kullanıcı ve makine politikasını sorgular. Eğer sürüm değerleri aynı ise politikalar tekrar Ahenk'e gönderilmez, boş cevap döndürülür. Eğer sürüm değerleri farklıysa, politikalar Ahenk'e cevap olarak döndürülür. 

İlgili Ahenk kendisine gelen politikayı işlettikten sonra, hata yada başarılı durumunda, Lider'e sonuç mesajı dönmektedir. Bu sonuç mesajları ise PolicyManagerImpl sınıfı dinlemektedir. Söz konusu sınıf gelen politika sonucuna göre veritabanında Command Execution Result kaydı oluşturur ve gelen bilgileri bu kayıt ile saklar.
