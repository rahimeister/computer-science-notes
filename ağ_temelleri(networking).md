# AĞ TEMELLERİ (NETWORKING)

## 1. IP (Internet Protocol)

 Internet Protocol (IP), internete bağlı cihazların birbirini tanıyabilmesi ve veri alışverişi yapabilmesi için kullanılan bir adresleme sistemidir. Her cihazın kendine özgü bir IP adresi bulunmaktadır.

 ### 1.1 IP Adresinin Görevleri
  * Ağ üzerindeki cihazların tanımlanmasını sağlar
  * Veri paketlerinin doğru hedefe ulaşmasını sağlar
  * Cihazlar arası iletişimi mümkün kılar

 ### 1.2 IP Adresi Türleri
  * IPv4: Sayısal formattadır (örnek: 192.168.1.1) ve en yaygın kullanılan sürümdür
  * IPv6: Daha geniş adres kapasitesine sahiptir (örnek: 2001:0db8:85a3::8a2e:0370:7334)

 ### 1.3 IP Adresi Öğrenme Yöntemleri
  * Tarayıcı üzerinden:
  * Arama motoruna “IP adresim nedir” yazılarak genel (public) IP adresi öğrenilebilir.

  Windows işletim sistemi üzerinden:

    ipconfig

  Bu komut sonucunda “IPv4 Address” kısmı yerel IP adresini gösterir.

  Mobil cihazlarda:
    Ayarlar → Wi-Fi → Bağlı olunan ağ üzerinden IP bilgisi görüntülenebilir.

 ### 1.4 Güvenlik Açısından IP

 IP adresleri tek başına bir sistemi ele geçirmek için yeterli değildir. Ancak aşağıdaki amaçlarla kullanılabilir:

  * Yaklaşık konum tespiti
  * Sunuculara istek gönderme
  * Ağ güvenliği testlerinde hedef belirleme



## 2. Port Kavramı

 Port, bir cihaz üzerinde çalışan servis veya uygulamaların birbirinden ayrılmasını sağlayan sayısal adreslerdir. IP adresi cihazı tanımlarken, port numarası o cihazdaki servisi belirler.

 ### 2.1 Port Mantığı
  * IP adresi: Cihazın adresi
  * Port: Cihaz üzerindeki uygulama
   
   Örnek kullanım:

      192.168.1.10:80

  Bu ifade, ilgili cihaz üzerindeki web sunucusuna erişimi temsil eder.

 ### 2.2 Yaygın Portlar
  80 → HTTP
  443 → HTTPS 
  21 → FTP
  22 → SSH
  3306 → MySQL

 ### 2.3 Port Türleri
  * Açık Port: Dış bağlantılara açık
  * Kapalı Port: Erişime kapalı
  * Filtreli Port: Güvenlik duvarı tarafından engellenmiş

 ### 2.4 Güvenlik Açısından Portlar

 Açık portlar, potansiyel saldırı yüzeyi oluşturur. Ancak bir portun açık olması tek başına güvenlik açığı olduğu anlamına gelmez.

 ### 2.5 Port Tarama (Nmap Örnekleri)

  * Temel tarama:
    ```bash
    nmap 192.168.1.1
    ```
  * Tüm portların taranması:
    ```bash
    nmap -p- 192.168.1.1
    ```
  * Servis ve versiyon bilgisi:
    ```bash
    nmap -sV 192.168.1.1
    ```
  * Hızlı ve gelişmiş tarama:
    ```bash
    nmap -sS -sV -T4 192.168.1.1
    ```


