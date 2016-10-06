## Lider Ahenk Cluster Kurulumu
######Bu rehberde Lider Ahenk sunucularının Kolay Kurulum uygulaması kullanılarak cluster (küme) şeklinde nasıl kurulacağı anlatılacaktır.

### Ön Gereksinimler
######Lider Ahenk sunucuları minimum düğüm sayılarıyla cluster olarak kurulduğunda aşağıdaki gibi bir makine düzenine ihtiyaç vardır:

##### 1) MariaDB Cluster Kurulumu için:
######En az 3 sanal veya fiziksel makine gerekmektedir.
######Not 1: MariaDB'nin Karaf üzerinde çalışan sürücüsü yük dengeleme işini kendisi yaptığı için bu node'ların önünde herhangi bir ekstra yük dengeleyiciye ihtiyaç yoktur.
######Not 2: MariaDB'nin yapısı gereği split-brain vs. sorunlarına karşı MariaDB cluster yapısı diğer bileşenlerin aksine en az üç düğüm ile ayağa kalkmaktadır.

##### 2) Ejabberd Cluster Kurulumu için:
######En az 3 sanal veya fiziksel makine gerekmektedir. (2 makine Ejabberd düğümü ve 1 makine yük dengeleyici)

##### 3) Karaf (Lider) Cluster Kurulumu için:
######En az 3 sanal veya fiziksel makine gerekmektedir. (2 makine Karaf düğümü ve 1 makine yük dengeleyici)

##### OPSİYONEL: Dosya Sunucusu
######Dosya sunucusu olarak kullanılmak üzere yukarıda verilen sisteme bir makine daha eklenebilir.

######NOT: Yük dengeleyici olarak çalışacak olan makineler çok ağır işlemler yapmayacağından, daha düşük özellikli seçilebilir.

##### İşletim Sistemi:
######Sistem Debian versiyon 8.0 Jessie dağıtımı ve üzerinde test edilmiştir, farklı dağıtımlar önerilmez.


