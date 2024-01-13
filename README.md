# OpenWebServis-Projesi

## 1. Sanal Makine Oluşturma
### Dağıtım Seçimi
- Rocky 9, Debian 12 veya Ubuntu LTS Sunucu 22.04 dağıtımlarından Ubuntu seçildi.

### Minimum Paket Seçimi
- Dağıtım en az sayıda paketle kurulmuş ve ilgili servisler dışında gereksiz paketler yüklenmemiştir.

## 2. Kullanıcı ve SSH Bağlantısı
### Kullanıcı Oluşturma
- Root dışında `kullanici_adiniz` adında bir kullanıcı oluşturuldu.

### SSH Anahtarlı Bağlantı
- Kullanıcıya kendi bilgisayarımdan SSH ile başarıyla bağlandım. Bağlantı anahtar tabanlı gerçekleştirildi.
    - Makinenin adı superkulanc'dır. Konsol üzerinden "ssh superkulanc@ipadress" komutu ile ssh bağlantısı yapılır.

### Parola ile Doğrulama Kapatma
- SSH servisi için parola ile doğrulama kapatıldı.
    -  Kendi bilgisayarımın konsolundan `ssh-keygen -t rsa -b 2048` komutu ile sanal anahtar tanımlanmıştır.
    -  Oluşturulan sanal anahtar `ssh-copy-id superkullanc@sanalmakineipaddress` komutu ile sanal makineye      kopyalanır.
    -  Parola ile doğrulama olmadan bağlantı sağlanmış olur. 

## 3. Güvenlik Duvarı Ayarları
### Güvenlik Duvarı
- Sadece SSH ve web servisi dışında hiçbir isteği kabul etmeyecek şekilde güvenlik duvarı ayarlandı.
    -  Sanal makine üzerinde "sudo apt install ufw" komutu ile ufw  kurulumu yapılır. (Linux sistemlerdinde kullanılan bir güvenlik duvarı yönetim aracıdır.)
    -  `sudo ufw allow 80` ile HTTP bağlantılarına izin verildi.
    -  `sudo ufw allow 443` ile HTTPS bağlantılarına izin verildi.
    -  `sudo ufw default deny incoming` ile dış dünyaya yönlendirilen trafiğin entlenmesi ve engellenmesi sağlanır.
    - `sudo ufw default allow outgoing` ile sanal makineden dış dünaya çıkan trafik denetlenerek kabul edilir.
    - `sudo ufw enable` ile ufw komutlarının aktif edilerek makine her açıldığında otomatik olarak işlenmesi sağlanmış olur.

## 4. Web Sunucusu Kurulumu
### Web Sunucusu Seçimi
- Apache web sunucusunu tercih ettim ve başarıyla kurulum gerçekleştirildi.
    - `sudo apt update` ile ilk olarak paket listesi güncellendi.
    - `sudo apt install apache2` ile apache web sunucusu kuruldu.
    -  `sudo apt apache2 start` ile apache web sunucusu çalıştırıldı.
    - `sudo systemctl enable apache2` ile sistem tekrar başlatıldığında servisin otomatik olarak başlamsı sağlanmış olur.

## 5. Web Servisi Ayarları
### Alan Adları
- Apache, bugday.org, buğday.org, ozgurstaj2024.com alan adlarına hizmet verecek şekilde konfigüre edildi.
    - `sudo nano /etc/hosts` ile host dosyası açıldı. 
    -  `ipddress localhost` ve `ipaddress super-kulanc-VirtualBox` ile sanal makine ip adresi değiştirildi.
    -  `ipaddress bugday.org, ipaddress www.bugday.org, ipaddress buğday.org, ipaddress www.buğday.org, ipaddress ozgurstaj2024.com, ipaddress www.ozgurstaj2024.com` komutları yazılarak ip adreslerine domainler atandı.
    - Yapılan değişiklikler hosts dosyasına kaydedildi.
  
## 6. bugday.org için WordPress Kurulumu
### WordPress
- `sudo apt update` ile ilk olarak paket listesi güncellendi.
- `sudo apt install php libapache2-mod-php php-mysql` ile PHP modülü, apache ile PHP arasında etkileşimi sağlayan modül ve MySQL veritabanı ile PHP'nin etkileşimini sağlayan modül kurulumu sağlandı.
- `sudo mkdir /var/www/html/bugday.org` ile bugday.org klasörü oluşturuldu.
- `cd /var/www/html/bugday.org` ile oluşturulan klasörün içine gidildi.
-  `sudo wget -c https://wordpress.org/lastest.tar.gz` ile wordpress dosyaları indirildi.
- `sudo tar -xzvf latest.tar.gz` ile tar.gz içindeki wordpres dosyalarını klasör içine çıkarıldı."
- `sudo chown -R www-data:www-data /var/www/html/bugday.org` ve `sudo chown -R 755 www-data:www-data /var/www/html/bugday.org` ile klasör ve alt klsörlerine okuma ve yazma izinleri verildi.