## 3. DNS (Domain Name System)

 Domain Name System (DNS), alan adlarını IP adreslerine çeviren bir sistemdir. Kullanıcıların kolay hatırlanabilir alan adları ile internet kaynaklarına erişmesini sağlar.

 ### 3.1 Çalışma Mantığı

  Bir kullanıcı bir web sitesine erişmek istediğinde:

   1. Alan adı (örneğin: google.com) girilir
   2. DNS sunucusuna sorgu gönderilir
   3. İlgili IP adresi elde edilir
   4. Bağlantı IP adresi üzerinden gerçekleştirilir
 
 ### 3.2 DNS Kayıt Türleri
  **A Kaydı**: Alan adını IPv4 adresine çevirir
  **AAAA Kaydı**: Alan adını IPv6 adresine çevirir
  **MX Kaydı**: E-posta sunucularını belirtir
  **CNAME**: Bir alan adını başka bir alan adına yönlendirir
 
 ### 3.3 DNS Sorgulama

  Windows:
   ```bash
   nslookup google.com
   ```
  Linux / Mac:
   ```bash
   dig google.com
   ```
 ### 3.4 DNS’in Güvenlik Açısından Önemi

 DNS, internet altyapısının temel bileşenlerinden biri olmasına rağmen çeşitli güvenlik riskleri barındırır. Saldırganlar DNS mekanizmasını kullanarak bilgi toplayabilir veya kullanıcıları zararlı sistemlere yönlendirebilir.

  Başlıca Güvenlik Riskleri
   **DNS Enumeration:** Alt alan adlarının keşfedilmesiyle gizli servislerin ortaya çıkarılması
   **DNS Spoofing:** Kullanıcıların sahte IP adreslerine yönlendirilmesi
   **DNS Misconfiguration:** Hatalı yapılandırmalar sonucu yetkisiz erişim
   **DNS Amplification:** DDoS saldırılarında kullanılması



## 4. TCP (Transmission Control Protocol)
 Transmission Control Protocol (TCP), ağlar arasında güvenilir veri iletimi sağlayan bağlantı odaklı bir taşıma katmanı protokolüdür. Veri gönderimi öncesinde gönderici ve alıcı arasında bir bağlantı kurulur ve iletim bu bağlantı üzerinden gerçekleştirilir.

 ### 4.1 TCP’nin Çalışma Mantığı
  TCP, veri iletimine başlamadan önce üç aşamalı bir bağlantı süreci (3-way handshake) gerçekleştirir:
 
 1. İstemci bağlantı isteği gönderir (SYN)
 2. Sunucu bu isteğe yanıt verir (SYN-ACK)
 3. İstemci bağlantıyı onaylar (ACK)
 
 Bu süreç sonrasında veri iletimi başlar.

 ### 4.2 TCP’nin Özellikleri
 * Bağlantı odaklıdır
 * Güvenilir veri iletimi sağlar
 * Veri kaybı durumunda yeniden gönderim yapar
 * Paketlerin sıralı iletilmesini garanti eder
 * Hata kontrol mekanizmasına sahiptir

 ### 4.3 TCP Kullanım Alanları
 * Web servisleri (HTTP, HTTPS)
 * E-posta protokolleri (SMTP, IMAP)
 * Dosya transferi (FTP)
 
 ### 4.4 Güvenlik Açısından TCP
 TCP protokolü aşağıdaki saldırı türlerine karşı hassas olabilir:
   * SYN Flood saldırısı: Çok sayıda sahte bağlantı isteği gönderilerek sunucunun  kaynaklarının tüketilmesi
   * Session Hijacking: Aktif bir oturumun ele geçirilmesi
   * Port tarama: Açık servislerin tespit edilmesi



