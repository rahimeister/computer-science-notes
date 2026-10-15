# HTTP VE WEB TEKNOLOJİLERİNE DERİNLEMESİNE BAKIŞ 
## 1. Giriş 

Web uygulamalarının temelinde istemci (client) ile sunucu (server) arasındaki iletişim bulunur. Bir kullanıcı web sitesinde bir sayfayı açtığında, giriş yaptığında, bir form gönderdiğinde veya API üzerinde veri istendiğinde arka planda HTTP protokolü kullanılarak istek ve cevap alışverişi gerçekleştirilir.

Bu iletişim genel olarak şu şekilde gerçekleşir.

```text
Client (Frontend)
       |
       | HTTP Request
       v
Server (Backend)
       |
       | HTTP Response
       v
Client (Frontend)
```

HTTP'nin çalışma mantığını anlamak; frontend, backend, API, authentication, authorization ve web güvenliği gibi konuları anlamak açısından oldukça önemlidir.

Bu raporda:

- HTTP request yapısı 
- HTTP response yapısı 
- HTTP metodları 
- HTTP status code'ları
- CORS
- Same-Origin Policy 
- Preflight Request
- Frontend-backend iletişimi 
- HTTP'nin güvenlik açısından önemi 

incelenecektir. 


## 2. HTTP Nedir? 
HTTP, **HyperText Transfer Protocol** ifadesinin kısaltmasıdır.

Web üzerindeki istemci ve sunucuların birbiriyle iletişim kurmasını sağlayan uygulama katmanı protokolüdür.

Örneğin kullanıcı tarayıcıya:

`http://example.com`

yazdığında tarayıcı sunucuya bir HTTP isteği gönderir.

Sunucu bu isteği işler ve HTTP response ile cevap verir. 

Basitleştirilmiş şekilde:

```text
Tarayıcı
   |
   | HTTP Request
   v
Web Sunucusu
   |
   | HTTP Response
   v
Tarayıcı
```

HTTP'nin önemli özelliklerinden biri **stateless (durumsuz)** olmasıdır.

Yani HTTP protokolünün kendisi, önceki istekleri otomatik olarak hatırlamaz.

Örneğin:
```text 
Request 1 → Login
Request 2 → Profile
Request 3 → Settings
```

Bu isteklerin birbirleriyle ilişkilendirilmesi için cookie, session veya token gibi mekanizmalar kullanılabilir.

## 3. HTTP ve HTTPS Farkı 

HTTP verileri tek başına şifreli bir iletişim sağlamaz.

HTTPS ise HTTP'nin TLS kullanılarak güvenli hale getirilmiş versiyonudur.

```text
HTTP
Client ──────────────── Server
        Şifreleme yok


HTTPS
Client ════════════════ Server
        TLS ile korunan iletişim
```

HTTPS kullanıldığında istemci ile sunucu arasındaki iletişimin gizliliği ve bütünlüğü korunmaya çalışılır. 

Örneğin HTTP üzerinde hassas bilgilerin gönderilmesi güvenli değildir. 

```
username = rahime 
password = 123456
```

HTTPS kullanıldığında ise iletişim TLS tarafından korunur.

Bu nedenle günümüzde web uygulamalarında HTTPS kullanılması temel güvenlik gereksinimlerinden biridir.

## 4. HTTP Request Yapısı 

HTTP Request, istemcinin sunucuya gönderdiği istektir.
Temel olarak şu bölümlerden oluşur:

- Request Line
- Headers
- Body

**Örnek:**
```http
POST /api/login HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer TOKEN

{
    "username": "rahime",
    "password": "123456"
}
```

Burada:
- POST → HTTP metodu
- /api/login → İstenen kaynak
- HTTP/1.1 → HTTP sürümü
- Host → Hedef sunucu
- Content-Type → Gönderilen verinin türü
- Authorization → Kimlik doğrulama bilgisi
- JSON → Request body



## 5. Request Line

HTTP request'in ilk satırına **request line** denir.

Örneğin:

```http
GET /users HTTP/1.1
```

Bu satır üç temel bölümden oluşur:

```text
GET       /users       HTTP/1.1
 |           |             |
Method    Resource       Version
```

### 5.1 Method

Sunucuda hangi işlemin yapılacağını belirtir.

Örneğin:

```text
GET
POST
PUT
PATCH
DELETE
```

