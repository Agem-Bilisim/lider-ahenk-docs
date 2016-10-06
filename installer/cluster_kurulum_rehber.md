### Lider Ahenk Cluster Kurulumu
######Bu rehberde Lider Ahenk sunucularının Kolay Kurulum uygulaması kullanılarak cluster (küme) şeklinde nasıl kurulacağı anlatılacaktır.
####Ön Gereksinimler
######Lider Ahenk sunucuları minimum node (düğüm) sayılarıyla cluster olarak kurulduğunda aşağıdaki gibi bir makine düzenine ihtiyaç vardır:

##### 1) MariaDB Cluster Kurulumu için:
######En az 3 makine 
######Not 1: MariaDB'nin Karaf üzerinde çalışan driver yük dengeleme işini kendisi yaptığı için bu node'ların önünde herhangi bir ekstra load balancer'a ihtiyaç yoktur.
######Not 2: MariaDB'nin yapısı gereği brain-splitting vs. sorunlarına karşı MariaDB cluster yapısı diğer bileşenlerin aksine en az üç node ile ayağa kalkmaktadır.
##### 2) Ejabberd Cluster Kurulumu için:
