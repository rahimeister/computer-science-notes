# DESING PATTERNS (TASARIM DESENLERİ)

## 1. GİRİŞ 
Tasarım desenleri (Design Patterns), yazılım geliştirme sürecinde sık karşılaşılan tasarım problemlerine yönelik, tekrar kullanılabilir ve kendini kanıtlamış çözüm yaklaşımlarıdır.

Tasarım desenleri doğrudan kopyalanıp kullanılan hazır kodlar değildir. Bunun yerine, sınıfların ve nesnelerin nasıl organize edilebileceğini, sorumlulukların nasıl ayrılacağını ve bileşenlerin birbiriyle nasıl iletişim kuracağını açıklayan tasarım yaklaşımlarıdır.

Bu raporda Singleton, Factory, Strategy, Observer, Adapter ve Decorator desenleri incelenecek; kullanım amaçları, avantajları, dezavantajları ve örnek uygulamaları ele alınacaktır.



## 2. Tasarım Desenleri Neden Kullanılır?

Yazılım geliştirme sürecinde yalnızca çalışan kod yazmak yeterli değildir. Kodun zaman içerisinde değiştirilebilir, test edilebilir, anlaşılabilir ve sürdürülebilir olması da gerekir.

Tasarım desenleri bu hedeflere ulaşmaya yardımcı olur.

Başlıca kullanım amaçları şunlardır:

- **Tekrarlanan çözümler:** Benzer tasarım problemlerinde bilinen yaklaşımlardan yararlanmak.

- **Bakım kolaylığı:** Değişikliklerin etkisini belirli bileşenlerle sınırlamak.

- **Genişletilebilirlik:** Yeni özellikleri mevcut kodun tamamını değiştirmeden ekleyebilmek.

- **Düşük bağımlılık:** Bileşenlerin birbirlerinin iç ayrıntılarına daha az bağımlı olmasını sağlamak.

- **Test edilebilirlik:** Bileşenleri bağımsız biçimde test etmeyi kolaylaştırmak.

- **Ortak teknik dil:** Geliştiricilerin tasarım kararlarını daha hızlı ve net biçimde tartışabilmesi.

**Önemli:** Her problemi bir tasarım deseniyle çözmek gerekmez. Gereksiz desen kullanımı, basit bir sistemi karmaşık hâle getirebilir.



## 3. Tasarım Desenlerinin Sınıflandırılması

Tasarım desenleri genellikle üç temel grupta incelenir:

Kategori | Amaç | Örnekler | 
|--- | --- | ---|
|Creational (Yaratımsal) | Nesnelerin oluşturulmasını düzenlemek |Singleton, Factory|
|Structural (Yapısal) | Sınıflar ve nesneler arasındaki yapıyı düzenlemek| Adapter, Decorator
|Behavioral (Davranışsal) | Nesnelerin davranışlarını ve iletişimini düzenlemek| StrategyObserver



## 4. Singleton Pattern


Singleton, bir sınıftan uygulama kapsamında tek bir örnek oluşturulmasını ve bu örneğe ortak bir erişim noktası sağlanmasını amaçlayan yaratım desenidir.

### 4.1. Kullanım Alanları

- Uygulama genelinde ortak yapılandırma yönetimi

- Tek bir örneğin kullanılmasının istendiği servisler

- Belirli kaynakların merkezi yönetimi

- Singleton kullanırken nesnenin yaşam döngüsü, eşzamanlı erişim ve test edilebilirlik konuları dikkate alınmalıdır.

### 4.2. C# Örneği


```csharp
public sealed class AppConfiguration
{
    private static readonly AppConfiguration instance =
        new AppConfiguration();

    private AppConfiguration() { }

    public static AppConfiguration Instance => instance;
}
```


**Kullanımı:**
```csharp
var config = AppConfiguration.Instance;
```


Bu örnekte kurucu metot dışarıdan erişime kapatılmıştır. Statik Instance özelliği aynı örneğe erişim sağlar.

### 4.3. Avantajları ve Dezavantajları

**Avantajları**

- Tek bir örneğe erişimi kolaylaştırır.

- Nesne oluşturulmasını merkezi hâle getirir.

**Dezavantajları**

