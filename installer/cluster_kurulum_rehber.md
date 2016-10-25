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

###### NOT: Artı ve eksi şeklindeki butonlarla düğüm sayısı arttırılıp azaltılabilir.

###### Aşağıdaki gibi gerekli genel bilgileri ve düğüm bilgilerini dolduruyoruz.

![cluster_mariadb_conf_completed](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_mariadb_conf_completed.png)

##### 2.2.2 MariaDB Kurulum Onayı
###### Devam ettiğimizde, yapılacak MariaDB kurulumunun bilgilerini gösteren ve onay isteyen aşağıdaki gibi bir sayfayla karşılaşıyoruz.

![cluster_db_confirm](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_db_confirm.png)

##### 2.2.3 MariaDB Kurulum Durumu
###### Devam ettiğimizde kurulum aşağıdaki gibi başlıyor. Bu sayfada kurulumda yapılan işlemlerle ilgili bilgilendirmeler ve sürecin durumu yer alır.

![cluster_db_installation](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_db_installation.png)

###### MariaDB cluster kurulumu aşağıdaki gibi başarıyla tamamlandığında devam ediyoruz.
###### NOT: Kurulumların yapıldığı makinelerin özellikleri, internet bağlantı hızı ve ağ trafiği gibi parametrelere bağlı olarak kurulumun tamamlanması uzun sürebilir.

![cluster_db_installation_done](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_db_installation_done.png)

##### 2.3 OpenLDAP Kurulumu
###### MariaDB kurulumunun tamamlanmasından sonra devam ettiğimizde OpenLDAP kurulumuna geçiyoruz.

##### 2.3.1 OpenLDAP Kurulumu için Erişim Bilgileri
###### OpenLDAP kurulacak olan makineye bağlanmak için iki seçeneğimiz var. Birincisi kullanıcı adı ve parola bilgisini kullanmak, ikincisi ise daha önceden tanımlanmış bir özel anahtarı kullanmak.

![cluster_ldap_access](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_access.png)

###### Bu örnekte kullanıcı adı ve parola bilgisini kullanacağız. Yukarıdaki gibi gerekli bilgileri girip devam ediyoruz ve uygulama girdiğimiz bilgileri aşağıdaki gibi test ediyor.

![cluster_ldap_conn_check](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_conn_check.png)

![cluster_ldap_conn_success](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_conn_success.png)

###### Test başarılı olduktan sonra "Ok" a basarak devam ediyoruz.

##### 2.3.2 Yeni Bir OpenLDAP Kurulumu veya Varolan OpenLDAP'ın Konfigürasyonu
###### Kolay kurulum uygulamasıyla yeni bir OpenLDAP kurabilir veya varolan bir OpenLDAP'ı Lider Ahenk kullanımına uygun hale getirmek için konfigüre edebilirsiniz. Bu örnekte yeni bir kurulum yapacağız, "Yeni bir LDAP kur" seçeneğini aşağıdaki gibi seçerek devam ediyoruz.

![cluster_ldap_create_new](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_create_new.png)

##### 2.3.3 OpenLDAP Kurulum Yönteminin Seçilmesi
###### Kurulum yönteminin seçileceği bu sayfada iki seçenek var. Birincisi seçeceğimiz bir DEB dosyasından kurulum yapmak diğeri ise DEB dosyasının indirilebileceği bir URL üzerinden kurulum yapmak.

###### ![cluster_ldap_install_method](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_install_method.png)

###### Kolay Kurulum uygulamasında kurulum için gerekli DEB dosyaları uygulamanın içinde gömülü olarak gelir ve özel durumlar haricinde başka bir dosya veya URL seçmenize gerek yoktur.
###### Bu sayfada doldurulması gereken tek alan "LDAP root parolası" alanıdır. Bu alana istediğimiz bir şifreyi yukarıdaki gibi girdikten sonra devam ediyoruz.

##### 2.3.4 OpenLDAP Konfigürasyonu
###### Kurulum yöntemini seçtikten sonra aşağıdaki gibi konfigürasyon sayfasıyla karşılaşıyoruz. Görüldüğü gibi başlangıçta girdiğiniz Organizasyon CN'i parametresine göre birçok parametre hazır olarak gelir.