| Method | Açıklama |
|---|---|
| `GET` | Veri almak için kullanılır. |
| `POST` | Veri göndermek veya yeni kaynak oluşturmak için kullanılır. |
| `PUT` | Bir kaynağı tamamen güncellemek için kullanılır. |
| `PATCH` | Bir kaynağın belirli alanlarını güncellemek için kullanılır. |
| `DELETE` | Bir kaynağı silmek için kullanılır. |

### 5.2 Resource

İstenen kaynağın adresini belirtir.

Örneğin:

```text
/users
/products
/api/login
/api/users/15
```

Bu adresler, sunucuda erişilmek istenen kaynağa veya API endpoint'ine örnektir.

### 5.3 HTTP Version

İletişimde kullanılan HTTP sürümünü belirtir.

Örneğin:

```text
HTTP/1.1
HTTP/2
HTTP/3
```

HTTP sürümleri, protokolün farklı özelliklerini ve iletişim yöntemlerini tanımlar.



## 6.HTTP Methotları
HTTP metodları, istemcinin sunucudan ne yapmak istediğini belirtir.

|Method | Açıklama |
| --- | --- |
|`GET`| Veri almak|
|`POST`| Veri göndermek / kaynak oluşturmak|
|`PUT`| Kaynağı tamamen güncellemek |
|`PATCH`| Kaynağın belirli bir bölümünü güncellemek|
|`DELETE`| Kaynak silmek|


### 6.1 GET
Sunucudan veri almak için kullanılır.

**Örneğin:**

```http
GET /api/users HTTP/1.1
```

**Amaç:**
` Kullanıcı listesini getir.`

GET isteklerinde genellikle request body kullanılmaz.


### 6.2 POST

Yeni veri oluşturmak veya sunucuya veri göndermek için kullanılır.

**örneğin:**

```http
POST /api/users HTTP/1.1
Content-Type: application/json

{
    "name": "Rahime",
    "email": "rahime@example.com"
}
```

Burada frontend yeni bir kullanıcı oluşturmak için backend'e veri göndermektedir.


### 6.3 PUT 
Mevcut bir kaynağı tamamen güncellemek için kullanılabilir.

```http
PUT /api/users/15 HTTP/1.1
Content-Type: application/json

{
    "name": "Rahime",
    "email": "new@example.com"
}
```


### 6.4 PATCH

Bir kaynağın yalnızca belirli bir bölümünü güncellemek için kullanılabilir.

Örneğin sadece e-posta adresini değiştirmek:

```http
PATCH /api/users/15 HTTP/1.1
Content-Type: application/json

{
    "email": "new@example.com"
}
```


### 6.5 DELETE 
Bir kaynağı silmek için kullanılır.

```http
DELETE /api/users/15 HTTP/1.1
```
**Burada:**

`15 numaralı kullanıcıyı sil.`

anlamı çıkarılabilir.



## 7. HTTP Headers

HTTP header'ları request veya response hakkında ek bilgiler taşır.

**Örneğin:**
```http
Content-Type: application/json
Authorization: Bearer TOKEN
User-Agent: Mozilla/5.0
Accept: application/json
```

Bazı önemli header'lar:

|Header | Açıklama |
| --- | ---|
|`Host`| İstek yapılan sunucuyu belirtir. |
|`Content-Type`| Gönderilen verinin türünü belirtir.|
|` Accept` | İstemcinin kabul ettiği veri türlerini belirtir.|
|`Authorization`| Kimlik doğrulama bilgisi taşıyabilir.|
|`User-Agent`| İstemci hakkında bilgi verir.|
|`Cookie`| Cookie bilgilerini taşır.|
|`Origin`| İsteğin geldiği origin'i belirtir.|
|`Referer`| İsteğin hangi sayfadan geldiğini gösterebilir.|


Header'lar web güvenliği açısından da oldukça önemlidir.

## 8. Request Body 
Request body, istemcinin sunucuya gönderdiği verileri içerebilir.

**Örneğin:**
```http 
POST /api/users HTTP/1.1
Content-Type: application/json

{
    "name": "Rahime",
    "age": 21
}
```

Buradaki JSON bölümü request body'dir:
```http
{
    "name": "Rahime",
    "age": 21
}
```

Body özellikle POST, PUT ve PATCH gibi metotlarda sık kullanılır.



## 9. HTTP Response Yapısı

Sunucunun istemciye gönderdiği cevaba HTTP response denir.

Temel yapısı:

Status Line
Headers

Body

**Örnek:**
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "message": "Login successful"
}
```

**Burada:**
```http
HTTP/1.1 → HTTP sürümü
200      → Status Code
OK       → Status Message
```
bulunmaktadır.


## 10. Response Status Line

Response'un ilk satırına status line denir.

**Örneğin:**
```http
HTTP/1.1 200 OK
```
**Burada:**
```http
HTTP/1.1 → HTTP sürümü
200      → Status Code
OK       → Açıklama
```
bulunur.

Status code, istemcinin gönderdiği isteğin sonucunu anlamasına yardımcı olur.



## 11. Response Headers

Sunucu response içerisinde çeşitli header'lar gönderebilir.

**Örneğin:**
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 120
``` 

Güvenlik açısından önemli bazı response header'ları:
```
Content-Security-Policy
Strict-Transport-Security
X-Content-Type-Options
X-Frame-Options
Access-Control-Allow-Origin
```

Bu header'lar web uygulamasının güvenliğini artırmaya yardımcı olabilir.



## 12. Response Body

Sunucunun istemciye gönderdiği asıl veri response body içerisinde bulunabilir.

**Örneğin:**
```http
{
    "id": 15,
    "name": "Rahime",
    "role": "student"
}
```

Frontend bu JSON verisini alarak kullanıcıya gösterebilir.



## 13. HTTP Status Code'lar

HTTP status code'ları üç basamaklıdır.

İlk basamak kodun genel kategorisini belirtir.

|Aralık |  Kategori |
|--- | ---|
| 1xx | Bilgilendirme |
| 2xx | Başarılı |
| 3xx | Yönlendirme |
| 4xx | İstemci hatası |
| 5xx | Sunucu hatası |



## 14. 1xx - Bilgilendirme

1xx kodları bilgilendirme amaçlıdır.

**Örneğin:**

`100 Continue`

İstemcinin isteğinin devam ettirilebileceğini belirtir.

Günlük web geliştirmede 2xx, 3xx, 4xx ve 5xx kodları daha sık karşımıza çıkar.



## 15. 2xx - Başarılı İstekler

### 15.1 200 OK

İstek başarılıdır.

```http 
HTTP/1.1 200 OK
```

**Örneğin:**
```http
GET /api/users
```

isteğine kullanıcı listesi döndürülmesi.

### 15.2 201 Created

Yeni bir kaynak başarıyla oluşturulduğunda kullanılabilir.

```http 
HTTP/1.1 201 Created
```

**Örneğin:**

`POST /api/users`

ile yeni kullanıcı oluşturulması.

### 15.3 204 No Content

İstek başarılıdır ancak response body içerisinde içerik bulunmaz.

**Örneğin:**

`DELETE /api/users/15`

başarıyla gerçekleştiğinde:

```http
HTTP/1.1 204 No Content
```
döndürülebilir.



## 16. 3xx - Yönlendirme

3xx kodları istemcinin başka bir kaynağa yönlendirilmesi gibi durumlarda kullanılır.

Önemli örnekler:

301 Moved Permanently
302 Found
304 Not Modified

### 16.1 301 Moved Permanently

Kaynağın kalıcı olarak başka bir adrese taşındığını belirtir.

### 16.2 302 Found

Kaynağın geçici olarak başka bir adreste bulunduğunu belirtmek için kullanılabilir.

### 16.3 304 Not Modified

İstemcinin elindeki cache içerisindeki kaynağın değişmediğini belirtmek için kullanılır.



## 17. 4xx - İstemci Hataları 
4xx kodları genellikle istemci tarafından gönderilen istekte problem olduğunu gösterir.


### 17.1 400 Bad Request 
Sunucu isteği anlayamadığında veya isteğin formatı hatalı olduğunda kullanılabilir.

`
HTTP/1.1 400 Bad Request
`

Örneğin beklenen JSON formatı:
`
{
    "name": "Rahime"
}
`
iken hatalı bir veri gönderilmesi.



### 17.2 401 Unauthorized

Kimlik doğrulama gerektiğinde veya gönderilen kimlik doğrulama bilgilerinin geçerli olmadığı durumlarda kullanılabilir.

**Örneğin:**

`
GET /api/profile
`
isteği token olmadan gönderildiğinde:

`
HTTP/1.1 401 Unauthorized
`
dönebilir.

Önemli ayrım:

401 → Authentication ile ilgili problem
403 → Authorization / erişim yetkisi ile ilgili problem


### 17.3 403 Forbidden
Sunucu isteği anlamıştır ancak istemcinin erişim izni yoktur.

Örneğin normal bir kullanıcının admin paneline erişmeye çalışması:

`
HTTP/1.1 403 Forbidden
`
ile sonuçlanabilir.

### 17.4 404 Not Found
İstenen kaynak bulunamadığında kullanılır.

**Örneğin:**
`
GET /api/users/999999
`
ve böyle bir kullanıcı yoksa:

`
HTTP/1.1 404 Not Found
`
dönebilir.

### 17.5 405 Method Not Allowed

Kaynak mevcut olsa da kullanılan HTTP metodu desteklenmiyorsa kullanılabilir.

Örneğin:
`
GET /api/users
`

destekleniyor ancak:
`
DELETE /api/users
`
desteklenmiyorsa 405 Method Not Allowed döndürülebilir.

### 17.6 429 Too Many Requests

İstemcinin çok fazla istek göndermesi durumunda kullanılabilir.
`
HTTP/1.1 429 Too Many Requests
`
Örneğin bir API'ye kısa sürede çok fazla istek gönderilmesi.

Bu mekanizma özellikle rate limiting uygulamalarında önemlidir.



## 18. 5xx - Sunucu Hataları

5xx kodları genellikle sunucu tarafında meydana gelen problemlere işaret eder.

### 18.1 500 Internal Server Error

Sunucuda beklenmeyen bir hata meydana geldiğinde kullanılabilir.

`
HTTP/1.1 500 Internal Server Error
`

Örneğin backend'de beklenmeyen bir exception oluşması.

### 18.2 502 Bad Gateway

Bir sunucu, gateway veya proxy olarak başka bir sunucudan geçersiz bir cevap aldığında kullanılabilir.

`
HTTP/1.1 502 Bad Gateway
`

### 18.3 503 Service Unavailable

Sunucunun veya servisin geçici olarak kullanılamadığını belirtir.
`
HTTP/1.1 503 Service Unavailable
`
**Örneğin:**

- Sunucu bakımda
- Servis aşırı yoğun
- Backend geçici olarak çalışmıyor

gibi durumlarda görülebilir.




## 19. Önemli Status Code Özeti

| Kod   | Anlamı                | Kategori       |
| ----- | --------------------- | -------------- |
| `200` | OK                    | Başarılı       |
| `201` | Created               | Başarılı       |
| `204` | No Content            | Başarılı       |
| `301` | Moved Permanently     | Yönlendirme    |
| `302` | Found                 | Yönlendirme    |
| `304` | Not Modified          | Yönlendirme    |
| `400` | Bad Request           | İstemci hatası |
| `401` | Unauthorized          | İstemci hatası |
| `403` | Forbidden             | İstemci hatası |
| `404` | Not Found             | İstemci hatası |
| `405` | Method Not Allowed    | İstemci hatası |
| `429` | Too Many Requests     | İstemci hatası |
| `500` | Internal Server Error | Sunucu hatası  |
| `502` | Bad Gateway           | Sunucu hatası  |
| `503` | Service Unavailable   | Sunucu hatası  |




## 20. CORS Nedir?

CORS, **Cross-Origin Resource Sharing** ifadesinin kısaltmasıdır.

CORS, bir web sayfasının farklı bir origin üzerinde bulunan kaynaklara erişmesine ilişkin tarayıcı güvenlik mekanizmasıdır.

Burada önemli kavram:

**Origin**

kavramıdır.

Bir origin genel olarak şu üç bileşenden oluşur:

**Scheme + Host + Port**

**Örneğin:**


http://localhost:3000


ile:

http://localhost:8000


aynı host üzerinde olmasına rağmen portları farklı olduğu için farklı origin'lerdir.



## 21. Same-Origin Policy

CORS'u anlamak için önce **Same-Origin Policy (SOP)** kavramını anlamak gerekir.

Tarayıcılar güvenlik nedeniyle bir origin'den çalışan JavaScript kodunun başka bir origin'deki kaynaklara erişimini varsayılan olarak kısıtlar.

**Örneğin:**
Frontend

http://localhost:3000


Backend:

http://localhost:8000

olsun.

Frontend:
fetch("http://localhost:8000/api/users")

şeklinde istek gönderdiğinde backend gerekli CORS izinlerini vermemişse tarayıcı response'un frontend tarafından kullanılmasını engelleyebilir.



