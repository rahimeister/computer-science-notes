# TESTİNG MANTIĞI

## 1. Testing Nedir?

Testing (Yazılım Testi), geliştirilen yazılımın beklenen şekilde çalışıp çalışmadığını kontrol etmek için gerçekleştirilen işlemlerdir.

Yazılım geliştirme sürecinde yazılan kodun yalnızca çalışması yeterli değildir. Kodun farklı senaryolarda doğru sonuç üretmesi, beklenmeyen durumlarda hata vermemesi ve yapılan yeni geliştirmelerin mevcut işlevleri bozmaması gerekir.

Testing sayesinde:

- Yazılım hataları erken aşamada tespit edilir.

- Kodun güvenilirliği artırılır.

- Yeni özellikler eklenirken mevcut işlevlerin bozulması önlenir.

- Kodun bakımı ve geliştirilmesi kolaylaşır.



## 2. Unit Test Nedir?

Unit Test (Birim Testi), yazılımın en küçük test edilebilir parçalarının bağımsız olarak kontrol edilmesidir.

Genellikle bir fonksiyonun, metodun veya sınıfın beklenen çıktıyı üretip üretmediği test edilir.

**Örnek**

Bir toplama fonksiyonumuz olduğunu düşünelim:

```
def toplama(a, b):
    return a + b
```

Bu fonksiyonun doğru çalışıp çalışmadığını kontrol edelim:

```
def test_toplama():
    assert toplama(5, 3) == 8
    assert toplama(10, 20) == 30
    assert toplama(-5, 5) == 0
```

Burada assert ifadesi, beklenen sonuç ile gerçek sonucu karşılaştırır.

Sonuç beklenen değerle aynıysa test başarılı olur. Farklıysa test başarısız olur.

**Unit Test Özellikleri**

- Küçük kod parçalarını test eder.

- Genellikle hızlı çalışır.

- Diğer sistem bileşenlerinden bağımsızdır.

- Hataların hangi fonksiyonda veya metotta olduğunu bulmayı kolaylaştırır.

**Kullanılan araçlar:**

- Python: PyTest, unittest

- C#: xUnit, NUnit, MSTest

- Java: JUnit



## 3. Integration Test Nedir?

Integration Test (Entegrasyon Testi), farklı yazılım bileşenlerinin birbiriyle doğru şekilde iletişim kurup kurmadığını kontrol eder.

Unit test tek bir fonksiyona odaklanırken integration test birden fazla bileşenin birlikte çalışmasını inceler.

**Örnek**

Bir kullanıcı kayıt sistemini düşünelim.

Bu sistemde:

1. Kullanıcı kayıt formunu doldurur

2. Uygulama kullanıcı bilgilerini işler.

3. Veritabanına bağlantı kurulur.

4. Kullanıcı bilgileri veritabanına kaydedilir.

Integration test, bu bileşenlerin birlikte doğru çalışıp çalışmadığını kontrol eder.

Örneğin kullanıcı kayıt fonksiyonu çağrıldığında bilgilerin gerçekten veritabanına kaydedilip kaydedilmediği test edilir.

## 4. E2E Test Nedir?

E2E (End-to-End) Test, bir uygulamanın başlangıcından son kullanıcıya ulaşan işlem sonucuna kadar bütün bir iş akışının test edilmesidir.

Bu testlerde sistem, gerçek kullanıcı davranışlarına benzer şekilde çalıştırılır.

**Örnek**

Bir e-ticaret uygulamasını düşünelim.

Kullanıcının:

- Siteye giriş yapması,

- Ürün araması,

- Ürünü sepete eklemesi,

- Sipariş oluşturması,

- Sipariş onayını görmesi.

Bu işlemlerin tamamının sorunsuz gerçekleşip gerçekleşmediği E2E test ile kontrol edilir.

**Kullanılan araçlar:**

- Selenium

- Playwright

- Cypress



## 5. Unit Test, Integration Test ve E2E Test Arasındaki Farklar

| Özellik | Unit Test | Integration Test | E2E Test |
|---|---|---|---|
| Test kapsamı | Tek birim | Birden fazla bileşen | Tüm iş akışı |
| Hız | Çok hızlı | Orta | Genellikle yavaş |
| Amaç | Fonksiyon doğruluğu | Bileşen uyumu | Kullanıcı senaryosu |
| Örnek | Toplama fonksiyonu | API ve veritabanı | Kullanıcı siparişi |

### Test Piramidi

Yazılım projelerinde genellikle aşağıdaki test piramidi yaklaşımı kullanılır:

- En altta çok sayıda Unit Test,
- Ortada daha az sayıda Integration Test,
- En üstte daha az sayıda E2E Test bulunur.

Bunun nedeni unit testlerin hızlı ve düşük maliyetli olması, E2E testlerin ise daha kapsamlı ve bakım maliyetinin daha yüksek olmasıdır.



## 6. Mocking Nedir?

Mocking, test sırasında gerçek bir bağımlılık yerine kontrollü, sahte bir nesne veya davranış kullanılmasıdır.

Bir fonksiyonu test ederken onun bağlı olduğu başka sistemlerin gerçekten çalışmasını istemeyebiliriz.

Örneğin:

- Veritabanı bağlantısı,

- Harici API servisleri,

- E-posta gönderim sistemi,

- Ödeme servisleri.

Bu bağımlılıkları test ortamında taklit etmek için mocking kullanılır.

**Mocking Neden Kullanılır?**

**1.Bağımsız test:** Test edilen fonksiyonun diğer sistemlere bağımlılığı azaltılır.

**2.Hız:** Gerçek API veya veritabanı bağlantısı kurmaya gerek kalmaz.

**3.Kontrollü senaryolar:** Başarılı ve başarısız yanıtlar kolayca simüle edilir.

**4.Maliyet:** Harici servislerin gereksiz yere çağrılması önlenir.

**5.Güvenilirlik:** İnternet bağlantısı veya harici servislerin durumundan bağımsız test yapılabilir.

**Python ile Mocking Örneği**

```python
from unittest.mock import Mock

api = Mock()

api.get_user.return_value = {
    "id": 1,
    "name": "Ali"
}

def kullanici_getir(api, user_id):
    return api.get_user(user_id)

sonuc = kullanici_getir(api, 1)

assert sonuc["name"] == "Ali"

api.get_user.assert_called_once_with(1)

```

Bu örnekte gerçek bir API çağrısı yapılmaz. Mock nesnesi, API'nin davranışını taklit eder ve önceden belirlenen sonucu döndürür.




## 7. Testing Neden Önemlidir?

Testing, yalnızca mevcut hataları bulmak için değil, yazılımın gelecekte güvenle geliştirilebilmesi için de önemlidir.

Örneğin bir projeye yeni özellik eklediğimizi düşünelim. Yeni özellik, daha önce sorunsuz çalışan başka bir fonksiyonun davranışını değiştirmiş olabilir.

Testler sayesinde bu değişikliklerin mevcut sistemi bozup bozmadığını kontrol edebiliriz.

Testing'in sağladığı avantajlar:

- Kod kalitesini artırır.

- Hataların erken tespit edilmesini sağlar.

- Geliştirme sürecinde güven oluşturur.

- Refactoring işlemlerini kolaylaştırır.

- Bakım maliyetlerini azaltabilir.

- Kodun büyümesini daha güvenli hale getirir.



## 8. Sonuç

Yazılım testleri, sürdürülebilir ve güvenilir yazılım geliştirmenin temel parçalarından biridir.

Unit testler küçük kod parçalarının doğruluğunu, integration testler bileşenlerin birlikte çalışmasını, E2E testler ise kullanıcıdan başlayan bütün iş akışının doğruluğunu kontrol eder.

Mocking ise test sırasında dış bağımlılıkları kontrol altına alarak daha bağımsız ve öngörülebilir testler yazmamızı sağlar.

Temel Kazanım: Testing yaklaşımını benimsemek, yazılımın yeni özelliklerle büyürken mevcut işlevlerini kaybetmesini önlemeye yardımcı olur. Böylece daha güvenilir, sürdürülebilir ve bakımı kolay yazılımlar geliştirilebilir.



## 9. Kaynakça

1. **Pytest Documentation:** https://docs.pytest.org/
   - Python ile Unit Test yazımı ve test süreçleri.

2. **Python unittest Documentation:** https://docs.python.org/3/library/unittest.html
   - Unit Test ve Mocking işlemleri.

3. **Python unittest.mock Documentation:** https://docs.python.org/3/library/unittest.mock.html
   - Mock nesneleri ve bağımlılıkların taklit edilmesi.

4. **Playwright Documentation:** https://playwright.dev/docs/intro
   - E2E test süreçleri.

5. **Martin Fowler – Practical Test Pyramid:** https://martinfowler.com/articles/practical-test-pyramid.html
   - Unit, Integration ve E2E testlerinin karşılaştırılması ve test piramidi yaklaşımı.