![cluster_ldap_conf](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_conf.png)

###### Konfigürasyon parametrelerinin açıklamaları:
###### - *Organization* : Organizasyon ismi.
###### - *Organization CN* : Organizasyon yaygın ismi (Organization Common Name). Bu parametreyle ilgili detaylı bilgi 2.1.3'te verilmiştir.
###### - *Base DN* : LDAP ağacınızın kök DN'i.
###### - *Base CN* : LDAP'ın kök CN'i.
###### - *Config Admin DN* : LDAP'ı konfigüre etmek için kullanılan kullanıcının DN'i.
###### - *Config Admin DN Password* : LDAP'ı konfigüre etmek için kullanılan kullanıcının parolası.
###### - *Admin CN* : Admin kullanıcısı için CN.
###### - *Admin CN Password* : Admin kullanıcısı için parola.
###### - *Lider IP Address* : Tekil Karaf kurulumlarında Karaf'ın adresi, cluster Karaf kurulumlarında ise Karaf düğümlerinin önünde duracak yük dağıtıcının adresi girilmelidir.
###### - *Lider Console User Password* : Lider yönetim panelini kullanacak olan kullanıcı için şifre.
###### - *Lider Console User* : Lider yönetim panelini kullanacak olan kullanıcı için kullanıcı adı.

###### Bu sayfa özel bir durum yoksa yapılması gereken aşağıdaki gibi şifrelerin ve "Lider IP Address" alanına Karaf yük dağıtıcısı makinesinin adresinin girilmesidir.

![cluster_ldap_conf_completed](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_conf_completed.png)

##### 2.3.4 OpenLDAP Kurulum Onayı
###### Devam ettiğimizde, yapılacak OpenLDAP kurulumunun bilgilerini gösteren ve onay isteyen aşağıdaki gibi bir sayfayla karşılaşıyoruz.

![cluster_ldap_confirm](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_confirm.png)

###### Devam ettiğimizde kurulum aşağıdaki gibi başlıyor. Bu sayfada kurulumda yapılan işlemlerle ilgili bilgilendirmeler ve sürecin durumu yer alır.

![cluster_ldap_installation](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_ldap_installation.png)

###### OpenLDAP kurulumu aşağıdaki gibi başarıyla tamamlandığında devam ediyoruz.
###### NOT: Kurulumların yapıldığı makinelerin özellikleri, internet bağlantı hızı ve ağ trafiği gibi parametrelere bağlı olarak kurulumun tamamlanması uzun sürebilir.

##### 2.4 Ejabberd Cluster Kurulumu
##### 2.4.1 Ejabberd Cluster Kurulum ve Konfigurasyon Bilgilerinin Girilmesi
###### OpenLDAP kurulumu bittikten sonra Ejabberd cluster kurulumuna geçilir ve aşağıdaki gibi kurulum ve konfigurasyon bilgilerinin girileceği bir sayfayla karşılaşırız.

![cluster_xmpp_conf](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_xmpp_conf.png)

