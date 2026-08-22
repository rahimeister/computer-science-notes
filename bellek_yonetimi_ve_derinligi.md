# Bellek ve Performans Derinliği: Stack, Heap, Garbage Collection ve Memory Leak


## Giriş 
Modern yazılım geliştirme süreçlerinde bellek yönetimi, uygulamaların performansını doğrudan etkileyen en önemli konulardan biridir. Bir program çalıştırıldığında veriler belleğin farklı bölgelerinde depolanır ve işletilir. Bu bölgelerin en önemlileri Stack (Yığın Bellek) ve Heap (Öbek Bellek) olarak adlandırılır.

Belleğin doğru bir şekilde yönetilmesi, uygulamaların daha hızlı, güvenli ve verimli çalışmasını sağlar. Yanlış bellek yönetimi ise performans sorunlarına, gereksiz kaynak tüketimine ve uygulamanın çökmesine neden olabilir.

Bu raporda Stack ve Heap bellek yapıları, Garbage Collection mekanizması ve Memory Leak kavramı ayrıntılı olarak incelenmiştir.



## 1. Stack ve Heap Arasındaki Fark 

### 1.1 Stack (Yığın Bellek) Nedir?

Stack, program çalışırken kullanılan geçici verilerin depolandığı bellek alanıdır. Fonksiyon çağrıları, yerel değişkenler ve parametreler Stack içerisinde tutulur.

Bir fonksiyon çağrıldığında Stack üzerinde yeni bir alan oluşturulur. Fonksiyonun çalışması tamamlandığında ise ayrılan alan otomatik olarak silinir.


#### Stack'in özellikleri
 - Veriler otomatik olarak yönetilir.
 - Bellek ayırma ve silme işlemleri oldukça hızlıdır.
 - Yerel değişkenler burada depolanır.
 - Bellek kapasitesi sınırlıdır.
 - LIFO (Last In, First Out) mantığıyla çalışır.


#### Stack çalışma mantığı 

main()
   ↓

fonksiyonA()
   ↓

fonksiyonB()

FonksiyonB tamamlandıktan sonra bellekten çıkarılır. Daha sonra fonksiyonA sonlandırılır ve en son main fonksiyonu Stack'ten kaldırılır.

#### Stack örneği
```
def hesapla():
    sayi = 10
    return sayi
hesapla()
```

Bu örnekte "sayi" adlı değişken Stack üzerinde oluşturulur. Fonksiyon tamamlandığında ise bellekten otomatik olarak kaldırılır.


### 1.2 Heap (Öbek Bellek) Nedir?

Heap, programın çalışma süresi boyunca oluşturulan nesnelerin saklandığı bellek alanıdır.

Dinamik olarak oluşturulan veriler Heap içerisinde tutulur. Stack'e göre daha büyük bir kapasiteye sahiptir. Ancak veri erişim hızı Stack'e göre daha düşüktür.


#### Heap'in özellikleri 

- Nesneler burada depolanır.
- Bellek kapasitesi daha geniştir.
- Bellek yönetimi daha karmaşıktır.
- Veri oluşturma ve silme işlemleri daha yavaştır.
- Dinamik veri yapıları burada saklanır.


#### Heap örneği 
```
liste = [1, 2, 3, 4, 5]
```
Bu örnekte oluşturulan liste Heap belleğinde saklanır.


### 1.3 Stack ve Heap Karşılaştırması

| Özellik | Stack | Heap |
| --- | --- | --- |
| Veri türü | Yerel değişkenler | Nesneler |
| Yönetim | Otomatik | Dinamik |
| Hız | Daha hızlı | Daha yavaş |
| Boyut | Sınırlı | Daha geniş |
| Bellek tahsisi | Statik | Dinamik |
| Veri silme | Otomatik | Çöp toplayıcı (Garbage Collector) veya program tarafından |



## 2. Garbage Collection (GC) Nasıl Çalışır?

### 2.1 Garbage Collection Nedir?

Garbage Collection(Çöp Toplama), artık kullanılmayan nesnelerin bellekten otomatik olarak kaldırılmasını sağlayan bir bellek yönetim mekanizmasıdır.

Python, Java ve C# gibi programlama dilleri, bellek yönetimini kolaylaştırmak için Garbage Collection sistemini kullanılır. 

GC'nin temel amacı gereksiz nesneleri tespit ederek belleği temizlemektir.


