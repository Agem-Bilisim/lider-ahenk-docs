###Ahenk Geliştirme Ortamı Kurulumu

###Bağımlılıklar ve Bileşenler <br />
* **git:** Projenin sürüm kontrol ve kaynak kod yönetimi için git'i terminal üzerinde ```sudo apt-get install git``` ile kurabilirsiniz.
* **depo ekleme:** [Bu adresteki](https://github.com/Pardus-Kurumsal/lider-ahenk-installer/blob/master/ahenk-installer/conf/liderahenk.list) dosyayı source list'inize ekleyip `sudo apt-get update` komutu ile etkin hale getirin.
* **python3.4+:** Güncel linux sürümlerinde hazır olarak gelmektedir.https://www.python.org/downloads/ adresinden indirilip kurulabilir.
* **pip3:** Python modüllerini kurmak için gerekli olan bu paketi `sudo apt-get install python3-pip` terminal komutu ile kurulabilir.
* **dev paketleri:** Python'un genişletilmiş geliştirme paketlerinden
 **python3-dev**'i`sudo apt-get install python3-dev`
 **libff**'i `sudo apt-get install libffi-dev`
 **libssl**'i `sudo apt-get install libssl-dev`
 terminal komutları ile kurabilirsiniz.
* **sleekmpp:** `sudo apt-get install python3-sleekxmpp` komutu ile kurulabilir.
* **paramiko:** ssh protokolü üzerinden dosya transferini sağlamak için gerekli olan bu modül`sudo pip3 install paramiko`
* **psutil:** Sistem temel bilgilerine erişim için kullanılan psutil `sudo pip3 install psutil` 
* **cpuinfo:** İşlemci bilgilerine erişim için kullanılan cpuinfo `sudo pip3 install py-cpuinfo` 


Bir Python projesi herhangi bir metin editöründe geliştirilebilir ya da geliştirme kitleri tercih edilebilir. En gelişmiş ve ücretsiz ide'lerden biri olan [pycharm](https://www.jetbrains.com/pycharm/download/)'ın community versiyonu yanı sıra [plugin desteğiyle eclipse](http://www.pydev.org/manual_101_install.html) ile de python projesi geliştirilebilir. Ahenk üzerinde sqlite çalıştırmaktadır. Bu ahenk veritabanını içeriğini görüntülemek-düzenlemek için [sqlite studio](http://sqlitestudio.pl/?act=download) gibi veritabanı araçları kullanabilirsiniz.

* IDE'ler üzerinde çalışırken varsayılan yorumlayıcınızın **python3.4+** olmasına ve ahenkd.py'ı **start** argümanı ile çalıştırdığınıza emin olun.

* Yukardaki paketler Pardus Kurumsal 5 için sıralanmıştır. Bazı paketler işletim sisteminizin dağıtımına göre hali hazırda varolabilir ya da tanımlanmış depolarda bulunmayabilir.

---
###Pycharm İçin İpucu

Ahenk'e plugin geliştirmek için Ahenk Core'u tamamen kurup geliştirdiğiniz eklentiyi çekirdeğe entegre etmek zorunda değilsiniz(plugin şablonu üzerinden giderek, Konsoldan belirlenen json verisinin gelidiğini varsayarak eklenti geliştirilebilir); fakat kontrollü bir işleyiş denetimi, Ahenk çekirdeğinin sağladığı servislerin kolayca kullanımı için Ahenk çekirdeğinin kullanılması önerilmektedir.

Yukarda belirtilen bağımlılıkları kurduktan sonra geliştirme ortamı için Pycharm'ı kullanabilirsiniz. [Pycharm](https://www.jetbrains.com/pycharm/download/)'a hızlıca gözatmak için [Quick Start Guide](https://www.jetbrains.com/help/pycharm/5.0/quick-start-guide.html)'a bakabilirsiniz.

**Not:** Ahenk sistem üzerinde çalışırken root hakkı gerektiren operasyonlar yapıldığı için Pycharm'ı sudo ile çalıştırmak gerekir. Eğer sudo hakkı olmayan bir kullanıcı ile aşağıdaki yapılandırma ayarlarını yaptıysanız, sudo ile açtığınızda bu ayarların bir kısmı etkin olmayabilir; bu işlemleri bir defaya mahsus tekrarlamanız gerekebilir.

**Projeyi Açmak:** `File->Open->(ahenk_projesinin_yolu)` ile projeyi seçin. Ardından Sol taraftaki dizin ağacından `../ahenk/opt/ahenk` yolundaki (opt altındaki) **ahenk** klasörüne sağ tıklayıp `Mark Directory As-> Sources Root` ile kök dizin seviyesini belirleyin.

**Varsayılan Yorumlayıcıyı Değiştirmek:** `File->Settings` ile gelen ekranda **interpreter** diye arattıktan sonra gelen ekranda **Project Interpreter** select box'undan **python3.4**'ü seçin. Bu ekranda aynı zamanda Python modülleri de kolay bir şekilde kurulabilir. Bunun için Ekrandaki tablonun sağ tarafındaki **+** simgesine tıklayıp module isimlerini aratıp kurabilirsiniz(Eğer bu module kurarken hata alıyorsanız geçerli python versiyonunuzu ve kurmak istediğiniz python kütüphanesinin bağımlılıklarını kontrol ediniz.).

**Projenin Debug Yapılandırılması:** `Run->Edit Configurations..` ile açılan ekranın sol üst kısmında **+** ikonu ile yeni bir python konfigurasyonu ekleyelim. Bu konfigurasyona ahenk ismini verdikten sonra **Script** parametresi olarak ahenkd.py'ı gösterin (`/opt/ahenk/ahenkd.py` gibi bir yol olmalı). `ahenkd.py` Ahenk'in başlatıldığı script'tir. Bu script'e **start**, **stop**, **restart**, **status** gibi parametreler geçilebilir. Ahenk'i çalıştırmak için **Edit Configuration** ekranındayken **Script parameters**'e `start` parametresi geçmemiz gerekir. Son olarak projenin yorumlayıcısını **Python interpreter** alanından **python 3.4**'e çekip `Apply->Ok` butonlarına basınız.

Ahenk projesini **Shift+F10** ile koşturabilir,**Shift+F9** ile debug edebilirsiniz.(Ya da çalıştırmak istediğiniz py'a sağ tıklayıp run ya da debug seçenekleri ile çalıştırabilirsiniz)

---

###Projeyi Yerel Dosyaya Çekmek - Çalıştırmak<br />
1. Proje dosyalarını barındıracağınız bir klasör oluşturun.(**git/** gibi ```-mkdir git```)<br />
2. Oluşturduğumuz git dosyası içinde
```git clone https://github.com/Pardus-Kurumsal/ahenk.git``` komutu ile projeyi yerel dosyanıza çekin.<br />
3. ```../git/ahenk/etc/``` altındaki **ahenk** klasöründe bulunan **ahenk.conf** dosyasını **host**, **port**, **receiverjid**, **pluginfolderpath** ve **receivefileparam** alanların değerlerini sisteminize göre düzenleyin; sonra da **git/ahenk/etc/** dizini altındaki **ahenk** klasörünü  ```/etc/``` yolunun altına kopyalayın.( conf dosyasındaki parametrelerin nelere karşılık geldiğini görmek için [bu linkteki dokümanı](https://github.com/Agem-Bilisim/lider-ahenk-docs/blob/master/ahenk/sss.md) inceleyebilirsiniz.)
4. **Ahenk** daemon'u başlatmak için  **../git/ahenk/opt/ahenk/** yolundaki **ahenkd.py**'ı ```sudo python3 ahenkd.py start``` komutuyla çalıştırıyoruz.(IDE kullanıyorsak `start` argümanı vererek `ahenkd.py` scriptini çalıştırıyoruz.)
5. `ahenkd.py` scripti **start**, **status**, **stop**, **restart**, **clean** gibi farklı parametreler alabilir.Eğer politika eklentileri üzerinde çalışıyorsak, politikaların yüklenebilmesi için kullanıcıların hedef makinede(ahenk kurulu) oturum açmaları gerekir. Belirlenen politikalar oturum açılırken **Lider**'den alınıp çalıştırılır. **Ahenk**'in çalıştığı makinenin sürekli oturum açma-kapatma işlemleri ile vakit kaybetmek istemiyorsanız; ```sudo python3 ahenkd.py login user_name desktop_environment_type :display_number``` ile istenilen kullanıcının oturum açmış gibi **Ahenk**'i tetiklemesini sağlayabilirsiniz.
