# Security Basics — Güvenlik Temelleri 

Web ve yazılım geliştirme sürecinde uygulamanın yalnızca doğru çalışması yeterli değildir. Uygulama aynı zamanda kullanıcı verilerini, sistem kaynaklarını ve erişim yetkilerini güvenli şekilde yönetmelidir. 

Bu bölümde temel web güvenliği kavramları ele alınmaktadır.


- Authentication ve Authorization
- JWT (JSON Web Token)
- OAuth
- SQL Injection
- XSS (Cross-Site Scripting)
- Güvenli yazılım geliştirme prensipleri

---

## 1. Authentication
**Authentication (Kimlik Doğrulama)**, bir kullanıcının gerçekten iddia ettiği kişi olup olmadığının doğrulanmasıdır.

  
Temel Soru:
    **"Sen kimsin?"**

Örneğin bir kullanıcı sisteme giriş yaparken kullanıcı adı ve şifresini girer. Sistem bu bilgileri kontrol ederek kullanıcının kimliğini doğrular.


### Authentication yöntemleri
- Kullanıcı adı ve şifre 
- E-Posta doğrulaması 
- SMS doğrulama 
- Tek kullanımlık kodlar (OTP)
- İki faktörlü kimlik doğrulama (2FA)
- Biyometrik doğrulama 

### Basit Authentication akışı

Kullanıcı
   |
   | Kullanıcı adı + Şifre
   v
Sunucu
   |
   | Bilgileri kontrol et
   v
Kimlik doğrulandı
   |
   v
Oturum / Token oluştur

Authentication işlemi başarılı olduğunda kullanıcı sisteme giriş yapabilir. 

---

## 2. Authorization
**Authorization (Yetkilendirme)**, kimliği doğrulanmış bir kullanıcının hangi kaynaklara ve işlemlere erişebileceğini belirleme işlemidir. 

Temel soru:
**"Ne yapmaya yetkin var?"**

Örneğin bir sistemde:

```text
Normal Kullanıcı
    ├── Profilini görüntüleyebilir
    ├── Yazı okuyabilir
    └── Yorum yazabilir

Admin
    ├── Profilini görüntüleyebilir
    ├── Yazı okuyabilir
    ├── Yorum yazabilir
    ├── Yazı silebilir
    └── Kullanıcı yönetebilir
```

Burada kullanıcıların sisteme giriş yapması **Authentication**, hangi işlemleri gerçekleştirebildikleri ise **Authorization** ile ilgilidir.

---

## 3. Authentication ve Authorization Farkı

Bu iki kavram sıklıkla birbirine karıştırılır.

| Özellik    | Authentication                    | Authorization                       |
| ---------- | --------------------------------- | ----------------------------------- |
| Türkçesi   | Kimlik Doğrulama                  | Yetkilendirme                       |
| Temel soru | Sen kimsin?                       | Ne yapabilirsin?                    |
| Amaç       | Kullanıcının kimliğini doğrulamak | Erişim izinlerini belirlemek        |
| Örnek      | Şifre ile giriş                   | Admin paneline erişim               |
| Genellikle | Önce gerçekleşir                  | Kimlik doğrulamadan sonra uygulanır |



### Kısa örnek
Bir banka uygulaması düşünelim:

1. Kullanıcı giriş yapar 
            ↓
2. Kimliği doğrulanır
            ↓
3. Hesabına erişim izni verilir.
            ↓
4. Sadece yetkili olduğu hesapları görüntüler 

İlk aşama **Authentication**, son aşama **Authorization** ile ilgilidir.

---

## 4. JWT 
**JWT (JSON Web Token)**, taraflar arasında bilgilerin JSON tabanlı bir token içerisinde taşınmasını sağlayan standart bir token formatıdır.

Web uygulamalarında kullanıcı oturumu ve API erişimi gibi senaryolarda kullanılabilir.

JWT kullanıldığında kullanıcının her API isteğinde kullanıcı adı ve şifresini tekrar göndermesi yerine bir token gönderilebilir.

### Temel JWT Akışı