- Global durum ve gizli bağımlılıklar oluşturabilir.

- Birim testlerinde bağımlılıkları değiştirmeyi zorlaştırabilir.

- Her sınıf için tek örnek gereksinimi bulunmaz.




## 5. Factory Pattern


Factory yaklaşımı, nesne oluşturma sorumluluğunu nesneyi kullanan koddan ayırır. Böylece istemci kodun somut sınıfları doğrudan oluşturma ihtiyacı azaltılabilir.

Factory Method ve Simple Factory farklı yaklaşımlardır. Aşağıdaki örnek, basit bir factory uygulamasıdır.

### 5.1. C# Örneği

```csharp
public interface INotification
{
    void Send(string message);
}

public class EmailNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"E-posta: {message}");
    }
}

public class SmsNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"SMS: {message}");
    }
}

public static class NotificationFactory
{
    public static INotification Create(string type)
    {
        return type.ToLowerInvariant() switch
        {
            "email" => new EmailNotification(),
            "sms" => new SmsNotification(),
            _ => throw new ArgumentException(
                "Desteklenmeyen bildirim türü.")
        };
    }
}
```

**Kullanımı:**

```csharp

INotification notification =
    NotificationFactory.Create("email");

notification.Send("İşlem tamamlandı.");

```

### 5.2. Avantajları ve Dezavantajları

**Avantajları**

- Nesne oluşturma mantığını tek bir yerde toplar.

- İstemci kodun somut sınıflara bağımlılığını azaltabilir.

- Yeni nesne türlerinin eklenmesini kolaylaştırabilir.


**Dezavantajları**

- Basit uygulamalarda gereksiz bir katman oluşturabilir.

- Factory sınıfı çok sayıda koşulla büyüyebilir.



## 6. Strategy Pattern

Strategy, aynı amaca hizmet eden farklı algoritmaların ortak bir arayüz arkasında tanımlanmasını ve ihtiyaç duyulduğunda birbirleriyle değiştirilebilmesini sağlar.

### 6.1. Kullanım Alanları

- Farklı ödeme yöntemleri

- Sıralama algoritmaları

- İndirim hesaplama yöntemleri

- Farklı kimlik doğrulama stratejileri

### 6.2. C# Örneği

```csharp
public interface IDiscountStrategy
{
    decimal Apply(decimal price);
}

public class NoDiscount : IDiscountStrategy
{
    public decimal Apply(decimal price) => price;
}

public class TenPercentDiscount : IDiscountStrategy
{
    public decimal Apply(decimal price) => price * 0.90m;
}

public class PriceCalculator
{
    private readonly IDiscountStrategy strategy;

    public PriceCalculator(IDiscountStrategy strategy)
    {
        this.strategy = strategy;
    }

    public decimal Calculate(decimal price)
    {
        return strategy.Apply(price);
    }
}
```

**Kullanımı:**
```csharp
var calculator =
    new PriceCalculator(new TenPercentDiscount());

Console.WriteLine(calculator.Calculate(100m));
```
**Çıktı:**

```
90.00
```

### 6.3. Avantajları ve Dezavantajları

**Avantajları**

- Algoritmalar birbirinden ayrılır.

- Yeni stratejiler mevcut hesaplama sınıfı değiştirilmeden eklenebilir.

- Her strateji bağımsız olarak test edilebilir.

**Dezavantajları**

- Çok basit işlemler için fazla sayıda sınıf oluşturabilir.

- Stratejilerin seçilmesi ve yapılandırılması gerekir.



## 7. Observer Pattern

Observer, bir nesnenin durumunda değişiklik olduğunda ona abone olan diğer nesnelerin bilgilendirilmesini sağlayan davranışsal desendir.

### 7.1. Kullanım Alanları

- Olay tabanlı sistemler

- Kullanıcı arayüzü güncellemeleri

- Bildirim sistemleri

- Sensör ve izleme uygulamaları

### 7.2. C# Örneği

```csharp
public class NewsPublisher
{
    public event Action<string>? NewsPublished;

    public void Publish(string news)
    {
        NewsPublished?.Invoke(news);
    }
}
```
**Kullanımı:**

