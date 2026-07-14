# 🚀 Bash Scripting ve Sistem Otomasyonu: Kapsamlı Geliştirici Notları

Bu doküman, günlük tekrarlayan (repetitive) geliştirme ve sistem yönetimi görevlerini milisaniyeler içinde otonom hale getirmek için hazırlanmış kapsamlı bir rehberdir.

## 📌 1. Arayüzler ve Bash Kavramı
* **GUI (Grafiksel Kullanıcı Arayüzü):** İkonlara tıkladığımız görsel arayüzdür. Manuel ilerler, otomasyon kurgulamak zordur.
* **CLI (Komut Satırı Arayüzü):** GUI'nin aksine yazılı komutlarla sistemle konuştuğumuz arayüzdür. 
    * *Avantajı:* Tek bir komutla yüzlerce işlem yapmak veya binlerce log satırını saniyeler içinde analiz etmek mümkündür.
* **Shell (Kabuk):** İşletim sisteminde (özellikle Linux'ta) yazılan komutları yorumlayan ve çalıştıran ana motor/programdır.
* **Bash (Bourne Again Shell):** Shell'in en popüler implementasyonudur. Bash, sadece komut girmek için değil; değişkenler, döngüler ve koşullar içeren tam teşekküllü bir programlama dilidir.

## 📌 2. Geliştirme Ortamının Hazırlanması
* **MacOS Kullanıcıları:** Bash halihazırda yüklüdür fakat sistem genellikle `zsh` (Z-Shell) kullanır.
    * *Geçiş:* Terminale sadece `bash` yazılarak anında Bash ortamına geçilir.
* **Windows Kullanıcıları:** En stabil ve Microsoft tarafından resmi desteklenen yöntem **WSL (Windows Subsystem for Linux)** kurmaktır. Sanal makine (VM) hantallığı olmadan native bir Linux ortamı sunar.

## 📌 3. Manuel Log Analiz Komutları (Scripting Öncesi)
Script yazmaya başlamadan önce, manuel olarak sistem loglarını nasıl taradığımızı anlamak gerekir:
* **`grep` Komutu:** Belirli bir kelimeyi dosya içinde arar.
    * *Örnek:* `grep "ERROR" application.log` (Sadece ERROR geçen satırları getirir).
    * **`-c` Parametresi:** Sadece sayım (count) yapar. `grep -c "FATAL" system.log` (Kaç tane FATAL hatası olduğunu sayar).
* **`find` Komutu:** Gelişmiş dosya arama komutudur.
    * *Zaman Bazlı Arama:* `find . -name "*.log" -mtime -1` 
        * `.`: Bulunulan dizin (current directory).
        * `-name "*.log"`: Sonu .log ile bitenler (Regex bozulmaması için tırnak içine alınır).
        * `-mtime -1`: Modifikasyon zamanı 24 saatten (1 günden) az olanlar. (Sadece güncel dosyaları tarayarak kodun çalışma hızını artırır).

## 📌 4. Bash Script Dosyası Oluşturma ve Temel Kurallar
Yazılım geliştirme süreçlerinde manuel terminal komutları yerine tüm mantık tek bir script dosyasına bağlanır.
* **Dosya Oluşturma:** `touch analyze_logs.sh` (Uzantının `.sh` olması Unix sistemler için zorunlu değildir, ancak editörlerin syntax highlighting yapabilmesi ve geliştiricilerin dosyayı tanıması için bir best-practice'dir).
* **Shebang Satırı (`#!/bin/bash`):**
    * Scriptin en üst (ilk) satırına yazılır.
    * İşletim sistemine bu dosyanın spesifik olarak `bash` yorumlayıcısı ile çalıştırılması gerektiğini söyler.
* **Çalıştırma İzni (Execute Permission):**
    * Dosya oluşturulduğunda varsayılan olarak okunabilir/yazılabilirdir ama çalıştırılamaz.
    * `./analyze_logs.sh` yapıldığında `Permission Denied` alınır.
    * *Çözüm:* `chmod +x analyze_logs.sh` komutu ile executable (çalıştırılabilir) hale getirilir.

## 📌 5. Clean Code ve Esneklik: Değişkenler ve Mutlak Yollar
Script'in farklı ortamlarda veya modüllerde çökmeden çalışabilmesi için sabit (hard-coded) değerlerden kaçınılmalıdır.
* **Mutlak Yol (Absolute Path) Kullanımı:** 
    * Dosya yollarını `./logs` gibi göreceli vermek yerine `/Users/user/logs` şeklinde mutlak yol verilmelidir. Böylece script işletim sisteminin neresinden tetiklenirse tetiklensin log klasörünü doğru bulur.
* **Değişken Atamaları (Variables):**
    * *Kural:* Eşittir işaretinin etrafında asla boşluk bırakılmaz. `LOG_DIR="/Users/user/logs"`
    * *Kullanım:* Çağırılırken başına `$` işareti konur: `echo $LOG_DIR`
* **Çıktı Formatlama:**
    * `echo -e`: Escape (kaçış) karakterlerini yorumlamayı açar.
    * *Örnek:* `echo -e "\nLog Analizi Başlıyor...\n"` (`\n` ile alt satıra geçerek terminal çıktısını daha temiz hale getirir).

## 📌 6. Gelişmiş Veri Yapıları: Diziler ve Command Substitution
* **Diziler (Arrays):** Birden fazla değeri (örneğin farklı hata türlerini) tek değişkende tutmak için kullanılır.
    * *Tanımlama:* `ERROR_PATTERNS=("ERROR" "FATAL" "CRITICAL")` (Virgül kullanılmaz, değerler boşlukla ayrılır).
    * *İndeksleme:* Elemanlara erişirken süslü parantez zorunludur: `${ERROR_PATTERNS[1]}` (Çıktı: FATAL).
* **Command Substitution (Komut İkamesi):**
    * Bir Linux terminal komutunun bastığı çıktıyı yakalayıp bir Bash değişkenine atama işlemidir.
    * *Syntax:* `$()` yapısı kullanılır.
    * *Örnek:* `MODIFIED_LOGS=$(find "$LOG_DIR" -name "*.log" -mtime -1)` (Güncel log dosyalarının listesini otonom olarak değişkene çeker).

## 📌 7. Dinamik Analiz İçin Döngüler (For Loops)
Sistemdeki log dosyası sayısı veya aranacak hata tipi dinamik olarak değişebilir. Kod tekrarını önlemek için döngüler (For-Loop) kullanılır.
* **Temel Yapı:** 
    ```bash
    for LOG_FILE in $MODIFIED_LOGS; do
        # Dosya bazlı işlemler burada yapılır
    done
    ```
* **Dizilerde İterasyon Kuralı:**
    * Dizi elemanlarının tümünü ayrı ayrı varlıklar (individual entities) olarak döndürmek için `[@]` parametresi kullanılmalıdır.
    * *Doğru Kullanım:* `for PATTERN in "${ERROR_PATTERNS[@]}"; do`
    * (Eğer `*` kullanılırsa dizi içindeki tüm elemanları birleştirip tek bir uzun metin yapar ve kurgu bozulur).

## 📌 8. Loglama ve Raporlama (Redirection / Çıktı Yönlendirme)
Scriptin terminalde kayıp giden çıktılarını kalıcı bir dosya formatına dönüştürme işlemidir.
* **`>` (Overwrite - Üzerine Yazma):** 
    * Dosya yoksa oluşturur. Varsa, içindeki eski verileri tamamen siler ve sıfırdan yazar. 
    * *Kullanım Yeri:* Rapor dosyasının en başına ilk başlığı atarken (`echo "GÜNLÜK RAPOR" > report.txt`).
* **`>>` (Append - Sonuna Ekleme):** 
    * Dosyanın mevcut içeriğine dokunmaz, yeni veriyi (append) en alt satıra ekler. 
    * *Kullanım Yeri:* Döngü içerisinde analiz edilen her yeni log satırını bozmadan rapora aktarırken (`echo "$ERROR_COUNT" >> report.txt`).

## 📌 9. Koşullu İfadeler (Conditionals) ile Otonom Uyarılar
Otomatik bir script sadece logları okumakla kalmamalı, sistemde bir anomali gördüğünde karar verip uyarabilmelidir.
* **If-Else Yapısı:**
    * Köşeli parantez içlerinde, değişkenlerin yanında ve operatörlerde her zaman boşluk olmak zorundadır: `[ "$COUNT" -gt 10 ]`
    * Büyüktür işareti olarak `>` yerine **`-gt` (greater than)** kullanılır.
    * Başlangıç `if`, bitiş ise `fi` anahtar kelimesi ile ifade edilir.
* **Uyarı Senaryosu:** Hata sayısı 10'u geçerse terminalde acil durum bas.
    ```bash
    if [ "$ERROR_COUNT" -gt 10 ]; then
        echo -e "🚨 AKSİYON GEREKLİ: $LOG_FILE dosyasında limit aşımı ($ERROR_COUNT adet $PATTERN hatası)!"
    fi
    ```

## 📌 10. Sonuç ve Reel Kullanım Senaryoları
Yazılan bu script mantığı, modern CI/CD ve otomasyon döngülerinin omurgasıdır. Bu yetenekler ile şunlar kurgulanabilir:
* **Geliştirici Ortamı Kurulumu:** Projeye yeni katılan bir mühendisin makinesinde paket bağımlılıklarını kuran, repoları klonlayan ve ortam değişkenlerini ayarlayan tek bir `setup.sh` yazılabilir. Günler sürecek manuel işlemler tek tuşla biter.
* **Disk ve Kaynak Yönetimi:** Sunucularda disk dolmasını önlemek için eski log dosyalarını periyodik (örneğin cronjob aracılığıyla) tarayan; kapasite sınıra ulaştığında eski logları sıkıştıran veya silen otonom disk yönetim scriptleri kurgulanabilir.
