# Veritabanı Mantığı (SQL Temelleri)

## Giriş

Günümüzde geliştirilen yazılımların büyük bir bölümü verilerle çalışmaktadır. Kullanıcı bilgileri, ürünler, siparişler, mesajlar ve daha birçok veri, güvenli ve düzenli bir şekilde veritabanlarında saklanır. Veritabanları yalnızca verileri depolamakla kalmaz; bu verilere hızlı erişim, güncelleme ve yönetim imkânı da sağlar.

İlişkisel veritabanları (Relational Databases), günümüzde en yaygın kullanılan veritabanı modelidir. Bu modelde veriler tablolar halinde tutulur ve tablolar arasında belirli ilişkiler kurularak veri bütünlüğü korunur.

Bu dokümanda, ilişkisel veritabanlarının temel çalışma mantığı ele alınacak; Primary Key, Foreign Key, SELECT, JOIN, GROUP BY ve Index gibi SQL'in temel kavramları örneklerle açıklanacaktır.



## Veritabanı Nedir?

Veritabanı (Database), verilerin belirli bir düzen içerisinde saklandığı ve yönetildiği yapılardır. Bir veritabanı sayesinde bilgiler güvenli bir şekilde depolanabilir, gerektiğinde hızlıca sorgulanabilir ve güncellenebilir.

Günlük hayatta kullanılan birçok uygulama veritabanı kullanmaktadır. Örneğin;

- Bir sosyal medya uygulamasında kullanıcı hesapları, gönderiler ve yorumlar,
- Bir e-ticaret uygulamasında ürünler, siparişler ve müşteriler,
- Üniversite otomasyon sistemlerinde öğrenciler, dersler ve notlar

veritabanlarında tutulur.

Veritabanı yönetim sistemleri (Database Management System - DBMS), bu verilerin oluşturulmasını, saklanmasını ve yönetilmesini sağlayan yazılımlardır. Günümüzde yaygın olarak kullanılan veritabanı yönetim sistemleri arasında MySQL, PostgreSQL, Microsoft SQL Server ve Oracle Database yer almaktadır.



## Relational Database (İlişkisel Veritabanı)

İlişkisel veritabanı (Relational Database), verilerin satır ve sütunlardan oluşan tablolar içerisinde saklandığı veritabanı modelidir. Bu modelde her tablo belirli bir veri kümesini temsil eder ve tablolar arasında çeşitli ilişkiler kurulabilir. Bu sayede veriler düzenli bir şekilde saklanırken veri tekrarının azaltılması ve veri bütünlüğünün korunması hedeflenir.

Bir ilişkisel veritabanında her tablo bağımsız bir yapıya sahip olsa da, gerektiğinde diğer tablolarla bağlantı kurabilir. Bu bağlantılar genellikle Primary Key ve Foreign Key kullanılarak oluşturulur. Böylece farklı tablolarda bulunan veriler birbiriyle ilişkilendirilebilir ve tek bir sorgu ile anlamlı sonuçlar elde edilebilir.

Örneğin bir e-ticaret sisteminde kullanıcı bilgileri, ürün bilgileri ve sipariş bilgileri ayrı tablolarda tutulur. Siparişler tablosunda bulunan kullanıcı bilgisi, Users tablosundaki ilgili kullanıcı kaydına bağlanır. Bu sayede aynı kullanıcı bilgileri her sipariş için tekrar tekrar saklanmak yerine yalnızca bir kez tutulur.

### Örnek Tablolar

**Users**

| id | ad | soyad |
|----|------|---------|
| 1 | Ahmet | Yılmaz |
| 2 | Ayşe | Demir |

**Orders**

| id | user_id | ürün |
|----|---------|--------|
| 1 | 1 | Laptop |
| 2 | 2 | Telefon |

Yukarıdaki örnekte **Orders** tablosunda bulunan `user_id` alanı, **Users** tablosundaki ilgili kullanıcıyı ifade etmektedir. Böylece her siparişin hangi kullanıcıya ait olduğu kolaylıkla belirlenebilir.



## Primary Key

