<div align="center">

# 🐧 Linux Bash Scripting & RegEx

### Sıfırdan İleri Seviyeye Bash Script Yazımı, Sistem Otomasyonu ve Regular Expressions Rehberi

[![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.linux.org/)
[![Regex](https://img.shields.io/badge/RegEx-2496ED?style=for-the-badge&logo=regex&logoColor=white)](https://en.wikipedia.org/wiki/Regular_expression)
[![License](https://img.shields.io/badge/License-Educational-blue?style=for-the-badge)](#-lisans)
[![Diller](https://img.shields.io/badge/Dil-TR%20%2F%20EN-green?style=for-the-badge)](#-dosya-yapısı)

**Log analizi • Sistem yönetimi • DevOps otomasyonu • CI/CD • AI Agent altyapısı**

[Genel Bakış](#-genel-bakış) •
[Dosya Yapısı](#-dosya-yapısı) •
[İçerikler](#-içerikler) •
[Hızlı Başlangıç](#-hızlı-başlangıç) •
[Öğrenme Yolu](#-önerilen-öğrenme-yolu) •
[SSS](#-sık-sorulan-sorular)

</div>

---

## 🎯 Genel Bakış

**Linux Bash Scripting & RegEx**, terminal üzerinden çalışan her geliştiricinin, sistem yöneticisinin ve otomasyon mühendisinin ihtiyaç duyacağı temelden ileri seviyeye kadar uzanan bilgiyi tek bir repoda toplar.

Bu doküman seti şu problemleri çözmek için tasarlanmıştır:

| Problem | Bu Repodaki Çözüm |
|---|---|
| Binlerce satırlık log dosyasında hata aramak saatler sürüyor | `grep` + RegEx ile saniyeler içinde pattern eşleştirme |
| Tekrarlayan terminal komutları zaman kaybettiriyor | Bash script'leri ile tam otomasyon |
| Scriptler ortam değiştiğinde çöküyor | Clean code, mutlak yol, `set -euo pipefail` pratikleri |
| Sistem sağlığı manuel kontrol ediliyor | Hazır `health-check.sh`, `disk-alert.sh` şablonları |
| CI/CD ve AI ajanları için sağlam script altyapısı eksik | Hata yönetimi, `trap`, exit code standartları |

> 💡 **Kimler için uygun?** Linux'a yeni başlayanlar, DevOps mühendisleri, backend geliştiriciler, sistem yöneticileri ve otonom otomasyon/AI-agent altyapısı kuranlar.

---

## 📁 Dosya Yapısı

```
linux_bash_scripting-Regex/
└── linux_bash_scripting&Regex/
    ├── Bash_Scripting_Kapsamli_Gelistirici_Notlari.md      # Otomasyon odaklı geliştirici rehberi
    ├── Grep_ve_RegEx_Kapsamli_Notlari.md                   # grep + RegEx derinlemesine
    ├── Module-14-Bash-Scripting-Giris-TR.md                # Modül 14 — Türkçe (1800+ satır)
    └── Module-14-Introduction-to-Bash-Scripting.md         # Modül 14 — İngilizce (1800+ satır)
```

| Dosya | Dil | Satır | Seviye | Odak |
|---|:---:|:---:|---|---|
| `Bash_Scripting_Kapsamli_Gelistirici_Notlari.md` | 🇹🇷 | ~97 | Orta | Otomasyon mühendisliği zihniyeti |
| `Grep_ve_RegEx_Kapsamli_Notlari.md` | 🇹🇷 | ~79 | Orta | Pattern matching & log filtreleme |
| `Module-14-Bash-Scripting-Giris-TR.md` | 🇹🇷 | ~1868 | Başlangıç → İleri | Uçtan uca Bash scripting müfredatı |
| `Module-14-Introduction-to-Bash-Scripting.md` | 🇬🇧 | ~1868 | Başlangıç → İleri | Yukarıdakinin İngilizce tam sürümü |

---

## 📚 İçerikler

### 1️⃣ Bash Scripting ve Sistem Otomasyonu — Kapsamlı Geliştirici Notları

> 📄 `Bash_Scripting_Kapsamli_Gelistirici_Notlari.md`

Günlük tekrarlayan geliştirme ve sistem yönetimi görevlerini **milisaniyeler içinde otonom** hale getirmeye odaklanan, üst seviye bir mühendislik rehberidir.

<details>
<summary><strong>📖 Bölümleri görüntülemek için tıkla</strong></summary>

| # | Bölüm | Öne Çıkanlar |
|:-:|---|---|
| 1 | Arayüzler ve Bash Kavramı | GUI vs CLI vs Shell vs Bash farkları |
| 2 | Geliştirme Ortamının Hazırlanması | macOS (`zsh` → `bash` geçişi), Windows (WSL kurulumu) |
| 3 | Manuel Log Analiz Komutları | `grep "ERROR" app.log`, `grep -c`, `find -mtime -1` |
| 4 | Bash Script Dosyası Oluşturma | `touch`, shebang (`#!/bin/bash`), `chmod +x` |
| 5 | Clean Code ve Esneklik | Hard-coded değerlerden kaçınma, mutlak yol kullanımı |
| 6 | Gelişmiş Veri Yapıları | Diziler (arrays), Command Substitution `$(...)` |
| 7 | Dinamik Analiz İçin Döngüler | `for` döngüleriyle dosya/log taraması |
| 8 | Loglama ve Raporlama | Redirection (`>`, `>>`, `2>&1`) |
| 9 | Koşullu İfadeler ile Otonom Uyarılar | Eşik değer bazlı otomatik alarm mantığı |
| 10 | Sonuç ve Reel Kullanım Senaryoları | Üretim ortamı örnekleri |

</details>

---

### 2️⃣ grep ve RegEx Kapsamlı Notları

> 📄 `Grep_ve_RegEx_Kapsamli_Notlari.md`

Büyük ve karmaşık log dosyalarıyla (`auth.log` gibi) çalışırken veri filtreleme, pattern eşleştirme ve manipülasyon üzerine — CI/CD ve AI-agent senaryoları göz önünde bulundurularak hazırlanmıştır.

<details>
<summary><strong>📖 Bölümleri görüntülemek için tıkla</strong></summary>

| # | Bölüm | Öne Çıkanlar |
|:-:|---|---|
| 1 | RegEx Nedir? | Karmaşık karakter yığınlarında pattern arama mantığı |
| 2 | GREP Komutu ve Temel Pattern Eşleştirme | `grep "apple" fruits.txt`, GREP açılımı hatırlatıcısı |
| 3 | Meta Karakterler | `.` (herhangi bir karakter), `^` (satır başı), `$` (satır sonu) |
| 4 | Gelişmiş Filtreleme | `[aeiou]` (karakter kümesi), `[^aeiou]` (negasyon), `\|` (OR) |
| 5 | Quantifiers | Kaç kez eşleşeceğini belirleyen niceleyiciler |
| 6 | Escape Karakteri `\` | Özel karakterleri literal olarak eşleştirme |
| 7 | Real-World Application | `auth.log` üzerinde uygulamalı analiz |
| 8 | Otonom Otomasyonlardaki Rolü | RegEx'in AI-agent ve otomasyon boru hatlarındaki yeri |

**Örnek komutlar:**
```bash
grep "ERROR" application.log          # Sadece ERROR satırlarını getir
grep -c "FATAL" system.log            # FATAL sayısını say
grep "^b" fruits.txt                  # "b" ile başlayanlar
grep "y$" fruits.txt                  # "y" ile bitenler
grep "apple\|blueberry" fruits.txt    # OR eşleştirme
grep "[^aeiou]" fruits.txt            # Sesli harf İÇERMEYENLER
```

</details>

---

### 3️⃣ Modül 14: Bash Scripting'e Giriş (TR) 🇹🇷

> 📄 `Module-14-Bash-Scripting-Giris-TR.md` — **En kapsamlı doküman (1800+ satır)**

Sıfırdan başlayıp gerçek dünya script şablonlarına kadar uzanan, örnek kod bloklarıyla desteklenmiş tam bir eğitim modülü.

<details>
<summary><strong>📖 14.1 – 14.14 arası tüm konuları görüntülemek için tıkla</strong></summary>

| # | Konu | İçerik Detayı |
|---|---|---|
| **14.1** | Bash Scripting Nedir? | Neden öğrenilmeli, Bash vs diğer scripting dilleri |
| **14.2** | İlk Scriptiniz | Dosya oluşturma, shebang, `chmod +x`, Hello World |
| **14.3** | Değişkenler | Tanımlama kuralları, string/sayısal türler, ortam vs yerel, quoting (`" "` vs `' '`), read-only değişkenler |
| **14.4** | Kullanıcı Girdisi | `read -p`, `read -s` (şifre), `read -t` (timeout), çoklu değer okuma |
| **14.5** | Aritmetik İşlemler | `$(( ))`, `let`, `expr`, `bc` (ondalık), artırma/azaltma |
| **14.6** | Koşul İfadeleri | `if/elif/else`, string kontrolleri, dosya kontrolleri, `&&`/`\|\|`/`!`, `[[ ]]` |
| **14.7** | Karşılaştırma Operatörleri | `-eq -ne -gt -ge -lt -le`, string (`=`, `!=`, `-z`, `-n`), dosya testleri (`-f -d -e -r -w -x -s`) |
| **14.8** | Case İfadeleri | `case` sözdizimi, pattern eşleştirme |
| **14.9** | Döngüler | `for` (liste/aralık/adım/C-tarzı/dosya), `while`, `until`, `break`/`continue` |
| **14.10** | Fonksiyonlar | Tanımlama, parametreler (`$1 $2...`), dönüş değerleri, `local` değişkenler |
| **14.11** | Dosyalarla Çalışmak | Satır satır okuma, array'e okuma, yazma/ekleme, heredoc |
| **14.12** | Komut Satırı Argümanları | `$0 $1-$9 $# $@ $* $? $$ $!` özel değişkenleri |
| **14.13** | Çıkış Kodları ve Hata Yönetimi | `set -euo pipefail`, `trap` ile temizlik, güvenli script şablonu |
| **14.14** | Pratik Script Örnekleri | 5 tam üretim senaryosu (aşağıda listelenmiştir) |

</details>

<details>
<summary><strong>🛠️ 20 Hazır Script Örneği (kopyala-yapıştır kullanıma hazır)</strong></summary>

| Script | Amaç |
|---|---|
| `system-info.sh` | Sistem bilgilerini görüntüler |
| `greeting.sh` | Etkileşimli selamlama |
| `calculator.sh` | Basit hesap makinesi |
| `tip-calculator.sh` | Bahşiş hesaplayıcı |
| `file-checker.sh` | Dosya özelliklerini kontrol eder |
| `service-menu.sh` | Basit servis yöneticisi |
| `day-info.sh` | Haftanın günü bilgisi |
| `countdown.sh` | Geri sayım zamanlayıcısı |
| `rename-files.sh` | Toplu dosya yeniden adlandırma |
| `multiplication-table.sh` | Çarpım tablosu |
| `log-analyzer.sh` | Log dosyası analiz aracı |
| `arguments.sh` | Komut satırı argümanlarını gösterir |
| `validate-args.sh` | Argüman doğrulama |
| `process-files.sh` | Birden fazla dosyayı işler |
| `template.sh` | Güvenli, üretime hazır script şablonu |
| `disk-alert.sh` | Disk kullanımı eşik uyarısı |
| `backup.sh` | Dizin yedekleme |
| `create-users.sh` | Dosyadan toplu kullanıcı oluşturma |
| `health-check.sh` | Uptime, CPU, RAM, disk, süreç, ağ raporu |
| `system-tool.sh` | Menü tabanlı interaktif sistem aracı |

</details>

---

### 4️⃣ Module 14: Introduction to Bash Scripting (EN) 🇬🇧

> 📄 `Module-14-Introduction-to-Bash-Scripting.md`

Yukarıdaki modülün **birebir İngilizce tam sürümü** — aynı 14.1–14.14 içindekiler tablosu, aynı 20 script örneği, uluslararası ekipler ve İngilizce dokümantasyon gereken projeler için.

---

## 🗺️ Önerilen Öğrenme Yolu

```mermaid
graph LR
    A[📘 Modül 14: Giriş] --> B[Değişkenler & Girdi]
    B --> C[Koşullar & Karşılaştırma]
    C --> D[Döngüler & Fonksiyonlar]
    D --> E[Hata Yönetimi & set -euo pipefail]
    E --> F[🔍 grep & RegEx Notları]
    F --> G[🚀 Otomasyon Geliştirici Notları]
    G --> H[✅ Kendi Production Scriptini Yaz]
```

1. **Başlangıç:** `Module-14-Bash-Scripting-Giris-TR.md` ile temelleri öğren (14.1 → 14.9)
2. **Orta seviye:** Fonksiyonlar, dosya işlemleri, argümanlar (14.10 → 14.12)
3. **İleri seviye:** Hata yönetimi ve güvenli script şablonu (14.13 → 14.14)
4. **Filtreleme becerisi:** `Grep_ve_RegEx_Kapsamli_Notlari.md` ile pattern matching'i pekiştir
5. **Mühendislik zihniyeti:** `Bash_Scripting_Kapsamli_Gelistirici_Notlari.md` ile otomasyon mimarisi kur

---

## 🚀 Hızlı Başlangıç

```bash
# 1. Repoyu klonla
git clone https://github.com/BerattCelikk/linux_bash_scripting-Regex.git
cd "linux_bash_scripting-Regex/linux_bash_scripting&Regex"

# 2. İstediğin notu terminalde oku
cat Module-14-Bash-Scripting-Giris-TR.md | less

# 3. Ya da bir editörde aç (VS Code önerilir)
code .
```

**Kendi ilk script'ini oluştur:**

```bash
touch ornek-script.sh
chmod +x ornek-script.sh

cat << 'EOF' > ornek-script.sh
#!/bin/bash
set -euo pipefail

echo "Merhaba, $(whoami)! Bugün $(date '+%Y-%m-%d')."
EOF

./ornek-script.sh
```

---

## ⭐ Öne Çıkan Teknik Konular

<table>
<tr>
<td width="50%">

**🔤 RegEx Meta Karakterleri**
```
.    → herhangi bir karakter
^    → satır başı
$    → satır sonu
[ ]  → karakter kümesi
[^ ] → negasyon
|    → OR
\    → escape
```

</td>
<td width="50%">

**🛡️ Güvenli Script Standartları**
```bash
#!/bin/bash
set -e   # hata varsa dur
set -u   # tanımsız değişken hatası
set -o pipefail   # pipe hatalarını yakala

trap cleanup EXIT   # çıkışta temizlik
```

</td>
</tr>
</table>

- **Değişken güvenliği:** her zaman `"$degisken"` şeklinde quote kullan
- **Fonksiyon izolasyonu:** fonksiyon içi değişkenlerde `local` kullan
- **Anlamlı isimlendirme:** `x` yerine `disk_kullanim_yuzdesi`
- **Argüman doğrulama:** script çalışmadan önce `$#` kontrolü yap
- **Gerçek dünya senaryoları:** `auth.log` analizi, disk uyarısı, otomatik yedekleme, toplu kullanıcı oluşturma, sistem sağlık kontrolü

---

<div align="center">

**[BerattCelikk](https://github.com/BerattCelikk)** tarafından hazırlanmıştır

⭐ Faydalı bulduysanız repoya yıldız vermeyi unutmayın!

</div>
