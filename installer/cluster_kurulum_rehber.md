## LİDER AHENK CLUSTER KURULUMU
###### Bu rehberde Lider Ahenk sunucularının Kolay Kurulum uygulaması kullanılarak cluster (küme) şeklinde nasıl kurulacağı anlatılacaktır.

### 1. ÖN GEREKSİNİMLER
###### Lider Ahenk sunucuları minimum düğüm sayılarıyla cluster olarak kurulduğunda aşağıdaki gibi bir makine düzenine ihtiyaç vardır:

#### 1.1 MariaDB Cluster Kurulumu için:
###### En az 3 sanal veya fiziksel makine gerekmektedir.
###### Not 1: MariaDB'nin Karaf üzerinde çalışan sürücüsü yük dengeleme işini kendisi yaptığı için bu düğümlerin önünde herhangi bir ekstra yük dengeleyiciye ihtiyaç yoktur.
###### Not 2: MariaDB'nin yapısı gereği split-brain vs. sorunlarına karşı MariaDB cluster yapısı diğer bileşenlerin aksine en az üç düğüm ile ayağa kalkmaktadır.

#### 1.2 OpenLDAP Kurulumu için (normal kurulum):
###### 1 sanal veya fiziksel makine gerekmektedir. (Cluster olmayan normal kurulum yapılacağı için)

#### 1.3 Ejabberd Cluster Kurulumu için:
###### En az 3 sanal veya fiziksel makine gerekmektedir. (2 makine Ejabberd düğümü ve 1 makine yük dengeleyici)

#### 1.4 Karaf (Lider) Cluster Kurulumu için:
###### En az 3 sanal veya fiziksel makine gerekmektedir. (2 makine Karaf düğümü ve 1 makine yük dengeleyici)

###### NOT: Yük dengeleyici olarak çalışacak olan makineler çok ağır işlemler yapmayacağından, daha düşük özellikli seçilebilir.

#### 1.5 Dosya Sunucusu (Opsiyonel)
###### Dosya sunucusu olarak kullanılmak üzere yukarıda verilen sisteme bir makine daha eklenebilir.
###### Veya yukarıda bahsedilen makinelerden biri aynı zamanda dosya sunucusu olarak kullanılabilir. Örneğin Ejabberd veya Karaf cluster'larının yük dengleyicileri.

#### 1.6 İşletim Sistemi:
###### Sistem Debian versiyon 8.0 Jessie dağıtımı ve üzerinde test edilmiştir, farklı dağıtımlar önerilmez.

### 2. KURULUM SÜRECİ
#### 2.1 Başlangıç ve Genel Parametreler

##### 2.1.1 Versiyon Denetimi

###### Kolay Kurulum Uygulaması'nı çalıştırdıktan sonra Lider Kur'a basınız.
###### Uygulama, aşağıdaki gibi güncel versiyon kontrolü yapar.

![cluster_version_check](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_version_check.png)

###### Eğer kullandığınız versiyon güncel değilse aşağıdaki gibi 'Hayır, güncel versiyonu indir.' butonuna basarak yönlendirildiğiniz sayfadan güncel versiyonu indirebilirsiniz.

![cluster_version_check_fail](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_version_check_fail.png)

###### Kullandığınız versiyon güncel uygulama otomatik olarak devam edecektir.

##### 2.1.2 Bileşen Seçimi
###### Versiyon denetiminden sonra aşağıdaki gibi kurmak istediğiniz bileşenleri seçebilirsiniz. Bu rehber örnek olarak tüm bileşenler kurulacaktır.

![cluster_component_selection](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_component_selection.png)

##### 2.1.3 Organizasyon İsim ve CN'inin Belirlenmesi
###### Bileşen seçimini tamamladıktan sonra, kurulum yapılacak organizasyonun isim ve CN değerinin belirlenmesine geliyor.

![cluster_organization](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_organization.png)

