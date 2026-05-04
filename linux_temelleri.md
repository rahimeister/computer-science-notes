# LINUX TEMELLERİ

## 1. Giriş 
 Linux, açık kaynaklı, güvenli ve çok kullanıcılı bir işletim sistemidir. Sunucu sistemlerinden siber güvenliğe, yazılım geliştirmeden veri analizine kadar birçok alanda yaygın olarak kullanılmaktadır. Linux’un güçlü yönü, terminal üzerinden kontrol edilebilmesi ve esnek yapısıdır. Bu raporda temel terminal komutları, paket yönetimi, dosya izinleri ve servis yönetimi ele alınmıştır.



## 2. Terminal Komutları 
 Linux sistemlerinde terminal, kullanıcı ile işletim sistemi arasında doğrudan iletişim sağlar. 

 
 ### 2.1 ls(Listeleme)
 Dizin içeriğini listeler 

 - `ls` --> dosyaları gösterir
 - `ls -l` --> detaylı listeleme
 - `ls -a` --> gizli dosyaları göster


 ### 2.3 cd (Dizin Değiştirme)
 Dizinler arasında geçiş yapar 

 - `cd /home` --> belirli dizine gider.
 - `cd ..` --> bir üst dizin 
 - `cd ~` -->  ana dizin 


 ### 2.3 mkdir (Klasör Oluşturma)
 Yeni klasör oluşturur:

 - `mkdir proje`


 ### 2.4 grep (Arama)
 Dosya içinde metin arar.

 - `grep "error" log.txt`


 ### 2.5 chmod (Sistem İzleme)
 Dosya izinleribni düzenler 

 - `chmod 755 script.sh`


 ### 2.6 top (Sistem)
 Çalışan süreçleri ve kaynak kullanımını gösterir. 

 - `top` 




## 3. Paket Yönetimi 
 Linux’te yazılımlar paket yöneticileri ile kurulur, güncellenir ve kaldırılır. Dağıtıma göre farklı araçlar kullanılır:

 ### 3.1 APT (Debian/Ubuntu)

 - `sudo apt update`
 - `sudo apt install nginx`


 ### 3.2 DNF (Fedora)

 - `sudo dnf install nginx`


 ### 3.3 Pacman (Arch Linux)

 - `sudo pacman -S nginx`

 **Önemi**: Yazılımların güvenli ve düzenli şekilde yönetilmesini sağlar.



## 4. Dosya İzinleri 
 Linux’te her dosyanın erişim izinleri vardır. Bu izinler güvenliği sağlar.

 **İzin Türleri:**

 - r --> okuma
 - w --> yazma
 - x --> çalıştırma

 **Kullanıcı Grupları:**

 - Owner(sahip) 
 - Group (grup)
 - Others (diğerleri)

 **Örnek:**
     `chmod 755 file.sh`
 
 - 7 --> tam yetki(owner)
 - 5 --> okuma + çalıştırma(group/others)

 **Önemi:** Sistem güvenliği için kritik rol oynar.




# 5. Servis Yönetimi (systemctl)
 Linux sistemlerinde servisler arka planda çalışan süreçlerdir. systemctl komutu ile yönetilir.

 **Temel Komutlar:**

 - `sudo systemctl start nginx`  --> Nginx servisinin başlatılmasını sağlar.
 - `sudo systemctl stop nginx`  --> Nginx servisinin durdurulmasını sağlar.
 - `sudo systemctl restart nginx`  --> Nginx’i yeniden başlatır.
 - `sudo systemctl status nginx `  --> Nginx servisinin durumunu gösterir.

 **Servis açılışta başlatma:**

 - `sudo systemctl enable nginx`

 **Önemi:** Sunucu servislerini kontrol etmek için kullanılır.




# 6. Kazanç ve Sonuç 
 Linux temel kavramlarını öğrenmek, yazılım geliştirme ve sistem yönetimi açısından büyük avantaj sağlar. Terminal komutları sistemle hızlı etkileşim sağlar, paket yöneticileri yazılım kurulumunu kolaylaştırır, dosya izinleri güvenliği artırır ve systemctl servis kontrolünü mümkün kılar.

 Bu bilgiler, özellikle yazılım geliştiriciler, siber güvenlik uzmanları ve sistem yöneticileri için temel altyapı oluşturur.


 