## 22. Origin Nedir?

Aşağıdaki URL'yi ele alalım:

https://example.com:443/users


**Origin:**

https://example.com

şeklindedir.

Burada:

Scheme → https
Host   → example.com
Port   → 443

bulunur.

Aşağıdaki adresler aynı origin değildir:

https://example.com
http://example.com
https://api.example.com
https://example.com:8080

Çünkü scheme, host veya port farklıdır.



## 23. CORS Nasıl Çalışır?

Frontend:

http://localhost:3000

Backend:

http://localhost:8000

olsun.

Frontend:

fetch("http://localhost:8000/api/users")

şeklinde istek gönderir.

Tarayıcı backend'in response header'larını kontrol eder.

Backend Örneğin:

Access-Control-Allow-Origin: http://localhost:3000

gönderirse bu origin için kaynak paylaşımına izin verilebilir.

Basitleştirilmiş yapı:

```text
Frontend
localhost:3000
     |
     | HTTP Request
     v
Backend
localhost:8000
     |
     | Access-Control-Allow-Origin
     v
Browser
     |
     | Response'u frontend'e kullandırır
     v
Frontend
```


## 24. Access-Control-Allow-Origin

CORS'un en bilinen response header'larından biridir.

**Örneğin:**

`Access-Control-Allow-Origin:` https://example.com

Bu header, belirtilen origin için kaynak paylaşımına izin verilmesini ifade eder.

Bazı durumlarda:

`Access-Control-Allow-Origin: *`

kullanılabilir.

Ancak `*` kullanımı her senaryoda uygun değildir.

Özellikle credentials/cookie kullanılan uygulamalarda CORS yapılandırması dikkatli yapılmalıdır.



## 25. Preflight Request

Bazı cross-origin isteklerden önce tarayıcı gerçek isteği göndermeden önce preflight request gönderebilir.

Bu istek:

**OPTIONS**

metodunu kullanır.

**Örneğin:**

```text
OPTIONS /api/users HTTP/1.1
Origin: https://frontend.example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type, Authorization
```

Sunucu uygun cevap verirse tarayıcı asıl isteği gönderebilir.

Örneğin:
```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: Content-Type, Authorization
```
Bu süreç basitleştirilmiş olarak:

```text
Browser
   |
   | OPTIONS /api/users
   | "POST yapabilir miyim?"
   v
Server
   |
   | 204 No Content
   | "Evet, POST yapabilirsin."
   v
Browser
   |
   | POST /api/users
   v
Server
```

şeklinde düşünülebilir.



## 26. CORS Hatası Nasıl Görünür?

Frontend'de:

fetch("http://localhost:8000/api/users")

çalıştırıldığında backend gerekli CORS header'ını göndermiyorsa tarayıcı konsolunda CORS ile ilgili hata görülebilir.

**Örneğin:**
```http
Access to fetch at 'http://localhost:8000/api/users'
from origin 'http://localhost:3000'
has been blocked by CORS policy.
```

Burada önemli nokta:

CORS çoğunlukla tarayıcının uyguladığı bir güvenlik mekanizmasıdır.

CORS hatası görmek her zaman backend'in isteği hiç almadığı anlamına gelmez. Özellikle preflight gibi durumlarda browser ile server arasındaki sürecin hangi aşamada başarısız olduğunu Developer Tools üzerinden incelemek gerekir.



## 27. Frontend ve Backend İletişimi

Modern web uygulamalarında frontend ve backend genellikle ayrı katmanlar olarak çalışabilir.

**Örneğin:**
```text
Frontend
React / Vue / Angular
       |
       | HTTP Request
       v
Backend
Django / Node.js / ASP.NET
       |
       v
Database
```

Frontend kullanıcı arayüzünü oluştururken backend iş mantığını ve veri işlemlerini yönetebilir.

Örneğin kullanıcı giriş yapmak istediğinde:

1. Kullanıcı username/password girer.
             ↓
2. Frontend POST request gönderir.
             ↓
3. Backend bilgileri kontrol eder.
             ↓
4. Backend response oluşturur.
             ↓
5. Frontend response'u işler.



## 28. Gerçek Bir Login Örneği

Frontend:

```http
POST /api/login HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "username": "rahime",
    "password": "example-password"
}
```

Backend kullanıcı bilgilerini kontrol eder.

