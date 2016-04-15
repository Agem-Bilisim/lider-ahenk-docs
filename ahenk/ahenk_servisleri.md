# Ahenk Servisleri

Ahenk uygulaması ayağa kalkarken, ahenkd.py çalışmaya başladığında, eklentiler veya ahenk çekirdeği üzerinde kulanılabilmesi amacıyla bazı özelleşmiş yeteneklere sahip servisleri singleton yapıya ship scope'ta hizmet vermek için saklanır. Bu servisleri kullanmak için Scope sınıfından tekil nesneye erişip istenilen nesnenin getter fonksiyonu kullanmak yeterlidir.

Bu servisler şunlardır:
* **Config Manager**: Yapılandırma dosyasının okunması, değiştirilmesi, yazılmasını sağlar.
* **Logger**: Farklı seviyelerde log dosyasına kayıt düşmek için kullanılır. Kaydedilen loglar ```/var/ahenk/log/ahenk.log``` dosyasına kaydedilir. Ahenk'in baştan başlatılması ile kayıtlar silinmez. Genel kayıt mesajı standardı: 
> logger.debug('[ExecutionManager] Politika işlemeye başlandı')

* **EventManager**: Bu nesne kullanılarak event'ler ile fonksiyonlar eşleştirilir, event fırlatılabilir. Böylece uygulamanın herhangi bir yerinden fırlatılan event ile önceden tanımlanmış event-actionlar sayesinde fonksiyon tetiklenir.

* **AhenkDbService**: Ahenk'in kullandığı sqlite için temel veritabanı işlemlerini gerçekleştirmek için kullanılır.

* **MessageManager**: Temel işleyişleri gerçekleştirmek için kullanılan json mesajlarını oluşturmak için kullanılır.

> message = scope.getMessageManager().policy_request_msg('user_name')


* **TaskManager**: Görev ve politikalar üzerinde kaydetmek, eklemek, silmek gibi temel işlemleri gerçekleştirir.

* **Registration**: Ahenk uygulaması çalışmaya başladığında lider tarafından doğrulanmasını sağlar. Doğrulanmamışsa ya da ilk defa çalıştırılıyorsa kendisini doğrulaması için lider ile gerekli protokolü başlatır.

* **ExecutionManager**: Ahenk ve Lider çekirdeği arasında belirlenen protokolleri ve iletişim şablonlarını tanımlar ve EventManager kullanarak bu mesaj şablonlarının doğrulanmasını gerçekleştirir.

* **Messager**: İletişim yöntemlerini tanımlar ve gerçekler. Messager ile mesaj-dosya alıp, gönderebilir; XMPP bağlantısı açıp kapatılabilir. Bir çeşit XMPP Client'ıdır.

* **MessageResponseQueue**: Mesaj kuyruğudur. Ayrı bir thread olarak çalışır. Bu kuyruğa koyulan iletiler lidere mesaj olarak gönderilir.

