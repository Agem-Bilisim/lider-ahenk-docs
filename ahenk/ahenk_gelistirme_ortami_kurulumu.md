###Ahenk Geliştirme Ortamı Kurulumu

###Bağımlılıklar ve Bileşenler <br />
* **git:** Projenin sürüm kontrol ve kaynak kod yönetimi için git'i terminal üzerinde ```sudo apt-get install git``` ile kurabilirsiniz.
* **python3.4+:** Güncel linux sürümlerinde hazır olarak gelmektedir.https://www.python.org/downloads/ adresinden indirilip kurulabilir.
* **pip3:** Python modüllerini kurmak için gerekli olan bu paketi `sudo apt-get install python3-pip` terminal komutu ile kurulabilir.
* **dev paketleri:** Python'un genişletilmiş geliştirme paketlerinden
 **python3-dev**'i`sudo apt-get install python3-dev`
 **libff**'i `sudo apt-get install libffi-dev`
 **libssl**'i `sudo apt-get install libssl-dev`
 terminal komutları ile kurabilirsiniz.
* **slixmpp:** Açık kaynak xmpp python client projesidir. Ahenk için küçük düzenlemeler yapılmış hali https://github.com/volkansahin/slixmpp bu adrestedir. Sistemde '''cython''', '''libidn11''' ve '''libidn11-dev''' paketlerinin olması gerekir. Bunun için '''sudo apt-get install cython libidn11 libidn11-dev''' komutunu çalıştırınız. slixmpp dosyası içinde ```python3 setup.py install``` komutu ile kurulabilir.
* **paramiko:** ssh protokolü üzerinden dosya transferini sağlamak için gerekli olan bu modül`sudo pip3 install paramiko`
* **psutil:** Sistem temel bilgilerine erişim için kullanılan psutil `sudo pip3 install psutil` 


Bir Python projesi herhangi bir metin editöründe geliştirilebilir ya da geliştirme kitleri tercih edilebilir. En gelişmiş ve ücretsiz ide'lerden biri olan [pycharm](https://www.jetbrains.com/pycharm/download/#section=linux)'ın community versiyonu yanı sıra [plugin desteğiyle eclipse](http://www.pydev.org/manual_101_install.html) ile de python projesi geliştirilebilir. Ahenk üzerinde sqlite çalıştırmaktadır. Bu ahenk veritabanını içeriğini görüntülemek-düzenlemek için [sqlite studio](http://sqlitestudio.pl/?act=download) gibi veritabanı araçları kullanabilirsiniz.

* IDE'ler üzerinde çalışırken varsayılan yorumlayıcınızın **python3.4+** olmasına ve ahenkd.py'ı **start** argümanı ile çalıştırdığınıza emin olun.

* Yukardaki paketler Pardus Kurumsal 5 için sıralanmıştır. Bazı paketler işletim sisteminizin dağıtımına göre hali hazırda varolabilir ya da tanımlanmış depolarda olmayabilir.

---

###Projeyi Yerel Dosyaya Çekmek - Çalıştırmak<br />
1. Proje dosyalarını barındıracağınız bir klasör oluşturun.(**git/** gibi ```-mkdir git```)<br />
2. Oluşturduğumuz git dosyası içinde
```git clone https://github.com/Pardus-Kurumsal/ahenk.git``` komutu ile projeyi yerel dosyanıza çekin.<br />
3. ```../git/ahenk/etc/``` altındaki **ahenk** klasöründe bulunan **ahenk.conf** dosyasını **host**, **port**, **receiverjid**, **pluginfolderpath** ve **receivefileparam** alanların değerlerini sisteminize göre düzenleyin; sonra da **git/ahenk/etc/** dizini altındaki **ahenk** klasörünü  ```/etc/``` yolunun altına kopyalayın.
4. ahenk daemon'u başlatmak için  **../git/ahenk/opt/ahenk/** yolundaki **ahenkd.py**'ı ```sudo python3 ahenkd.py start``` komutuyla çalıştırıyoruz.