Primary Key (Birincil Anahtar), bir tablodaki her satırı benzersiz olarak tanımlayan sütundur. Bir tabloda yalnızca bir adet Primary Key bulunabilir ve bu alanın değeri her kayıt için farklı olmak zorundadır. Bu sayede her satır diğer satırlardan kolayca ayırt edilebilir.

Primary Key alanı boş (NULL) değer alamaz ve aynı değerin birden fazla kayıtta bulunmasına izin verilmez. Bu özellikler, veri bütünlüğünün korunmasına yardımcı olur ve kayıtların güvenilir bir şekilde yönetilmesini sağlar.

Örneğin bir kullanıcı tablosunda her kullanıcıya ait benzersiz bir kimlik numarası bulunur. Bu kimlik numarası Primary Key olarak kullanıldığında, aynı kullanıcıya ait bilgilerin karışması veya tekrar eden kayıtların oluşması engellenmiş olur.

### Örnek

**Users**

| id | ad | soyad |
|----|------|---------|
| 1 | Ahmet | Yılmaz |
| 2 | Ayşe | Demir |
| 3 | Mehmet | Kaya |

Bu tabloda **id** sütunu Primary Key'dir. Çünkü her kullanıcıyı benzersiz olarak tanımlar.

### SQL Örneği

```sql
CREATE TABLE Users (
    id INT PRIMARY KEY,
    ad VARCHAR(50),
    soyad VARCHAR(50)
);
```


## Foreign Key

Foreign Key (Yabancı Anahtar), bir tablodaki sütunun başka bir tablonun Primary Key alanını referans göstermesini sağlayan anahtardır. Bu yapı sayesinde tablolar arasında ilişki kurulabilir ve veriler birbiriyle bağlantılı şekilde yönetilebilir.

Foreign Key kullanımı, veri tekrarını azaltırken aynı zamanda veri bütünlüğünün korunmasına da yardımcı olur. Örneğin bir sipariş kaydı oluşturulurken siparişi veren kullanıcının bilgileri tekrar tekrar yazılmaz. Bunun yerine kullanıcının kimliğini temsil eden değer kullanılarak ilgili kullanıcı kaydına bağlantı kurulur.

Bu yöntem hem depolama alanının daha verimli kullanılmasını sağlar hem de verilerin güncellenmesini kolaylaştırır. Kullanıcı bilgilerinde yapılacak bir değişiklik yalnızca Users tablosunda gerçekleştirilir ve bu değişiklik ilgili tüm kayıtlar için geçerli olur.

### Örnek

**Users**

| id | ad |
|----|------|
| 1 | Ahmet |
| 2 | Ayşe |

**Orders**

| id | user_id | ürün |
|----|---------|--------|
| 1 | 1 | Laptop |
| 2 | 2 | Telefon |

Bu örnekte **Orders** tablosundaki `user_id` sütunu, **Users** tablosundaki `id` sütununu referans almaktadır. Böylece her siparişin hangi kullanıcıya ait olduğu kolayca belirlenebilir.

### SQL Örneği

```sql
CREATE TABLE Orders (
    id INT PRIMARY KEY,
    user_id INT,
    product VARCHAR(100),

    FOREIGN KEY (user_id)
    REFERENCES Users(id)
);
```


## SELECT

SELECT, SQL dilinde veritabanından veri okumak için kullanılan temel komuttur. Bir tablodaki tüm kayıtları veya yalnızca belirli sütunları görüntülemek amacıyla kullanılır. Veritabanı üzerinde herhangi bir değişiklik yapmaz; yalnızca istenen verileri sorgulayarak kullanıcıya gösterir.

SELECT komutu, genellikle WHERE, ORDER BY ve GROUP BY gibi diğer SQL ifadeleriyle birlikte kullanılarak daha özelleştirilmiş sorgular oluşturulmasını sağlar.

### Tablodaki Tüm Verileri Listeleme

```sql
SELECT *
FROM Users;
```

Bu sorgu, **Users** tablosundaki tüm sütunları ve tüm kayıtları getirir.

### Belirli Sütunları Listeleme

```sql
SELECT ad, soyad
FROM Users;
```

Bu sorgu yalnızca **ad** ve **soyad** sütunlarını görüntüler.

### Koşullu Sorgu

```sql
SELECT *
FROM Users
WHERE id = 2;
```

Bu sorgu, **id** değeri 2 olan kullanıcıya ait bilgileri getirir.



## JOIN

İlişkisel veritabanlarında bilgiler çoğu zaman farklı tablolarda saklanır. Bu nedenle birden fazla tablodaki verileri birlikte görüntülemek gerektiğinde JOIN işlemi kullanılır.

JOIN, ortak bir sütun üzerinden tablolar arasında ilişki kurarak tek bir sorgu sonucunda birden fazla tablodaki bilgilerin bir araya getirilmesini sağlar. Bu yöntem veri tekrarını önlediği gibi verilerin daha düzenli yönetilmesine de katkı sağlar.

En yaygın kullanılan JOIN türü **INNER JOIN**'dir. INNER JOIN, yalnızca her iki tabloda da eşleşen kayıtları getirir.

### Örnek

**Users**

| id | ad |
|----|------|
| 1 | Ahmet |
| 2 | Ayşe |

**Orders**

| id | user_id | ürün |
|----|---------|--------|
| 1 | 1 | Laptop |
| 2 | 2 | Telefon |

### SQL Örneği

```sql
SELECT Users.ad,
       Orders.ürün
FROM Users
INNER JOIN Orders
ON Users.id = Orders.user_id;
```

### Sonuç

| ad | ürün |
|------|---------|
| Ahmet | Laptop |
| Ayşe | Telefon |

Bu sorgu sayesinde sipariş bilgileri ile kullanıcı bilgileri tek bir sonuç tablosunda görüntülenebilir.



## GROUP BY

GROUP BY ifadesi, aynı özelliğe sahip kayıtları gruplandırmak amacıyla kullanılır. Genellikle COUNT(), SUM(), AVG(), MAX() ve MIN() gibi toplulaştırma fonksiyonları ile birlikte kullanılır.

Bu ifade sayesinde veriler belirli kriterlere göre özetlenebilir ve analiz edilebilir.

Örneğin bir şirket, her şehirde kaç müşterisinin bulunduğunu öğrenmek isteyebilir. Bu durumda GROUP BY kullanılarak aynı şehirde bulunan kayıtlar tek grup altında toplanır.

### SQL Örneği

```sql
SELECT sehir,
COUNT(*) AS KullaniciSayisi
FROM Users
GROUP BY sehir;
```

### Sonuç

| Şehir | Kullanıcı Sayısı |
|--------|------------------|
| İstanbul | 15 |
| Ankara | 8 |
| İzmir | 5 |

Bu sorgu, her şehirde bulunan kullanıcı sayısını hesaplayarak özet bir sonuç oluşturur.



## Index

Veritabanlarında kayıt sayısı arttıkça verilere erişim süresi de uzayabilir. Özellikle milyonlarca kaydın bulunduğu tablolarda istenen veriye ulaşmak için tüm kayıtların tek tek taranması sorguların yavaş çalışmasına neden olur. Bu sorunu azaltmak amacıyla **Index** yapısı kullanılır.

Index, belirli sütunlar üzerinde oluşturulan ve verilere daha hızlı erişilmesini sağlayan özel bir veri yapısıdır. Bir kitabın sonundaki dizin (index) mantığına benzer şekilde çalışır. Kitabın tamamını tek tek okumak yerine dizin yardımıyla ilgili sayfaya doğrudan ulaşılabilir. Veritabanlarında da Index sayesinde istenen kayda daha kısa sürede erişilebilir.

Özellikle sık kullanılan arama ve filtreleme işlemlerinde Index kullanımı sorgu performansını önemli ölçüde artırır. Ancak her sütun için Index oluşturmak doğru bir yaklaşım değildir. Çünkü Index yapıları ek depolama alanı kullanır ve veri ekleme, silme veya güncelleme işlemlerinde de güncellenmeleri gerektiğinden bu işlemleri bir miktar yavaşlatabilir.

Bu nedenle Index'ler, sorgularda sık kullanılan sütunlar üzerinde oluşturulmalıdır.

### SQL Örneği

```sql
CREATE INDEX idx_users_name
ON Users(ad);
```