## 5. UDP (User Datagram Protocol)
 User Datagram Protocol (UDP), bağlantısız veri iletimi sağlayan bir taşıma katmanı protokolüdür. Veri gönderimi sırasında herhangi bir bağlantı kurulmaz ve paketler doğrudan hedefe iletilir.

 ### 5.1 UDP’nin Çalışma Mantığı
 UDP’de veri gönderimi için önceden bağlantı kurulmasına gerek yoktur. Gönderilen veri paketleri (datagramlar), hedefe doğrudan iletilir ve alıcı tarafından kontrol edilmeden kabul edilir.

 ### 5.2 UDP’nin Özellikleri
 * Bağlantısızdır
 * Düşük gecikme süresi sağlar
 * Hızlı veri iletimi sunar
 * Paket kaybı kontrolü yoktur
 * Veri sıralaması garanti edilmez

 ### 5.3 UDP Kullanım Alanları
 * DNS sorguları
 * Video ve ses akışı (streaming)
 * Online oyunlar
 * VoIP uygulamaları

 ### 5.4 Güvenlik Açısından UDP
 UDP protokolü aşağıdaki güvenlik risklerini barındırabilir:
 
   * UDP Flood saldırısı: Yoğun trafik ile sistem kaynaklarının tüketilmesi
   *  Amplification saldırıları: Küçük isteklerle büyük yanıtlar elde edilerek DDoS yapılması
   * Spoofing: Kaynak IP adresinin sahte olarak gösterilmesi


 ### 6. TCP ve UDP Karşılaştırması

 TCP ve UDP protokolleri, veri iletiminde farklı yaklaşımlar benimsemektedir. Aşağıdaki tabloda bu iki protokolün temel farkları karşılaştırmalı olarak gösterilmiştir:

 | Özellik             | TCP (Transmission Control Protocol)        | UDP (User Datagram Protocol)       |
 |---------------------|------------------------------------------- |------------------------------------|
 | Bağlantı Türü       | Bağlantı odaklı                            | Bağlantısız                        |
 | Güvenilirlik        | Yüksek (veri doğrulama ve yeniden gönderim)| Düşük (garanti yok)                | 
 | Veri Sıralaması     | Garantili                                  | Garantili değil                    |
 | Hata Kontrolü       | Var                                        | Yok                                |
 | Hız                 | Daha yavaş                                 | Daha hızlı                         |
 | Başlık (Header)     | Daha büyük                                 | Daha küçük                         |
 | Kullanım Amacı      | Kritik veri iletimi                        | Gerçek zamanlı veri iletimi        |
 | Örnek Kullanım      | HTTP, HTTPS, FTP, SMTP                     | DNS, VoIP, video akışı, oyunlar    |

  **TCP güvenilir veri iletimi gerektiren uygulamalarda tercih edilirken, UDP düşük gecikme süresi gerektiren gerçek zamanlı uygulamalarda tercih edilmektedir.**





## 7. Paket Yapısı ve Çalışma Mantığı 
 Ağ iletişiminde veriler, tek parça halinde gönderilmez. Bunun yerine daha küçük parçalara bölünerek paket (packet) adı verilen yapılara dönüştürülür. Bu yöntem, veri iletimini daha verimli ve yönetilebilir hale getirir.

 ### 7.1 Paket Nedir?
  Paket, bir ağ üzerinden iletilen veri birimidir. Her paket, sadece veriyi değil aynı zamanda iletim için gerekli kontrol bilgilerini de içerir.
 
  Bir paket genel olarak üç temel bölümden oluşur:
 
   **Header (Başlık):** Kaynak ve hedef IP adresi gibi yönlendirme bilgilerini içerir
   **Payload (Veri):** Taşınan asıl veridir
   **Footer (Sonlandırıcı):** Hata kontrolü için kullanılan bilgiler içerir (her protokolde bulunmayabilir)

 ### 7.2 Paketlerin İletim Süreci
  Veri gönderimi sırasında aşağıdaki adımlar gerçekleşir:
 
   1. Gönderilmek istenen veri küçük parçalara bölünür
   2. Her parçaya bir başlık (header) eklenir
   3. Paketler ağ üzerinden hedefe gönderilir
   4. Paketler hedefte yeniden birleştirilir
   5. Bu süreç sayesinde veri kaybı durumunda sadece eksik paketler tekrar gönderilir.

 ### 7.3 Paketlerin Ağ Üzerinde Taşınması
 Paketler ağ üzerinde farklı yollar (routing) kullanarak hedefe ulaşabilir. Bu nedenle:
 
  * Paketler farklı zamanlarda ulaşabilir
  * Sıraları karışabilir
  * Bazı paketler kaybolabilir
  * Bu durum özellikle TCP ve UDP protokolleri arasındaki farkı ortaya çıkarır.

 ### 7.4 Güvenlik Açısından Paket Yapısı
 Paket yapısı, ağ güvenliği açısından önemli riskler barındırabilir:
 
  **Packet Sniffing:** Ağ trafiğinin dinlenmesi
  **Packet Injection:**  Ağa sahte paket eklenmesi
  **Man-in-the-Middle (MITM):** İletişimin ortasında veri manipülasyonu

 Bu tür saldırıları önlemek için şifreleme (encryption) ve güvenlik protokolleri kullanılmaktadır.



