#Ahenk Çalışma Mekanikleri

###Event Tetiklemek 

Ahenk [pam](http://tldp.org/HOWTO/User-Authentication-HOWTO/x115.html) gibi modülleri kullanarak sistem üzerindeki gerçekleştirilen kullanıcı girişi-çıkışı, sistemin kapanması vb.. aktiviteleri algılayabilir. Örneğin giriş yapan kullanıcının politikasının lider'den istenmesi için **oturum açma** işleminin farkedilmesi.  Bu ve benzeri işlemler temelde arka planda bir terminal komutu çalıştırmaktadır. Eklenti geliştirme sırasında, benzer şekilde event tetiklemek için; ahenk çalıştığı sırada komut satırından ```python3 ahenkd.py [event] [parameters]``` şablonunda çalıştırılabilir. Örneğin ```python3 ahenkd.py login volkan ``` gibi ... 


---



###Eklenti Yapısı

Bir ahenk eklentisinin(plugin) dosya yapısı **plugins** dizini altında aşağıdaki gibidir:
 <br />
**myplugin/** <br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** main.py<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** policy.py<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** safe.py<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** taskId1.py <br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** taskId1.py <br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** **api/**<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** _plugin_name_Service.py<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** environmentA1.* <br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** environmentA2_.* <br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**L** Default.* <br />
   
* Bu standart yapıdaki bir eklentinin **taskId1** isimli görevin çalıştırılması için Ahenk çekirdeği **taskId1.py** daki task **handle_task(task,context)** fonksiyonunu tetikler. Bu fonksiyon task işleminin başlangıç noktası olarak varsayılabilir. **handle_task** 2 tane parametre alır. İlk parametre olan **task** eklenti geliştiricisinin lider bileşeninden gönderdiği json iletisidir. Görevin ihtiyaç duyduğu veriler eklentiyi tasarlayanın belirlediği yapıyla json mesajı olarak bu parametre ile değerlendirilebilir.

* İkinci parametre olan **context** ahenk çekirdeği ile eklentinin ortak map i olarak düşünülebilir. **context** aracılığı ile **görev** in sonucu hakkında bilgi mesajı, veri, bu verinin tipi,... gibi içerikler döndürülebilir.

* Politikanın işleyişi Görev ile benzerdir. Her eklentinin bir policy.py dosyası bulunur ve gelen profil datasının tamamının işlenmesinden sorumludur.


---

###Api Yapısı

Bazı Görev ya da Profil operasyonları sistem bileşenleri,türleri ya da versiyonları ile doğrudan ilgilidir. Bu tip bağımlılıklar genellikle bilgisayar mimarilerinden, grafik arayüz bileşenlerinden, kernel versiyonlarından kaynaklanmaktadır. Bu bağımlılık tipleri öngörülemeyecek şekilde değişebilir veya artabilir.Bu noktada hedef bilgisayarlar göz önünde bulundurularak eklenti operasyonlarının işlevselliğini sürdürebilmesi için bağımlılıklara uygun çeşitli çözümler geliştirmek eklenti geliştiricisinin sorumluluğundadır. Bu amaçla geliştiriciden beklenen yapı yukardaki dosya hiyerarşisine bağlı olarak dinamik nesne erişimi sağlayan api mekanizmasıdır. Bir örnekle açıklamak gerekirse; X eklentisinin a işlevi(ekran görüntüsü alma operasyonu gibi) makine tipi ve masaüstü platformuna bağımlılığı bulunur ve her bir ikili bağımlılık için ayrı ayrı ele alınması gerekir.
Bu amaçla olası hedef makineler için a işlevini gerçekleştiren aşağıdaki gibi sınıflar oluşturulması beklenir.
* ltsp_kde.py 
* x2go_kde.py 
* ltsp_gnome.py 
* x2go_gnome.py
* ltsp_default.py 
* x2go_default.py 
* default_kde.py 
* default_gnome.py 
* default_gnome.py 
* default_default.py 
(varyasyon sayısı bağımlılık sayısına göre artıp azalabilir. Örn: x64 ve x86 mimari bağımlılığının eklenmesi)
Bu sınıfları aynı interface i gerçekleyen sınıflar olarak düşünebiliriz.

Son olarak plugin_nameService.py da Ahenk çekirdeğinden alınan bağımlılık tipi bilgisine göre geçerli nesne a işlevini gerçekleştirmek üzere gerekli yerlerde kullanılır.
