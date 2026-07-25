# API Mantığı (REST ve JSON Temelleri)

## 1. Giriş

Günümüzde web, mobil ve masaüstü uygulamaları kullanıcıların ihtiyaçlarını karşılamak amacıyla sürekli olarak veri alışverişi yapmaktadır. Bu veri alışverişinin güvenli, hızlı ve düzenli bir şekilde gerçekleştirilebilmesi için uygulamalar arasında belirli iletişim mekanizmaları kullanılmaktadır. Bu mekanizmaların en yaygın olanlarından biri API (Application Programming Interface) yapılarıdır.

API'ler, farklı yazılım sistemlerinin birbirleriyle iletişim kurmasını sağlayarak modern yazılım geliştirme süreçlerinde önemli bir rol üstlenmektedir. Özellikle REST mimarisi ve JSON veri formatı, günümüzde geliştirilen web servislerinde en sık kullanılan teknolojiler arasında yer almaktadır.

Bu raporda API kavramı, API'lerin kullanım amacı, REST mimarisinin temel prensipleri, HTTP metotları (GET, POST, PUT ve DELETE), JSON veri yapısı ve basit bir endpoint tasarımı incelenmiştir.



## 2. API Nedir?

API (Application Programming Interface), farklı yazılımların belirli kurallar çerçevesinde birbirleriyle iletişim kurmasını sağlayan bir arayüzdür. API sayesinde bir uygulama başka bir uygulamadan veri talep edebilir, veri gönderebilir veya belirli işlemleri gerçekleştirebilir.

Modern yazılım geliştirme süreçlerinde istemci (client) ile sunucu (server) arasındaki veri alışverişi çoğunlukla API'ler aracılığıyla gerçekleştirilir. Böylece uygulamalar veritabanına doğrudan erişmek yerine API üzerinden güvenli ve düzenli bir şekilde haberleşir.

Örneğin, bir mobil uygulamanın kullanıcı bilgilerini görüntülemesi veya bir e-ticaret sitesinin ürün listesini sunması API kullanımı sayesinde gerçekleşmektedir. API, istemciden gelen isteği sunucuya iletir, gerekli işlemleri gerçekleştirir ve elde edilen sonucu tekrar istemciye döndürür.         



## 3. API Neden Kullanılır?

API'ler, farklı yazılım sistemlerinin güvenli ve düzenli bir şekilde iletişim kurmasını sağlamak amacıyla kullanılmaktadır. Günümüzde web, mobil ve masaüstü uygulamaları aynı verilere ihtiyaç duyabilmektedir. API kullanımı sayesinde bu uygulamalar, veritabanına doğrudan erişmek yerine ortak bir arayüz üzerinden veri alışverişi gerçekleştirir.

API kullanımının sağladığı başlıca avantajlar şunlardır:

- **Güvenlik:** Veritabanı doğrudan dış dünyaya açılmaz. API yalnızca izin verilen işlemleri gerçekleştirir ve gerekli verileri paylaşır.
- **Merkezi Yönetim:** Aynı API, web uygulamaları, mobil uygulamalar ve diğer istemciler tarafından ortak olarak kullanılabilir.
- **Kolay Bakım:** Sistemde yapılan değişiklikler API üzerinden yönetildiği için istemci uygulamaların etkilenme olasılığı azalır.
- **Standart İletişim:** API'ler belirli kurallar doğrultusunda çalıştığından uygulamalar arasında düzenli ve tutarlı bir veri alışverişi sağlanır.
- **Yeniden Kullanılabilirlik:** Bir kez geliştirilen API, farklı projelerde ve platformlarda tekrar kullanılabilir.



## 4. REST Mimarisi

REST (Representational State Transfer), web servislerinin belirli kurallar çerçevesinde tasarlanmasını sağlayan bir mimari yaklaşımdır. REST, API geliştirme sürecinde standart bir yapı oluşturmayı amaçlar ve günümüzde en yaygın kullanılan API mimarilerinden biridir.

REST mimarisinde her veri bir **kaynak (resource)** olarak değerlendirilir. Bu kaynaklara URL'ler aracılığıyla erişilir. Örneğin, bir blog uygulamasında `/articles` adresi makaleleri, `/users` adresi ise kullanıcıları temsil edebilir.

REST yaklaşımında gerçekleştirilecek işlem URL ile değil, kullanılan HTTP metodu ile belirlenir. Aynı endpoint üzerinde farklı HTTP metotları kullanılarak veri listeleme, oluşturma, güncelleme ve silme işlemleri gerçekleştirilebilir. Bu yapı sayesinde API'ler daha düzenli, anlaşılır ve sürdürülebilir hale gelir.



## 5. HTTP Metotları

REST mimarisinde istemcinin sunucu üzerinde gerçekleştireceği işlemler HTTP metotları ile belirlenir. En yaygın kullanılan HTTP metotları GET, POST, PUT ve DELETE'tir.

### GET

GET metodu, sunucudan veri almak amacıyla kullanılır. Bu işlem mevcut verileri değiştirmez ve yalnızca istenen bilgilerin istemciye gönderilmesini sağlar.

**Örnek:**

```http
GET /articles
```

**Kullanım Örneği:** Bir blog uygulamasında tüm makalelerin listelenmesi veya bir e-ticaret sitesinde ürünlerin görüntülenmesi amacıyla kullanılır.


