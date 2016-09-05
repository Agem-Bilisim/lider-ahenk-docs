
#Ahenk Yapılandırma Dosyası
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
host = XXX.XXX.XXX.XXX
port = 5222
use_tls = false
receiverjid = lider_sunucu
receiverresource = Smack
servicename = im.agem.com.tr
receivefileparam = /tmp/

[SESSION]
agreement_timeout = 30
registration_timeout = 30
get_policy_timeout = 30

[MACHINE]
type = default

[MAIL]
smtp_host = smtp.mail_server_name.com
smtp_port = 587
from_username = username_mail
from_password = password_mail
to_address = target_mail_address@mail_server.com

```
###BASE

**logconfigurationfilepath:**  log yapılandırma ayarlarını barındıran dosyanın yoludur. Varsayılan değer `/etc/ahenk/log.conf`'tur.
**dbpath:** Ahenk çekirdeğinin operasyonlarında kullandığı veritabanının yoludur. Varsayılan değer `/etc/ahenk/ahenk.db`'dir.

###PLUGIN

**pluginfolderpath:** Ahenk eklentilerinin bulunduğu dizin yoludur. Varsayılan değer `/opt/ahenk/plugins/`'dir.
**mainmodulname:** Eklentiler Ahenk' e yüklenmesinde kullanılan temel py dosyasının adıdır. Varsayılan değer `main` 'dir

###CONNECTION

**uid:**Ahenk'in kendisini kaydetmek ve XMPP Sunucusuna bağlanmak için kullandığı biricik id numarasıdır. Bu alan Ahenk tarafından doldurulacaktır.

**password:** XMPP Sunucusuna bağlanırken kullanmak üzere oluşturulan şifredir. Bu alan Ahenk tarafından doldurulacaktır.

**host:** XMPP sunucusu ip adresidir.Aktif XMPP sunucusunun geçerli ve erişilebilir bir ip adresi girilmelidir.

**port:** XMPP sunucusuna erişim için kullanılacak port numarasıdır. Port numarası varsayılan değer olarak **5222**'dir. 5222 genelde TLS'i destekleyen yapılandırmalar için  kullanılır. Bu değerin yanısıra standartlaşmış diğer port numaraları da bulunmaktadır. Bu numaraları kullanırken XMPP Sunucunuzun yapılandırma ayarlarına dikkat etmeniz gerekmektedir. Ejabberd XMPP Sunucusu için  ilgili detaya [link üzerinden](https://docs.ejabberd.im/admin/guide/security/) erişebilirsiniz.

**use_tls:** XMPP sunucusu tls bağlantıları destekliyorsa bu alan *true* olarak doldurulmalı, aksi takdirde *false* olmalı.

**receiverjid:** Lider uygulamasına XMPP Sunucu üzerinden erişmek için gerekli olan kullanıcı adıdır.

**receiverresource:** Lider uygulamasına XMPP Sunucu üzerinden erişmek için gerekli olan [kaynak](https://wiki.xmpp.org/web/Jabber_Resources) adıdır. Eğer cluster yapıda bir Lider kullnıyorsanız bu alanı boş bırakınız.

**servicename:** XMPP Sunucusunun sağladığı sanal servis adıdır. Ahenk ve Lider hesapları bu servis üzerinde tanımlı olmalıdır.Aktif XMPP sunucusunun geçerli ve erişilebilir servis adı girilmelidir.

**receivefileparam:** Ahenk'e gelen dosyaların kaydedileceği dizin yoludur.

###SESSION

**agreement_timeout:** Kullanıcı sözleşmesinin kabulu için süre kısıtının saniye türünden değeri 

**registration_timeout:** Ahenk'in Lider'e kayıt işlemi için beklenecek azami süre değeri (saniye türünden)

**get_policy_timeout:** Kullanıcı oturum açtıktan sonra, Ahenk'in Lider'den güncel politikaları almak için beklediği azami süre (saniye türünden)

###MACHINE
**type:** Yaygın olmayan makine tiplerini saklamak ya da özel durumlarda kullanmak üzere makinelere verilebilecek değiştirilebilir alan

###MAIL
**smtp_host:** Mail servisin adresi (SMTP)

**smtp_port:** Mail servis kullanılabilir portu

**from_username :** Belirtilmiş mail sunucusunda tanımlı mail adresi

**from_password:** Yukardaki mail adresinin şifresi

**to_address:** Mailin gönderileceği hedef mail adresi
