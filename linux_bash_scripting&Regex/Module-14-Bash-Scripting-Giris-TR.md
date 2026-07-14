# MODÜL 14: Bash Scripting'e Giriş

## İçindekiler
- [14.1 Bash Scripting Nedir?](#141-bash-scripting-nedir)
- [14.2 İlk Scriptiniz](#142-ilk-scriptiniz)
- [14.3 Değişkenler](#143-degiskenler)
- [14.4 Kullanıcı Girdisi](#144-kullanici-girdisi)
- [14.5 Aritmetik İşlemler](#145-aritmetik-islemler)
- [14.6 Koşul İfadeleri (if/else)](#146-kosul-ifadeleri-ifelse)
- [14.7 Karşılaştırma Operatörleri](#147-karsilastirma-operatorleri)
- [14.8 Case İfadeleri](#148-case-ifadeleri)
- [14.9 Döngüler](#149-donguler)
- [14.10 Fonksiyonlar](#1410-fonksiyonlar)
- [14.11 Scriptlerde Dosyalarla Çalışmak](#1411-scriptlerde-dosyalarla-calismak)
- [14.12 Komut Satırı Argümanları](#1412-komut-satiri-argumanlari)
- [14.13 Çıkış Kodları ve Hata Yönetimi](#1413-cikis-kodlari-ve-hata-yonetimi)
- [14.14 Pratik Script Örnekleri](#1414-pratik-script-ornekleri)
- [Özet](#ozet)

---

## 14.1 Bash Scripting Nedir?

### Neden Bash Scripting Öğrenmeli?

Bir **Bash script**, Bash shell'in otomatik olarak çalıştırabileceği bir dizi komutu içeren düz metin dosyasıdır. Komutları tek tek yazmak yerine, hepsini bir dosyaya yazar ve birlikte çalıştırırsınız.

Bunu bir **tarif** gibi düşünün:
- Her adımı ezberlemek yerine, bir kez yazarsınız
- Herkes aynı sonucu almak için tarifi takip edebilir
- Tarifi istediğiniz zaman tekrar kullanabilirsiniz

### Bash Scriptleriyle Neler Yapabilirsiniz?

```bash
# Tekrarlayan görevleri otomatikleştirme
# - Günlük yedeklemeler
# - Log dosyası temizliği
# - Kullanıcı hesabı oluşturma

# Sistem yönetimi
# - Sunucu izleme
# - Servis yönetimi
# - Güvenlik denetimleri

# DevOps iş akışları
# - Deployment otomasyonu
# - Ortam kurulumu
# - CI/CD pipeline yardımcıları
```

### Bash vs Diğer Scripting Dilleri

```
Dil         | En İyi Kullanım Alanı        | Öğrenme Eğrisi
-----------|-----------------------------|---------------
Bash       | Sistem görevleri, otomasyon | Kolay
Python     | Karmaşık mantık, veri işleri| Orta
Perl       | Metin işleme                | Zor
PowerShell | Windows yönetimi            | Orta
```

Bash scripting şu durumlarda idealdir:
- Linux komutlarını zincirleme
- Sistem yönetimi görevlerini otomatikleştirme
- Hızlı, basit otomasyon scriptleri yazma

---

## 14.2 İlk Scriptiniz

### Script Dosyası Oluşturma

```bash
# Adım 1: Yeni bir dosya oluştur
touch hello.sh

# Adım 2: Favori editörünüzle açın
nano hello.sh
# veya
vim hello.sh
```

### Shebang Satırı

Her script, sisteme hangi yorumlayıcının kullanılacağını söyleyen bir **shebang** (`#!`) satırıyla başlamalıdır:

```bash
#!/bin/bash
# Bu, sisteme şunu söyler: "Bu scripti çalıştırmak için /bin/bash kullan"
```

### İlk Scriptiniz: Hello World

```bash
#!/bin/bash
# İlk Bash scriptim
# Yazar: Adınız
# Tarih: 2025-01-15

echo "Merhaba, Dünya!"
echo "Bash Scripting'e hoş geldiniz!"
echo "Bugün: $(date)"
echo "Şu kullanıcı olarak giriş yaptınız: $(whoami)"
echo "Mevcut dizin: $(pwd)"
```

### Scripti Çalıştırılabilir Yapmak

```bash
# Yöntem 1: Çalıştırma izni ekle
chmod +x hello.sh

# Çalıştır
./hello.sh

# Yöntem 2: Doğrudan bash ile çalıştır (chmod gerekmez)
bash hello.sh
```

### Çıktı

```
Merhaba, Dünya!
Bash Scripting'e hoş geldiniz!
Bugün: Wed Jan 15 10:30:00 UTC 2025
Şu kullanıcı olarak giriş yaptınız: john
Mevcut dizin: /home/john/scripts
```

### Script Dosyası Kuralları

```bash
# 1. .sh uzantısı kullanın (zorunlu değil, ama önerilir)
myscript.sh
backup.sh
setup.sh

# 2. Açıklayıcı isimler kullanın
# İyi:
backup-database.sh
create-user.sh
check-disk-space.sh

# Kötü:
script1.sh
test.sh
x.sh

# 3. Her zaman shebang ile başlayın
#!/bin/bash

# 4. Scriptin ne yaptığını açıklayan yorumlar ekleyin
#!/bin/bash
# Açıklama: Bu script veritabanını yedekler
# Kullanım: ./backup-database.sh [veritabani_adi]
# Yazar: John Doe
```

---

## 14.3 Değişkenler

### Değişkenler Nedir?

Değişkenler, **veri saklayan kutulardır**. Bunları içine bilgi koyduğunuz etiketli kutular gibi düşünün.

### Değişken Tanımlama

```bash
#!/bin/bash

# Basit değişken ataması
# ÖNEMLİ: = işaretinin etrafında boşluk olmamalı!
name="John"
age=25
city="Istanbul"

# DOĞRU:
name="John"

# YANLIŞ (hata verir):
# name = "John"    # = işaretinin etrafında boşluk
# name ="John"     # = işaretinden önce boşluk
# name= "John"     # = işaretinden sonra boşluk
```

### Değişkenleri Kullanma

```bash
#!/bin/bash

# Değişken değerine erişmek için $ kullanın
name="John"
age=25

echo "Adım $name"
echo "Ben $age yaşındayım"

# Süslü parantez kullanımı (netlik için önerilir)
echo "Adım ${name}"
echo "${name}, ${age} yaşında"

# Süslü parantezlerin ZORUNLU olduğu durumlar
file="report"
echo "${file}_2025.txt"    # Çıktı: report_2025.txt
echo "$file_2025.txt"      # YANLIŞ: $file_2025 değişkenini aramaya çalışır
```

### Değişken Türleri

```bash
#!/bin/bash

# String değişkenler
greeting="Hello World"
filename="backup.tar.gz"
path="/home/john/documents"

# Sayısal değişkenler
count=10
port=8080
max_retries=3

# Bir değişkende komut çıktısı (Command Substitution)
current_date=$(date)
current_user=$(whoami)
file_count=$(ls | wc -l)
disk_usage=$(df -h / | tail -1 | awk '{print $5}')

echo "Tarih: $current_date"
echo "Kullanıcı: $current_user"
echo "Mevcut dizindeki dosya sayısı: $file_count"
echo "Disk kullanımı: $disk_usage"
```

### Ortam Değişkenleri vs Yerel Değişkenler

```bash
#!/bin/bash

# Yerel değişken (sadece bu scriptte kullanılabilir)
my_var="Hello"

# Ortam değişkenleri (sistem genelinde kullanılabilir)
echo "Ev dizini: $HOME"
echo "Mevcut kullanıcı: $USER"
echo "Shell: $SHELL"
echo "Path: $PATH"
echo "Hostname: $HOSTNAME"

# Yerel bir değişkeni alt süreçlere aktarılabilir yapma
export MY_VAR="Ben export edildim"
```

### Salt Okunur (Read-Only) Değişkenler

```bash
#!/bin/bash

# Salt okunur (sabit) bir değişken oluştur
readonly PI=3.14159
readonly APP_NAME="MyApp"

echo "Pi değeri: $PI"
echo "Uygulama adı: $APP_NAME"

# Bu hataya sebep olur:
# PI=3.14    # Hata: PI: readonly variable
```

### Değişkenleri Tırnak İçine Alma (Quoting)

```bash
#!/bin/bash

name="John Doe"

# Çift tırnak: Değişkenler genişletilir (expand edilir)
echo "Hello, $name"           # Çıktı: Hello, John Doe

# Tek tırnak: Her şey birebir (literal) alınır
echo 'Hello, $name'           # Çıktı: Hello, $name

# En iyi uygulama: Değişkenlerinizi her zaman tırnak içine alın
filename="my file.txt"
# DOĞRU:
cat "$filename"               # Boşlukları doğru şekilde işler

# YANLIŞ:
# cat $filename               # Boşluklarda bozulur (şunu dener: cat my file.txt)
```

### Pratik Örnek: Sistem Bilgisi Scripti

```bash
#!/bin/bash
# system-info.sh - Sistem bilgilerini görüntüle

hostname=$(hostname)
os_name=$(uname -s)
kernel=$(uname -r)
uptime_info=$(uptime -p)
cpu_cores=$(nproc)
total_mem=$(free -h | awk '/Mem:/ {print $2}')
disk_free=$(df -h / | awk 'NR==2 {print $4}')
ip_address=$(hostname -I | awk '{print $1}')

echo "========== Sistem Bilgileri =========="
echo "Hostname     : $hostname"
echo "OS           : $os_name"
echo "Kernel       : $kernel"
echo "Uptime       : $uptime_info"
echo "CPU Çekirdek : $cpu_cores"
echo "Toplam RAM   : $total_mem"
echo "Boş Disk     : $disk_free"
echo "IP Adresi    : $ip_address"
echo "========================================"
```

---

## 14.4 Kullanıcı Girdisi

### `read` ile Girdi Okuma

```bash
#!/bin/bash

# Basit girdi
echo "Adınız nedir?"
read name
echo "Merhaba, $name!"

# Prompt ile girdi (-p)
read -p "Yaşınızı girin: " age
echo "$age yaşındasınız"

# Şifreler için sessiz girdi (-s)
read -sp "Şifrenizi girin: " password
echo    # Gizli girdiden sonra yeni satır
echo "Şifre alındı (uzunluk: ${#password})"

# Zaman aşımlı girdi (-t saniye)
read -t 10 -p "Adınızı girin (10 saniye): " name
if [ -z "$name" ]; then
    echo "Süre doldu! Varsayılan kullanılıyor."
    name="Guest"
fi
echo "Merhaba, $name!"

# Birden fazla değer okuma
echo "Ad ve soyadınızı girin:"
read first_name last_name
echo "Ad: $first_name"
echo "Soyad: $last_name"
```

### Pratik Örnek: Etkileşimli Selamlama Scripti

```bash
#!/bin/bash
# greeting.sh - Etkileşimli selamlama scripti

read -p "Adınız nedir? " name
read -p "Favori renginiz nedir? " color

echo ""
echo "Merhaba, $name!"
echo "$color rengini sevdiğinizi öğrenmek güzel."
echo "İyi günler!"
```

### Pratik Örnek: Basit Hesap Makinesi (Girdi)

```bash
#!/bin/bash
# calculator.sh - Basit etkileşimli hesap makinesi

read -p "İlk sayıyı girin: " num1
read -p "Operatörü girin (+, -, *, /): " operator
read -p "İkinci sayıyı girin: " num2

case $operator in
    +) result=$(echo "$num1 + $num2" | bc) ;;
    -) result=$(echo "$num1 - $num2" | bc) ;;
    \*) result=$(echo "$num1 * $num2" | bc) ;;
    /)
        if [ "$num2" = "0" ]; then
            echo "Hata: Sıfıra bölme!"
            exit 1
        fi
        result=$(echo "scale=2; $num1 / $num2" | bc)
        ;;
    *) echo "Geçersiz operatör!"; exit 1 ;;
esac

echo "Sonuç: $num1 $operator $num2 = $result"
```

---

## 14.5 Aritmetik İşlemler

### Aritmetik Yöntemleri

```bash
#!/bin/bash

a=10
b=3

# Yöntem 1: $(( )) - Çift parantez (ÖNERİLEN)
sum=$((a + b))
diff=$((a - b))
mult=$((a * b))
div=$((a / b))        # Sadece tam sayı bölmesi!
mod=$((a % b))        # Modulo (kalan)

echo "Toplam: $sum"       # 13
echo "Fark: $diff"        # 7
echo "Çarpım: $mult"      # 30
echo "Bölüm: $div"        # 3 (sadece tam sayı!)
echo "Kalan: $mod"        # 1

# Yöntem 2: let komutu
let result=a+b
let "result = a + b"   # Tırnak içinde boşluklar sorun değil
echo "Sonuç: $result"  # 13

# Yöntem 3: expr komutu (eski yöntem)
result=$(expr $a + $b)
echo "Sonuç: $result"  # 13
# Not: expr ile * karakteri escape edilmeli
result=$(expr $a \* $b)

# Yöntem 4: bc - Ondalık/kayan noktalı matematik için
result=$(echo "scale=2; 10 / 3" | bc)
echo "Sonuç: $result"  # 3.33

result=$(echo "scale=4; 22 / 7" | bc)
echo "Pi yaklaşık: $result"  # 3.1428
```

### Artırma ve Azaltma

```bash
#!/bin/bash

count=0

# Artırma
((count++))    # count = 1
((count++))    # count = 2
echo "$count"  # 2

# Azaltma
((count--))    # count = 1
echo "$count"  # 1

# Belirli bir değer ekleme/çıkarma
((count += 5))   # count = 6
((count -= 2))   # count = 4
((count *= 3))   # count = 12
echo "$count"    # 12
```

### Pratik Örnek: Bahşiş Hesaplayıcı

```bash
#!/bin/bash
# tip-calculator.sh

read -p "Fatura tutarını girin: " bill
read -p "Bahşiş yüzdesini girin (örn: 15): " tip_percent

tip_amount=$(echo "scale=2; $bill * $tip_percent / 100" | bc)
total=$(echo "scale=2; $bill + $tip_amount" | bc)

echo ""
echo "Fatura       : $bill TL"
echo "Bahşiş (%$tip_percent): $tip_amount TL"
echo "Toplam       : $total TL"
```

---

## 14.6 Koşul İfadeleri (if/else)

### Temel if İfadesi

```bash
#!/bin/bash

age=18

# Basit if
if [ $age -ge 18 ]; then
    echo "Reşitsiniz."
fi

# if-else
if [ $age -ge 18 ]; then
    echo "Oy kullanabilirsiniz."
else
    echo "Henüz oy kullanamazsınız."
fi

# if-elif-else
if [ $age -lt 13 ]; then
    echo "Çocuksunuz."
elif [ $age -lt 18 ]; then
    echo "Gençsiniz."
elif [ $age -lt 65 ]; then
    echo "Yetişkinsiniz."
else
    echo "Yaşlısınız."
fi
```

### Önemli Sözdizimi Kuralları

```bash
# DOĞRU:
if [ $age -ge 18 ]; then    # [ ] içindeki boşluklar ZORUNLUDUR!
    echo "Yetişkin"
fi

# YANLIŞ:
# if [$age -ge 18]; then    # Köşeli parantez içinde boşluk eksik!
# if [ $age -ge 18 ]then    # then'den önce noktalı virgül veya yeni satır eksik!
```

### String Kontrolleri

```bash
#!/bin/bash

name="John"

# String eşitliği
if [ "$name" = "John" ]; then
    echo "Merhaba, John!"
fi

# String eşitsizliği
if [ "$name" != "Admin" ]; then
    echo "Admin değilsiniz."
fi

# Boş string kontrolü
if [ -z "$name" ]; then
    echo "İsim boş"
else
    echo "İsim: $name"
fi

# Boş olmayan string kontrolü
if [ -n "$name" ]; then
    echo "İsim boş değil"
fi
```

### Dosya Kontrolleri

```bash
#!/bin/bash

file="/etc/passwd"
dir="/home/john"

# Dosyanın var olup olmadığını kontrol et
if [ -f "$file" ]; then
    echo "$file mevcut ve normal bir dosya"
fi

# Dizinin var olup olmadığını kontrol et
if [ -d "$dir" ]; then
    echo "$dir mevcut ve bir dizin"
fi

# Dosyanın okunabilir olup olmadığını kontrol et
if [ -r "$file" ]; then
    echo "$file okunabilir"
fi

# Dosyanın yazılabilir olup olmadığını kontrol et
if [ -w "$file" ]; then
    echo "$file yazılabilir"
fi

# Dosyanın çalıştırılabilir olup olmadığını kontrol et
if [ -x "/usr/bin/bash" ]; then
    echo "bash çalıştırılabilir"
fi

# Dosyanın boş OLMADIĞINI kontrol et
if [ -s "$file" ]; then
    echo "$file boş değil"
fi

# Dosya veya dizinin var olup olmadığını kontrol et (herhangi bir tür)
if [ -e "$file" ]; then
    echo "$file mevcut"
fi
```

### Koşulları Birleştirme (VE / VEYA)

```bash
#!/bin/bash

age=25
name="John"

# VE (AND): Her iki koşul da doğru olmalı
if [ $age -ge 18 ] && [ "$name" = "John" ]; then
    echo "John'sunuz ve reşitsiniz."
fi

# VEYA (OR): En az bir koşul doğru olmalı
if [ "$name" = "John" ] || [ "$name" = "Jane" ]; then
    echo "Merhaba, John veya Jane!"
fi

# DEĞİL (NOT): Koşulu tersine çevir
if [ ! -f "/tmp/lockfile" ]; then
    echo "Kilit dosyası mevcut değil."
fi

# Çift köşeli parantez [[ ]] kullanımı (Bash'e özgü, daha fazla özellik)
if [[ $age -ge 18 && "$name" == "John" ]]; then
    echo "John'sunuz ve reşitsiniz."
fi

# [[ ]] ile pattern eşleştirme
filename="report.pdf"
if [[ "$filename" == *.pdf ]]; then
    echo "Bu bir PDF dosyası"
fi
```

### Pratik Örnek: Dosya Kontrol Scripti

```bash
#!/bin/bash
# file-checker.sh - Dosya özelliklerini kontrol et

read -p "Dosya yolunu girin: " filepath

if [ ! -e "$filepath" ]; then
    echo "Hata: '$filepath' mevcut değil!"
    exit 1
fi

echo "=== Dosya Bilgisi: $filepath ==="

if [ -f "$filepath" ]; then
    echo "Tür: Normal dosya"
elif [ -d "$filepath" ]; then
    echo "Tür: Dizin"
elif [ -L "$filepath" ]; then
    echo "Tür: Sembolik bağlantı"
else
    echo "Tür: Diğer"
fi

[ -r "$filepath" ] && echo "Okunabilir: Evet" || echo "Okunabilir: Hayır"
[ -w "$filepath" ] && echo "Yazılabilir: Evet" || echo "Yazılabilir: Hayır"
[ -x "$filepath" ] && echo "Çalıştırılabilir: Evet" || echo "Çalıştırılabilir: Hayır"

if [ -f "$filepath" ]; then
    size=$(stat --printf="%s" "$filepath" 2>/dev/null)
    echo "Boyut: $size byte"
fi
```

---

## 14.7 Karşılaştırma Operatörleri

### Sayısal Karşılaştırma Operatörleri

```bash
# [ ] veya [[ ]] içinde kullanılır

# -eq   Eşittir
# -ne   Eşit değildir
# -gt   Büyüktür
# -ge   Büyük veya eşittir
# -lt   Küçüktür
# -le   Küçük veya eşittir

#!/bin/bash

a=10
b=20

[ $a -eq $b ] && echo "Eşit" || echo "Eşit değil"       # Eşit değil
[ $a -ne $b ] && echo "Eşit değil" || echo "Eşit"       # Eşit değil
[ $a -gt $b ] && echo "a > b" || echo "a <= b"          # a <= b
[ $a -ge $b ] && echo "a >= b" || echo "a < b"          # a < b
[ $a -lt $b ] && echo "a < b" || echo "a >= b"          # a < b
[ $a -le $b ] && echo "a <= b" || echo "a > b"          # a <= b
```

### String Karşılaştırma Operatörleri

```bash
# [ ] veya [[ ]] içinde kullanılır

# =     Eşittir (içeride [[ ]] kullanıyorsanız == kullanın)
# !=    Eşit değildir
# -z    String boş
# -n    String boş değil
# <     Küçüktür (alfabetik olarak, sadece [[ ]] içinde)
# >     Büyüktür (alfabetik olarak, sadece [[ ]] içinde)

#!/bin/bash

str1="hello"
str2="world"

[ "$str1" = "$str2" ] && echo "Aynı" || echo "Farklı"       # Farklı
[ "$str1" != "$str2" ] && echo "Farklı" || echo "Aynı"      # Farklı
[ -z "" ] && echo "Boş" || echo "Boş değil"                 # Boş
[ -n "$str1" ] && echo "Boş değil" || echo "Boş"            # Boş değil
```

### Dosya Test Operatörleri

```bash
# Yaygın dosya test operatörleri:

# -f file    Dosya mevcutsa ve normal bir dosyaysa doğru
# -d file    Dosya mevcutsa ve dizinse doğru
# -e file    Dosya mevcutsa (herhangi bir tür) doğru
# -r file    Dosya okunabilirse doğru
# -w file    Dosya yazılabilirse doğru
# -x file    Dosya çalıştırılabilirse doğru
# -s file    Dosya mevcutsa ve boş değilse doğru
# -L file    Dosya sembolik bağlantıysa doğru
# -O file    Dosyanın sahibiyseniz doğru
# -G file    Dosyanın grubu sizinkiyle eşleşiyorsa doğru

# Dosya karşılaştırma:
# file1 -nt file2    file1, file2'den daha yeni
# file1 -ot file2    file1, file2'den daha eski
```

### Hızlı Referans Tablosu

```
Sayısal:          String:            Dosya:
-eq  (==)         =  (eşit)          -f  (dosya mı)
-ne  (!=)         != (eşit değil)    -d  (dizin mi)
-gt  (>)          -z (boş mu)        -e  (var mı)
-ge  (>=)         -n (boş değil mi)  -r  (okunabilir)
-lt  (<)                             -w  (yazılabilir)
-le  (<=)                            -x  (çalıştırılabilir)
                                     -s  (boş değil)
```

---

## 14.8 Case İfadeleri

### Temel Sözdizimi

```bash
#!/bin/bash
# Case ifadesi - birden fazla if-elif'e alternatif

read -p "Bir meyve girin: " fruit

case $fruit in
    apple)
        echo "Elmalar kırmızı veya yeşildir."
        ;;
    banana)
        echo "Muzlar sarıdır."
        ;;
    orange)
        echo "Portakallar turuncudur."
        ;;
    *)
        echo "Bilinmeyen meyve: $fruit"
        ;;
esac
```

### Case ile Pattern Eşleştirme

```bash
#!/bin/bash
# Case pattern'leri destekler

read -p "Bir dosya adı girin: " filename

case $filename in
    *.txt)
        echo "Metin dosyası"
        ;;
    *.jpg|*.png|*.gif)
        echo "Resim dosyası"
        ;;
    *.sh)
        echo "Shell scripti"
        ;;
    *.tar.gz|*.tgz)
        echo "Sıkıştırılmış arşiv"
        ;;
    *)
        echo "Bilinmeyen dosya türü"
        ;;
esac
```

### Pratik Örnek: Servis Yöneticisi

```bash
#!/bin/bash
# service-menu.sh - Basit servis yöneticisi

echo "=== Servis Yöneticisi ==="
echo "1) Servisi başlat"
echo "2) Servisi durdur"
echo "3) Servisi yeniden başlat"
echo "4) Durumu kontrol et"
echo "5) Çıkış"
echo ""

read -p "Bir seçenek seçin [1-5]: " choice

case $choice in
    1)
        read -p "Servis adı: " service
        echo "$service başlatılıyor..."
        # sudo systemctl start $service
        ;;
    2)
        read -p "Servis adı: " service
        echo "$service durduruluyor..."
        # sudo systemctl stop $service
        ;;
    3)
        read -p "Servis adı: " service
        echo "$service yeniden başlatılıyor..."
        # sudo systemctl restart $service
        ;;
    4)
        read -p "Servis adı: " service
        echo "$service durumu kontrol ediliyor..."
        # sudo systemctl status $service
        ;;
    5)
        echo "Görüşürüz!"
        exit 0
        ;;
    *)
        echo "Geçersiz seçenek! Lütfen 1-5 arasında seçin."
        exit 1
        ;;
esac
```

### Pratik Örnek: Haftanın Günü

```bash
#!/bin/bash
# day-info.sh

day=$(date +%A)

case $day in
    Monday)
        echo "İş haftasının başlangıcı!"
        ;;
    Tuesday|Wednesday|Thursday)
        echo "Hafta ortası - devam edin!"
        ;;
    Friday)
        echo "Nihayet Cuma! Hafta sonu yaklaştı!"
        ;;
    Saturday|Sunday)
        echo "Hafta sonu! Dinlenme zamanı."
        ;;
esac
```

---

## 14.9 Döngüler

### for Döngüsü

```bash
#!/bin/bash

# Bir liste üzerinde döngü
for color in red green blue yellow; do
    echo "Renk: $color"
done
# Çıktı:
# Renk: red
# Renk: green
# Renk: blue
# Renk: yellow

# Bir aralık üzerinde döngü
for i in {1..5}; do
    echo "Sayı: $i"
done
# Çıktı: 1 2 3 4 5

# Adım aralıklı döngü
for i in {0..20..5}; do
    echo "Sayı: $i"
done
# Çıktı: 0 5 10 15 20

# C tarzı for döngüsü
for ((i=1; i<=5; i++)); do
    echo "Sayaç: $i"
done

# Dosyalar üzerinde döngü
for file in *.sh; do
    echo "Script: $file"
done

# Komut çıktısı üzerinde döngü
for user in $(cat /etc/passwd | cut -d: -f1 | head -5); do
    echo "Kullanıcı: $user"
done
```

### while Döngüsü

```bash
#!/bin/bash

# Temel while döngüsü
count=1
while [ $count -le 5 ]; do
    echo "Sayaç: $count"
    ((count++))
done
# Çıktı: 1 2 3 4 5

# Bir dosyayı satır satır okuma
while IFS= read -r line; do
    echo "Satır: $line"
done < /etc/hostname

# break ile sonsuz döngü
while true; do
    read -p "Çıkmak için 'quit' girin: " input
    if [ "$input" = "quit" ]; then
        echo "Görüşürüz!"
        break
    fi
    echo "Şunu söylediniz: $input"
done
```

### until Döngüsü

```bash
#!/bin/bash

# until: koşul doğru olana KADAR çalışır (while'ın tersi)
count=1
until [ $count -gt 5 ]; do
    echo "Sayaç: $count"
    ((count++))
done
# Çıktı: 1 2 3 4 5
```

### Döngü Kontrolü: break ve continue

```bash
#!/bin/bash

# break: Döngüden tamamen çık
for i in {1..10}; do
    if [ $i -eq 6 ]; then
        echo "$i'de döngü kırılıyor"
        break
    fi
    echo "Sayı: $i"
done
# Çıktı: 1 2 3 4 5 Breaking at 6

# continue: Mevcut iterasyonu atla, sonrakine geç
for i in {1..10}; do
    if [ $((i % 2)) -eq 0 ]; then
        continue    # Çift sayıları atla
    fi
    echo "Tek sayı: $i"
done
# Çıktı: 1 3 5 7 9
```

### Pratik Örnek: Geri Sayım Zamanlayıcısı

```bash
#!/bin/bash
# countdown.sh

read -p "Geri sayım süresini saniye olarak girin: " seconds

while [ $seconds -gt 0 ]; do
    echo -ne "\rKalan süre: ${seconds}s  "
    sleep 1
    ((seconds--))
done

echo -e "\rSüre doldu!              "
```

### Pratik Örnek: Toplu Dosya Yeniden Adlandırma

```bash
#!/bin/bash
# rename-files.sh - Tüm .txt dosyalarına ön ek ekle

read -p "Eklenecek ön eki girin: " prefix

count=0
for file in *.txt; do
    if [ -f "$file" ]; then
        mv "$file" "${prefix}_${file}"
        echo "Yeniden adlandırıldı: $file -> ${prefix}_${file}"
        ((count++))
    fi
done

echo "Toplam yeniden adlandırılan dosya: $count"
```

### Pratik Örnek: Çarpım Tablosu

```bash
#!/bin/bash
# multiplication-table.sh

read -p "Bir sayı girin: " num

echo "=== $num İçin Çarpım Tablosu ==="
for i in {1..10}; do
    result=$((num * i))
    echo "$num x $i = $result"
done
```

---

## 14.10 Fonksiyonlar

### Fonksiyon Tanımlama ve Çağırma

```bash
#!/bin/bash

# Yöntem 1: Standart sözdizimi
greet() {
    echo "Merhaba, Dünya!"
}

# Yöntem 2: 'function' anahtar kelimesiyle
function say_goodbye() {
    echo "Hoşça kal, Dünya!"
}

# Fonksiyonları çağırma
greet
say_goodbye
```

### Parametreli Fonksiyonlar

```bash
#!/bin/bash

# Parametrelere $1, $2, $3 vb. ile erişilir
greet_user() {
    echo "Merhaba, $1!"
    echo "$2 yaşındasınız."
}

# Argümanlarla çağırma
greet_user "John" 25
# Çıktı:
# Merhaba, John!
# 25 yaşındasınız.

# Birden fazla parametreli fonksiyon
create_file() {
    local filename=$1
    local content=$2

    echo "$content" > "$filename"
    echo "Dosya oluşturuldu: $filename"
}

create_file "note.txt" "Bu benim notum"
```

### Dönüş Değerleri

```bash
#!/bin/bash

# Fonksiyonlar değer değil, çıkış kodu (0-255) döndürür
# Değer "döndürmek" için echo kullanın

# Çıkış kodu döndürme
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0    # Başarılı (true)
    else
        return 1    # Başarısız (false)
    fi
}

if is_even 4; then
    echo "4 çifttir"
fi

# echo kullanarak değer döndürme
get_square() {
    local num=$1
    echo $((num * num))
}

result=$(get_square 5)
echo "5'in karesi: $result"    # 25
```

### Yerel (Local) Değişkenler

```bash
#!/bin/bash

# Fonksiyon içindeki değişkenler varsayılan olarak GLOBAL'dır!
# Fonksiyon kapsamlı yapmak için 'local' kullanın

my_function() {
    local local_var="Ben yerelim"
    global_var="Ben globalim"
    echo "Fonksiyon içinde: $local_var"
}

my_function
echo "Fonksiyon dışında: $global_var"     # Çalışır
echo "Fonksiyon dışında: $local_var"      # Boş (sadece yerel)
```

### Pratik Örnek: Loglama Fonksiyonu

```bash
#!/bin/bash
# Loglamayı standartlaştıran fonksiyonlar

log_info() {
    echo "[INFO]  $(date '+%Y-%m-%d %H:%M:%S') - $1"
}

log_warn() {
    echo "[WARN]  $(date '+%Y-%m-%d %H:%M:%S') - $1"
}

log_error() {
    echo "[ERROR] $(date '+%Y-%m-%d %H:%M:%S') - $1" >&2
}

# Kullanım
log_info "Script başladı"
log_info "Dosyalar işleniyor..."
log_warn "Disk alanı azalıyor"
log_error "Veritabanına bağlanılamadı"
log_info "Script tamamlandı"

# Çıktı:
# [INFO]  2025-01-15 10:30:00 - Script başladı
# [INFO]  2025-01-15 10:30:00 - Dosyalar işleniyor...
# [WARN]  2025-01-15 10:30:00 - Disk alanı azalıyor
# [ERROR] 2025-01-15 10:30:00 - Veritabanına bağlanılamadı
# [INFO]  2025-01-15 10:30:01 - Script tamamlandı
```

---

## 14.11 Scriptlerde Dosyalarla Çalışmak

### Dosyaları Kontrol Etme

```bash
#!/bin/bash
# Scriptlerde yaygın dosya işlemleri

# Bir dosya üzerinde işlem yapmadan önce kontrol et
backup_file() {
    local source=$1

    if [ ! -f "$source" ]; then
        echo "Hata: '$source' dosyası bulunamadı!"
        return 1
    fi

    cp "$source" "${source}.bak"
    echo "Yedek oluşturuldu: ${source}.bak"
}

backup_file "/etc/hosts"
```

### Dosya Okuma

```bash
#!/bin/bash

# Dosyayı satır satır oku
while IFS= read -r line; do
    echo "İşleniyor: $line"
done < input.txt

# Dosyayı bir array'e oku
mapfile -t lines < input.txt
echo "Toplam satır: ${#lines[@]}"
echo "İlk satır: ${lines[0]}"
echo "Son satır: ${lines[-1]}"

# Belirli alanları oku (CSV benzeri)
while IFS=, read -r name age city; do
    echo "Ad: $name, Yaş: $age, Şehir: $city"
done < data.csv
```

### Dosyaya Yazma

```bash
#!/bin/bash

# Dosyaya yaz (üzerine yaz)
echo "Hello World" > output.txt

# Dosyaya ekle
echo "Another line" >> output.txt

# Birden fazla satır yaz (heredoc)
cat > config.txt << EOF
# Yapılandırma Dosyası
hostname=server01
port=8080
debug=false
EOF

# Birden fazla satır ekle (append)
cat >> config.txt << EOF
# Ek ayarlar
log_level=info
EOF
```

### Pratik Örnek: Log Analiz Aracı

```bash
#!/bin/bash
# log-analyzer.sh - Basit log dosyası analiz aracı

logfile=$1

if [ -z "$logfile" ]; then
    echo "Kullanım: $0 <logfile>"
    exit 1
fi

if [ ! -f "$logfile" ]; then
    echo "Hata: '$logfile' dosyası bulunamadı!"
    exit 1
fi

total_lines=$(wc -l < "$logfile")
error_count=$(grep -ci "error" "$logfile")
warn_count=$(grep -ci "warn" "$logfile")

echo "=== Log Analizi: $logfile ==="
echo "Toplam satır : $total_lines"
echo "Hatalar      : $error_count"
echo "Uyarılar     : $warn_count"
echo "=============================="
```

---

## 14.12 Komut Satırı Argümanları

### Argümanlara Erişim

```bash
#!/bin/bash
# arguments.sh - Komut satırı argümanlarını göster

echo "Script adı  : $0"
echo "İlk argüman : $1"
echo "İkinci arg  : $2"
echo "Üçüncü arg  : $3"
echo "Tüm argümanlar : $@"
echo "Argüman sayısı : $#"
echo "Tümü tek string: $*"

# Çalıştır: ./arguments.sh hello world 42
# Çıktı:
# Script adı  : ./arguments.sh
# İlk argüman : hello
# İkinci arg  : world
# Üçüncü arg  : 42
# Tüm argümanlar : hello world 42
# Argüman sayısı : 3
# Tümü tek string: hello world 42
```

### Özel Değişkenler Referansı

```bash
# $0    Script adı
# $1-$9 Konumsal parametreler (argümanlar)
# $#    Argüman sayısı
# $@    Tüm argümanlar (ayrı kelimeler olarak)
# $*    Tüm argümanlar (tek bir string olarak)
# $?    Son komutun çıkış durumu
# $$    Mevcut scriptin process ID'si
# $!    Son arka plan komutunun process ID'si
```

### Argümanları Doğrulama

```bash
#!/bin/bash
# validate-args.sh

# Yeterli argüman sağlanıp sağlanmadığını kontrol et
if [ $# -lt 2 ]; then
    echo "Kullanım: $0 <kaynak> <hedef>"
    echo "Örnek: $0 /home/john/file.txt /backup/"
    exit 1
fi

source=$1
destination=$2

echo "'$source' -> '$destination' kopyalanıyor..."
cp "$source" "$destination"
echo "Tamamlandı!"
```

### Argümanlar Üzerinde Döngü

```bash
#!/bin/bash
# process-files.sh - Birden fazla dosyayı işle

if [ $# -eq 0 ]; then
    echo "Kullanım: $0 dosya1 [dosya2] [dosya3] ..."
    exit 1
fi

for file in "$@"; do
    if [ -f "$file" ]; then
        lines=$(wc -l < "$file")
        size=$(stat --printf="%s" "$file")
        echo "$file: $lines satır, $size byte"
    else
        echo "$file: BULUNAMADI"
    fi
done
```

---

## 14.13 Çıkış Kodları ve Hata Yönetimi

### Çıkış Kodlarını Anlamak

```bash
#!/bin/bash

# Her komut bir çıkış kodu döndürür:
# 0     = Başarılı
# 1-255 = Hata (farklı hatalar için farklı kodlar)

# $? ile çıkış kodunu kontrol et
ls /etc/passwd
echo "Çıkış kodu: $?"    # 0 (başarılı)

ls /nonexistent
echo "Çıkış kodu: $?"    # 2 (dosya yok)
```

### Çıkış Kodları Ayarlama

```bash
#!/bin/bash
# deploy.sh

if [ $# -eq 0 ]; then
    echo "Hata: Ortam belirtilmedi!"
    exit 1    # Hata kodu 1 ile çık
fi

environment=$1

case $environment in
    dev|staging|prod)
        echo "$environment ortamına deploy ediliyor..."
        exit 0    # Başarılı
        ;;
    *)
        echo "Hata: Bilinmeyen ortam '$environment'"
        echo "Kullanın: dev, staging veya prod"
        exit 2    # Farklı hata kodu
        ;;
esac
```

### set ile Hata Yönetimi

```bash
#!/bin/bash

# Bir komut başarısız olursa hemen çık
set -e

# Tanımlanmamış değişkenleri hata olarak ele al
set -u

# Pipe hatalarında başarısız ol
set -o pipefail

# Üçünü birleştir (scriptler için önerilir)
set -euo pipefail

# Hata yönetimi ile örnek
echo "Dizin oluşturuluyor..."
mkdir -p /tmp/myproject

echo "Dizine giriliyor..."
cd /tmp/myproject

echo "Dosya indiriliyor..."
# curl başarısız olursa, script burada durur (set -e sayesinde)
# curl -O https://example.com/file.tar.gz

echo "Tamamlandı!"
```

### Trap: Çıkışta Temizlik

```bash
#!/bin/bash
# Temizlik yapan script

# Geçici bir dosya oluştur
tmpfile=$(mktemp)

# Temizliği ayarla - script çıktığında (normal veya hatayla) çalışır
cleanup() {
    echo "Geçici dosyalar temizleniyor..."
    rm -f "$tmpfile"
}
trap cleanup EXIT

# Geçici dosyayı kullan
echo "Geçici dosyayla çalışılıyor: $tmpfile"
echo "some data" > "$tmpfile"

# Bir şey başarısız olsa bile, temizlik otomatik olarak çalışır
```

### Pratik Örnek: Güvenli Script Şablonu

```bash
#!/bin/bash
# template.sh - Güvenli bir script şablonu

set -euo pipefail

# Sabitler
readonly SCRIPT_NAME=$(basename "$0")
readonly LOG_FILE="/tmp/${SCRIPT_NAME}.log"

# Loglama
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# Temizlik
cleanup() {
    log "Script tamamlandı. Temizleniyor..."
}
trap cleanup EXIT

# Argüman doğrulama
if [ $# -lt 1 ]; then
    echo "Kullanım: $SCRIPT_NAME <argüman>"
    exit 1
fi

# Ana mantık
log "Script şu argümanla başladı: $1"
log "İşlemler yapılıyor..."

# Kodunuz buraya

log "Tüm işlemler başarıyla tamamlandı."
```

---

## 14.14 Pratik Script Örnekleri

### Örnek 1: Disk Alanı Uyarısı

```bash
#!/bin/bash
# disk-alert.sh - Disk kullanımı eşiği aştığında uyar

threshold=80

df -H | awk 'NR>1 {print $5 " " $6}' | while read -r usage mount; do
    # % işaretini kaldır
    usage_num=${usage%\%}

    if [ "$usage_num" -gt "$threshold" ]; then
        echo "UYARI: $mount, ${usage} dolu!"
    fi
done
```

### Örnek 2: Yedekleme Scripti

```bash
#!/bin/bash
# backup.sh - Basit dizin yedekleme

set -euo pipefail

# Yapılandırma
SOURCE_DIR=${1:-"/home/$USER/documents"}
BACKUP_DIR="/tmp/backups"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="backup_${DATE}.tar.gz"

# Yedekleme dizini yoksa oluştur
mkdir -p "$BACKUP_DIR"

# Kaynak dizini kontrol et
if [ ! -d "$SOURCE_DIR" ]; then
    echo "Hata: Kaynak dizin '$SOURCE_DIR' bulunamadı!"
    exit 1
fi

# Yedek oluştur
echo "$SOURCE_DIR için yedek oluşturuluyor..."
tar -czf "${BACKUP_DIR}/${BACKUP_FILE}" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"

# Doğrula
if [ -f "${BACKUP_DIR}/${BACKUP_FILE}" ]; then
    size=$(du -h "${BACKUP_DIR}/${BACKUP_FILE}" | cut -f1)
    echo "Yedekleme başarılı!"
    echo "Dosya: ${BACKUP_DIR}/${BACKUP_FILE}"
    echo "Boyut: $size"
else
    echo "Yedekleme başarısız!"
    exit 1
fi
```

### Örnek 3: Kullanıcı Hesabı Oluşturucu

```bash
#!/bin/bash
# create-users.sh - Bir dosyadan birden fazla kullanıcı oluştur

# Girdi dosyası formatı: kullaniciadi,tamad,grup
# Örnek: john,John Smith,developers

set -euo pipefail

if [ $# -ne 1 ]; then
    echo "Kullanım: $0 <kullanicilar_dosyasi>"
    echo "Dosya formatı: kullaniciadi,tamad,grup"
    exit 1
fi

USERS_FILE=$1

if [ ! -f "$USERS_FILE" ]; then
    echo "Hata: '$USERS_FILE' dosyası bulunamadı!"
    exit 1
fi

while IFS=, read -r username fullname group; do
    # Boş satırları ve yorumları atla
    [[ -z "$username" || "$username" =~ ^# ]] && continue

    # Kullanıcının var olup olmadığını kontrol et
    if id "$username" &>/dev/null; then
        echo "ATLANDI: '$username' kullanıcısı zaten mevcut"
        continue
    fi

    # Grup yoksa oluştur
    if ! getent group "$group" &>/dev/null; then
        echo "Grup oluşturuluyor: $group"
        sudo groupadd "$group"
    fi

    # Kullanıcı oluştur
    echo "Kullanıcı oluşturuluyor: $username ($fullname), grup: $group"
    sudo useradd -m -c "$fullname" -g "$group" "$username"

    echo "'$username' kullanıcısı başarıyla oluşturuldu"
done < "$USERS_FILE"

echo "Tüm kullanıcılar işlendi!"
```

### Örnek 4: Sistem Sağlık Kontrolü

```bash
#!/bin/bash
# health-check.sh - Temel sistem sağlık kontrolü

echo "====================================="
echo "    SİSTEM SAĞLIK KONTROLÜ RAPORU"
echo "    $(date '+%Y-%m-%d %H:%M:%S')"
echo "====================================="
echo ""

# 1. Sistem Uptime
echo "--- Uptime ---"
uptime -p
echo ""

# 2. CPU Yükü
echo "--- CPU Yükü ---"
load=$(uptime | awk -F'load average:' '{print $2}')
echo "Ortalama Yük: $load"
echo ""

# 3. Bellek Kullanımı
echo "--- Bellek Kullanımı ---"
free -h | awk 'NR==2{printf "Kullanılan: %s / %s (%.1f%%)\n", $3, $2, $3/$2*100}'
echo ""

# 4. Disk Kullanımı
echo "--- Disk Kullanımı ---"
df -h / | awk 'NR==2{printf "Kullanılan: %s / %s (%s)\n", $3, $2, $5}'
echo ""

# 5. En Çok CPU Kullanan 5 Süreç
echo "--- En Çok CPU Kullanan 5 Süreç ---"
ps aux --sort=-%cpu | head -6 | awk '{printf "%-10s %5s%% %5s%% %s\n", $1, $3, $4, $11}'
echo ""

# 6. Giriş Yapmış Kullanıcılar
echo "--- Giriş Yapmış Kullanıcılar ---"
who | awk '{print $1, "from", $5, "since", $3, $4}'
echo ""

# 7. Ağ Durumu
echo "--- Ağ ---"
ip -4 addr show | grep inet | awk '{print $NF, $2}'
echo ""

echo "====================================="
echo "    Sağlık kontrolü tamamlandı!"
echo "====================================="
```

### Örnek 5: Menü Tabanlı Sistem Aracı

```bash
#!/bin/bash
# system-tool.sh - Etkileşimli sistem yönetim aracı

show_menu() {
    echo ""
    echo "==============================="
    echo "   Sistem Yönetim Aracı"
    echo "==============================="
    echo "1) Sistem bilgisini göster"
    echo "2) Disk kullanımını göster"
    echo "3) Bellek kullanımını göster"
    echo "4) Aktif kullanıcıları göster"
    echo "5) Çalışan servisleri göster"
    echo "6) Dosya ara"
    echo "0) Çıkış"
    echo "==============================="
}

while true; do
    show_menu
    read -p "Bir seçenek seçin [0-6]: " choice

    case $choice in
        1)
            echo ""
            echo "Hostname: $(hostname)"
            echo "OS: $(uname -o)"
            echo "Kernel: $(uname -r)"
            echo "Uptime: $(uptime -p)"
            ;;
        2)
            echo ""
            df -h | head -10
            ;;
        3)
            echo ""
            free -h
            ;;
        4)
            echo ""
            who
            ;;
        5)
            echo ""
            systemctl list-units --type=service --state=running | head -20
            ;;
        6)
            read -p "Aranacak dosya adını girin: " fname
            echo "Aranıyor..."
            find /home -name "$fname" 2>/dev/null
            ;;
        0)
            echo "Görüşürüz!"
            exit 0
            ;;
        *)
            echo "Geçersiz seçenek!"
            ;;
    esac
done
```

---

## Özet

### Script Yapısı

```bash
#!/bin/bash
# Açıklama: Bu script ne yapar
# Kullanım: ./script.sh [argümanlar]
# Yazar: Adınız

set -euo pipefail        # Güvenlik ayarları

# Değişkenler
MY_VAR="value"

# Fonksiyonlar
my_function() {
    local param=$1
    echo "$param"
}

# Ana mantık
if [ $# -lt 1 ]; then
    echo "Kullanım: $0 <argüman>"
    exit 1
fi

my_function "$1"
```

### Temel Kavramlar

1. **Shebang**: `#!/bin/bash` - sisteme hangi yorumlayıcının kullanılacağını söyler
2. **Değişkenler**: `name="value"` - `=` etrafında boşluk yok
3. **Kullanıcı Girdisi**: `read -p "Prompt: " variable`
4. **Aritmetik**: `$((ifade))` veya ondalıklar için `bc`
5. **Koşullar**: `if [ koşul ]; then ... fi`
6. **Case**: `case $var in pattern) ... ;; esac`
7. **For Döngüsü**: `for item in list; do ... done`
8. **While Döngüsü**: `while [ koşul ]; do ... done`
9. **Fonksiyonlar**: `func_name() { ... }`
10. **Argümanlar**: `$1, $2, $#, $@`
11. **Çıkış Kodları**: `0` = başarılı, `1-255` = hata

### Değişken Hızlı Referansı

```bash
$0             # Script adı
$1, $2...      # Konumsal argümanlar
$#             # Argüman sayısı
$@             # Tüm argümanlar (ayrı kelimeler)
$?             # Son komutun çıkış kodu
$$             # Mevcut scriptin PID'i
$USER          # Mevcut kullanıcı adı
$HOME          # Ev dizini
$PWD           # Mevcut dizin
$(command)     # Komut ikamesi (command substitution)
```

### Karşılaştırma Operatörleri Hızlı Referansı

```
Sayılar:            String'ler:          Dosyalar:
-eq  (eşit)         =   (eşit)           -f (dosya mı)
-ne  (eşit değil)   !=  (eşit değil)     -d (dizin mi)
-gt  (büyük)        -z  (boş)            -e (var mı)
-ge  (büyük/eşit)   -n  (boş değil)      -r (okunabilir)
-lt  (küçük)                             -w (yazılabilir)
-le  (küçük/eşit)                        -x (çalıştırılabilir)
```

### En İyi Uygulamalar

```bash
# 1. Her zaman shebang kullanın
#!/bin/bash

# 2. Güvenlik ayarlarını kullanın
set -euo pipefail

# 3. Değişkenlerinizi tırnak içine alın
echo "$variable"    # Şöyle değil: echo $variable

# 4. Fonksiyonlarda local kullanın
my_func() {
    local my_var="value"
}

# 5. Argümanları doğrulayın
if [ $# -lt 1 ]; then
    echo "Kullanım: $0 <arg>"
    exit 1
fi

# 6. Anlamlı değişken isimleri kullanın
backup_directory="/backups"    # Şöyle değil: bd="/backups"

# 7. Netlik için yorumlar ekleyin
# Disk kullanım yüzdesini hesapla
usage=$((used * 100 / total))

# 8. Tekrar kullanılabilir kod için fonksiyon kullanın
log() { echo "[$(date)] $1"; }

# 9. Hataları düzgün şekilde yönetin
if ! command -v curl &>/dev/null; then
    echo "Hata: curl kurulu değil"
    exit 1
fi

# 10. Geçici dosyaları temizleyin
trap 'rm -f /tmp/myscript_*' EXIT
```

---

**İyi Scriptlemeler!**