## 8. OSI Modeli ve Katmanlı Yapı 
 OSI (Open Systems Interconnection) modeli, ağ iletişimini anlamak ve standartlaştırmak amacıyla oluşturulmuş 7 katmanlı bir referans modelidir. Bu model, veri iletiminin her aşamasını katmanlara ayırarak daha düzenli ve anlaşılır hale getirir.

 ### 8.1 OSI Katmanları
 OSI modeli aşağıdaki 7 katmandan oluşur:

| Katman No | Katman Adı                   | Görevi                                                       |
|-----------|------------------------------|--------------------------------------------------------------|
| 7         | Uygulama (Application)       | Kullanıcıya en yakın katmandır, uygulamalarla iletişim kurar |
| 6         | Sunum (Presentation)         | Veri formatlama ve şifreleme işlemlerini gerçekleştirir      |
| 5         | Oturum (Session)             | İletişim oturumlarını yönetir                                |
| 4         | Taşıma (Transport)           | Veri iletimini sağlar (TCP / UDP)                            |
| 3         | Ağ (Network)                 | IP adresleme ve yönlendirme işlemlerini gerçekleştirir       |
| 2         | Veri Bağlantı (Data Link)    | MAC adresleme ve hata kontrolü sağlar                        |
| 1         | Fiziksel (Physical)          | Verinin fiziksel ortamda iletilmesini sağlar                 |


 ### 8.2 Veri İletim Süreci (Encapsulation)
 OSI modelinde veri gönderilirken her katmanda ek bilgiler eklenir. Bu sürece encapsulation (kapsülleme) denir.
 
 Veri aşağıdaki sırayla ilerler:
 
  * Uygulama katmanında veri oluşturulur 
  * Her katmanda veriye yeni başlıklar (header) eklenir
  * Fiziksel katmanda veri bitler halinde iletilir
  * Alıcı tarafta ise bu işlem tersine çevrilir (decapsulation).


 ### 8.3 Katmanlara Göre Veri Birimleri

| Katman       | Veri Birimi                          |
|--------------|--------------------------------------|
| Application  | Data                                 |
| Transport    | Segment (TCP) / Datagram (UDP)       |
| Network      | Packet                               |
| Data Link    | Frame                                |
| Physical     | Bit                                  |
 
 

 ### 8.4 Güvenlik Açısından OSI Modeli
 Her katman farklı güvenlik riskleri barındırır:
 
   **Application Layer:** XSS, SQL Injection
   **Transport Layer:** Port tarama, SYN Flood
   **Network Layer:** IP Spoofing
   **Data Link Layer:** MAC spoofing
 
 Bu nedenle ağ güvenliği, tüm katmanları kapsayacak şekilde ele alınmalıdır.



## 9. Ping Komutu 
 Ping, bir cihazın başka bir cihaza ağ üzerinden ulaşıp ulaşamadığını test etmek için kullanılan temel bir ağ aracıdır. Bu komut, hedefe ICMP (Internet Control Message Protocol) paketleri göndererek yanıt almayı amaçlar.
 
 ### 9.1 Çalışma Mantığı 
 Ping komutu, hedef cihaza “echo request” paketi gönderir. Hedef cihaz bu pakete “echo reply” ile yanıt verirse bağlantının aktif olduğu anlaşılır.

  ```bash
  ping google.com
  ```
 
 ### 9.3 Sağladığı Bilgiler
  * Bağlantı durumu (erişim var/yok)
  * Gecikme süresi (latency)
  * Paket kaybı oranı
 
 ### 9.4 Kullanım Amacı
 * Ağ bağlantısını test etmek
 * Sunucunun aktif olup olmadığını kontrol etmek

