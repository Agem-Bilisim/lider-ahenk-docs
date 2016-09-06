# Terimler

* **Görev (Task)**: Ahenklere doğrudan gönderilen ve ahenk kurulu bilgisayar üzerinde çalıştırılan anlık işlerdir. Ahenk yüklü bilgisayar aktif durumda ise gönderilen *görev* anında uygulanır. Lider Arayüz üzerinden görev göndermek için herhangi bir profil oluşturmaya gerek yoktur. Genellikle bağımsız işlemler için geliştirilen eklentiler tarafından kullanılır. Örneğin; Ekran görüntüsü alma eklentisini ele alalım. Ekran görüntüsü alma  anlık bir işlemdir; bu işlevin görev olarak geliştirilmesi gerekir.

* **Profil (Profile)**: Bir eklentide gerçekleştirilebilecek yapılandırma ayarlarının bütününü ifade eder. Bir veya birden fazla profil bir araya gelerek politikayı oluşturur. Tek başına bilgisayar ya da kullanıcı odaklı çalıştırılamaz. Bir politika üzerine eklendikten sonra kullanılabilir. Bilgisayar ya da kullanıcı odaklı çalıştırılan politika kullanıcının sonraki ilk oturum açılışında çalıştırılır.
Örneğin; Tarayıcı eklentisi için; Lider Arayüz üzerinden eklenti aracılığı ile anasayfa belirleme, engelli sayfalar... gibi yapılandırma ayarlarının belirlenip kaydedilmesi ile profil oluşturulabilir.

* **Politika (Policy)**: Bir ya da daha fazla çalıştırılabilen profillerin bir araya gelmesi ile oluşturulur. Politika ile eklentilerin sağladığı imkanların-kısıtlamaların bir kitlenin özelliklerine göre işletilmesi sağlanabilir. Politikalar kullanıcı girişi esnasında Lider'den sorgulanarak uygulanır. Örneğin; hiyerarşik yapıdaki bir kurumda *Genel Kurul Üyeleri* için bir politika oluşturulup, politikaya istenilen eklentiden profil eklenebilir. (Masaüstü arka planı kurum logosu, masaüstü mesajı kurum başkanının mesajı). Başka bir politikada ise çalışanlar için belirlenen profiller(Tarayıcı anasayfası kurumun resmi sitesi, Oturum kapanış saati 17:00) eklenerek, çalışanlar üzerinde işletilebilir.

* **Kullanıcı Politikası**: Kişiler üzerinde çalıştırılan politikalardır. Kişi hangi bilgisayar başında olursa olsun, üzerinde tanımlı politikalar oturum açıldığından itibaren gerçekleştirilir. (Ezilemez olarak tanımlanan profiller hariç)

* **Makina Politikası** Bilgisayarlar üzerinde çalıştırılan politikalardır. Bilgisayarda hangi kullanıcı oturum açmış olursa olsun, üzerinde tanımlı politikalar bilgisayarda oturum açıldığında gerçekleştirilir.(Ezilemez olarak tanımlanan profiller hariç)

* **Alarm** Oluşturulan rapor tanımlarına aynı zamanda alarm eklemek de mümkündür. Alarm olarak tanımlanmış rapor tanımları tanım esnasında belirlenmiş belirli aralıkla kontrol edilir ve elde edilen kayıt (satır) sayısı eşik değeri geçer ise tanımlanmış e-posta adreslerine e-posta aracılığıyla rapor çıktısını içeren bildirim gönderilir.

* **OSGI modülü (Bundle)** OSGI teknolojisinde, kendi başına çalışan veya bir başkasının çalışmasına katkı sağlayan Java ile geliştirilmiş proje parçacıklarına modül adı verilir. Örneğin Lider 20'den fazla modülden oluşur ve her bir modülün (örneğin veritabanı erişim katmanı, LDAP istemcisi, RESTful web servis, XMPP istemcisi olmak gibi) ayrı birer sorumluluğu/işlevi vardır.

* **OSGI servis** OSGI konteynırı üzerinde çalışan her bir modül birbirinden bağımsız bir bağlamda (Farklı sınıf yükleyicilerine sahip) çalıştırılmaktadır. Dolayısıyla bir modüldeki bir sınıfın başka bir modüldeki sınıfa erişmesi için
özel bir yaklaşım olarak erişilmek istenen sınıf OSGI servisi olarak tanımlanır. Bu sayede söz konusu sınıfa sistemde çalışan herhangi bir sınıf gerekli yapılandırma ayarları (bkz. blueprint.xml) yaptıktan sonra erişebilir.
