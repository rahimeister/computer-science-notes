# Concurrency & Parallel Programming

## 1. Giriş

Modern bilgisayar sistemlerinde işlemciler birden fazla çekirdeğe (core) sahiptir. Bu nedenle uygulamaların aynı anda birden fazla işi yönetebilmesi ve işlemci kaynaklarını verimli kullanabilmesi önem kazanmıştır. Bu amaçla **Concurrency (Eşzamanlılık)** ve **Parallelism (Paralellik)** kavramları kullanılmaktadır.

Bu yaklaşımlar sayesinde uygulamalar daha hızlı çalışabilir, sistem kaynakları daha verimli kullanılabilir ve kullanıcı deneyimi iyileştirilebilir.

---

## 2. Concurrency (Eşzamanlılık)

Concurrency, birden fazla görevin aynı zaman dilimi içerisinde ilerleyebilmesidir.

Burada görevler gerçekten aynı anda çalışmak zorunda değildir. İşletim sistemi veya çalışma zamanı (runtime), görevler arasında hızlı geçiş yaparak sanki aynı anda çalışıyorlarmış gibi bir görünüm oluşturur.

### Özellikleri

- Birden fazla iş yönetilebilir.
- Kaynak kullanımı daha verimlidir.
- Özellikle I/O işlemlerinde performans artışı sağlar.
- Tek çekirdekli sistemlerde de uygulanabilir.

### Örnek

Bir web tarayıcısında:

- Sayfa yüklenirken dosya indirilebilir.
- Kullanıcı arayüzü cevap vermeye devam eder.

Bu durum concurrency örneğidir.

---

## 3. Parallelism (Paralellik)

Parallelism, birden fazla görevin fiziksel olarak aynı anda çalıştırılmasıdır.

Bu yaklaşım genellikle çok çekirdekli işlemcilerde kullanılır. Her çekirdek farklı bir görevi aynı anda işleyebilir.

### Özellikleri

- Gerçek eş zamanlı çalışma sağlar.
- CPU yoğun işlemlerde performans artışı sağlar.
- Çok çekirdekli sistemlerden maksimum verim alınmasını sağlar.

### Örnek

Bir görüntü işleme uygulamasında:

- Resim dört parçaya bölünür.
- Her parça farklı çekirdekte işlenir.
- Sonuçlar birleştirilir.

Bu durum parallelism örneğidir.

---

## 4. Concurrency ve Parallelism Arasındaki Fark

| Concurrency | Parallelism |
|------------|------------|
| Birden fazla görevin ilerleyebilmesini sağlar | Birden fazla görevin aynı anda çalışmasını sağlar |
| Tek çekirdekte uygulanabilir | Genellikle çok çekirdek gerektirir |
| Görevler arasında geçiş yapılır | Görevler eş zamanlı yürütülür |
| Kaynak yönetimine odaklanır | Performans artışına odaklanır |

### Benzetme

**Concurrency**

Bir aşçının tek başına aynı anda birçok yemeği hazırlaması, birinden diğerine geçerek ilerlemesi.

**Parallelism**

Birden fazla aşçının aynı anda farklı yemekleri hazırlaması.

---

## 5. Race Condition (Yarış Durumu)

Race Condition, birden fazla iş parçacığının (thread) aynı veriye eş zamanlı erişmesi sonucunda beklenmeyen sonuçların ortaya çıkmasıdır.

Programın sonucu, iş parçacıklarının çalışma sırasına bağlı hale gelir.

### Örnek Senaryo

Bir banka hesabında 1000 TL bulunduğunu düşünelim.

İki farklı thread aynı anda:

- Bakiyeyi okur.
- 100 TL çeker.
- Yeni bakiyeyi yazar.

Doğru sonuç:

```text
1000 - 100 - 100 = 800 TL
```

Ancak threadler aynı veriyi aynı anda okursa sonuç yanlışlıkla 900 TL olabilir.

Bu durum Race Condition olarak adlandırılır.

### Sonuçları

- Veri tutarsızlığı
- Beklenmeyen hatalar
- Güvenlik problemleri
- Sistem kararsızlığı

---

## 6. Mutex