## 10.Traceroute Komutu
 Traceroute, bir paketin hedefe ulaşana kadar geçtiği ağ cihazlarını (router’ları) gösteren bir ağ analiz aracıdır.

 ### 10.1 Çalışma Mantığı
 Traceroute, paketlerin yaşam süresi (TTL - Time To Live) değerini artırarak her adımda hangi yönlendiriciden geçtiğini tespit eder.

 ### 10.2 Kullanım Örneği
 Windows:
  ```bash 
  tracert google.com
  ```
 Linux / Mac:
  ```bash
  traceroute google.com
  ```

 ### 10.3 Sağladığı Bilgiler
 * Paketin geçtiği yönlendiriciler
 * Her adım için gecikme süreleri
 * Ağdaki darboğaz noktaları
 
 ### 10.4 Kullanım Amacı
 * Ağ yolunu analiz etmek
 * Bağlantı problemlerinin nerede oluştuğunu tespit etmek



## Nslookup Komutu 
 Nslookup, DNS sorguları yapmak ve alan adlarının IP adreslerini öğrenmek için kullanılan bir araçtır.

 ### 11.1 Çalışma Mantığı 
 Nslookup, DNS sunucusuna sorgu göndererek alan adının karşılık geldiği IP adresini veya DNS kayıtlarını elde eder.

 ### 11.2 Kullanım Örneği 
  ```bash
  nslookup google.com
  ``` 

 ### 11.3 Sağladığı Bilgiler
 * Alan adının IP adresi
 * DNS sunucu bilgileri
 * DNS kayıtları

 ### 11.4 Kullanım Amacı 
 * DNS sorunlarını analiz etmek 
 * Alan Adı çözümleme işlemlerini kontrol etmek 



## 12. Sonuç 
 Bu raporda, ağ temellerinin en önemli bileşenleri olan IP, port, DNS, TCP, UDP, paket yapısı ve OSI modeli detaylı bir şekilde incelenmiştir. Bu kavramlar, bilgisayar ağlarının nasıl çalıştığını anlamak için temel yapı taşlarını oluşturmaktadır.

 IP adresleme sistemi sayesinde cihazlar ağ üzerinde tanımlanabilirken, port yapısı ile bu cihazlar üzerindeki servisler ayrıştırılmaktadır. DNS sistemi ise kullanıcı dostu alan adlarının IP adreslerine çevrilmesini sağlayarak internet kullanımını kolaylaştırmaktadır.

 TCP ve UDP protokolleri, veri iletiminde farklı yaklaşımlar sunarak çeşitli ihtiyaçlara çözüm üretmektedir. TCP güvenilir veri iletimi sağlarken, UDP daha hızlı ancak daha az güvenilir bir iletişim sunmaktadır. Paket yapısı ve OSI modeli ise verinin ağ üzerinde nasıl iletildiğini katmanlı bir yapı ile açıklamaktadır.

 Ayrıca ping, traceroute ve nslookup gibi temel ağ araçları, ağ bağlantılarının test edilmesi ve analiz edilmesi açısından önemli rol oynamaktadır.

 Sonuç olarak, bu kavramların anlaşılması yalnızca ağların çalışma prensibini kavramak için değil, aynı zamanda ağ güvenliği açısından da büyük önem taşımaktadır. Doğru yapılandırılmayan sistemler çeşitli güvenlik açıklarına neden olabileceğinden, bu temel bilgilerin bilinmesi kritik bir gerekliliktir.



## 13. Kaynakça

- Kurose, J. F., & Ross, K. W. (2021). *Computer Networking: A Top-Down Approach*. Pearson.

- Tanenbaum, A. S., & Wetherall, D. J. (2011). *Computer Networks*. Pearson.

- RFC 791 – Internet Protocol. (1981).  
  https://www.rfc-editor.org/rfc/rfc791

- RFC 793 – Transmission Control Protocol. (1981).  
  https://www.rfc-editor.org/rfc/rfc793

- RFC 768 – User Datagram Protocol. (1980).  
  https://www.rfc-editor.org/rfc/rfc768

- Microsoft Docs. (2024). *Networking Commands (ping, tracert, nslookup)*  
  https://learn.microsoft.com/

- Cloudflare Learning Center. (2024). *What is DNS?*  
  https://www.cloudflare.com/learning/dns/what-is-dns/