Yukarıdaki örnekte **Users** tablosundaki **ad** sütunu üzerinde bir Index oluşturulmuştur. Böylece kullanıcı adına göre yapılan sorgular daha hızlı çalışacaktır.



## Gerçek Hayattan Bir Örnek

Bir e-ticaret uygulaması geliştirildiğini düşünelim. Bu sistemde kullanıcılar, ürünler ve siparişler farklı tablolarda saklanmaktadır.

- **Users** tablosu kullanıcı bilgilerini,
- **Products** tablosu ürün bilgilerini,
- **Orders** tablosu ise sipariş bilgilerini içerir.

Bir kullanıcı sipariş geçmişini görüntülemek istediğinde sistem yalnızca Orders tablosunu değil, aynı zamanda Users ve Products tablolarını da kullanır. Siparişi veren kullanıcı bilgisi Users tablosundan, satın alınan ürün bilgileri ise Products tablosundan alınır. Bu işlem sırasında tablolar arasında JOIN kullanılarak ilişkiler kurulabilir.

Ayrıca sistemde milyonlarca sipariş kaydı bulunduğu düşünüldüğünde, kullanıcı veya sipariş numarası üzerinde oluşturulan Index yapıları sayesinde sorgular çok daha kısa sürede tamamlanabilir. Böylece uygulamanın performansı artar ve kullanıcılar verilere daha hızlı erişebilir.



## SQL Komutlarının Kısa Özeti

| Komut | Açıklama |
|-------|----------|
| SELECT | Veritabanından veri okumak için kullanılır. |
| INSERT | Tabloya yeni kayıt ekler. |
| UPDATE | Mevcut kayıtları günceller. |
| DELETE | Kayıtları siler. |
| WHERE | Belirli koşullara göre filtreleme yapar. |
| ORDER BY | Sonuçları istenilen sıraya göre sıralar. |
| GROUP BY | Aynı özellikteki kayıtları gruplandırır. |
| JOIN | Birden fazla tabloyu ilişkilendirerek sorgular. |



## En İyi Uygulamalar

Veritabanı tasarımı yapılırken bazı temel prensiplere dikkat edilmesi hem performans hem de veri bütünlüğü açısından önemlidir.

- Her tabloda benzersiz bir Primary Key bulunmalıdır.
- Tablolar arasındaki ilişkiler Foreign Key kullanılarak oluşturulmalıdır.
- Gereksiz veri tekrarından kaçınılmalıdır.
- Sorgularda mümkün olduğunca yalnızca ihtiyaç duyulan sütunlar seçilmelidir.
- Büyük veri kümelerinde performansı artırmak amacıyla uygun sütunlarda Index kullanılmalıdır.
- Gereksiz sayıda Index oluşturmaktan kaçınılmalıdır.



## Sonuç

İlişkisel veritabanları, modern yazılım geliştirme süreçlerinin temel bileşenlerinden biridir. Verilerin düzenli bir şekilde saklanması, tablolar arasında ilişki kurulması ve sorguların verimli bir şekilde çalıştırılması için SQL'in sunduğu temel kavramların iyi anlaşılması gerekir.

Bu dokümanda ilişkisel veritabanı yapısı, Primary Key ve Foreign Key kavramları ile SELECT, JOIN, GROUP BY ve Index gibi temel SQL konuları ele alınmıştır. Bu kavramlar yalnızca SQL öğrenme sürecinde değil, aynı zamanda gerçek dünya projelerinde veritabanı tasarımı ve yönetimi açısından da önemli bir yere sahiptir.

Temel SQL bilgisine sahip olmak, daha karmaşık veritabanı işlemlerini öğrenmek için sağlam bir altyapı oluşturur ve geliştiricilerin daha verimli uygulamalar geliştirmesine katkı sağlar.



## Kaynakça

1. PostgreSQL Documentation – https://www.postgresql.org/docs/
2. MySQL Documentation – https://dev.mysql.com/doc/
3. Microsoft Learn – SQL Documentation – https://learn.microsoft.com/sql/
4. W3Schools SQL Tutorial – https://www.w3schools.com/sql/
5. GeeksforGeeks SQL – https://www.geeksforgeeks.org/sql/

