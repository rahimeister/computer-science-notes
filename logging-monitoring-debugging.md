# Logging, Monitoring & Debugging
## Uygulamalarda Loglama, İzleme ve Hata Ayıklama

## 1. Giriş

Yazılım uygulamalarının geliştirilmesi kadar, uygulamalar canlı ortama alındıktan sonra sağlıklı bir şekilde çalışmasının sağlanması da önemlidir. Uygulamalar çalışırken beklenmeyen hatalar, performans problemleri, veritabanı bağlantı sorunları ve sistem kesintileri meydana gelebilir.

Bu tür durumların tespit edilmesi, nedenlerinin araştırılması ve çözüme kavuşturulması için Logging, Monitoring, Debugging ve Exception Handling kavramlarından yararlanılır.

Logging, uygulama içerisinde gerçekleşen olayların kayıt altına alınmasını sağlar. Monitoring, uygulamanın ve sistem kaynaklarının çalışma durumunu takip eder. Debugging, meydana gelen hataların nedenlerini araştırarak çözüm geliştirmeye yardımcı olur. Exception Handling ise program çalışırken ortaya çıkan beklenmeyen durumların kontrollü şekilde yönetilmesini sağlar.

Bu dokümanda söz konusu kavramların temel çalışma mantığı, Python üzerinden uygulama örnekleri ve yazılım geliştirme ile siber güvenlik süreçlerindeki önemleri ele alınmaktadır.

---

## 2. Logging (Loglama)

### 2.1. Loglama Nedir?

Logging, bir yazılım uygulaması çalışırken meydana gelen olayların belirli bilgilerle birlikte kayıt altına alınması işlemidir.

Bir uygulama içerisinde kullanıcı girişi, veri kaydetme, veritabanı bağlantısı, dosya işlemleri veya hata oluşumu gibi birçok olay gerçekleşir. Bu olayların kayıt altına alınması, uygulamanın geçmişte nasıl çalıştığının incelenmesini sağlar.

Örneğin bir kullanıcı web uygulamasına giriş yapmaya çalıştığında aşağıdaki gibi kayıtlar oluşturulabilir:

```text
2026-11-26 10:15:20 INFO Kullanıcı giriş yaptı.
2026-11-26 10:16:05 WARNING Başarısız giriş denemesi.
2026-11-26 10:17:12 ERROR Veritabanı bağlantısı başarısız.
```

Bu kayıtlar incelendiğinde uygulamada hangi olayların gerçekleştiği ve hangi işlemlerin başarısız olduğu anlaşılabilir.

Log kayıtları genellikle konsola, dosyalara veya merkezi log yönetim sistemlerine aktarılabilir.

### 2.2. Log Seviyeleri

Her olay aynı önem derecesine sahip değildir. Bu nedenle log kayıtları önem seviyelerine göre sınıflandırılır.

Python'un standart `logging` kütüphanesinde beş temel log seviyesi bulunmaktadır.

| Seviye | Açıklama | Örnek |
|---|---|---|
| DEBUG | Ayrıntılı hata ayıklama bilgileri | Değişken değerinin kontrol edilmesi |
| INFO | Normal çalışma hakkında bilgilendirme | Uygulamanın başlatılması |
| WARNING | Dikkat edilmesi gereken durumlar | Disk alanının azalması |
| ERROR | Bir işlemin başarısız olması | Veritabanına veri kaydedilememesi |
| CRITICAL | Sistemin çalışmasını ciddi şekilde etkileyen durumlar | Temel sistem bileşeninin çalışamaz hale gelmesi |

**DEBUG:** Uygulamanın çalışma sürecini ayrıntılı incelemek için kullanılır. Özellikle geliştirme aşamasında faydalıdır.

**INFO:** Uygulamanın normal çalışma sürecindeki önemli olayları kaydeder.

**WARNING:** Henüz ciddi bir hataya dönüşmemiş ancak ileride sorun oluşturabilecek durumları belirtir.

**ERROR:** Bir işlemin başarısız olduğunu gösterir. Ancak bu durum uygulamanın tamamının durduğu anlamına gelmez.

**CRITICAL:** Uygulamanın veya önemli bir bileşenin çalışmasını engelleyebilecek ciddi problemleri ifade eder.