Mutex (Mutual Exclusion), aynı anda yalnızca bir thread'in kritik bölgeye erişmesini sağlayan senkronizasyon mekanizmasıdır.

Bir thread mutex'i kilitlediğinde diğer threadler beklemek zorunda kalır.

### Çalışma Mantığı

1. Thread mutex'i alır.
2. Kritik bölgeye girer.
3. İşlemini tamamlar.
4. Mutex'i serbest bırakır.

### Avantajları

- Race Condition oluşmasını önler.
- Veri bütünlüğünü korur.
- Kullanımı kolaydır.

### Dezavantajları

- Fazla kullanılması performansı düşürebilir.
- Yanlış kullanımı deadlock'a neden olabilir.

---

## 7. Semaphore

Semaphore, belirli sayıda thread'in aynı kaynağa erişmesine izin veren senkronizasyon mekanizmasıdır.

Mutex yalnızca 1 thread'e izin verirken, semaphore belirlenen sayıda thread'e izin verebilir.

### Örnek

Bir sistemde aynı anda en fazla 5 veritabanı bağlantısı kurulabiliyorsa:

- Semaphore değeri = 5
- İlk 5 thread bağlanabilir.
- Altıncı thread beklemek zorunda kalır.

### Türleri

#### Binary Semaphore

Değeri yalnızca:

- 0
- 1

olabilir.

Davranış olarak mutex'e benzer.

#### Counting Semaphore

0'dan büyük sayılar alabilir.

Belirli sayıda erişime izin verir.

---

## 8. Lock Kavramı

Lock, kritik bölgelerin korunması için kullanılan genel bir senkronizasyon yöntemidir.

C# dilinde `lock` anahtar kelimesi ile kullanılabilir.

### Örnek

```csharp
private readonly object _lock = new object();

lock (_lock)
{
    // Kritik bölge
}
```

Bu yapı sayesinde aynı anda yalnızca bir thread kod bloğunu çalıştırabilir.

### Avantajları

- Kullanımı kolaydır.
- Race Condition problemlerini azaltır.
- Kod okunabilirliğini artırır.

---

## 9. Mutex, Semaphore ve Lock Karşılaştırması

| Özellik | Mutex | Semaphore | Lock |
|----------|--------|------------|--------|
| Aynı anda erişebilen thread sayısı | 1 | N | 1 |
| Kullanım amacı | Tek erişim | Çoklu kontrollü erişim | Kod bloğu koruma |
| Kullanım kolaylığı | Orta | Orta | Kolay |
| Performans | Orta | Orta | Yüksek |

---

## 10. Sonuç

Concurrency ve Parallel Programming modern yazılım geliştirme süreçlerinin temel konularındandır. Concurrency, birden fazla görevin aynı zaman diliminde yönetilmesini sağlarken, Parallelism görevlerin fiziksel olarak aynı anda çalıştırılmasını sağlar.

Bu yapılarda ortaya çıkabilecek Race Condition problemlerini önlemek için Mutex, Semaphore ve Lock gibi senkronizasyon mekanizmaları kullanılır. Bu mekanizmaların doğru kullanılması, çok çekirdekli sistemlerde güvenli, performanslı ve ölçeklenebilir yazılımlar geliştirilmesini sağlar.

### Kazanımlar

- Çok çekirdekli sistemlerin çalışma mantığını öğrenme
- Thread güvenliği (Thread Safety) kavramını anlama
- Race Condition problemlerini tespit edebilme
- Mutex, Semaphore ve Lock kullanımını öğrenme
- Güvenli ve yüksek performanslı uygulamalar geliştirebilme
- Paralel programlama temellerini kavrama

---

## Kaynakça

- Microsoft Learn – Managed Threading  
  https://learn.microsoft.com/en-us/dotnet/standard/threading/

- Microsoft Learn – Mutexes  
  https://learn.microsoft.com/en-us/dotnet/standard/threading/mutexes

- Microsoft Learn – Semaphore and SemaphoreSlim  
  https://learn.microsoft.com/en-us/dotnet/standard/threading/semaphore-and-semaphoreslim

- Microsoft Learn – lock Statement (C#)  
  https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/lock

- Oracle Documentation – Concurrency Tutorial  
  https://docs.oracle.com/javase/tutorial/essential/concurrency/