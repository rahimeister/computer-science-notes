# Yazılım Mimarisi Temelleri


## Giriş

Yazılım geliştirme sürecinde yalnızca kod yazmak yeterli değildir. Bir uygulamanın düzenli, sürdürülebilir ve geliştirilebilir olması için doğru bir yazılım mimarisine ihtiyaç vardır. Yazılım mimarisi, uygulamanın bileşenlerinin nasıl organize edileceğini, birbirleriyle nasıl iletişim kuracağını ve sistemin nasıl büyütülebileceğini belirleyen temel yapıdır. Bu raporda Monolith ve Microservice mimarileri, MVC ve MVVM tasarım desenleri, State Management kavramı ve Katmanlı Mimari ele alınmıştır.


## 1. Monolith ve Microservice Mimarisi

### Monolith (Tek Parça Mimari)

Monolith mimaride uygulamanın tüm bileşenleri tek bir proje içerisinde bulunur. Kullanıcı yönetimi, ürünler, siparişler ve diğer tüm modüller aynı uygulamanın bir parçasıdır.


### Avantajları
- Geliştirmesi ve kurulumu kolaydır.
- Küçük ve orta ölçekli projeler için uygundur.
- Tek sunucu üzerinde çalıştırılabilir.
- Hata ayıklama süreci daha basittir.


### Dezavantajları
- Proje büyüdükçe kod karmaşık hale gelir.
- Güncelleme sırasında tüm uygulamanın yeniden dağıtılması gerekir.
- Ölçeklendirme esnek değildir.


## Microservice (Mikro Servis Mimarisi)

Microservice mimarisinde uygulama, birbirinden bağımsız çalışan küçük servislerden oluşur. Her servis belirli bir görevi yerine getirir ve gerektiğinde ayrı olarak geliştirilebilir veya güncellenebilir.

### Avantajları
- Bağımsız geliştirme ve dağıtım imkânı sağlar.
- Büyük projelerde daha kolay ölçeklenebilir.
- Bir serviste oluşan hata tüm sistemi etkilemez.
- Farklı ekipler aynı anda farklı servisler üzerinde çalışabilir.

### Dezavantajları
- Kurulumu ve yönetimi daha karmaşıktır.
- Servisler arası iletişim ek maliyet oluşturur.
- İzleme ve hata ayıklama süreçleri daha zordur.



## Monolith ve Microservice Karşılaştırması

| Monolith | Microservice |
|-----------|--------------|
| Tek proje yapısı | Birden fazla bağımsız servis |
| Kurulumu kolaydır | Yönetimi daha karmaşıktır |
| Küçük projeler için uygundur | Büyük projeler için uygundur |
| Tek uygulama olarak dağıtılır | Her servis ayrı dağıtılabilir |
| Ölçeklendirme sınırlıdır | Servis bazlı ölçeklendirme yapılabilir |



## 2. MVC ve MVVM Tasarım Desenleri
### MVC (Model - View - Controller)

MVC, yazılım geliştirmede en yaygın kullanılan mimari desenlerden biridir.

### Bileşenleri

#### Model

Uygulamanın verilerini ve iş mantığını temsil eder.

#### View

Kullanıcının gördüğü arayüzdür.

#### Controller

Kullanıcıdan gelen istekleri işler, Model ile iletişim kurar ve sonucu View'a gönderir.


### Çalışma Mantığı

Kullanıcı
    ↓
Controller
    ↓
Model
    ↓
Veritabanı
    ↓
Controller
    ↓
View
    ↓
Kullanıcı


### MVVM (Model - View - ViewModel)

MVVM özellikle masaüstü ve mobil uygulamalarda yaygın olarak kullanılan bir tasarım desenidir.

### Bileşenleri

#### Model

Veriyi temsil eder.

#### View

Kullanıcının gördüğü arayüzdür.

#### ViewModel

View ile Model arasında köprü görevi görür ve arayüzün güncellenmesini yönetir.

#### Avantajları

- Kodun okunabilirliğini artırır.
- Arayüz ile iş mantığını birbirinden ayırır.
- Test edilebilirliği kolaylaştırır.
- Kodun bakımını ve geliştirilmesini kolaylaştırır.
- Büyük projelerde daha düzenli bir yapı oluşturur.

#### Dezavantajları

- Küçük projelerde gereksiz karmaşıklık oluşturabilir.
- Öğrenmesi MVC'ye göre daha zordur.
- İlk kurulum süreci daha fazla zaman alabilir.
- Basit uygulamalarda ek katmanlar nedeniyle geliştirme süresi uzayabilir.


## 3. State Management

State (Durum), uygulamanın belirli bir andaki verilerini ifade eder.

Örneğin;

- Kullanıcının giriş yapıp yapmadığı
- Sepette bulunan ürünler
- Tema seçimi
- Bildirim sayısı
- Dil ayarı

uygulamanın durum bilgileri arasında yer alır.

State Management ise bu bilgilerin düzenli şekilde yönetilmesini sağlayan yaklaşımdır.

Modern yazılım geliştirme süreçlerinde özellikle React, Flutter ve Vue gibi teknolojilerde state yönetimi büyük önem taşımaktadır. 


## 4. Katmanlı Mimari (Layered Architecture)

Katmanlı mimari, uygulamanın sorumluluklarını farklı katmanlara ayırarak daha düzenli bir yapı oluşturmayı amaçlar.

En yaygın yapı şu şekildedir:

Controller
    ↓
Service
    ↓
Repository
    ↓
Database

### Controller

Kullanıcıdan gelen istekleri karşılar ve uygun servisi çağırır.

### Service

İş kurallarının bulunduğu katmandır. Doğrulama, hesaplama ve uygulama mantığı burada yer alır.

### Repository

Veritabanı işlemlerini gerçekleştirir. Veri ekleme, silme, güncelleme ve
sorgulama işlemleri bu katmanda yapılır.

Database

Verilerin kalıcı olarak saklandığı katmandır.

### Katmanlı Mimarinin Avantajları
- Kod tekrarını azaltır.
- Bakımı kolaylaştırır.
- Test edilebilirliği artırır.
- Büyük projelerde düzenli bir yapı sağlar.
- Yeni özellik eklemeyi kolaylaştırır.


## Sonuç

Yazılım mimarisi, bir uygulamanın sürdürülebilir, geliştirilebilir ve ölçeklenebilir olması açısından büyük önem taşır. Küçük projelerde Monolith mimarisi yeterli olurken, büyük ölçekli sistemlerde Microservice mimarisi daha uygun bir çözüm sunmaktadır. MVC ve MVVM tasarım desenleri, kodun daha düzenli yazılmasını sağlarken, State Management uygulamanın durum bilgisini yönetmeye yardımcı olur. Katmanlı mimari ise sorumlulukları farklı katmanlara ayırarak daha okunabilir ve bakımı kolay yazılımlar geliştirilmesine katkı sağlar. Bu mimari yaklaşımların öğrenilmesi, profesyonel yazılım geliştirme süreçlerini daha iyi anlamak ve kaliteli projeler geliştirebilmek açısından önemli bir adımdır.