###### Cluster genel bilgilerinin açıklamaları:
###### - *Servis Adı* : XMPP servisinin adı (Virtual Host)
###### - *SSH Port* : Kurulum yapılırken kullanılacak SSH portu
###### - *LDAP Sunucu Adresi* : LDAP sunucusunun adresi
###### - *LDAP Root DN* : LDAP'taki admin kullanıcısının DN'i
###### - *LDAP Root Parolası* : LDAP'taki admin kullanıcısının parolası
###### - *LDAP base dn* : LDAP ağacının kök DN'i
###### - *Ejabberd Admin Parolası* : Ejabberd kurulumunda oluşturulacak olan admin kullanıcısının parolası
###### - *Lider Sunucu Kullanıcı Adı* : Lider sunucu(lar) için Ejabberd üzerinde oluşturulacak kullanıcının adı
###### - *Lider Sunucu Kullanıcı Parolası* : Lider sunucu(lar) için Ejabberd üzerinde oluşturulacak kullanıcının parolası
###### - *HAProxy Adresi* : Ejabberd düğümlerinin önüne kurulacak yük dağıtıcının adresi
###### - *HAProxy Makinesinin Root Parolası* : Ejabberd düğümlerinin önüne kurulacak yük dağıtıcının parolası
###### - *Özel anahtar kullan* : Eğer makinelere kullanıcı adı/şifre ikilisiyle değil özel anahtarla bağlanılacaksa işaretlenir ve "Özel Anahtar Yükle" butonuna basarak anahtar dosyası seçilir.
###### - *Passphrase* : Seçilen anahtarda passphrase varsa girilmelidir.
###### XMPP düğüm bilgilerinin açıklamaları:
###### - *Düğüm IP'si* : Ejabberd düğümünün kurulacağı adres
###### - *Düğüm Adı* : Kurulacak Ejabberd düğümüne verilecek isim
###### - *Düğüm Kullanıcı Adı* : Ejabberd düğümü kurulacak makine için SSH bağlantısında kullanılacak kullanıcı adı
###### - *Düğüm Kullanıcı Parolası* : Ejabberd düğümü kurulacak makine için SSH bağlantısında kullanılacak parola
###### - *Düğüm Var* : Eğer bu adres daha önceden kurulum uygulamasıyla kurulmuş çalışan bir Ejabberd düğümüyse işaretlenir. (Varolan cluster'a düğüm eklenirken kullanılır)

###### NOT: Artı ve eksi şeklindeki butonlarla düğüm sayısı arttırılıp azaltılabilir.

###### Aşağıdaki gibi gerekli genel bilgileri ve düğüm bilgilerini dolduruyoruz.

![cluster_xmpp_conf_completed](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_xmpp_conf_completed.png)

##### 2.4.2 Ejabberd Kurulum Onayı

###### Devam ettiğimizde, yapılacak MariaDB kurulumunun bilgilerini gösteren ve onay isteyen aşağıdaki gibi bir sayfayla karşılaşıyoruz.

![cluster_xmpp_confirm](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_xmpp_confirm.png)

##### 2.4.3 Ejabberd Kurulum Durumu
###### Devam ettiğimizde kurulum aşağıdaki gibi başlıyor. Bu sayfada kurulumda yapılan işlemlerle ilgili bilgilendirmeler ve sürecin durumu yer alır.

![cluster_xmpp_installation](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_xmpp_installation.png)

###### Ejabberd cluster kurulumu aşağıdaki gibi başarıyla tamamlandığında devam ediyoruz.
###### NOT: Kurulumların yapıldığı makinelerin özellikleri, internet bağlantı hızı ve ağ trafiği gibi parametrelere bağlı olarak kurulumun tamamlanması uzun sürebilir.

![cluster_xmpp_installation_done](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_xmpp_installation_done.png)

##### 2.5 Karaf (Lider) Cluster Kurulumu
##### 2.5.1 Karaf Cluster Kurulum ve Konfigurasyon Bilgilerinin Girilmesi
###### Ejabberd cluster kurulumu bittikten sonra Karaf cluster kurulumuna geçilir ve aşağıdaki gibi kurulum ve konfigurasyon bilgilerinin girileceği bir sayfayla karşılaşırız.

![cluster_lider_conf_1](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_lider_conf_1.png)

![cluster_lider_conf_2](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_lider_conf_2.png)

