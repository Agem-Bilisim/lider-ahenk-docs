## Servisler

Lider, Karaf OSGI konteynırı üzerinde çalışan birden çok OSGI servisi sunmaktadır.
Bu servislerin bir çoğu Lider'in bel kemiğini oluşturan yetkilendirme/doğrulama, veritabanı erişim katmanı, 
LDAP ve XMPP istemcileri gibi temel işlevleri oluşturur.

Geri kalanları ise Lider üzerinde eklenti çalıştırmaya olanak sağlayan bazı arayüzleri oluşturmaktadır.

Lider Ahenk süreçlerinde önemli rol oynayan bazı OSGI servislerini listelemek gerekirse:

### ServiceRegistryImpl.java

Tüm eklentiler ve bazı Lider modülleri tarafından sunulan ve görev (task) işletilme esnasında kullanılan sınıfların 
bir liste halinde tutulduğu servistir. Söz konusu sınıflar ICommand arayüzünü gerçekleştiren sınıflardan oluşmaktadır.
ServiceRegistry servisi Karaf'a yeni bir eklenti yüklenmesi veya varolan bir eklentinin çıkarılması durumunda kendi ICommand 
listesini güncelleyecek şekilde, sistemi her zaman dinler durumda çalışmaktadır. Karaf'ta varolan ICommand sınıflarının
listesini görebilmek için Karaf terminali üzerinden service:list ICommand komutu çalıştırılabilir.

### ServiceRouterImpl.java

Lider Arayüz üzerinden gelen görev isteminin doğrulama/yetkilendirme işlemini AuthServiceImpl aracılığıyla gerçekleştirmekten
ve eğer istemi yapan kullanıcının bu görevi çalıştırmaya yetkisi varsa ServiceRegistryImpl servisinde kayıtlı ilgili ICommand
sınıfını tetiklemekten sorumludur. Eğer ilgili ICommand sınıfında bu görevin Ahenk üzerinde çalıştırılması belirtildiyse,
söz konusu görev istemi TaskManagerImpl servisine iletilerek süreç devam ettirilir. Tüm bu anlatılan adımlar esnasında herhangi
bir hata olması durumunda Lider Arayüz'e cevap olarak hata mesajı döndürülür.

### LDAPServiceImpl.java

Lider üzerindeki LDAP işlemlerinin yapıldığı istemci sınıf bu servistir. Bu bağlamda diğer servislerin kullanabileceği LDAP arama,
yeni oluşturma, güncelleme, silme vb. işlevleri sunmakla yükümlüdür.

### MessagingServiceImpl.java

XMPP istemcisini kullanarak çeşitli mesajlar göndermeye olanak sağlayan yardımcı servistir. Lider'deki tüm servisler bu sınıfı
kullanarak Ahenk veya Lider Arayüz'e mesaj göndermektedir.

### TaskManagerImpl.java

Ahenk yüklü bilgisayar üzerinde çalıştırılması gereken görevlerin ele alındığı servistir. Görev mesajının oluşturulmasından ve
ilgili Ahenk'lere XMPP mesajının gönderilmesinden sorumludur. Bununla birlikte 'ileri tarihli' görevlerin belirlenen zamanları 
geldiğinde çalıştırılmasını sağlamak üzere belirli aralıklarla veritabanında görevleri sorgulamak ve zamanı gelen 'ileri tarihli'
görevleri de benzer şekilde işletmekten sorumludur. 

Son olarak da Ahenk'ten dönen görev sonucuna dair mesajları dinler, mesaj sonucuyla ilgili veritabanı kaydını oluşturur ve görevin
işletildiğine dair bir Event fırlatarak ilgili dinleyici sınıfların uyarılmasını sağlar.

### LiderConsoleNotifier.java

Ahenk'ten dönen görev sonucuna ait Event'i dinleyen servislerden biridir. Gelen görev sonucuna göre, bunun hangi Lider Arayüz
kullanıcısından gönderildiğini bulup, bulunan kullanıcıya XMPP mesajı göndererek son durumu bilgilendirir.

### PluginNotifier.java

Ahenk'ten dönen görev sonucuna ait Event'i dinleyen bir diğer servistir. Gelen görev sonucuna göre, ITaskAwareCommand arayüzünü
gerçekleştiren sınıfları tetiklemekten sorumludur. Bu sayede söz konusu ITaskAwareCommand arayüzü, eklentiler tarafından görev
sonuçlarını dinlemek ve bu sonuçlara göre çeşitli işlemler yapmak için kullanılabilir.

### PolicySubscriberImpl.java

Ahenk'ten gelen politika sorgulama isteklerinin karşılandığı servis burasıdır. Ahenk'ten gelen XMPP mesajında; Ahenk yüklü 
bilgisayara giriş yapan kullanıcının adı ile kullanıcının ve Ahenk'in politika versiyon numaraları yer almaktadır.
Gelen bu bilgiler kullanılarak kullanıcının ve Ahenk'in son ve güncel birer politikası veritabanından okunarak gelen versiyon
numaralarıyla karşılaştırılır. Eğer numaralar aynı ise boş mesaj, değilse veritabanından okunan politikalar cevap olarak ilgili
Ahenk'e gönderilir. Bu sayede aynı politikaların tekrar tekrar XMPP üzerinden gönderilmesinin de önüne geçilmiştir.

### DefaultRegistrationSubscriberImpl.java

Ahenk yüklendikten sonra ilk çalıştırılmasında Lider'e kayıt işlemini başlatmaktadır. Söz konusu kayıt sürecinde Ahenk'e XMPP 
bağlantısında kullanacağı JID değeri atanmakta, Ahenk yüklü bilgisayara ait (CPU, disk, BIOS gibi) donanım bilgileri toplanıp
veritabanında saklanmakta ve Ahenk'in LDAP ağacı üzerinde (Uncategorized alt-ağacında) kaydı oluşturulmaktadır. Söz konusu süreç
bu servis ile tanımlanmış varsayılan süreçtir ve Karaf üzerinde kayıt sürecini ele alan başka bir servis bulunmadığında devreye
girmektedir. Dolayısıyla IRegistrationSubscriber arayüzünü gerçekleştiren herhangi bir sınıf (servis) oluşturularak kayıt süreci
esnek bir şekilde ele alınabilmektedir.

### PluginDbServiceImpl.java

Eklentiler tarafından kullanılmak üzere hazırlanılmış, veritabanı CRUD işlemlerini sunan bir servistir. Arama, kaydetme, güncelleme ve silme gibi metotları sunmaktan sorumludur.

### MailServiceImpl.java

Lider'den ve eklentilerinden e-posta göndermek için kullanılabilen OSGI servisidir. Örneğin belirli aralıklarla rapor alarmları sorgulandıktan sonra tanımlı eşik değer geçildiğinde bu servisin e-posta gönderme metodu aracılığıyla e-posta gönderilebilir.
