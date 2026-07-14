# 🚀 Linux Sistem Otomasyonu: grep ve RegEx Kapsamlı Notları

Bu doküman, büyük ve karmaşık log dosyaları ile çalışırken (örneğin auth.log), grep komutu ve Regular Expressions (RegEx) kullanarak otonom otomasyonlar kurmayı, verileri filtrelemeyi ve manipüle etmeyi anlatır. Özellikle CI/CD süreçleri, sistem yönetimi ve AI ajanları (autonomous agents) kurgularken ihtiyaç duyulacak en kritik konular ele alınmıştır.

## 📌 1. RegEx (Regular Expressions) Nedir?
RegEx, karmaşık karakter yığınları (sea of characters) içinde spesifik bir metin örüntüsünü (pattern) bulmayı sağlayan gizli bir koddur. Binlerce satırlık log dosyasında sadece IP adreslerini veya "error" kelimesini saniyeler içinde çekip almanıza yarar. Bu özellik manuel olarak yapılamayacak hızda otomasyonlar yazmanızı sağlar.

## 📌 2. GREP Komutu ve Temel Pattern Eşleştirme (Pattern Matching)
GREP (Global Regular Expression Print), Linux'ta belirtilen bir desene uyan satırları arayıp ekrana basan ana komuttur.

**Pratik Kısaltma (Best Practice):** GREP kelimesini "**G**ather **R**elevant **E**rrors **P**romptly" (İlgili Hataları Hızlıca Topla) olarak aklınızda tutabilirsiniz.

* **Tam Kelime Eşleşmesi (Exact Word):**
    * *Komut:* `grep "apple" fruits.txt`
    * *İşlev:* Dosyada sadece "apple" kelimesinin geçtiği satırları getirir.

## 📌 3. Meta Karakterler (Metacharacters): RegEx'in Süper Kahramanları
Meta karakterler, sıradan harflerin yapamadığı eşleştirme, gruplama ve manipülasyonları yapmamızı sağlayan özel sembollerdir.

* **`.` (Nokta) - Herhangi Bir Tek Karakter:**
    * *İşlev:* Yeni satır (newline) hariç herhangi bir tek karakterle eşleşir. Wildcard (joker) gibidir.
    * *Örnek:* `grep "c.e" fruits.txt` (Örneğin "coe", "c1e", "c e" gibi eşleşmeler bulur).
* **`^` (Şapka/Caret) - Satır Başlangıcı:**
    * *İşlev:* Belirtilen karakterin sadece satırın en başında olması durumunda eşleşir.
    * *Örnek:* `grep "^b" fruits.txt` (Sadece "b" ile başlayan satırları getirir).
* **`$` (Dolar) - Satır Sonu:**
    * *İşlev:* Belirtilen karakterin satırın en sonunda olması durumunda eşleşir.
    * *Örnek:* `grep "y$" fruits.txt` (Sadece "y" harfi ile biten satırları getirir).

## 📌 4. Meta Karakterlerle Gelişmiş Filtreleme (Karakter Kümeleri)
Köşeli parantezler `[ ]` kullanılarak birden fazla karakter kombinasyonu aranabilir.

* **Karakter Seti İçi Arama (Within a Set):**
    * *Komut:* `grep "[aeiou]" fruits.txt`
    * *İşlev:* Satırda a, e, i, o, veya u sesli harflerinden herhangi birisi varsa eşleşir.
* **Karakter Seti Dışı Arama (Not Within a Set - Negation):**
    * *Komut:* `grep "[^aeiou]" fruits.txt`
    * *İşlev:* Köşeli parantez içindeki `^` işareti seti "olumsuzlar" (negates). Sadece sesli harf **içermeyen** (sessiz harfler vb.) karakterlerle eşleşir.