### Veri Tabanı
- Wordpress'te bir yazı yazıp o yazyıa bir dosya yükleyebilmek için veri tabanı kurulumu yapılması gerekmektedir.
    - `sudo mysql -u root` ile mysql veri tabanına giriş yapıldı.
    - `CREATE DATABASE bugday;` ile Database oluşturuldu.
    - `CREATE USER 'bugdayuser'@'localhost' IDENTIFIED '1234-AAaa';` ile kullanıcı ve parola oluşturuldu.
    - `GRANT ALL PRIVILEGES ON bugday* TO 'bugdayuser'@'localhost';` ile oluşturulan kullanıcıya Database'de yetki verildi. 

### Yazı ve Dosya Yükleme
- WordPress'te yeni bir yazı (post) yazıldı ve o yazıya bir dosya yüklendi.
    - Admin Paneline veri tabanında belirtilen kullanıcı adı ve şifre ile giriş yapıldı.
    - New Post kısmına girildi. Yazı yazıldı (ÇİFTÇİ) ve yazıya dosya yüklendi ve paylaşıldı.

### SEO Uyumlu URL
- Yeni yazıya SEO-uyumlu bir URL'den başarıyla ulaşıldı. (ör: https://bugday.org/2024/02/03/ciftci/).
    - New Post kısmında yazı yazılıp yazıya dosya yükleme işlemi yapıldıktan sonra URL otmatik olarak oluşmaktadır.

## 7. ozgurstaj2024.com için Web Sitesi Oluşturma
### Web Sayfası
### WordPress
- sudo apt update` ile ilk olarak paket listesi güncellendi.
- `sudo apt install php libapache2-mod-php php-mysql` ile PHP modülü, apache ile PHP arasında etkileşimi sağlayan modül ve MySQL veritabanı ile PHP'nin etkileşimini sağlayan modül kurulumu sağlandı.
- `sudo mkdir /var/www/html/bugday.org` ile bugday.org klasörü oluşturuldu.
- `cd /var/www/html/ozgurstaj2024.com` ile oluşturulan klasörün içine gidildi.
- `sudo wget -c https://wordpress.org/lastest.tar.gz` ile wordpress dosyaları indirildi.
- `sudo tar -xzvf latest.tar.gz" ile tar.gz` içindeki wordpres dosyalarını klasör içine çıkarıldı."
- `sudo chown -R www-data:www-data /var/www/html/ozgurstaj2024.com` ve `sudo chown -R 755 www-data:www-data /var/www/html/ozgurstaj2024.com` ile klasör ve alt klsörlerine okuma ve yazma izinleri verildi.

### Veri Tabanı
- `sudo mysql -u root` ile mysql veri tabanına giriş yapıldı.
- `CREATE DATABASE ozgurstaj;` ile Database oluşturuldu.
-  ``CREATE USER 'ozgurstajuser'@'localhost'`` IDENTIFIED 'parola'"; ile kullanıcı ve parola oluşturuldu.
-  `GRANT ALL PRIVILEGES ON ozgurstaj* TO 'ozgurstajuser'@'localhost';` ile oluşturulan kullanıcıya Database'de yetki verildi. 

### Yazı
- PHP'de

```php

<?php
for ($i = 1; $i <= 100; $i++) {
    // Kullanıcılarımın kişisel verilerini toplamayacağım.
}
?>
```
ile ekrana 100 kere yazdırıldı.

### Giriş Adresi & Parola Koruması
- `ozgurstaj.com` adresine girildi ve wordpress kurulum ekranında `ad.soyad` user kısmına `parola` password kısmına yazıldı.
- `/var/www/html/ozgurstaj2024.com` ile ozgurstaj2024.com klsörüne gidildi.
- `Wp-login.php` ismi `yonetim.php` ile değiştirildi.
- `yonetim.php` dosyası içine girildi ve tüm "wp-login" referansları "yonetim" olarak değiştirildi ve kaydedildi.
- Kullanıcı oturumunu kapatmak için `wp-login.php` dosyasını kullandığı için `/var/www/html/bugday.org /twentytwo/functions.php` dosyası açıldı.
- Açılan dosyaya 
```php
// Filter & Function to rename the WordPress logout URL
add_filter( 'logout_url', 'my_logout_page', 10, 2 );
function my_logout_page( $logout_url) {
return home_url( '/my-secret-login.php'); // The name of your new login file
}

// Filter & Function to rename Lost Password URL
add_filter( 'lostpassword_url', 'my_lost_password_page', 10, 2 );
function my_lost_password_page( $lostpassword_url ) 
{
return home_url( '/my-secret-login.php?action=lostpassword'); // The name of your new login file
}
```
eklendi ve kullanıcı çıkışı "yonetim.php" bağlandı.
- Admin paneline "ozgurstaj2024.com/yonetim.php/" adresinde ulaşılabilmektedir. ".php" uzantısının kaldırılarak "ozgurstaj2024.com/yonetim" adresinden ulaşabilmek için
kök dizinininde .htaccess dosyasına aşağıdaki komutlar ile kural eklendi.
```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^([^.]+)$ $1.php [NC,L]
```
- Eklenen komutlar çalışmadı. "ozgurstaj2024.com/yonetim.php" ile admin paneline ulaşdı.

### Erişilebilirlik
- hosts dosyası üzerinden "ipaddress www.ozgurstaj2024.com ozgurstaj2024.com" yazılarak iki domainden'de ulaşılması sağlandı.