Başarılıysa:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "message": "Login successful",
    "token": "example-token"
}
```
Başarısızsa:
```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
    "message": "Invalid credentials"
}
```

dönebilir.

Burada frontend response status code'a göre farklı işlemler yapabilir.



## 29. HTTP'nin Güvenlik Açısından Önemi

HTTP'yi sadece veri alışverişi olarak görmek yeterli değildir.

Web güvenliği açısından HTTP'nin birçok önemli noktası vardır.


### 29.1 HTTPS Kullanımı

HTTP yerine HTTPS kullanılması iletişimin TLS ile korunmasına yardımcı olur.

HTTP  → TLS ile korunmayan iletişim
HTTPS → TLS ile korunan iletişim


### 29.2 Authentication

Authentication:

"Sen kimsin?"

sorusuna cevap verir.

**Örnekler:**

- **Username + Password:** Kullanıcının kullanıcı adı ve parolasıyla kimliğini doğrulaması.
- **Token:** Kimlik doğrulama sonrasında verilen veya kullanılan bir belirteç aracılığıyla isteğin doğrulanması.
- **Session (Oturum):** Kullanıcının giriş yaptıktan sonra sunucuyla olan oturum durumunun takip edilmesi.

Bu yöntemler, uygulamanın tasarımına göre birlikte kullanılabilir.


### 29.3 Authorization

Authorization:

"Bu işlemi yapmaya yetkin var mı?"

sorusuna cevap verir.

**Örneğin:**

Normal User → Kullanıcı profilini görüntüleyebilir.
Admin       → Kullanıcıları silebilir.



## 30. CORS ve Güvenlik

CORS yanlış yapılandırılırsa web uygulamasının güvenlik modeli zayıflayabilir.

Örneğin gereksiz şekilde çok geniş origin izinleri vermek risk oluşturabilir.

**Örnek:**

`Access-Control-Allow-Origin: *`

Ancak bu header'ın tek başına bulunması otomatik olarak bir güvenlik açığı olduğu anlamına gelmez.

CORS yapılandırması uygulamanın ihtiyacına göre yapılmalıdır.

Özellikle:

- Cookie
- Session
- Authorization
- Hassas API'ler

kullanılıyorsa CORS politikası dikkatli tasarlanmalıdır.



## 31. HTTP Header'ları ve Güvenlik

Bazı HTTP response header'ları güvenlik açısından önemlidir.

**Content-Security-Policy**

XSS gibi saldırıların etkisini azaltmaya yardımcı olabilir.

**Örneğin:**

`Content-Security-Policy: default-src 'self'`

**Strict-Transport-Security**

Tarayıcıya HTTPS kullanılması gerektiğini belirtmek için kullanılabilir.

`Strict-Transport-Security: max-age=31536000`

**X-Content-Type-Options**

Tarayıcının MIME type sniffing davranışını sınırlandırmaya yardımcı olabilir.

`X-Content-Type-Options: nosniff`

**X-Frame-Options**

Sayfanın frame içerisinde açılmasını sınırlandırmak için kullanılabilir.

`X-Frame-Options: DENY`



## 32. HTTP ve API İlişkisi

API'ler frontend ile backend arasında veri alışverişini sağlayabilir.

**Örneğin:**

`
GET /api/products
`
isteği:

```http
[
    {
        "id": 1,
        "name": "Laptop"
    },
    {
        "id": 2,
        "name": "Keyboard"
    }
]
```
şeklinde JSON response döndürebilir.

Frontend bu veriyi alıp kullanıcı arayüzünde gösterebilir.

Bu nedenle API geliştirirken HTTP'nin aşağıdaki bölümleri iyi anlaşılmalıdır:

```text
HTTP Method
     ↓
URL / Endpoint
     ↓
Headers
     ↓
Request Body
     ↓
Status Code
     ↓
Response Body
```



## 33. HTTP İsteğinin Baştan Sona İncelenmesi

Örneğin kullanıcı bir ürün listesini görüntülemek istesin.

1. Kullanıcı butona basar

"Ürünleri Getir"

2. Frontend request gönderir

```http
GET /api/products HTTP/1.1
Host: example.com
Accept: application/json
```
3. Backend isteği alır

Backend:
```http
GET
/api/products
```
isteğini değerlendirir.

4. Database'den veri alınır

Backend → Database

5. Backend response oluşturur
```http
HTTP/1.1 200 OK
Content-Type: application/json
```
6. JSON verisi gönderilir
```http