### POST

POST metodu, sunucu üzerinde yeni bir kaynak oluşturmak için kullanılır. İstemci tarafından gönderilen veriler işlenerek veritabanına yeni bir kayıt eklenir.

**Örnek:**

```http
POST /articles
```
**Kullanım Örneği:** Yeni bir kullanıcı hesabı oluşturulması, yeni bir makalenin eklenmesi veya yeni bir siparişin sisteme kaydedilmesi amacıyla kullanılır.


### PUT

PUT metodu, mevcut bir kaynağın güncellenmesini sağlar. Genellikle belirli bir kaynağın tüm bilgilerinin güncellenmesi amacıyla kullanılır.

**Örnek:**

```http
PUT /articles/2
```
**Kullanım Örneği:** Bir kullanıcının profil bilgilerinin güncellenmesi veya bir blog yazısının içeriğinin değiştirilmesi işlemlerinde kullanılır.


### DELETE

DELETE metodu, mevcut bir kaynağın silinmesini sağlar.

**Örnek:**

```http
DELETE /articles/2
```
**Kullanım Örneği:** Bir kullanıcının hesabının silinmesi veya artık kullanılmayan bir blog yazısının sistemden kaldırılması amacıyla kullanılır.


Bu HTTP metotları sayesinde REST API'lerde veri listeleme, oluşturma, güncelleme ve silme işlemleri standart bir şekilde gerçekleştirilmektedir.



## 6. JSON Yapısı

JSON (JavaScript Object Notation), uygulamalar arasında veri alışverişi yapmak amacıyla kullanılan hafif ve okunabilir bir veri formatıdır. Günümüzde REST API'lerde istemci ile sunucu arasındaki veri iletişimi çoğunlukla JSON formatı kullanılarak gerçekleştirilmektedir.

JSON yapısı anahtar (key) ve değer (value) çiftlerinden oluşur. Anahtarlar çift tırnak içerisinde yazılır ve her anahtarın karşısında bir değer bulunur. Bu değer; metin, sayı, mantıksal ifade, dizi veya başka bir JSON nesnesi olabilir.

### JSON Nesnesi Örneği

```json
{
    "id": 1,
    "ad": "Rahime",
    "yas": 21,
    "universite": "Osmaniye Korkut Ata Üniversitesi"
}
```

Birden fazla verinin gönderilmesi gerektiğinde JSON dizileri kullanılır.

### JSON Dizisi Örneği

```json
[
    {
        "id": 1,
        "title": "Python"
    },
    {
        "id": 2,
        "title": "Django"
    }
]
```

JSON formatı; okunabilir olması, düşük veri boyutu sunması ve birçok programlama dili tarafından desteklenmesi nedeniyle günümüzde en yaygın veri alışverişi formatlarından biri olarak kullanılmaktadır.



## 7. Basit Endpoint Tasarımı

Endpoint, bir API içerisinde belirli bir kaynağa erişmek için kullanılan URL adresidir. İstemci uygulamalar, sunucu üzerindeki verilere erişmek veya işlem yapmak için endpoint'lere HTTP istekleri gönderir.

REST mimarisinde endpoint'ler, erişilecek kaynağı temsil ederken yapılacak işlem HTTP metodu ile belirlenir. Bu yaklaşım API'lerin daha düzenli, anlaşılır ve standart bir yapıda geliştirilmesini sağlar.

Aşağıdaki tabloda bir blog uygulaması için örnek endpoint tasarımı verilmiştir.

| HTTP Metodu | Endpoint | Açıklama |
|-------------|----------|----------|
| GET | `/articles` | Tüm makaleleri listeler. |
| GET | `/articles/5` | ID'si 5 olan makaleyi getirir. |
| POST | `/articles` | Yeni bir makale oluşturur. |
| PUT | `/articles/5` | ID'si 5 olan makaleyi günceller. |
| DELETE | `/articles/5` | ID'si 5 olan makaleyi siler. |

Bu yapı, REST standartlarına uygun basit bir endpoint tasarımını göstermektedir.



## 8. Sonuç

API'ler, günümüzde farklı yazılım sistemlerinin güvenli ve düzenli bir şekilde iletişim kurmasını sağlayan temel teknolojilerden biridir. REST mimarisi, API geliştirme sürecine standart bir yapı kazandırırken; HTTP metotları veri üzerinde gerçekleştirilecek işlemleri tanımlar. JSON veri formatı ise istemci ve sunucu arasındaki veri alışverişini kolaylaştıran, hafif ve okunabilir bir yapı sunar.

Bu raporda API kavramı, kullanım amaçları, REST mimarisinin temel prensipleri, HTTP metotları, JSON veri yapısı ve basit bir endpoint tasarımı incelenmiştir. Bu konuların öğrenilmesi, backend geliştirme sürecini anlamak ve modern web servisleri geliştirebilmek açısından önemli bir temel oluşturmaktadır.



## 9. Kaynakça

1. Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures*. University of California, Irvine.

2. Mozilla Developer Network (MDN). *HTTP Overview*. https://developer.mozilla.org/

3. JSON.org. *Introducing JSON*. https://www.json.org/

4. Microsoft Learn. *Web API Documentation*. https://learn.microsoft.com/

5. RESTful API Tutorial. *REST Architecture*. https://restfulapi.net/



