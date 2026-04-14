# İŞLETİM SİSTEMİ TEMELLERİ (OS Basics)

## Kernel Nedir? 
**Kernel (çekirdek)**, bir işletim sisteminin en temel ve en kritik parçasıdır. Bilgisayarın donanımı ile yazılımlar(uygulamalar) arasında köprü görevi görür.

Basitçe söylemek gerekirse 
Kernel = bilgisayarın **yöneticisi** 

### Kernel Ne İşe Yarar? 
Kernel sistemde her şeyi kontrol eder:
- **İşlem yönetimi**: Hangi program ne zaman çalışacak karar verir
- **Bellek yönetimi**: RAM’in hangi programa ne kadar verileceğini ayarlar
- **Donanım kontrolü**: Klavye, fare disk gibi cihazlarla iletişimi sağlar 
- **Dosya sistemi yönetimi**: Dosyaların nasıl okunup yazılacağını kontrol eder.




## Süreç(process) - iş parçacığı(thread) farkı 

**Süreç (process)** ve **iş parçacığı (thread)**, işletim sistemlerinde (özellikle Linux gibi sistemlerde) en temel kavramlardan ikisidir. Aralarındaki fark şu şekildedir:

### Süreç (Process) Nedir?
Bir programın çalışır halidir.

Örnek:

- Goodle Chrome açtığında → bir süreç oluşur
- Visual Studio Code açtığında → başka bir süreç oluşur

Her süreç:

- Kendi bellek alanına sahiptir
- Diğer süreçlerden izole (bağımsız) çalışır

### İş Parçaçcığı (Thread) Nedir?
Bir sürecin içinde çalışan küçük görev birimleridir.
Yani:
- **Thread** = sürecin alt görev birimleri 

Örnek:
- Chrome içinde:
    - Bir thread sayfayı yükler
    - Bir videoyu oynatır 
    - Bir kullanıcı girişini dinler


### Temel Farklar 
- Süreçler bağımsızdır, thread'ler aynı sürece bağlıdır
- Süreçler ayrı bellek kullanır, thread'ler ortak bellek kullanır
- Süreçler daha yavaştır, thread'ler daha hızlıdır
- Süreçler arası iletişim zordur, thread'ler arası iletişim kolaydır
- Bir süreç çökerse diğerleri etkilenmez, thread çökerse tüm süreç etkilenebilir



## Bellek Yönetimi Nedir ve Nasıl Yapılır?
Bellek yönetimi, işletim sisteminin (özellikle Linux gibi sistemlerde) en önemli görevlerinden biridir. Amaç, sistemdeki **RAM (ana bellek)** kullanımını düzenlemek ve çalışan programlara gerekli belleği verimli şekilde dağıtmaktır.

### Bellek Yönetimi Nasıl Yapılır?
İşletim sistemi bellek yönetimini şu yöntemlerle gerçekleştirir:

- Bellek Tahsisi (Allocation):
  Çalıştırılan her programa ihtiyaç duyduğu kadar bellek alanı ayrılır.

- Bellek Geri Alma (Deallocation): 
  Program kapandığında veya işi bittiğinde kullandığı bellek geri alınır ve başka programlara verilir.

- Sanal Bellek (Virtual Memory):
  RAM yetersiz kaldığında, disk alanı geçici bellek gibi kullanılır. Böylece daha fazla program çalıştırılabilir.

- Sayfalama (Paging):
  Bellek, küçük parçalara (sayfalara) bölünerek daha düzenli ve verimli kullanım sağlanır.

- Segmentasyon (Segmentation):
  Bellek, programın mantıksal bölümlerine göre (kod, veri vb.) ayrılır.

- Koruma (Memory Protection):
  Bir programın başka bir programın belleğine izinsiz erişmesi engellenir.




## CPU Zamanlayıcıları Nedir?
CPU zamanlayıcıları, işletim sisteminin (örneğin Linux) işlemciyi hangi sürece ne zaman vereceğini belirleyen mekanizmalardır. Amaç, CPU’nun en verimli şekilde kullanılmasını sağlamak ve sistemdeki tüm süreçlerin adil bir şekilde çalışmasını düzenlemektir.

### CPU Zamanlayıcıları Nasıl Çalışır?
İşletim sistemi, çalışmayı bekleyen süreçler arasından belirli kriterlere göre seçim yapar ve CPU’yu ilgili sürece tahsis eder. Bu seçim işlemi belirli algoritmalara göre yapılır. 

### Yaygın CPU Zamanlama Algoritmaları
- FCFS (First Come First Served):
  İlk gelen süreç ilk çalıştırılır.

- SJF (Shortest Job First):
  En kısa sürede tamamlanacak süreç önce çalıştırılır.

- Round Robin:
  Her sürece belirli bir zaman dilimi (quantum) verilir ve sırayla çalıştırılır.

- Priority Scheduling:
  Önceliği yüksek olan süreçler önce çalıştırılır.

- Multilevel Queue:
  Süreçler farklı kuyruklara ayrılır ve her kuyruk farklı şekilde yönetilir.


### CPU Zamanlayıcılarının Amaçları
- CPU kullanımını artırmak
- Bekleme süresini azaltmak
- Adil bir sistem sağlamak
- Tepki süresini iyileştirmek