###### Cluster genel bilgilerinin açıklamaları:
###### - *LDAP Sunucu Adresi* : LDAP sunucusunun adresi
###### - *LDAP port* : LDAP sunucusunun hizmet verdiği port
###### - *LDAP Admin kullanıcısı* : LDAP'taki admin kullanıcısının DN'i
###### - *LDAP Admin kullanıcı şifresi* : LDAP'taki admin kullanıcısının parolası
###### - *LDAP base dn* : LDAP ağacının kök DN'i
###### - *LDAP SSL Kullan* : Karaf'ın LDAP ile konuşurken SSL kullanması için seçilir. Bu konfigürasyonun çalışabilmesi kurulumdan sonra ekstra konfigürasyon gerekir.
###### - *XMPP Sunucu adresi* : Tekil Ejabberd kurulumunda Ejabberd'ın adresi, cluster Ejabberd kurulumlarında yük dengeleyicinin adresi yazılır.
###### - *XMPP Port* : Ejabberd'ın hizmet verdiği port
###### - *XMPP Lider Kullanıcısı* : Ejabberd üzerinde Lider sunucu için oluşturulmuş olan kullanıcının adı
###### - *XMPP Lider Kullanıcı Şifresi* : Ejabberd üzerinde Lider sunucu için oluşturulmuş olan kullanıcının şifresi
###### - *Servis Adı* : XMPP servisinin adı (Virtual Host)
###### - *XMPP Azami bağlantı deneme sayısı* : XMPP bağlantısı kurulurken maksimum deneme sayısı
###### - *XMPP paket zamanaşımı* : Milisaniye cinsinden paket zamanaşımı
###### - *XMPP ping zamanaşımı* : Milisaniye cinsinden ping zamanaşımı
###### - *XMPP SSL Kullan* : Karaf'ın Ejabberd ile konuşurken SSL kullanması için seçilir. Bu konfigürasyonun çalışabilmesi kurulumdan sonra ekstra konfigürasyon gerekir.
###### - *Veritabanı Sunucu Adresi* : MariaDB tekil olarak kurulduysa 'mariadb adresi':'port' şeklinde yazılır. Cluster olarak kurulduysa 'mariadb düğüm adresi':'port','mariadb düğüm adresi':'port','mariadb düğüm adresi':'port' şeklinde yazılır.
###### - *Veritabanı Kullanıcı Adı* : Karaf'ın MariaDB'ye bağlantı kurarken kullanacağı kullanıcı adı
###### - *Veritabanı Şifresi* : Karaf'ın MariaDB'ye bağlantı kurarken kullanacağı kullanıcı şifresi
###### - *Veritabanı İsmi* : MariaDB üzerinde kullanılacak veritabanı
###### - *Ajan LDAP base dn* : Ajanların LDAP'ta varsayılan olarak kayıt olacağı DN
###### - *Ajan LDAP id bilgisi* : Ajan kayıtları için ID özniteliği
###### - *Ajan LDAP jid bilgisi* : XMPP iletişimi için JID özniteliği
###### - *Ajan LDAP sınıfları* : Ajanı tanımlayan LDAP obje sınıfları
###### - *LDAP Kullanıcı base dn* : Kullanıcı kayıtları için ID özniteliği
###### - *LDAP Kullanıcı id özellikleri* : User kayıtları için ID özniteliği
###### - *LDAP Kullanıcı yetkinlik özellikleri* : Kullanıcı ayrıcalıkları için öznitelikler
###### - *LDAP Kullanıcı sınıfları* : Kullanıcıyı tanımlayan LDAP obje sınıfları
###### - *LDAP Kullanıcı grubu sınıfları* : Kullanıcı grubunu tanımlayan LDAP obje sınıfları
###### - *Dosya Sunucusu Protokolü* : Dosya transferi için kullanılacak protokol
###### - *Dosya Sunucusu Adresi* : Dosya sunucusunun adresi
###### - *Dosya Sunucusu Portu* : Dosya sunucusunun hizmet verdiği port
###### - *Dosya Sunucusu Kullanıcı Adı* : Dosya sunucusuna bağlantı kurulurken kullanılacak kullanıcı adı
###### - *Dosya Sunucusu Parolası* : Dosya sunucusuna bağlantı kurulurken kullanılacak parola
###### - *Dosya Sunucusu Eklenti Yolu* : Dosya sunucusunda Ahenk eklentilerinin bulunduğu yol
###### - *Dosya Sunucusu Sözleşme Yolu* : Dosya sunucusunda kullanıcı sözleşmesinin bulunduğu yol
###### - *Dosya Sunucusu Ahenk Dosya Yolu* : Ahenklerin dosya transferinde kullanacağı yol
###### - *HAProxy Adresi* : Karaf düğümlerinin önüne kurulacak yük dağıtıcının adresi
###### - *HAProxy Makinesinin Root Parolası* : Karaf düğümlerinin önüne kurulacak yük dağıtıcının parolası
###### - *Özel anahtar kullan* : Eğer makinelere kullanıcı adı/şifre ikilisiyle değil özel anahtarla bağlanılacaksa işaretlenir ve "Özel Anahtar Yükle" butonuna basarak anahtar dosyası seçilir.
###### - *Passphrase* : Seçilen anahtarda passphrase varsa girilmelidir.