### 2.2 Garbage Collection Çalışma Mantığı 

Garbage Collection süreci genel olarak üç aşamadan oluşur.


#### 1. Nesnelerin oluşturlması 

```
veri = [10, 20, 30]
```
Nesne Heap belleğinde oluşturulur.


#### 2. Referansların İzlenmesi 
```
veri = None 
```
Nesneye ait referans kaldırılır. 



#### 3. Kullanılamayan nesnenin silinmesi 

Garbage Collector, artık herhangi bir değişken tarafından kullanılamayan nesneyi tespit eder ve bellekten kaldırır.


### 2.3 Python'da Referans Sayma Mekanizması 

Python, öncelikle referans sayma yöntemini kullanır. 

```
a = [1, 2, 3]
b = a
```

Bu durumda nesnenin iki farklı referansı vardır. 

```
a = None 
b = None
``` 

Her iki referans da kaldırıldığında nesne bellekten silinebilir. 


### 2.4 Döngüsel Referans Problemi 

Bazı durumlarda iki nesne birbirini referans gösterebilir.

```
liste1 = []
liste2 =[]

liste1.append(liste2)

liste2.append(liste1)
```

Bu durumda referans sayısı sıfıra düşmeyebilir.

Python, bu tür durumları çözebilmek için ek bir Garbage Collection mekanizması kullanır.


### 2.5 Garbage Collection'ın Avantajları 

- Bellek yönetimini kolaylaştırır.
- Programlama hatalarını azaltır.
- Bellek sızıntılarının önüne geçer.
- Yazılım geliştirme sürecini hızlandırır.



## 3 Memory Leak Nedir? 

Memory Leak (Bellek Sızıntısı), artık kullanılmayan verilerin bellekte gereksiz yere tutulması durumudur. 

Bellekte sızıntı meydana geldiğinde uygulama giderek daha fazla bellek tüketir. 


### 3.1 Memory Leak Nasıl Oluşur?

#### Gereksiz nesnelerin saklanması 

```
veriler = []

while True:
    veriler.append("Yeni veri")
```

Bu örnekte liste sürekli büyür ve bellek kullanımı artar. 

#### Açık bırakılan kaynaklar 

```
dosya = open("veri.txt")
```

Dosya kapatılmazsa sistem kaynakları gereksiz yere kullanılabilir. 

Doğru kullanım:
```
with open("veri.txt") as dosya:
    veri = dosya.read() 
```


#### Olay dinleyicilerinin temizlenmemesi 

Arayüz uygulamalarında olay dinleyicileri kaldırmazsa nesneler bellekte kalmaya devam edebilir. 


#### Önbelleğin kontrolsüz büyümesi

Önbellek sistemlerinin sınırlandırılmaması bellek tüketimini arttırabilir.


### 3.2 Memory Leak Belirtileri 

- Uygulamanın yavaşlaması
- RAM kullanımının sürekli artması
- İşlemci kullanımının yükselmesi
- Programın yanıt vermemesi
- Uygulamanın çökmesi


### 3.3 Memory Leak'in Önlenmesi 

- Kullanılmayan nesneleri silmek
- Dosyaları ve ağ bağlantılarını kapatmak
- Önbellek boyutunu sınırlandırmak
- Gereksiz referansları kaldırmak
- Bellek analiz araçlarını kullanmak


## Sonuç 

Bellek yönetimi, yazılım geliştirme sürecinin temel bileşenlerinden biridir. Stack ve Heap bellek alanları farklı amaçlar için kullanılır. Stack daha hızlı ve sınırlı bir yapıya sahipken Heap daha esnek ancak daha karmaşık bir bellek alanıdır.

Garbage Collection mekanizması, kullanılmayan nesneleri otomatik olarak temizleyerek bellek yönetimini kolaylaştırır. Ancak bu mekanizma her zaman yeterli olmayabilir. Yanlış programlama uygulamaları sonucunda Memory Leak sorunları ortaya çıkabilir.

Bu nedenle yazılım geliştiricilerin bellek yönetimi konusunda bilgi sahibi olması, daha verimli ve güvenilir uygulamalar geliştirebilmeleri açısından önem taşımaktadır. 


## Kaynakça
- Python Documentation
- Oracle Java Documentation
- Microsoft .NET Documentation
- Silberschatz, A. (2018). Operating System Concepts.
- Sommerville, I. (2016). Software Engineering.