```text
Kullanıcı
   |
   | Kullanıcı adı + şifre
   v
Sunucu
   |
   | Kimlik doğrulama
   v
JWT oluştur
   |
   v
Kullanıcıya gönder
   |
   v
Sonraki API isteklerinde JWT gönder
   |
   v
Sunucu tokenı doğrular
```

Örneğin bir API isteği şu şekilde olabilir:

```http
GET /api/profile
Authorization: Bearer <TOKEN>
```

### 4.1 JWT Yapısı 
JWT üç bölümden oluşur:

```
Header.Payload.Signature
```

Bu bölümler `.` karakteriyle ayrılır.

**Header**
Tokenın türü ve kullanılan algoritma hakkında bilgi içerir.

```
{ 
    "alg": "HS256"
    "typ": "JWT"
}
```

**Payload** 
Token içerisinde taşınan bilgileri içerir.

Örneğin:

```
{
    "user_id": 25,
    "username": "Rahime",
    "role": "user"
}
```

Payload bölümünün Base64URL ile kodlanması, içeriğinin gizli olduğu anlamına gelmez.

Bu nedenle:
- Şifre
- API secret 
- Özel anahtar 
- Diğer hassas bilgiler

JWT payload içerisine konulmamalıdır.
 
**Signature** 
Tokenın değiştirilmediğini doğrulamaya yardımcı olan imza bölümüdür.

Basitleştirilmiş olarak:
```
Signature = 
Hash(
    Header + Payload + Secret Key 
)
```
şeklinde düşünülebilir. 



## 5.OAuth 

**OAuth**, bir uygulamanın başka bir servis üzerindeki kaynaklara belirli izinler kapsamında erişilebilmesini sağlayan bir yetkilendirme çerçevesidir. 

Günlük hayatta karşılaşılan örneklerden bazıları:

Google ile devam et
GitHub ile devam et 
Microsoft ile devam et 

OAuth sayesinde kullanıcı, üçüncü taraf uygulamaya kendi servis hesabının şifresini vermek zorunda kalmadan belirli erişim izinleri verebilir. 

---
### OAuth Mantığı

```text
Kullanıcı
    |
    | Uygulamaya erişmek ister
    v
Uygulama
    |
    | Yetkilendirme isteği
    v
OAuth Sağlayıcısı
    |
    | Kullanıcı kimliğini doğrular
    | Kullanıcı izin verir
    v
Yetkilendirme bilgisi
    |
    v
Uygulama
```

Örneğin:

```text
[ Google ile devam et ]
```

butonuna basıldığında kullanıcı ilgili sağlayıcının sayfasına yönlendirilebilir.

Kullanıcı burada kimliğini doğrular ve uygulamanın talep ettiği izinleri onaylar.

---

## 6. JWT ve OAuth Farkı 
JWT ve OAuth aynı kavram değildir.

**JWT bir token formatıdır.**

**OAuth ise yetkilendirme için kullanılan bir çerçevedir.**

| Özellik                                 | JWT                       | OAuth                             |
| --------------------------------------- | ------------------------- | --------------------------------- |
| Tür                                     | Token formatı             | Yetkilendirme çerçevesi           |
| Temel amacı                             | Bilgi/token taşımak       | Kaynaklara erişim yetkilendirmesi |
| Kullanım alanı                          | API ve oturum senaryoları | Üçüncü taraf servis erişimi       |
| Tek başına kimlik doğrulama sistemi mi? | Hayır                     | Hayır                             |

Bu nedenle:

```text
JWT ≠ OAuth
```

Ancak OAuth akışlarında JWT formatındaki tokenlar kullanılabilir.

---



## 7. SQL Injection
**SQL Injection**, kullanıcı tarafından sağlanan verilerin güvenli olmayan şekilde SQL sorgusuna dahil edilmesi sonucunda saldırganın SQL sorgusunun yapısını veya mantığını değiştirebilmesiyle ortaya çıkan bir güvenlik açığıdır. 

Özellikle kullanıcı girdilerinin SQL sorgusuna doğrudan birleştirilmesi risklidir. 

### 7.1 Güvensiz SQL Sorgusu 