Log seviyelerinin önem sıralaması şöyledir:

`DEBUG → INFO → WARNING → ERROR → CRITICAL`

### 2.3. Python ile Loglama

Python içerisinde loglama işlemleri için yerleşik olarak gelen `logging` kütüphanesi kullanılır.

Örnek:

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logging.debug("Uygulama kontrol ediliyor.")
logging.info("Uygulama başarıyla başlatıldı.")
logging.warning("Disk alanı azalıyor.")
logging.error("Veritabanı bağlantısı başarısız.")
logging.critical("Sistem kritik durumda!")
```

**Kodun açıklaması:**

- `import logging`: Python loglama kütüphanesini programa dahil eder.
- `basicConfig()`: Loglama yapılandırmasını belirler.
- `level=logging.DEBUG`: DEBUG ve daha yüksek seviyedeki kayıtların gösterilmesini sağlar.
- `format`: Log kayıtlarının hangi formatta görüntüleneceğini belirler.
- `logging.info()`, `logging.error()` gibi metotlar ilgili seviyede kayıt oluşturur.

Örnek çıktı:

```text
2026-11-26 10:00:00 - DEBUG - Uygulama kontrol ediliyor.
2026-11-26 10:00:01 - INFO - Uygulama başarıyla başlatıldı.
2026-11-26 10:00:02 - WARNING - Disk alanı azalıyor.
2026-11-26 10:00:03 - ERROR - Veritabanı bağlantısı başarısız.
2026-11-26 10:00:04 - CRITICAL - Sistem kritik durumda!
```

### 2.4. Log Kayıtlarını Dosyaya Yazma

Log kayıtlarının yalnızca terminal üzerinde görüntülenmesi yeterli olmayabilir. Özellikle canlı ortamlarda geçmiş kayıtların incelenebilmesi için dosyaya yazılması önemlidir.

Python'da `FileHandler` kullanılarak loglar bir dosyada saklanabilir.

```python
import logging

logging.basicConfig(
    filename="uygulama.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s",
    encoding="utf-8"
)

logging.info("Uygulama başlatıldı.")
logging.warning("Başarısız giriş denemesi.")
logging.error("Bir hata meydana geldi.")
```

Bu kod çalıştırıldığında uygulamanın bulunduğu çalışma dizininde `uygulama.log` isimli dosya oluşturulur.

Böylece uygulama kapatıldıktan sonra da kayıtlar incelenebilir.

### 2.5. İyi Loglama Uygulamaları

Loglama sisteminin etkili olabilmesi için bazı noktalara dikkat edilmelidir:

- Log seviyeleri olayın önemine uygun seçilmelidir.
- Kayıtlar tarih ve saat bilgisi içermelidir.
- Hataların anlaşılmasını sağlayacak yeterli bağlam bulunmalıdır.
- Gereksiz log kayıtları oluşturulmamalıdır.
- Log dosyalarının boyutu ve saklama süresi kontrol edilmelidir.
- Parola, erişim token'ı ve özel anahtar gibi hassas bilgiler kaydedilmemelidir.
- Log dosyalarına erişim yetkisi sınırlandırılmalıdır.

---

## 3. Exception Handling (İstisna Yönetimi)

### 3.1. Exception Nedir?

Exception, programın çalışması sırasında meydana gelen ve normal işlem akışını kesintiye uğratabilen beklenmeyen durumları ifade eder.

Yazılım geliştirme sırasında her işlemin sorunsuz gerçekleşeceği varsayılamaz.

Örneğin:

- Kullanıcının yanlış veri girmesi.
- Olmayan bir dosyanın açılmaya çalışılması.
- Bir sayının sıfıra bölünmesi.
- Veritabanı bağlantısının kesilmesi.
- Bir işlemin beklenmeyen veri türüyle gerçekleştirilmesi.

Python'da bu durumlara ilişkin farklı exception türleri bulunmaktadır.

| Exception | Açıklama |
|---|---|
| ZeroDivisionError | Bir sayının sıfıra bölünmesi |
| ValueError | Uygun türde ancak geçersiz bir değer kullanılması |
| TypeError | Uygun olmayan veri türleriyle işlem yapılması |
| FileNotFoundError | İstenen dosyanın bulunamaması |
| IndexError | Liste veya dizi sınırlarının dışında bir indekse erişilmesi |
| KeyError | Sözlükte bulunmayan bir anahtara erişilmesi |

### 3.2. Exception Handling Nedir?

Exception Handling, program çalışırken meydana gelen hataların yakalanması ve kontrollü şekilde yönetilmesini sağlayan yöntemdir.

Amaç, beklenmeyen bir durum oluştuğunda programın kontrolsüz şekilde sonlanmasını önlemek ve uygun bir hata yönetimi sağlamaktır.

Python'da exception handling için temel olarak şu yapılar kullanılır:

- `try`
- `except`
- `else`
- `finally`

**TRY:** Hata oluşma ihtimali bulunan kodların yazıldığı bölümdür.

**EXCEPT:** TRY bloğunda belirtilen hata meydana gelirse çalışır.

**ELSE:** TRY bloğu herhangi bir hata oluşturmadan tamamlanırsa çalışır.

**FINALLY:** Hata oluşsa da oluşmasa da çalışır. Genellikle kaynakların temizlenmesi gibi işlemlerde kullanılır.

### 3.3. Python ile Exception Handling Örneği

Aşağıdaki örnekte iki sayının birbirine bölünmesi işlemi gerçekleştirilmektedir.

```python
try:
    sayi1 = 10
    sayi2 = 0

    sonuc = sayi1 / sayi2