* **`|` (Veya / OR Operatörü):**
    * *Komut:* `grep "apple\|blueberry" fruits.txt`
    * *İşlev:* İki kelimeden herhangi birini bulursa eşleşir. Boru (pipe) karakterinin anlamını korumak için önüne ters eğik çizgi `\` (escape character) konur.

## 📌 5. Quantifiers (Belirleyiciler): Kaç Kez Eşleşecek?
Karakterlerin veya grupların kaç defa tekrar edeceğini belirlerler.

* **`*` (Asterisk) - Sıfır veya Daha Fazla (Zero or More):**
    * *Komut:* `grep "ap*le" fruits.txt`
    * *İşlev:* 'a' harfinden sonra sıfır adet veya birden fazla 'p' gelebileceğini söyler ("ale", "aple", "appple").
* **`+` (Artı) - Bir veya Daha Fazla (One or More):**
    * *İşlev:* Karakterden en az 1 tane olmak zorundadır.
* **`?` (Soru İşareti) - Sıfır veya Bir (Zero or One):**
    * *İşlev:* Karakter ya hiç yoktur ya da en fazla 1 tane vardır (opsiyonel karakterler için).
* **`{ }` (Süslü Parantezler) - Spesifik Tekrar Sayısı:**
    * *Tam Sayı:* `grep "r\{2\}" fruits.txt` (Tam olarak iki tane ardışık "r" olan satırları bulur - örn: "strawberry").
    * *Aralık (Range):* `grep "a\{1,3\}" fruits.txt` (Bir ile üç adet ardışık "a" olanları bulur).
* **Kombinasyon Örneği (Tekrarlayan Sesli Harfler):**
    * *Komut:* `grep "[aeiou]\{2\}" fruits.txt`
    * *İşlev:* "a,e,i,o,u" karakter setinden herhangi iki tanesinin yan yana geldiği satırları getirir.

## 📌 6. Escape Karakteri (Ters Eğik Çizgi `\`)
Meta karakterleri asıl özellikleri ile değil de normal, düz bir karakter (literal character) olarak kullanmak istediğinizde devreye girer.

* *Örnek:* Normalde komutlarda boşluk bırakmak sıkıntı yaratabilir. Ancak `grep "\ " fruits.txt` yazarsanız, sadece boşluk barındıran satırları (örn: "apple pie") bulursunuz. `\` buradaki metakarakter işlevini sıfırlar.

## 📌 7. Real-World Application: auth.log Analizi
Bir Linux sunucusunda (özellikle DevOps/Sistem Yöneticisi iseniz), `auth.log` dosyası devasa boyuttadır ve login işlemlerini (başarılı/başarısız SSH denemeleri, sudo komutları) barındırır. AI ajanlarınızı bu dosyaları otonom okuyacak şekilde ayarlarken bu metotlar kullanılır:

1.  **Potansiyel Hataları Ayıklama:**
    * `grep "session" /var/log/auth.log` (Sistemdeki oturumların (açılış/kapanış) durumlarını getirir).
2.  **Spesifik Kullanıcı Aktivitelerini Gözlemleme:**
    * *Komut:* `grep "sudo.*jeremy" /var/log/auth.log`
    * *Analiz:* "sudo" stringini bulur, `.*` ile aradaki tüm rastgele (wildcard) karakterleri kabul eder ve sonunda "jeremy" kelimesini bulur. Yani Jeremy adlı kullanıcının çalıştırdığı tüm `sudo` komutlarını süzer.
3.  **SSH Login Olayları:**
    * `grep "sshd.*session opened" /var/log/auth.log` (Sisteme yapılan başarılı SSH girişlerini filtreler).

## 📌 8. Otonom Otomasyonlardaki Rolü
Manuel arama veya bir dosyayı baştan aşağı okuma dönemini bitirip, bunu koda döktüğümüzde:
* **Log Rotasyonu ve Monitörleme:** Yazdığınız bir betik her gece log dosyalarını `grep` ve `regex` ile okuyabilir, eğer auth.log dosyasında çok fazla hatalı giriş (brute-force) tespit ederse otomatik olarak uyarı veya bloklama mekanizmasını tetikleyebilir.
* **Veri Temizleme:** Ham text (raw) verilerinden IP adreslerini, email adreslerini, ve error kodlarını (Örn. API yanıtlarındaki JSON hataları) çekmek için RegExp örüntüleri otomasyon scriptinizin en büyük yardımcısı olur.