Örneğin 

```text 
username = input("Kullanıcı adı: ")
password = input("Şifre: ") 
 
query = f"""
SELECT * FROM users
WHERE username = '{username}'
AND password = '{password}' """

```

Burada kullanıcıdan gelen değerler doğrudan SQL sorgusunun içerisine eklenmektedir.

Bu yaklaşım güvenli değildir.

Problem:

Kullanıcı Verisi
       +
SQL Sorgusu
       ↓
Tek bir SQL ifadesi

haline gelmesidir.

---

## 8. SQL Injection'dan Korunma 
SQL Injection'a karşı temel yöntemlerden biri **parametreli sorgular (parameterized queries)** kullanmaktır. 

Örneğin:
```text 
cursor.execute( 
    """ 
    SELECT * FROM users
    WHERE username = ? 
    AND password = ? 
    """, 
    (username, password) 
)

```
Burada SQL sorgusu ile kullanıcı tarafından sağlanan değerler birbirinden ayrılır. 

SQL Komutu 
    + 
Parametreler 
    ↓ 
Ayrı şekilde işlenir

**Ek güvenlik önlemleri**
- Parametreli sorgular kullanmak 
- ORM kullanırken güvenli sorgu yöntemlerini tercih etmek 
- Kullanıcı girdilerini doğrulamak 
- Veritabanı kullanıcılarının yetkilerini sınırlandırmak
- Hassas SQL hata detaylarını kullanıcıya göstermemek 

---

## 9. XSS 
**XSS (Cross-Site Scripting)**, saldırgan tarafından sağlanan içeriğin başka kullanıcıların tarayıcılarında istemci tarafı kod olarak çalıştırmasına yol açabilen bir web güvenlik açığıdır.


Örneğin bir yorum sistemi düşünelim:

Kullanıcı
    |
    | Yorum gönderir
    v
Sunucu
    |
    v
Veritabanı
    |
    v
Web sayfası
    |
    v
Başka kullanıcı

Uygulama kullanıcı tarafından gönderilen içeriği güvenli şekilde işleyemezse saldırganın eklediği HTML/JavaScript içeriği başka kullanıcıların tarayıcılarında  çalışabilir.

---

## 10. XSS Türleri 
XSS genellikle üç ana kategori altında incelenir. 

### 10.1 Stored XSS 
Saldırganın gönderdiği içerik sunucuda veya veritabanında saklanır. 

Saldırgan
    |
    | Zararlı içerik
    v
Veritabanı
    |
    v
Web sayfası
    |
    v
Başka kullanıcı

Örneğin bir yorum sisteminde güvenli şekilde işlenmeyen bir yorumun veritabanına kaydedilmesi Stored XSS riskine neden olabilir.

---

### 10.2 Reflected XSS 
Kullanıcı tarafından gönderilen veri sunucu tarafından işlenir ve  doğrudan HTTP yanıtına yansıtılır. 

İstek
  |
  v
Sunucu
  |
  | Kullanıcı verisini yanıt içerisinde döndürür
  v
Tarayıcı

Uygulama bu veriyi güvenli şekilde kodlamıyorsa XSS oluşabilir.

--- 

### 10.3 DOM-Based XSS 
DOM-Based XSS, istemci tarafındaki JavaScript kodunun kullanıcı tarafından kontrol edilebilen verileri güvenli olmayan şekilde DOM'a eklenmesi sonucunda ortaya çıkabilir. 

Örneğin:

```text 
element.innerHTML = userInput;
```
gibi bir kullanım, ```userInput``` güvenilir değilse risk oluşturabilir.

Uygun durumlarda 

```
element.textContent = userInput;
```

gibi güvenli DOM API'lerinin tercih edilmesi daha doğru bir yaklaşımdır.

---

## 11. XSS'ten Korunma 

XSS'e karşı temel güvenlik önlemleri:

### 11.1 Output Encoding
Kullanıcı tarafından girilen verilerin HTML veya JavaScript kodu olarak değil, veri olarak değerlendirilmesi sağlanmalıdır.

### 11.2 Güvenli DOM API'leri 

