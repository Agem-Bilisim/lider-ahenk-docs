# LDAP üzerinden doğrulama ve PAM yapılandırma ayarları

Pardus-GK yüklü bilgisayarlardan (istemci), LDAP sunucusu üzerinden kimlik doğrulamanın sağlanması için gerekli adımlar aşağıdaki gibidir.
LDAP sunucusunun önceden kurulu ve çalışır durumda olduğu varsayılmıştır.

1. Gerekli bağımlılıkları `sudo apt-get install -y libnss-ldapd libpam-ldapd ldap-utils` komutuyla yükleyin.
    1. Kurulum esnasında sorulan LDAP sunucu adresini girin ("ldap://ldap.agem.com.tr" gibi)
    2. LDAP sunucusu arama tabanını girin ("dc=agem,dc=com,dc=tr" gibi)
    3. Yapılandırılacak ad hizmetleri listesinden (en azından) _group_, _passwd_ ve _shadow_ seçeneklerini işaretleyin.

2. NSS hizmetinin LDAP ile çalışmasını sağlamak için `/etc/nsswitch.conf` dosyasını aşağıdaki gibi düzenleyin. Söz konusu dosyada en azından passwd, group ve shadow satırları aşağıdaki gibi olmalıdır.

    ```apacheconf
# /etc/nsswitch.conf
#
# Example configuration of GNU Name Service Switch functionality.
# If you have the `glibc-doc-reference' and `info' packages installed, try:
# `info libc "Name Service Switch"' for information about this file.

passwd:         files ldap
group:          files ldap
shadow:         files ldap
gshadow:        files

hosts:          files mdns4_minimal [NOTFOUND=return] dns
networks:       files

protocols:      db files
services:       db files
ethers:         db files
rpc:            db files

netgroup:       nis
    ```

3. LDAP bağlantı bilgilerini `/etc/nslcd.conf` dosyasını düzenleyerek aşağıdaki gibi tanımlayınız:

    ```apacheconf
# /etc/nslcd.conf
# nslcd configuration file. See nslcd.conf(5)
# for details.

# The user and group nslcd should run as.
uid nslcd
gid nslcd

# The location at which the LDAP server(s) should be reachable.
uri ldap://ldap.agem.com.tr

# The search base that will be used for all queries.
base dc=agem,dc=com,dc=tr

# The LDAP protocol version to use.
#ldap_version 3

# The DN to bind with for normal lookups.
binddn cn=annonymous,dc=agem,dc=com,dc=tr
bindpw 1

# The DN used for password modifications by root.
#rootpwmoddn cn=admin,dc=example,dc=com

# SSL options
#ssl off
#tls_reqcert never
tls_cacertfile /etc/ssl/certs/ca-certificates.crt

# The search scope.
scope sub
    ```

    > **Not**: Eğer LDAP adresi olarak alan adı kullanılacaksa `/etc/hosts` dosyasında da bu adresin tanımlanması gerekmektedir.

4. Değişikliklerin ele alınması için servis `sudo /etc/init.d/nslcd restart` komutuyla yeniden başlatırılır.

    > **İpucu**: Artık LDAP üzerinde tanımlı kullanıcılar `getent passwd` komutu sonucunda listeleniyor olmalıdırlar.
    
5. PAM modülü için LDAP bağlantı bilgileri `/etc/pam_ldap.conf` dosyası üzerinden aşağıdaki özniteliklerle tanımlanmalıdır:

    ```apacheconf
base dc=agem,dc=com,dc=tr
uri ldap://ldap.agem.com.tr
pam_filter objectclass=pardusAccount
    ```
    
6. Kullanıcı girişi sırasında eğer yoksa kullanıcı Ev dizininin yaratılması için `/usr/share/pam-configs/mkhomedir` dosyası aşağıdaki içerikle oluşturulmalıdır:

    ```apacheconf
Name: Create home directory during login
Default: yes
Priority: 900
Session-Type: Additional
Session:
        required        pam_mkhomedir.so umask=0022 skel=/etc/skel
    ```

7. Son adım olarak da `sudo pam-auth-update` komutuyla değişiklikler sisteme yansıtılır.