[
    {
        "id": 1,
        "name": "Laptop"
    }
]
```

7. Frontend veriyi gösterir

Laptop

kullanıcı arayüzünde görüntülenir.

## 34. HTTP Hatalarını Tespit Etme

Web geliştirirken HTTP hatalarını incelemek için tarayıcının Developer Tools bölümü oldukça önemlidir.

Genellikle:

F12

ile Developer Tools açılabilir.

Daha sonra:

Network

sekmesine gidilebilir.

Bir request seçildiğinde aşağıdaki bilgiler incelenebilir:

Request URL
Request Method
Status Code
Request Headers
Response Headers
Request Payload
Response

Örneğin:

Request Method: POST
Status Code: 401

görülüyorsa authentication problemi araştırılabilir.

Başka bir örnek:

Request Method: GET
Status Code: 404

ise endpoint'in yanlış olması veya kaynağın bulunamaması kontrol edilebilir.

## 35. Status Code'a Göre Sorun Analizi

Bir web uygulamasında hata ile karşılaşıldığında status code önemli bir başlangıç noktasıdır.

200
↓
İstek başarılı.

400
↓
İstek formatını veya gönderilen verileri kontrol et.

401
↓
Authentication bilgisini kontrol et.

403
↓
Authorization / yetki kontrolünü incele.

404
↓
URL veya endpoint'i kontrol et.

405
↓
HTTP metodunu kontrol et.

429
↓
Rate limit / fazla istek durumunu kontrol et.

500
↓
Backend loglarını ve server-side hataları kontrol et.

502
↓
Gateway / proxy / upstream servisi kontrol et.

503
↓
Servisin çalışıp çalışmadığını ve geçici yoğunluğu kontrol et.

## 36. Örnek Frontend-Backend Akışı

                 INTERNET
                    |
                    v
          ┌──────────────────┐
          │     FRONTEND     │
          │                  │
          │ React / Vue vb.  │
          └────────┬─────────┘
                   |
                   | HTTP Request
                   | GET /api/users
                   v
          ┌──────────────────┐
          │      BACKEND     │
          │                  │
          │ Django / Node.js │
          │ ASP.NET vb.      │
          └────────┬─────────┘
                   |
                   | Database Query
                   v
          ┌──────────────────┐
          │     DATABASE     │
          └────────┬─────────┘
                   |
                   | Data
                   v
          ┌──────────────────┐
          │      BACKEND     │
          └────────┬─────────┘
                   |
                   | HTTP Response
                   | 200 OK + JSON
                   v
          ┌──────────────────┐
          │     FRONTEND     │
          └──────────────────┘

## 37. Sık Yapılan Hatalar

### 37.1 401 ve 403'ü Karıştırmak

Yanlış yaklaşım:

401 = Yetkin yok
403 = Giriş yapmadın

Daha doğru yaklaşım:

401 → Authentication ile ilgili problem
403 → Kaynağa erişim izni yok

### 37.2 CORS'u Authentication Sanmak

**CORS:**

Kimlik doğrulama sistemi değildir.

CORS, tarayıcıların cross-origin kaynak paylaşımına ilişkin güvenlik mekanizmasıdır.

Authentication ise kullanıcının kimliğini doğrulama işlemidir.

### 37.3 Her Hatanın Backend'den Kaynaklandığını Düşünmek

**Örneğin:**

404

görmek her zaman backend'in bozuk olduğu anlamına gelmez.

Yanlış endpoint kullanılabilir.

Örneğin backend:

/api/users

beklerken frontend:

/api/user

gönderiyor olabilir.

### 37.4 Status Code'ları Görmezden Gelmek

Frontend sadece response body'ye bakmak yerine status code'u da değerlendirmelidir.

Örneğin:

if (response.status === 401) {
    // Kullanıcının tekrar giriş yapması gerekebilir.
}

## 38. HTTP ve Web Güvenliği Arasındaki İlişki

HTTP'nin temel yapısını anlamadan web güvenliğini anlamak zorlaşır.

Örneğin aşağıdaki konular HTTP ile doğrudan ilişkilidir:

HTTP
 │
 ├── Authentication
 │
 ├── Authorization
 │
 ├── Cookies
 │
 ├── Sessions
 │
 ├── JWT
 │
 ├── CORS
 │
 ├── CSRF
 │
 ├── XSS
 │
 ├── HTTPS
 │
 ├── Security Headers
 │
 └── Rate Limiting

Bu nedenle HTTP, web güvenliğinin temel yapı taşlarından biridir.

## 39. Genel Özet

HTTP, istemci ve sunucu arasındaki iletişimin temel protokollerinden biridir.

Bir HTTP iletişiminde genel olarak:

Request
   ↓
Server
   ↓
Response

akışı gerçekleşir.

Request içerisinde:

Method
URL
Headers
Body

bulunabilir.

Response içerisinde:

Status Code
Headers
Body

bulunabilir.

HTTP status code'ları ise iletişimin sonucunu anlamamıza yardımcı olur:

2xx → Başarılı
3xx → Yönlendirme
4xx → İstemci kaynaklı problem
5xx → Sunucu kaynaklı problem

CORS ise farklı origin'ler arasında kaynak paylaşımını kontrol eden tarayıcı tabanlı bir güvenlik mekanizmasıdır.

Frontend ve backend arasındaki iletişimde:

Frontend
   ↓
HTTP Request
   ↓
Backend
   ↓
HTTP Response
   ↓
Frontend

akışı temel alınır.

## 40. Sonuç

HTTP'yi yalnızca "web sitelerinin kullandığı protokol" olarak görmek yerine request, response, header, body, status code ve origin kavramlarıyla birlikte değerlendirmek web geliştirme ve siber güvenlik açısından daha doğru bir yaklaşımdır.

Özellikle bir web uygulamasında problem yaşandığında Developer Tools üzerinden Network sekmesini incelemek;

Request Method
Request URL
Status Code
Request Headers
Response Headers
Request Body
Response Body

gibi bilgilerin görülmesini sağlar.

Bu bilgiler sayesinde frontend ile backend arasındaki iletişim daha kolay analiz edilebilir.

CORS, HTTP status code'ları ve HTTP request/response yapısının öğrenilmesi; ilerleyen aşamalarda API güvenliği, authentication, authorization, JWT, session yönetimi, CSRF, XSS ve diğer web güvenliği konularının anlaşılması için sağlam bir temel oluşturur.

## 41. Öğrenilenler

Bu çalışmanın sonunda aşağıdaki konular öğrenilmiş olur:

HTTP'nin temel çalışma mantığı

HTTP request yapısı

HTTP response yapısı

HTTP methods

HTTP headers

Request ve response body

HTTP status code kategorileri

200, 201, 204 gibi başarılı response'lar

400, 401, 403, 404, 405 ve 429 gibi istemci hataları

500, 502 ve 503 gibi sunucu hataları

Same-Origin Policy

Origin kavramı

CORS

Preflight request

OPTIONS metodu

Access-Control-Allow-Origin

Frontend-backend iletişimi

Developer Tools Network sekmesi

HTTP'nin web güvenliği açısından önemi

## 42. Kaynakça

MDN Web Docs - HTTP

MDN Web Docs - HTTP Messages

MDN Web Docs - HTTP Response Status Codes

MDN Web Docs - CORS

MDN Web Docs - Same-Origin Policy

MDN Web Docs - HTTP Headers

RFC 9110 - HTTP Semantics

RFC 6454 - The Web Origin Concept

## 43. Genel Akış
```text

                    HTTP & WEB
                        |
          ┌─────────────┴─────────────┐
          │                           │
       REQUEST                    RESPONSE
          │                           │
    ┌─────┼─────┐               ┌────┼────┐
    │     │     │               │    │    │
 Method Headers Body         Status Headers Body
    │                           │
    └──────────┐      ┌─────────┘
               │      │
               v      v
             SERVER
               |
               v
          HTTP RESPONSE
               |
               v
            FRONTEND
```

**Sonuç:**

HTTP request/response yapısını anlamak, frontend ve backend arasındaki iletişimin nasıl gerçekleştiğini anlamanın temelidir.

HTTP status code'larını anlamak, web uygulamalarında meydana gelen problemlerin nereden kaynaklanabileceğini analiz etmeyi kolaylaştırır.

CORS ve Same-Origin Policy'yi anlamak, frontend'in farklı origin'lerde bulunan backend ve API'lerle nasıl iletişim kurduğunu anlamak açısından önemlidir.

Bu konuların birlikte öğrenilmesi, web geliştirme ve siber güvenlik alanlarında daha ileri konulara geçiş için sağlam bir temel oluşturur.
