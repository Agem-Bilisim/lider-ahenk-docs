#SSS#

###Ahenk loglarına nerden bakabilirim?
Ahenk uygulamasının logları /var/log/ dizini altında ahenk.log dosyasındadır. 
`tail -f /var/log/ahenk.log` terminal komutu ile eşzamanlı olarak logları takip edebilirsiniz.



###Registration işlemi gerçekleşmiyor-Ahenk ayağa kalkmıyor. Problem ne olabilir?
- Öncelikle XMPP Sunucunuzun, Lider uygulamanızın **açık** ve **erişilebilir** olduğundan emin olun.
- Kullandığınız sanal servisin kullanıcıları arasında Lider uygulamasının XMPP kullanıcı adının olduğuna ve bu kullanıcı adının aktif (online) kullanıcılar listesinde olduğunu kontrol edin.
- `/etc/ahenk/ahenk.conf` yapılandırma dosyasının parametrelerinin  doğru ve geçerli olduğunu kontrol edin.
- Bu yapılandırma dosyasının içeriği ve parametrelerin anlamlandırılması aşağıdaki gibidir.
- Ahenk, bir bilgisayar üzerinde ilk kez çalışırken, Lider'in kendisini kayıt etmesi için, anonim olarak Lider'e XMPP mesajı atmaya çalışır. XMPP Sunucunuzun anonim kullanımlar için yapılandırma ayarlarının gerçekleştirilmiş olması gerekmektedir. XMPP Sunucusu için Ejabberd kullanıyorsanız bu ayarlar `.../ejabberd16.x/conf/ejabberd.yml` dosyası içinde auth method kısmında olmalıdır.

###Ahenk yapılandırma ayarları doğru mu? Hangi parametre ne işe yarar?
```
[BASE]
logconfigurationfilepath = /etc/ahenk/log.conf
dbpath = /etc/ahenk/ahenk.db

[PLUGIN]
pluginfolderpath = /opt/ahenk/plugins/
mainmodulename = main

[CONNECTION]
uid = 1111111-2222-33333-4444-555555
password = aaaaa-bbbbb-ccccc-ddd-eeeeeeee
host = 172.165.8.X
port = 5222
receiverjid = lider_sunucu
receiverresource = Smack
servicename = agem.com.tr
fileserver = 
receivefileparam = /tmp/
```
**BASE**

**logconfigurationfilepath:**  log yapılandırma ayarlarını barındıran dosyanın yoludur. Varsayılan değer `/etc/ahenk/log.conf`'tur.
**dbpath:** Ahenk çekirdeğinin operasyonlarında kullandığı veritabanının yoludur. Varsayılan değer `/etc/ahenk/ahenk.db`'dir.

**PLUGIN**

**pluginfolderpath:** Ahenk eklentilerinin bulunduğu dizin yoludur. Varsayılan değer `/opt/ahenk/plugins/`'dir.
**mainmodulname:** Eklentiler Ahenk' e yüklenmesinde kullanılan temel py dosyasının adıdır. Varsayılan değer `main` 'dir

**CONNECTION**

**uid:**Ahenk'in kendisini kaydetmek ve XMPP Sunucusuna bağlanmak için kullandığı biricik id numarasıdır. Bu alan Ahenk tarafından doldurulacaktır.

**password:** XMPP Sunucusuna bağlanırken kullanmak üzere oluşturulan şifredir. Bu alan Ahenk tarafından doldurulacaktır.

**host:** XMPP sunucusu ip adresidir.Aktif XMPP sunucusunun geçerli ve erişilebilir adresi girilmelidir.

**port:** XMPP sunucusuna erişim için kullanılacak port numarasıdır. Port numarası varsayılan değer olarak **5222**'dir. 5222 genelde TLS'i destekleyen yapılandırmalar için  kullanılır. Bu değerin yanısıra standartlaşmış diğer port numaraları da bulunmaktadır. Bu numaraları kullanırken XMPP Sunucunuzun yapılandırma ayarlarına dikkat etmeniz gerekmektedir. Ejabberd XMPP Sunucusu için  ilgili detaya [link üzerinden](https://docs.ejabberd.im/admin/guide/security/) erişebilirsiniz.

**receiverjid:** Lider uygulamasına XMPP Sunucu üzerinden erişmek için gerekli olan kullanıcı adıdır.

**receiverresource:** Lider uygulamasına XMPP Sunucu üzerinden erişmek için gerekli olan [kaynak](https://wiki.xmpp.org/web/Jabber_Resources) adıdır.

**servicename:** XMPP Sunucusunun sağladığı sanal servis adıdır. Ahenk ve Lider hesapları bu servis üzerinde tanımlı olmalıdır.Aktif XMPP sunucusunun geçerli ve erişilebilir servis adı girilmelidir.

**fileserver:** Dosya transfer işlemleri sırasında kullanılmak üzere erişilmesi gereken dosya sunucusunun adresi.

**receivefileparam:** Ahenk'e gelen dosyaların kaydedileceği dizin yoludur.

###Ahenk bir kere register oldu fakat; register bilgilerini silip yeniden register etmek istiyorum. Ne yapmam lazım?
Ahenk üzerindeki veritabanı kayıtlarını kaldırmak ve register bilgilerini ahenk.conf dosyasından silmek için  `/opt/ahenk/` altında `sudo python3 ahenkd.py clean` komutunu çalıştırmanız yeterlidir. Ahenk bir sonraki açılışında, ilk kez çalışıyormuş gibi registration işlemini başlatacaktır.
