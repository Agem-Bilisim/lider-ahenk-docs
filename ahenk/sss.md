#SSS#

###Ahenk loglarına nerden bakabilirim?
Ahenk uygulamasının logları /var/log/ dizini altında ahenk.log dosyasındadır. 
`tail -f /var/log/ahenk.log` terminal komutu ile eşzamanlı olarak logları takip edebilirsiniz.

###Registration işlemi gerçekleşmiyor-Ahenk ayağa kalkmıyor. Problem ne olabilir?
- Öncelikle XMPP Sunucunuzun, Lider uygulamanızın **açık** ve **erişilebilir** olduğundan emin olun.
- Kullandığınız sanal servisin kullanıcıları arasında Lider uygulamasının XMPP kullanıcı adının olduğuna ve bu kullanıcı adının aktif (online) kullanıcılar listesinde olduğunu kontrol edin. (Bağlı kullanıcıları Ejabberd Web Console üzerinden takip edebilirsiniz)
- `/etc/ahenk/ahenk.conf` yapılandırma dosyasının parametrelerinin  doğru ve geçerli olduğunu kontrol edin.
- Bu yapılandırma dosyasının içeriği ve parametrelerin anlamlandırılması aşağıdaki gibidir.
- Ahenk, bir bilgisayar üzerinde ilk kez çalışırken, Lider'in kendisini kayıt etmesi için, anonim olarak Lider'e XMPP mesajı atmaya çalışır. XMPP Sunucunuzun anonim kullanımlar için yapılandırma ayarlarının gerçekleştirilmiş olması gerekmektedir. XMPP Sunucusu için Ejabberd kullanıyorsanız bu ayarlar `.../ejabberd16.x/conf/ejabberd.yml` dosyası içinde auth method kısmında olmalıdır.

###Ahenk bir kere register oldu fakat; register bilgilerini silip yeniden register etmek istiyorum. Ne yapmam lazım?
Ahenk üzerindeki veritabanı kayıtlarını kaldırmak ve register bilgilerini ahenk.conf dosyasından silmek için  `/opt/ahenk/` altında `sudo python3 ahenkd.py clean` komutunu çalıştırmanız yeterlidir. Ahenk bir sonraki açılışında, ilk kez çalışıyormuş gibi registration işlemini başlatacaktır.

**Not:** Yukardaki işlemlerin yanısıra Lider-Ahenk-Lider Arayüz versiyonlarının güncel ve bir biri ile uyumlu olduğundan emin olunuz.
