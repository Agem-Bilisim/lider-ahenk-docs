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



### Politika uygulama
