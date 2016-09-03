# Terimler

* **Görev (Task)**: Ahenklere doğrudan gönderilen ve ahenk kurulu bilgisayar üzerinde çalıştırılan anlık işlerdir. Ahenk yüklü bilgisayar aktif durumda ise gönderilen *görev* anında uygulanır. Lider Arayüz üzerinden görev göndermek için herhangi bir profil oluşturmaya gerek yoktur. Genellikle bağımsız işlemler için geliştirilen eklentiler tarafından kullanılır. Örneğin; Ekran görüntüsü alma eklentisini ele alalım. Ekran görüntüsü alma  anlık bir işlemdir; bu işlevin görev olarak geliştirilmesi gerekir.

* **Profil (Profile)**: Bir eklentide gerçekleştirilebilecek yapılandırma ayarlarının bütününü ifade eder. Bir veya birden fazla profil bir araya gelerek politikayı oluşturur. Tek başına bilgisayar ya da kullanıcı odaklı çalıştırılamaz. Bir politika üzerine eklendikten sonra kullanılabilir. Bilgisayar ya da kullanıcı odaklı çalıştırılan politika kullanıcının sonraki ilk oturum açılışında çalıştırılır.
Örneğin; Tarayıcı eklentisi için; Lider Arayüz üzerinden eklenti aracılığı ile anasayfa belirleme, engelli sayfalar... gibi yapılandırma ayarlarının belirlenip kaydedilmesi ile profil oluşturulabilir.

* **Politika (Policy)**: Bir ya da daha fazla çalıştırılabilen profillerin bir araya gelmesi ile oluşturulur. Politika ile eklentilerin sağladığı imkanların-kısıtlamaların bir kitlenin özelliklerine göre işletilmesi sağlanabilir.
Örneğin; hiyerarşik yapıdaki bir kurumda *Genel Kurul Üyeleri* için bir politika oluşturulup, politikaya istenilen eklentiden profil eklenebilir. (Masaüstü arka planı kurum logosu, masaüstü mesajı kurum başkanının mesajı). Başka bir politikada ise çalışanlar için belirlenen profiller(Tarayıcı anasayfası kurumun resmi sitesi, Oturum kapanış saati 17:00) eklenerek, çalışanlar üzerinde işletilebilir.

* **Kullanıcı Politikası**: Kişiler üzerinde çalıştırılan politikalardır. Kişi hangi bilgisayar başında olursa olsun, üzerinde tanımlı politikalar oturum açıldığından itibaren gerçekleştirilir. (Ezilemez olarak tanımlanan profiller hariç)

* **Makina Politikası** Bilgisayarlar üzerinde çalıştırılan politikalardır. Bilgisayarda hangi kullanıcı oturum açmış olursa olsun, üzerinde tanımlı politikalar bilgisayarda oturum açıldığında gerçekleştirilir.(Ezilemez olarak tanımlanan profiller hariç)
