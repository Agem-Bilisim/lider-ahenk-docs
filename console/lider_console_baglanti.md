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

######Görüldüğü gibi Lider Console uygulamasında henüz hiçbir bağlantı yok. Yeni bir bağlantı oluşturmak için ekranın sol alt kısmındaki "Connections" bölümünde üzerinde "LDAP" yazan ikona basıyoruz.
######Bastıktan sonra karşımıza aşağıdaki gibi "New LDAP Connection" isminde yeni bir diyalog çıkıyor.

![console-new-ldap-connection](http://www.agem.com.tr/console-screenshots/console-new-ldap-connection.png)

######Burada "Connection name" alanına istediğimiz bir isim veriyoruz. 
######Önemli olan nokta, "Hostname" alanına LDAP sunucusunun IP'si yazılmalıdır.
######Eğer Lider Console ve LDAP sunucusu aynı makinede çalışıyorsa IP adresi olarak "localhost" yapılabilir.
######Lider Console ve LDAP sunucu aynı adreste olmadığı halde "localhost" yazılırsa bağlantılı kurulamayacaktır.
######Aşağıdaki gibi gerekli alanları doldurduktan sonra "Check Network Parameter" butonuna basıyoruz.

![console-new-ldap-connection-completed](http://www.agem.com.tr/console-screenshots/console-new-ldap-connection-completed.png)

######Bu butona bastığımızda yazdığımız makineye bağlantı kontrolü yapılır, başarılı olursa aşağıdaki gibi bir diyalog çıkar.

![console-check-network-parameter](http://www.agem.com.tr/console-screenshots/console-check-network-parameter.png)

######"OK"'a basıp devam ediyoruz. Karşımıza aşağıdaki gibi "Authentication" sayfası çıkıyor.

![console-authentication](http://www.agem.com.tr/console-screenshots/console-authentication.png)

######Bu sayfada "Bind DN or user" alanına LDAP'taki Lider Console kullanıcısının DN'inin yazıyoruz.
######Bizim örneğimizde bu değer "`cn=lider_console,dc=tubitak,dc=gov,dc=tr`". 
######"Bind password" alanına ise Lider Console kullanıcısının şifresini yazıyoruz. Bu örnekte şifre "`secret`".
######Gerekli alanları aşağıdaki gibi doldurup "Check Authentication" butonuna basıyoruz.

![console-authentication-completed](http://www.agem.com.tr/console-screenshots/console-authentication-completed.png)

######Girilen bilgiler test ediliyor ve başarılı olursa karşımıza aşağıdaki gibi bir ekran geliyor.

![console-check-authentication](http://www.agem.com.tr/console-screenshots/console-check-authentication.png)

######"OK"'a basıp devam ediyoruz ve karşımıza aşağıdaki gibi "Browser Options" ekranı geliyor.

![console-browser](http://www.agem.com.tr/console-screenshots/console-browser.png)