except ZeroDivisionError:
    print("Hata: Bir sayı sıfıra bölünemez!")

else:
    print("Sonuç:", sonuc)

finally:
    print("İşlem tamamlandı.")
```

**Kodun çalışma mantığı:**

1. TRY bloğunda bölme işlemi gerçekleştirilmek istenir.
2. İkinci sayı sıfır olduğu için `ZeroDivisionError` oluşur.
3. EXCEPT bloğu hatayı yakalar ve kullanıcıya açıklayıcı mesaj gösterir.
4. İşlem başarılı tamamlanmadığı için ELSE bloğu çalışmaz.
5. FINALLY bloğu her durumda çalışır.

Bu yöntem sayesinde hata kontrollü biçimde ele alınır.

### 3.4. Exception Handling ve Logging Birlikte Kullanımı

Exception Handling ile Logging birlikte kullanıldığında meydana gelen hatalar hem yakalanabilir hem de kayıt altına alınabilir.

```python
import logging

logging.basicConfig(
    filename="hatalar.log",
    level=logging.ERROR,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

try:
    sonuc = 10 / 0

except ZeroDivisionError:
    logging.exception("Bölme işlemi sırasında hata oluştu.")
```

Bu örnekte:

- TRY bloğu riskli işlemi gerçekleştirir.
- EXCEPT bloğu oluşan hatayı yakalar.
- `logging.exception()` hata mesajını ve traceback bilgisini log dosyasına kaydeder.

Böylece hata yalnızca yakalanmakla kalmaz, daha sonra incelenebilmesi için kayıt altına alınır.

### 3.5. Hata Yönetiminde Dikkat Edilmesi Gerekenler

Exception Handling kullanılırken aşağıdaki noktalara dikkat edilmelidir:

- Mümkün olduğunca belirli exception türleri yakalanmalıdır.
- Hatalar sessizce geçiştirilmemelidir.
- Kullanıcıya teknik ayrıntılar yerine anlaşılır hata mesajları gösterilmelidir.
- Hassas bilgiler hata mesajlarında paylaşılmamalıdır.
- Kaynakların güvenli şekilde kapatılması sağlanmalıdır.
- Aynı hata gereksiz yere farklı katmanlarda tekrar tekrar loglanmamalıdır.

Özellikle `except: pass` kullanımı önerilmez. Çünkü bu yaklaşım meydana gelen hatayı gizleyerek sorunun tespit edilmesini zorlaştırabilir.

---

## 4. Monitoring (Sistem İzleme)

### 4.1. Monitoring Nedir?

Monitoring, bir yazılım uygulamasının, sunucunun ve sistem kaynaklarının düzenli veya sürekli olarak takip edilmesidir.

Bir uygulamanın çalışıyor olması her zaman sorunsuz çalıştığı anlamına gelmez.

Örneğin bir web uygulaması erişilebilir durumda olabilir ancak kullanıcıların yaptığı işlemler normalden çok daha uzun sürebilir. Bu durum performans probleminin göstergesi olabilir.

Monitoring sayesinde sistemin performansı takip edilir ve ortaya çıkabilecek sorunlar erken aşamada tespit edilebilir.

### 4.2. Monitoring Neden Gereklidir?

Monitoring, yazılım uygulamalarının güvenilir ve verimli şekilde çalışmasını destekler.

Başlıca faydaları şunlardır:

1. **Sorunların erken tespiti:** Sistem kaynaklarındaki anormal değişimler fark edilebilir.
2. **Performans takibi:** Uygulamanın yanıt süreleri ve kaynak kullanımı incelenebilir.
3. **Kesintilerin azaltılması:** Sorunlara daha erken müdahale edilmesine yardımcı olur.
4. **Kaynak yönetimi:** CPU, RAM ve disk kullanımının kontrol edilmesini sağlar.
5. **Kullanıcı deneyimi:** Yavaşlık ve erişim sorunlarının tespit edilmesini kolaylaştırır.
6. **Güvenlik:** Beklenmeyen trafik artışları ve olağan dışı sistem davranışları fark edilebilir.

### 4.3. İzlenen Temel Sistem Metrikleri

Monitoring sistemlerinde farklı performans ve kaynak metrikleri takip edilir.

| Metrik | Açıklama |
|---|---|
| CPU Kullanımı | İşlemcinin ne kadar yoğun çalıştığını gösterir. |
| RAM Kullanımı | Sistemin bellek tüketimini gösterir. |
| Disk Kullanımı | Depolama alanının doluluk durumunu gösterir. |
| Ağ Trafiği | Gelen ve giden ağ trafiğini takip eder. |
| Latency | Bir isteğin yanıtlanması için geçen süreyi ifade eder. |
| Throughput | Belirli bir zaman aralığında gerçekleştirilen işlem veya istek sayısını gösterir. |
| Error Rate | Başarısız işlemlerin toplam işlemlere oranını ifade eder. |
| Availability | Bir hizmetin belirlenen ölçüm koşullarına göre erişilebilir olma durumunu gösterir. |

Örneğin:

Bir web sunucusunun RAM kullanımı sürekli artıyorsa bellek tüketiminin nedenleri araştırılmalıdır.

CPU kullanımı uzun süre yüksek seviyede kalıyorsa sunucu performansı etkilenebilir.

Bir uygulamanın yanıt süresi artıyorsa veritabanı sorguları, ağ bağlantısı veya uygulama kodu incelenebilir.

Bu metrikler birlikte değerlendirildiğinde sistemin genel durumu hakkında daha doğru sonuçlara ulaşılabilir.

### 4.4. Monitoring Araçları

Sistem izleme işlemlerinde farklı araçlardan yararlanılır.

**Prometheus**

Prometheus, sistem ve uygulama metriklerini toplamak, zaman serisi verilerini saklamak ve bu verileri sorgulamak için kullanılan açık kaynaklı bir monitoring aracıdır.

Özellikle sunucu ve uygulama performansının izlenmesinde kullanılır.

Resmî web sitesi: https://prometheus.io/

**Grafana**

Grafana, farklı veri kaynaklarından alınan bilgileri grafikler ve dashboard'lar üzerinden görselleştirmeyi sağlayan bir platformdur.

Prometheus ile birlikte kullanılarak sistem metriklerinin anlaşılır şekilde takip edilmesini sağlar.

Resmî web sitesi: https://grafana.com/

**Zabbix**

Zabbix; sunucuların, ağ cihazlarının ve çeşitli sistem bileşenlerinin izlenmesini sağlayan açık kaynaklı bir monitoring çözümüdür.

Kaynak kullanımı ve sistem durumuna ilişkin verilerin takip edilmesine yardımcı olur.

Resmî web sitesi: https://www.zabbix.com/

**Datadog**

Datadog, uygulama performansı, altyapı, log ve metrik takibi gibi farklı izleme ihtiyaçlarını bir arada sunan ticari bir platformdur.

Özellikle bulut altyapılarında ve dağıtık uygulamalarda kullanılabilir.

Resmî web sitesi: https://www.datadoghq.com/

---

## 5. Debugging (Hata Ayıklama)

### 5.1. Debugging Nedir?

Debugging, yazılım içerisinde meydana gelen hataların nedenlerini tespit etmek ve bu hataları düzeltmek amacıyla gerçekleştirilen hata ayıklama sürecidir.

Yazılım geliştirme sırasında meydana gelen hatalara genel olarak bug adı verilir.

Bu hatalar uygulamanın yanlış sonuç üretmesine, beklenmeyen şekilde davranmasına veya tamamen çalışamaz hale gelmesine neden olabilir.

Debugging sürecinde kod adım adım incelenerek hatanın hangi işlemden kaynaklandığı belirlenir.

### 5.2. Hata Ayıklama Yöntemleri

Debugging sırasında farklı yöntemlerden yararlanılır.

**Breakpoint:** Kodun belirli bir satırda durdurulmasını sağlar. Böylece o noktadaki değişkenler ve programın durumu incelenebilir.

**Step Over:** Mevcut satırı çalıştırır ve sonraki satıra geçer.

**Step Into:** Çalıştırılan fonksiyonun içerisine girerek kodun nasıl çalıştığını incelemeyi sağlar.

**Variable Inspection:** Program çalışırken değişkenlerin mevcut değerlerini kontrol etmeye yardımcı olur.

**Stack Trace:** Hatanın meydana geldiği çağrı zincirini göstererek sorunun hangi fonksiyonlardan kaynaklanabileceğini anlamayı kolaylaştırır.

Bu yöntemler Visual Studio Code ve PyCharm gibi geliştirme ortamlarında kullanılabilir.

### 5.3. Debugging Süreci

Bir hata meydana geldiğinde sistematik şekilde ilerlemek gerekir.

Genel hata ayıklama süreci şu adımlardan oluşur:

1. **Hatanın belirlenmesi:** Uygulamada hangi işlemin başarısız olduğu tespit edilir.
2. **Log kayıtlarının incelenmesi:** Hatanın meydana geldiği zaman ve ilgili işlem araştırılır.
3. **Hatanın tekrarlanması:** Mümkünse hata kontrollü bir ortamda yeniden oluşturulur.
4. **Kodun incelenmesi:** Hatanın meydana geldiği kod bölümü araştırılır.
5. **Kök nedenin belirlenmesi:** Sorunun asıl kaynağı tespit edilir.
6. **Düzeltme yapılması:** Hatalı kod veya yapılandırma düzeltilir.
7. **Test edilmesi:** Yapılan değişikliğin sorunu çözüp çözmediği kontrol edilir.

Debugging yalnızca hatayı ortadan kaldırmak değil, hatanın neden meydana geldiğini anlamak ve tekrar oluşmasını önlemektir.

---

## 6. Production Ortamında Örnek Olay

Production, yazılım uygulamasının gerçek kullanıcılar tarafından kullanılan canlı çalışma ortamıdır.

Bir e-ticaret uygulaması üzerinden örnek bir senaryo ele alalım.

Bir kullanıcı ürün satın almak istediğinde ödeme işlemi başarısız oluyor. Aynı zamanda diğer kullanıcılar da benzer sorunlar yaşamaya başlıyor.

Bu durumda aşağıdaki süreç izlenebilir:

**1. Monitoring ile sorunun fark edilmesi**

Uygulamanın hata oranında artış olduğu ve yanıt sürelerinin uzadığı görülür.

**2. Logging kayıtlarının incelenmesi**

Log kayıtlarında ödeme işlemlerinin başarısız olduğuna ilişkin ERROR seviyesinde kayıtlar bulunur.

Örnek:

```text
2026-11-26 14:30:10 ERROR Payment service connection failed.
```

**3. Exception Handling ile hatanın yönetilmesi**

Ödeme işlemi sırasında oluşan bağlantı hatası yakalanır. Kullanıcıya işlemin tamamlanamadığına ilişkin anlaşılır bir mesaj gösterilir.

Böylece uygulamanın kontrolsüz şekilde sonlanması önlenir.

**4. Debugging ile sorunun araştırılması**

Kod ve sistem yapılandırması incelenir. Ödeme servisi ile uygulama arasındaki bağlantı problemi araştırılır.

**5. Sorunun çözülmesi ve test edilmesi**

Bağlantı problemi giderildikten sonra ödeme işlemleri tekrar test edilir. Monitoring üzerinden hata oranının normale dönüp dönmediği takip edilir.

Bu örnek, Logging, Monitoring, Exception Handling ve Debugging kavramlarının birlikte nasıl çalıştığını göstermektedir.

---

## 7. Siber Güvenlik Açısından Önemi

Logging ve Monitoring yalnızca yazılım performansının takip edilmesinde değil, siber güvenlik süreçlerinde de önemli bir yere sahiptir.

Sistemlerde gerçekleşen olayların kayıt altına alınması, şüpheli aktivitelerin incelenmesine ve güvenlik olaylarının araştırılmasına yardımcı olur.

Örneğin:

- Çok sayıda başarısız kullanıcı girişimi.
- Yetkisiz erişim denemeleri.
- Beklenmeyen yönetici işlemleri.
- Olağan dışı ağ trafiği.
- Şüpheli API istekleri.
- Sistem üzerinde gerçekleştirilen yetki değişiklikleri.

Bu olaylar log kayıtları üzerinden incelenebilir.

SOC (Security Operations Center) ekipleri, güvenlik olaylarını analiz ederken farklı sistemlerden gelen log verilerinden yararlanırlar.

Örneğin belirli bir IP adresinden kısa süre içerisinde çok sayıda başarısız giriş denemesi gelmesi, brute-force saldırısı ihtimalinin araştırılmasını gerektirebilir.

Bu süreçte log analizi, şüpheli davranışların belirlenmesi ve olayların zaman çizelgesinin oluşturulması açısından önemlidir.

Ancak log kayıtlarının güvenli şekilde tutulması gerekir.

Dikkat edilmesi gereken başlıca noktalar:

- Kullanıcı parolaları loglara yazılmamalıdır.
- Erişim token'ları ve özel anahtarlar kaydedilmemelidir.
- Hassas kişisel bilgiler gereksiz yere saklanmamalıdır.
- Log dosyalarına erişim yetkileri sınırlandırılmalıdır.
- Kayıtların bütünlüğü ve saklama süresi korunmalıdır.
- Şüpheli olaylar uygun güvenlik süreçleriyle incelenmelidir.

---

## 8. Sonuç

Logging, Monitoring, Debugging ve Exception Handling, yazılım uygulamalarının güvenilir ve kontrollü şekilde çalıştırılmasında önemli rol oynayan kavramlardır.

Logging sayesinde uygulamada meydana gelen olaylar kayıt altına alınır. Exception Handling ile beklenmeyen hatalar kontrollü şekilde yönetilir. Monitoring sayesinde sistem performansı ve çalışma durumu takip edilir. Debugging ise meydana gelen hataların nedenlerinin araştırılmasını ve düzeltilmesini sağlar.

Bu kavramların birlikte kullanılması, özellikle production ortamlarında meydana gelen sorunların daha hızlı tespit edilmesine ve çözülmesine yardımcı olur.

Ayrıca log kayıtlarının siber güvenlik açısından incelenmesi, şüpheli aktivitelerin tespit edilmesi ve güvenlik olaylarının araştırılması açısından önemlidir.

Sonuç olarak bir yazılımın başarılı olması yalnızca doğru çalışmasına değil, aynı zamanda meydana gelen sorunların izlenebilir, anlaşılabilir ve yönetilebilir olmasına da bağlıdır.

---

## 9. Kaynakça

1. Python Software Foundation – [Logging HOWTO](https://docs.python.org/3/howto/logging.html)
2. Python Software Foundation – [Logging Library Reference](https://docs.python.org/3/library/logging.html)
3. Python Software Foundation – [Built-in Exceptions](https://docs.python.org/3/library/exceptions.html)
4. Python Software Foundation – [Errors and Exceptions](https://docs.python.org/3/tutorial/errors.html)
5. Prometheus – [Official Documentation](https://prometheus.io/docs/introduction/overview/)
6. Grafana – [Official Documentation](https://grafana.com/docs/)
7. Zabbix – [Official Documentation](https://www.zabbix.com/documentation/current/en)
8. Datadog – [Official Documentation](https://docs.datadoghq.com/)
9. OWASP – [Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