###### XMPP düğüm bilgilerinin açıklamaları:
###### - *Düğüm IP'si* : Karaf düğümünün kurulacağı adres
###### - *Düğüm Kullanıcı Adı* : Karaf düğümü kurulacak makine için SSH bağlantısında kullanılacak kullanıcı adı
###### - *Düğüm Kullanıcı Parolası* : Ejabberd düğümü kurulacak makine için SSH bağlantısında kullanılacak parola
###### - *Düğüm XMPP Resource* : XMPP mesajlaşmasında kullanılacak kaynak adı
###### - *Düğüm XMPP Resource* : XMPP mesajlaşmasında kullanılacak öncelik numarası
###### - *Düğüm Var* : Eğer bu adres daha önceden kurulum uygulamasıyla kurulmuş çalışan bir Karaf düğümüyse işaretlenir. (Varolan cluster'a düğüm eklenirken kullanılır)

###### NOT: Artı ve eksi şeklindeki butonlarla düğüm sayısı arttırılıp azaltılabilir.

###### Aşağıdaki gibi gerekli genel bilgileri ve düğüm bilgilerini dolduruyoruz.

![cluster_lider_conf_1](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_lider_conf_1.png)

![cluster_lider_conf_2_completed](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_lider_conf_2_completed.png)

##### 2.5.2 Ejabberd Kurulum Onayı

###### Devam ettiğimizde, yapılacak MariaDB kurulumunun bilgilerini gösteren ve onay isteyen aşağıdaki gibi bir sayfayla karşılaşıyoruz.

![cluster_lider_confirm](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_lider_confirm.png)

##### 2.5.3 Lider Kurulum Durumu
###### Devam ettiğimizde kurulum aşağıdaki gibi başlıyor. Bu sayfada kurulumda yapılan işlemlerle ilgili bilgilendirmeler ve sürecin durumu yer alır.