Kullanıcı verileri DOM'a eklenirken uygun API'ler kullanılmalıdır.
Örneğin:

```text
element.textContent = userInput
```

### 11.3 Content Security Policy 
**CSP (Content Security Policy)**, tarayıcıya hangi kaynakların yüklenebileceği ve hangi içeriklerin çalıştırılabileceği konusunda kurallar tanımlamaya yardımcı olur.

### 11.4 HttpOnly Cookie
Oturum bilgilerinin cookie içerisinde tutulduğu durumlarda `HttpOnly` özelliği, cookie'nin JavaScript tarafından okunmasını engellemeye yardımcı olur.

### 11.5 Input Validation
Kullanıcıdan alınan veriler beklenen veri türü ve formatına göre doğrulanmalıdır.

Ancak yalnızca input validation'a güvenmek yerine **output encoding** gibi savunma yöntemleri de uygulanmalıdır.

---

## 12. Güvenli Yazılım Geliştirme 
Güvenlik, uygulama tamamlandıktan sonra eklenecek bir özellik olarak düşünülmemelidir.

Yazılım geliştirme sürecinin başından itibaren dikkate alınmalıdır.


### 12.1 Kullanıcı Verilerine Güvenme 

Temel yaklaşım:

Kullanıcı tarafından gönderilen veriler doğrudan güvenilir kabul edilmemelidir. 


### 12.2 Şifreleri Güvenli Saklama 
Şifreler düz metin olarak veritabanında tutulmamalıdır.

Yanlış:

`password = "1234569"`

Bunun yerine parola için güvenli ve amaca uygun bir parola hashleme mekanizması kullanılmalıdır.


### 12.3 Least Privilege 
**Principle of Least Privilege**, kullanıcıya veya uygulamaya yalnızca ihtiyaç duyduğu yetkilerin verilmesi prensibidir.

Örneğin:

Normal kullanıcı
    → Kendi profilini düzenleyebilir

Admin
    → Kullanıcıları yönetebilir

Normal kullanıcının admin yetkilerine sahip olması gereksiz bir güvenlik riski oluşturabilir.


### 12.4 Gizli Bilgileri Kaynak Kodunda Tutmama 
Aşağıdaki yaklaşım uygun değildir. 

`SECRET_KEY = "123456"`

Gizli anahtarlar, parola ve API anahtarları gibi hassas bilgiler yerine ortam değişkenleri veya uygun secret yönetim çözümleri kullanılmalıdır. 

Örneğin: 

```text 
import os 

SECRET_KEY = os.getenv("SECRET_KEY")
```

### 12.5 HTTPS KULLANIMI 
HTTP üzerinden gönderilen veriler ağ üzerinde korunmasız kalabilir.

HTTPS, istemci ile sunucu arasındaki iletişimin TLS üzerinden korunmasını sağlar. 

HTTP Client ------------------> Server Korumasız iletişim 

HTTPS Client ==================> Server TLS

### 12.6 Güvenli Hata Yönetimi

Kullanıcıya gereğinden fazla sistem bilgisi gösterilmemelidir.

Örneğin:

**Yanlış:**

```text
Database connection failed:
Server = 192.168.x.x
Database = users
SQL = SELECT ...
```

Bunun yerine kullanıcıya daha genel bir hata mesajı gösterilebilir:

> Bir hata oluştu. Lütfen daha sonra tekrar deneyin.

Ayrıntılı hata bilgileri geliştirici tarafında loglanabilir.

---
## 13. Kavramların Birlikte Kullanılması

Bir web uygulamasında bu güvenlik kavramları farklı katmanlarda birlikte kullanılabilir.

```text
                       WEB UYGULAMASI
                              |
             +----------------+----------------+
             |                                 |
      Authentication                    Authorization
             |                                 |
       Kullanıcı kim?                    Ne yapabilir?
             |
            JWT
             |
        API İstekleri
             |
       +-----+------+
       |            |
       v            v
  Veritabanı     Tarayıcı
       |            |
       v            v
SQL Injection      XSS
Koruması          Koruması
```