```csharp
var publisher = new NewsPublisher();

publisher.NewsPublished += news =>
    Console.WriteLine($"Yeni haber: {news}");

publisher.Publish("Yeni sürüm yayımlandı.");
```

**Çıktı:**

```
Yeni haber: Yeni sürüm yayımlandı.
```

Bu örnekte C# event mekanizması, Observer yaklaşımını uygulamak için kullanılmıştır.

###  7.3. Avantajları ve Dezavantajları

**Avantajları**

- Yayıncı ve dinleyiciler arasındaki bağımlılığı azaltabilir.

- Birden fazla bileşenin olaylara tepki vermesini sağlar.

**Dezavantajları**

- Çok sayıda abonelik olduğunda olay akışını takip etmek zorlaşabilir.

- Aboneliklerin zamanında kaldırılmaması bellek veya yaşam döngüsü sorunlarına yol açabilir.

- Olayların hangi sırayla işlendiği önem kazanabilir.



## 8. Adapter Pattern

Adapter, birbiriyle doğrudan uyumlu olmayan iki arayüz arasında dönüştürücü görevi görür. Mevcut bir sınıfı, istemcinin beklediği arayüz üzerinden kullanmayı mümkün kılar.

### 8.1. Kullanım Alanları

Harici kütüphanelerin sisteme entegre edilmesi

Eski sistemlerle yeni sistemlerin birlikte çalışması

Farklı servislerin ortak bir arayüz altında kullanılması

### 8.2. C# Örneği

```csharp
public interface IPaymentProcessor
{
    void Pay(decimal amount);
}

public class LegacyPaymentService
{
    public void MakePayment(double amount)
    {
        Console.WriteLine($"Ödeme alındı: {amount}");
    }
}

public class PaymentAdapter : IPaymentProcessor
{
    private readonly LegacyPaymentService legacyService;

    public PaymentAdapter(LegacyPaymentService legacyService)
    {
        this.legacyService = legacyService;
    }

    public void Pay(decimal amount)
    {
        legacyService.MakePayment((double)amount);
    }
}
```

**Kullanımı:**
```
IPaymentProcessor processor =
    new PaymentAdapter(new LegacyPaymentService());

processor.Pay(250m);
```

### 8.3. Avantajları ve Dezavantajları

**Avantajları**

- Mevcut kodu değiştirmeden entegrasyon sağlayabilir.

- Farklı arayüzleri ortak bir sözleşme üzerinden kullanmayı kolaylaştırır.


**Dezavantajları**

- Ek bir sınıf ve dönüştürme katmanı oluşturur.

- Veri tipleri veya davranışlar arasındaki uyumsuzluklar dikkatli ele alınmalıdır.



## 9. Decorator Pattern

Decorator, bir nesnenin davranışını onu saran başka nesneler aracılığıyla genişletir. Kalıtım yoluyla çok sayıda alt sınıf oluşturmak yerine, davranışlar çalışma zamanında birleştirilebilir.

### 9.1. Kullanım Alanları

- Loglama

- Önbellekleme (caching)

- Yetkilendirme kontrolleri

- Veri akışlarına ek özellikler ekleme

### 9.2. C# Örneği

```csharp
public interface IMessageService
{
    void Send(string message);
}

public class BasicMessageService : IMessageService
{
    public void Send(string message)
    {
        Console.WriteLine(message);
    }
}

public class LoggingMessageDecorator : IMessageService
{
    private readonly IMessageService service;

    public LoggingMessageDecorator(IMessageService service)
    {
        this.service = service;
    }

    public void Send(string message)
    {
        Console.WriteLine("Mesaj gönderiliyor...");
        service.Send(message);
        Console.WriteLine("İşlem tamamlandı.");
    }
}
```
**Kullanımı:**
```
IMessageService service =
    new LoggingMessageDecorator(new BasicMessageService());

service.Send("Merhaba!");
```
**Çıktı:**
```
Mesaj gönderiliyor...
Merhaba!
İşlem tamamlandı.
```

### 9.3. Avantajları ve Dezavantajları

**Avantajları**

- Mevcut sınıfı değiştirmeden yeni davranışlar eklenebilir.

- Birden fazla decorator birleştirilebilir.

