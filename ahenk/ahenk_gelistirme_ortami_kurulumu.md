###Ahenk Geliştirme Ortamı Kurulumu

###Bağımlılıklar <br />
* **python3.4+:** Güncel linux sürümlerinde hazır olarak gelmektedir.https://www.python.org/downloads/ adresinden indirilip kurulabilir.
* **slixmpp:** Açık kaynak xmpp python client projesidir. Ahenk için küçük düzenlemeler yapılmış hali https://github.com/volkansahin/slixmpp bu adrestedir. Sistemde cython ve libidn olması gerekir.slixmpp dosyası içinde ```python3 setup.py install``` komutu ile kurulabilir.
* **git:** Projenin sürüm kontrol ve kaynak kod yönetimi için git'i terminal üzerinde ```sudo apt-get install git``` ile kurabilirsiniz.

Bir Python projesi herhangi bir metin editöründe geliştirilebilir ya da geliştirme kitleri tercih edilebilir. En gelişmiş ve ücretsiz ide'lerden biri olan [pycharm](https://www.jetbrains.com/pycharm/download/#section=linux)'ın community versiyonu yanı sıra [plugin desteğiyle eclipse](http://www.pydev.org/manual_101_install.html) ile de python projesi geliştirilebilir. Ahenk üzerinde sqlite çalıştırmaktadır. Bu ahenk veritabanını içeriğini görüntülemek-düzenlemek için [sqlite studio](http://sqlitestudio.pl/?act=download) gibi veritabanı araçları kullanabilirsiniz.

---

###Pycharm Kullanımı<br />

---

###Projeyi Yerel Dosyaya Çekmek - Çalıştırmak<br />
1. Proje dosyalarını barındıracağınız bir klasör oluşturun.(**git/** gibi ```-mkdir git```)<br />
2. Oluşturduğumuz git dosyası içinde
```git clone https://github.com/Pardus-Kurumsal/ahenk.git``` komutu ile projeyi yerel dosyanıza çekin.<br />
3. ```../git/ahenk/etc/``` altındaki **ahenk** klasöründe bulunan **ahenk.conf** dosyasını pluginfolderpath ve receivefileparam alanların değerlerini sisteminize göre düzenleyin; sonra da **ahenk** klasörünü  ```/etc/``` yolunun altına kopyalayın.
4. ahenk daemon'u çalıştırmak için  **../git/ahenk/opt/ahenk/** yolundaki **ahenkd.py**'ı ```sudo python3 ahenkd.py start``` komutuyla çalıştırıyoruz.
