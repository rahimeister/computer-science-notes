# DOSYA SİSTEMLERİ VE DEPOLAMA MANTIĞI 

## 1. Giriş
 Bilgisayar sistemlerinde veri depolama ve erişim performansı, hem kullanılan donanıma (HDD/SSD) hem de dosya sistemine bağlıdır. Bu raporda NTFS, ext4 ve APFS dosya sistemleri incelenmekte; ayrıca blok yapısı kavramı ve HDD ile SSD’nin çalışma prensipleri açıklanmaktadır. Amaç, veri okuma/yazma hızlarının arkasındaki temel mekanizmaları anlamaktır.


## 2. Dosya Sistemleri

### 2.1 NTFS (New Technology File System)
 Microsoft tarafından geliştirilen NTFS, Windows işletim sistemlerinden varsayılan dosya sistemidir.

 **Temel Özellikler:** 
 - Journaling (günlükleme )
 - Dosya izinleri (ACL)
 - Büyük dosya desteği 
 - Şifreleme ve sıkıştırma


 **Avantajlar:** 
 - Yüksek güvenlik 
 - Stabil yapı 
 - Windows ile tam uyum 


 **Dezavantajları:** 
 - Diğer işletim sistemelerinde sınırlı destek 


---

### 2.2 ext4 (Fourth Extended File System)
 Linux sistemlerinde yaygın olarak kullanılan açık kaynaklı bir dosya sistemidir.

 **Temel Özellikler:** 
 - Journaling destekleme 
 - Yüksek performans
 - Düşük gecikme
 - Gelişmiş inode yapısı 

 **Avantajlar:** 
 - Hızlı ve stabil 
 - Açık kaynak 
 - Büyük veri desteği 

 **Dezavantajlar:** 
 - Windows ile uyumsuzluk 


 ---

### 2.3 APFS (Apple File System)
 Apple tarafından geliştirilen modern bir dosya sistemidir.

 **Temel Özellikler:**
 - SSD optimizasyonu 
 - Snapshot desteği 
 - Copy-on-write (COW)
 - Güçlü şifreleme

 **Avantajlar:**
 - Çok yüksek hız (SSD üzerinde) 
 - Veri güvenliği 
 - Verimli alan kullanımı 

 **Dezavantajları:**


 ---


## 3. Blok Yapısı Nedir? 
 Blok yapısı, disk üzerinde verilerin saklandığı en küçük adreslenebilir birimdir.


### 3.1 Temel Kavramlar  
 - Diskler belirli boyutlarda bloklara ayrılır (örneğin 4 KB)
 - Her dosya bu bloklar üzerinde saklanır 
 - Bir dosya birden fazla blok kullanabilir

### 3.2 Neden Önemlidir?
 - Küçük blok --> daha az alan israfı ama daha fazla yönetim maliyeti 
 - Büyük blok --> daha hızlı erişim ama boş alan kaybı 

### 3.3 Fragmentation (Parçalama)
 - Dosyanın farklı bloklara dağılmasıdır
 - HDD'de performansı ciddi düşürür 
 - SDD'de etkisi daha azdır


---

## 4. HDD (Hard Disk Drive) Çalışma Prensibi 

### 4.1 Yapısı 
 - Manyetik döner diskler (platter)
 - Okuma/yazma kafası 
 - Motor

### 4.2 Çalışma Mantığı 
 - Disk döner
 - Kafa doğru konuma gider 
 - Veri manyetik olarak okunur/yazılır

### 4.3 Performansı Etkileyen Faktörler 
 - Seek Time (kafanın konumlanma süresi)
 - Rotational Latency (diskin dönme süresi)
 - Fragmentation 


 ---

## 5. SSD (Solid State Drive) Çalışma Prensibi 

### 5.1 Yapısı 
 - MAND flash bellek
 - Hareketli parça yok 
 - Kontrolcü (controller)

### 5.2 Çalışma Mantığı 
 - Veri elektriksel olarak saklanır 
 - Doğrudan erişim (random access)

### 5.3 Özellikler 
- Çok düşük gecikme 
- Yüksek okuma/yazma hızı
- Sessiz ve dayanıklı 


---

## 6. HDD ve SSD Karşılaştırılması 

| Özellik        | HDD                  | SSD                  |
|----------------|----------------------|----------------------|
| Hız            | Düşük                | Çok yüksek           |
| Gecikme        | Yüksek               | Çok düşük            |
| Dayanıklılık   | Düşük (mekanik)      | Yüksek               |
| Fiyat          | Ucuz                 | Daha pahalı          |
| Gürültü        | Var                  | Yok                  |


---

## 7. Dosya Sistemleri ile Depolama İlişkisi
 Dosya sistemi ve donanım birlikte performansı belirler:

 - NTFS + HDD --> orta performans
 - ext4 + SSD --> yüksek performans
 - APFS + SSD --> maksimum performans 


### 7.1 Hız Nereden Geliyor?

 Veri hızını etkileyen temel faktörler:

 1. **Donanım Türü** 
    - SSD --> hızlı erişim 
    - HDD --> mekanik gecikme

 2. **Dosya Sistemi Yapısı**
    - Journaling --> güvenlik ama ek işlem 
    - COW --> veri güvenliği + hızlı snapshot 

 3. **Blok Yönetimi** 
    - Verimli blok dağılımı --> hızlı erişim 
    - Fragmentation --> yavaşlama 


---

## 8. Kazanım (Öğrenme Çıktısı)

 Bu konuların anlaşılmasıyla:
 - Veri okuma/yazma hızlarının neden değiştiği anlaşılır
 - SSD’nin neden daha hızlı olduğu kavranır
 - Dosya sistemlerinin performansa etkisi öğrenilir
 - Sistem optimizasyonu yapılabilir

 
---

## 9. Sonuç 
 Dosya sistemleri ve depolama teknolojileri, bilgisayar performansının temel bileşenleridir. NTFS, ext4 ve APFS farklı ihtiyaçlara yönelik optimize edilmiştir. HDD ve SSD arasındaki farklar ise fiziksel yapıdan kaynaklanmaktadır. En yüksek performans, doğru dosya sistemi ile uygun donanımın birlikte kullanılmasıyla elde edilir.


---

## 10. Kaynakça
 - Tanenbaum, A. S. – Modern Operating Systems
 - Stallings, W. – Operating Systems
 - Microsoft NTFS Documentation
 - Linux Kernel Documentation (ext4)
 - Apple Developer Documentation (APFS)



 