![cluster_lider_installation](http://www.agem.com.tr/cluster-kurulum-screenshots/cluster_lider_installation.png)

###### Lider cluster kurulumu aşağıdaki gibi başarıyla tamamlandığında devam ediyoruz.
###### NOT: Kurulumların yapıldığı makinelerin özellikleri, internet bağlantı hızı ve ağ trafiği gibi parametrelere bağlı olarak kurulumun tamamlanması uzun sürebilir.

###### Lider cluster'ı da kurulduktan sonra sistemler kontrol edilmelidir. Daha sonra yönetim arayüzünden kullanıma başlanabilir.

###Lider Console'da Yeni Bağlantı Rehberi
######Bu rehberde Lider Ahenk kurulumu tamamlandıktan sonra Lider Console uygulaması üzerinden nasıl bağlantı kurulacağı anlatılacaktır. Öncelikle Lider Ahenk sisteminin kurulduğu ve rehberde kullanılacak olan ortamla ilgili bilgi aşağıdaki bölümde verilecektir.
#####Rehber'in Yazıldığı Ortam Bilgileri:
######Rehberde kullanılan Lider Ahenk sisteminin kurulu olduğu ortamla ilgili bilgiler aşağıdaki gibidir:
- MariaDB veritabanı, OpenLDAP sunucusu, Ejabberd XMPP sunucusu ve Lider Sunucu aynı makinede çalışmaktadır. Çalıştıkları makinenin IP adresi: `192.168.1.111`
- LDAP sunucusunda Base DN: `dc=tubitak,dc=gov,dc=tr`
- LDAP'taki Lider Console kullanıcısının adı: `lider_console`
- LDAP'taki Lider Console kullanıcısının şifresi: `secret`

######Lider Console uygulamasını çalıştırdığımızda karşımıza aşağıdaki gibi bir ekran geliyor.

![console-acilis](http://www.agem.com.tr/console-screenshots/console-acilis.png)

######Görüldüğü gibi Lider Console uygulamasında henüz hiçbir bağlantı yok. Yeni bir bağlantı oluşturmak için ekranın sol alt kısmındaki "Connections" bölümünde üzerinde "LDAP" yazan ikona tıklıyoruz.
######Tıkladıktan sonra karşımıza aşağıdaki gibi "New LDAP Connection" isminde yeni bir diyalog çıkıyor.

![console-new-ldap-connection](http://www.agem.com.tr/console-screenshots/console-new-ldap-connection.png)

######Burada "Connection name" alanına belirlediğimiz bir bağlantı adı giriyoruz.
######Bu noktada, "Hostname" alanına LDAP sunucusunun IP'sinin yazılması gerektiği göz önünde bulundurulmalı.
######Eğer Lider Console ve LDAP sunucusu aynı makinede çalışıyorsa IP adresi olarak "localhost" yapılabilir.
######Lider Console ve LDAP sunucu aynı adreste olmadığı halde "localhost" yazılırsa bağlantılı kurulamayacaktır.
######Aşağıdaki gibi gerekli alanları doldurduktan sonra "Check Network Parameter" butonuna tıklıyoruz.

![console-new-ldap-connection-completed](http://www.agem.com.tr/console-screenshots/console-new-ldap-connection-completed.png)

######Bu butona tıkladığımızda yazdığımız makineye bağlantı kontrolü yapılır, başarılı olursa aşağıdaki gibi bir diyalog çıkar.

![console-check-network-parameter](http://www.agem.com.tr/console-screenshots/console-check-network-parameter.png)

######"OK"'a tıklayıp devam ediyoruz. Karşımıza aşağıdaki gibi "Authentication" sayfası çıkıyor.

![console-authentication](http://www.agem.com.tr/console-screenshots/console-authentication.png)

######Bu sayfada "Bind DN or user" alanına LDAP'taki Lider Console kullanıcısının DN'inin yazıyoruz.
######Bizim örneğimizde bu değer "`cn=lider_console,dc=tubitak,dc=gov,dc=tr`".
######"Bind password" alanına ise Lider Console kullanıcısının şifresini yazıyoruz. Bu örnekte şifre "`secret`".
######Gerekli alanları aşağıdaki gibi doldurup "Check Authentication" butonuna tıklıyoruz.

![console-authentication-completed](http://www.agem.com.tr/console-screenshots/console-authentication-completed.png)

######Girilen bilgiler test ediliyor ve başarılı olursa karşımıza aşağıdaki gibi bir ekran geliyor.

![console-check-authentication](http://www.agem.com.tr/console-screenshots/console-check-authentication.png)

######"OK"'a tıklayıo devam ediyoruz ve karşımıza aşağıdaki gibi "Browser Options" ekranı geliyor.

![console-browser-options](http://www.agem.com.tr/console-screenshots/console-browser-options.png)

######Bu ekranda önce "Get base DNs from Root DSE" check'ini kaldırıyoruz ve "Fetch Base DNs" butonuna tıkyoruz.
######Karşımıza aşağıdaki gibi bir diyalog çıkıyor ve bulduğu "Base DN"'i gösteriyor.

![console-fetch-base-dns](http://www.agem.com.tr/console-screenshots/console-fetch-base-dns.png)

######"OK"'a tıklayıp devam ediyoruz.
######Eğer aşağıdaki gibi "Base DN" alanı istediğimiz değerle dolmazsa, istediğimiz değeri "Fetch Base DNs" butonunun altındaki ok butonuna tıkladıktan sonra seçebiliriz.

![console-browser-options-completed](http://www.agem.com.tr/console-screenshots/console-browser-options-completed.png)

######Bu işlemden sonra "Finish" butonuna tıkladıktan sonra bağlantımız aşağıdaki gibi oluşuyor ve otomatik olarak açılıyor. Otomatik olarak bağlanmazsa bağlantının üzerine çift tıklayarak bağlanabilirsiniz.

![console-connected](http://www.agem.com.tr/console-screenshots/console-connected.png)