OAuth ise uygulamanın harici bir kimlik sağlayıcısı veya kaynak sunucusu ile yetkilendirme işlemleri gerçekleştirdiği senaryolarda bu mimariye dahil olabilir.


---
## 14. Güvenlik Kontrol Listesi 
Bir web uygulaması geliştirirken aşağıdaki maddeler kontrol edilebilir. 

- [ ] Authentication doğru şekilde uygulanıyor mu?
- [ ] Authorization kontrolleri yapılıyor mu? 
- [ ] Kullanıcıların yetkileri sınırlandırılıyor mu? 
- [ ] Şifreler güvenli şekilde hashleniyor mu? 
- [ ] JWT kullanılıyorsa token güvenliği sağlanıyor mu? 
- [ ] OAuth akışı doğru uygulanıyor mu? 
- [ ] SQL Injection'a karşı parametreli sorgular kullanılıyor mu? 
- [ ] Kullanıcı girdileri güvenilir kabul edilmiyor mu? 
- [ ] XSS'e karşı güvenli çıktı işleme uygulanıyor mu? 
- [ ] Hassas bilgiler kaynak kodunda tutulmuyor mu? 
- [ ] HTTPS kullanılıyor mu? 
- [ ] Cookie güvenlik özellikleri uygun şekilde yapılandırılıyor mu? 
- [ ] Hata mesajlarında hassas bilgiler gösterilmiyor mu? 
- [ ] Gereksiz veritabanı ve sistem yetkileri kaldırılıyor mu?


---
# 15. Özet

| Kavram | Açıklama |
|---|---|
| **Authentication** | Kullanıcının kimliğini doğrulama |
| **Authorization** | Kullanıcının erişim ve işlem yetkilerini belirleme |
| **JWT** | Bilgilerin token formatında taşınmasını sağlayan standart |
| **OAuth** | Yetkilendirme ve üçüncü taraf kaynaklara erişim için kullanılan çerçeve |
| **SQL Injection** | SQL sorgularının kullanıcı girdisi üzerinden manipüle edilmesi riski |
| **XSS** | Güvenilmeyen içeriğin tarayıcıda kod olarak çalıştırılması riski |
| **HTTPS** | İstemci-sunucu iletişiminin TLS ile korunması |
| **Least Privilege** | Gerektiği kadar yetki verme prensibi |

---

# 16. Sonuç

Güvenli yazılım geliştirme yaklaşımının temelinde **"kullanıcı verisine güvenme ve her erişimi kontrol et"** anlayışı bulunur.

Bir uygulamanın çalışması tek başına yeterli değildir.

```text
Çalışan Yazılım
       +
Kimlik Doğrulama
       +
Yetkilendirme
       +
Güvenli Veri İşleme
       +
Güvenli Veritabanı Kullanımı
       +
Güvenli İletişim
       ↓
Güvenli Yazılım
```

Özellikle web uygulamalarında Authentication, Authorization, token yönetimi, güvenli veritabanı işlemleri ve kullanıcı girdilerinin güvenli işlenmesi temel güvenlik konuları arasında yer alır.

Bu nedenle güvenlik, uygulama geliştirildikten sonra eklenen bir özellik değil; **yazılımın tasarımından itibaren dikkate alınması gereken bir geliştirme prensibidir.**

---

## Temel Çıkarım

> **"Çalışıyor" bir yazılımın doğru çalıştığını gösterir; "güvenli" olması ise verileri ve erişimleri doğru şekilde koruduğunu gösterir.**


---

## 17. Kaynakça

1. OWASP Foundation — OWASP Cheat Sheet Series  
   https://cheatsheetseries.owasp.org/

2. OWASP Foundation — SQL Injection Prevention Cheat Sheet  
   https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html

3. OWASP Foundation — Cross Site Scripting Prevention Cheat Sheet  
   https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html

4. IETF — RFC 7519: JSON Web Token (JWT)  
   https://www.rfc-editor.org/rfc/rfc7519

5. IETF — RFC 6749: The OAuth 2.0 Authorization Framework  
   https://www.rfc-editor.org/rfc/rfc6749