###### Burada organizasyon kelimesinden kasıt kurulum yapılacak olan kurum ya da kuruluştur.
###### Örneğin TÜBİTAK'a kurulum yapılacak. Organizasyon ismi "TÜBİTAK", organizasyon CN'i ise "tubitak.gov.tr" olarak seçilebilir.
###### Ya da kurulum yapılacak organizasyon bir firma olabilir. Böyle bir durumda da benzer şekilde örnek olarak organizasyon ismi "AGEM Bilişim" ve organizasyon CN'i de "agem.com.tr" olarak belirlenebilir.
###### Burada girmiş olduğunuz değer önemlidir, çünkü kurulum sürecinde girmeniz gereken çoğu parametre bu isimlerden yola çıkarak hazır öneriler olarak getirilir.
###### Örnek olarak Organizasyon CN bölümüne "agem.com.tr" girerseniz, LDAP kurulumu sırasında Base DN değeri "dc=agem,dc=com,dc=tr" şeklinde otomatik doldurulmuş gelecektir (ve daha birçok değer), böylece işiniz çok kolaylaşacaktır.

##### 2.1.4 Bileşen Lokasyonlarının ve Kurulum Türünün Belirlenmesi

###### Organizasyon bilgilerini belirledikten sonra sıra aşağıdaki gibi bileşenlerin kurulucağı lokasyonları ve kurulum türlerini belirlemeye geliyor.

![cluster_component_location](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_component_location.png)

###### Burada cluster kurulum yapılacak olan bileşenlerin "küme olarak kur" seçeneklerini işaretliyoruz.
###### LDAP sunucusunu cluster olarak kurmayacağımız için LDAP kurulumunun yapılacağı adresi giriyoruz.
###### Cluster kurulum yapılacak olan bileşenlerin adres bilgilerini kurulum uygulamasının devamında ayrıca gireceğiz.

##### 2.2 MariaDB Cluster Kurulumu
##### 2.2.1 MariaDB Cluster Kurulum ve Konfigurasyon Bilgilerinin Girilmesi
###### Başlangıç ve genel parametreler bölümü bittikten sonra ilk olarak MariaDB cluster kurulumuna geçilir ve aşağıdaki gibi kurulum ve konfigurasyon bilgilerinin girileceği bir sayfayla karşılaşırız.

![cluster_mariadb_conf](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_mariadb_conf.png)

###### Sayfanın üst bölümünde MariaDB cluster'ıyla ilgili girilmesi gereken genel bilgiler, aşağıda ise sadece cluster düğümlerine ait girilmesi gereken bilgiler yer almaktadır.
###### Cluster genel bilgilerinin açıklamaları:
###### - *MariaDB Küme Adı* : MariaDB cluster'ına vermek istediğiniz isim.
###### - *MariaDB Root Parolası* : MariaDB veritabanı kurulurken belirlenecek olan root parolası
###### - *SSH Port* : MariaDB kurulurken makinelere bağlanmak için kullanılacak SSH bağlantısının port'u
###### - *SST Kullanıcı Adı* : State Snapshot Transfer kullanıcısı için kullanıcı adı
###### - *SST Kullanıcı Adı* : State Snapshot Transfer kullanıcısı için şifre
###### - *Özel anahtar kullan* : Eğer makinelere kullanıcı adı/şifre ikilisiyle değil özel anahtarla bağlanılacaksa işaretlenir ve "Özel Anahtar Yükle" butonuna basarak anahtar dosyası seçilir.
###### - *Passphrase* : Seçilen anahtarda passphrase varsa girilmelidir.
###### Düğüm bilgilerinin açıklamaları:
###### - *Düğüm IP'si* : Kurulum yapılacak düğümün adresi.
###### - *Düğüm Adı* : Konfigürasyonda düğüme verilecek isim
###### - *Düğüm Kullanıcı Adı* : Düğüme bağlanırken kullanılacak kullanıcı adı
###### - *Düğüm Kullanıcı Parolası* : Düğüme bağlanırken kullanılacak kullanıcı parolası
###### - *Düğüm Var* : Eğer bu adres daha önceden kurulum uygulamasıyla kurulmuş çalışan bir MariaDB düğümüyse işaretlenir. (Varolan cluster'a düğüm eklenirken kullanılır)

###### NOT: Artı şeklindeki butonla düğüm sayısı arttırılabilir.