- Sorumluluklar ayrı sınıflarda tutulabilir.

**Dezavantajları**

- Çok sayıda decorator kullanıldığında nesne yapısı karmaşıklaşabilir.

- Davranışların hangi sırayla uygulandığı sonucu değiştirebilir.

## 10. Tasarım Desenlerinin Karşılaştırılması

| Desen | Temel Amaç | Ne Zaman Tercih Edilir |
| ---| --- | ---|
|Singleton | Tek örnek sağlamak | Tek örnek gereksinimi gerçekten varsa|
|Factory | Nesne oluşturmayı ayırmak | Oluşturulacak türü seçme mantığı karmaşıklaşıyorsa|
|Strategy | Algoritmaları değiştirilebilir kılmak | Aynı işlem için birden fazla yöntem varsa
|Observer | Değişiklikleri abonelere bildirmek | Bir olayın birden fazla bileşeni etkilemesi gerekiyorsa
|Adapter| Uyumsuz arayüzleri birleştirmek | Mevcut bir sistem beklenen arayüze uymuyorsa
| Decorator | Davranışı genişletmek | Nesneye kalıtım oluşturmadan ek sorumluluklar eklemek gerekiyorsa



## 11. Junior Kodundan Profesyonel Koda

Yazılım geliştiriciliğinde deneyim, yalnızca daha fazla kod yazmakla ölçülmez. Asıl önemli noktalardan biri, problemi doğru modellemek ve gereksiz karmaşıklıktan kaçınmaktır.

Başlangıç seviyesinde bir geliştirici, çalışan sonuca ulaşmaya odaklanabilir. Deneyimli bir geliştirici ise aynı zamanda kodun ileride nasıl değişeceğini, test edileceğini ve diğer bileşenlerle nasıl etkileşime gireceğini düşünür.

Örneğin, bütün indirim hesaplamalarını tek bir metotta çok sayıda if-else koşuluyla yazmak başlangıçta yeterli olabilir. Fakat yeni indirim türleri eklendikçe metot büyüyebilir ve değişiklik yapmak zorlaşabilir.

Strategy deseni, farklı indirim algoritmalarını ayrı sınıflara ayırarak bu soruna çözüm sunabilir.

Ancak bu, her if-else yapısının Strategy desenine dönüştürülmesi gerektiği anlamına gelmez. Birkaç basit koşul için mevcut yaklaşım daha anlaşılır olabilir.

**Profesyonel kodun temel özellikleri:**

- Anlaşılabilir ve açık bir yapı

- Sınıflar arasında dengeli sorumluluk dağılımı

- Gerektiğinde genişletilebilirlik

- Test edilebilirlik

- Düşük ve kontrollü bağımlılık

- Gereksiz soyutlamalardan kaçınma

Tasarım desenlerini bilmek profesyonel yazılım geliştirmenin önemli bir parçasıdır; ancak tek başına yeterli değildir. Doğru desenin doğru problemde kullanılması gerekir.

## 12. Sonuç

Tasarım desenleri, yazılım sistemlerini daha anlaşılır, esnek ve sürdürülebilir hâle getirmek için kullanılan önemli tasarım yaklaşımlarıdır.

Singleton ve Factory nesne oluşturma süreçlerine; Strategy ve Observer davranışların yönetimine; Adapter ve Decorator ise yapısal esnekliğe odaklanır.

Bu desenleri öğrenmenin en iyi yolu, yalnızca tanımlarını ezberlemek değil, aynı problemi desen kullanmadan ve desen kullanarak çözmek, iki yaklaşımın avantajlarını ve maliyetlerini karşılaştırmaktır.

Temel amaç daha fazla sınıf yazmak değil, değişen gereksinimlere karşı daha iyi tasarım kararları verebilmektir.

## 13. Kaynaklar

- Refactoring.Guru — Design Patterns

- Microsoft Learn — C# Documentation

- Microsoft Learn — .NET Events

**Konu:** Software Engineering / Object-Oriented Design
**Dil:** Türkçe
**Örnekler:** C#
**Amaç:** Temel tasarım desenlerini anlamak ve sürdürülebilir yazılım tasarımı ilkelerini öğrenmek.




