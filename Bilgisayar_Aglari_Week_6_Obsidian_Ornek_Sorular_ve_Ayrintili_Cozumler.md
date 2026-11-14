---
title: "Week 6 — Her Bölüm İçin Örnek Sorular ve Ayrıntılı Çözümler"
aliases:
  - "Bilgisayar Ağları Week 6 Soru Bankası"
tags:
  - bilgisayar-aglari
  - week-6
  - veri-baglanti
  - stop-and-wait
  - go-back-n
  - selective-repeat
  - hdlc
  - ppp
  - vize
course: "Bilgisayar Ağları ve Veri İletişimi"
week: 6
type: "Çözümlü soru bankası"
source: "Week 6 (81 slayt), Tanenbaum Computer Networks 4. baskı temelli ders"
language: tr
---

# Week 6 — Bölüm Bölüm Örnek Sorular ve Ayrıntılı Çözümler

> [!info] Bu dosya nasıl kullanılmalı?
> Bu belge, [[Bilgisayar_Aglari_Week_6_Obsidian_Tum_Ders_Notlari|Week 6 Tüm Ders Notları]] dosyasının **tamamlayıcı çözümlü soru bankasıdır**. Önce soruyu kendin çöz; ardından **Çözüm** kısmını kontrol et. Bütün açıklamalar Türkçedir; İngilizce teknik terimler gerektiğinde parantez içinde verilmiştir.

> [!important] Kaynak ve gösterim uyarısı
> Sorular, işlenen altı konu bölümüne göre **özgün olarak hazırlanmıştır**; gerçek vize soruları değildir. ACK numarasının ‘son alınan’ mı yoksa ‘sonraki beklenen’ mi olduğunu soruda belirtilen kurala göre yorumla. Hesaplamalarda aksi belirtilmedikçe çerçeve hatası, işlem gecikmesi ve ACK iletim süresi ihmal edilir.

## İçindekiler

- [[#Bölüm 1 — Protokol Varsayımları, Çerçeveler ve Zamanlayıcılar|Bölüm 1: Protokol Varsayımları, Çerçeveler ve Zamanlayıcılar]] (Slayt 1–15, 6 soru)
- [[#Bölüm 2 — Protokol 1–3, Stop-and-Wait ve ARQ|Bölüm 2: Protokol 1–3, Stop-and-Wait ve ARQ]] (Slayt 16–28, 6 soru)
- [[#Bölüm 3 — Kayar Pencere, Piggybacking ve Pencere Yönetimi|Bölüm 3: Kayar Pencere, Piggybacking ve Pencere Yönetimi]] (Slayt 29–45, 7 soru)
- [[#Bölüm 4 — One-Bit, Go-Back-N ve Selective Repeat|Bölüm 4: One-Bit, Go-Back-N ve Selective Repeat]] (Slayt 46–59, 7 soru)
- [[#Bölüm 5 — Protokol Doğrulama, FSM ve Petri Ağları|Bölüm 5: Protokol Doğrulama, FSM ve Petri Ağları]] (Slayt 60–69, 6 soru)
- [[#Bölüm 6 — HDLC, PPP, LCP ve NCP|Bölüm 6: HDLC, PPP, LCP ve NCP]] (Slayt 70–81, 7 soru)
- [[#Kapsamlı Formül Föyü|Kapsamlı formül föyü]]
- [[#Karma Deneme — 20 Soru|Karma deneme]]
- [[#Sınavda Yapılan 12 Yaygın Hata|12 yaygın hata]]

---

## Bölüm I — Protokol Varsayımları, Çerçeveler ve Zamanlayıcılar

**Kapsam:** Slayt 1–15 · **Soru sayısı:** 6

### 1.1 — Kavramsal — Ağ katmanındaki paket ile veri bağlantı çerçevesi

**Soru:** Bir ana bilgisayar ağ katmanından 1.000 baytlık paket alıyor. Veri bağlantı katmanı ne yapar? Paket, çerçeve ve bit akışı arasındaki farkı açıkla.

> [!success]- Ayrıntılı çözümü göster
> **Adım 1 — Veri birimini tanımla.** Ağ katmanı, taşıtılmasını istediği **paketi** veri bağlantı katmanına verir.
>
> **Adım 2 — Kapsülle.** Veri bağlantı katmanı paket etrafına protokole göre başlık, gerektiğinde son ek ekleyerek **çerçeve (frame)** oluşturur. Kaynak/hedef bağlantı adresleri, kontrol ve hata tespiti alanları bulunabilir.
>
> **Adım 3 — İlet.** Fiziksel katman çerçevenin bitlerini fiziksel ortamda sinyallerle taşır. Alıcıda ters işlem uygulanır.
>
> **Sonuç:** Paket = ağ katmanı veri birimi; çerçeve = veri bağlantı veri birimi; bitler = fiziksel aktarımın parçaları. 1.000 baytlık paket, başlık ve son ek nedeniyle daha büyük çerçevede taşınabilir.
>

### 1.2 — Kavramsal — Simplex ile full-duplex

**Soru:** Simplex, half-duplex ve full-duplex iletişimi birbirinden ayır. Bir protokolün simplex veri gönderirken neden ters yönde ACK kullanabileceğini değerlendir.

> [!success]- Ayrıntılı çözümü göster
> **Simplex:** Veri yalnızca tek yönde akar. **Half-duplex:** İki yön mümkündür fakat aynı anda değil. **Full-duplex:** İki yönde eşzamanlı iletim yapılabilir.
>
> Dersin *simplex protocol* ifadesi çoğu kez **uygulama/ağ katmanı verisinin tek yönde akması** varsayımıdır. Buna rağmen alıcı veri bağlantı katmanında ters yönde ACK çerçevesi gönderebilir. Dolayısıyla ‘simplex veri akışı’ ile ‘fiziksel ortam hiçbir şekilde ters yönde iletişime izin vermez’ önermelerini eşitleme.
>

### 1.3 — Kavramsal — Protokol 1 varsayımları

**Soru:** Protokol 1 (Unrestricted Simplex Protocol) neden alıcıyı veya kanalı kontrol etmek zorunda değildir? Üç kritik ideal varsayımı yaz.

> [!success]- Ayrıntılı çözümü göster
> Protokol 1 için öğretici ideal varsayımlar: **(1)** iletim kanalı hatasızdır, çerçeveler kaybolmaz/bozulmaz; **(2)** alıcı sonsuz hızda işleyebilir, dolayısıyla akış kontrolü gerekmez; **(3)** veri hep tek yönde gönderilir.
>
> Bu koşullarda gönderici istediği zaman bir sonraki çerçeveyi gönderebilir. Gerçek ağda alıcı sınırlı kapasiteli ve kanal güvenilmez olabileceği için Protokol 1 tek başına yeterli değildir.
>

### 1.4 — Senaryo — Zamanlayıcı gerekliliği

**Soru:** Gönderici bir çerçeveyi yolluyor, alıcı aldığı hâlde ACK yolda kayboluyor. Zamanlayıcı yoksa ve varsa ne olur?

> [!success]- Ayrıntılı çözümü göster
> **Zamanlayıcı yoksa:** Gönderici onayı sonsuza kadar bekleyebilir; protokol ilerlemesi durur (liveness problemi).
>
> **Zamanlayıcı varsa:** Belirlenen süre sonunda **timeout** olayı oluşur; gönderici ilgili çerçeveyi yeniden gönderebilir. Ancak alıcı ilk gönderimi zaten teslim almış olabilir. Bu nedenle yalnızca zaman aşımı yeterli değildir; **sıra numarasıyla kopya tespiti** de gereklidir.
>
> **Sınav mesajı:** Zamanlayıcı kaybolan ACK/veri durumunda ilerlemeyi sağlar; sıra numarası tekrar teslimi engeller.
>

### 1.5 — Kavramsal — Hata denetimi ve akış kontrolü

**Soru:** Alıcı veri işleyemediği için tamponu doluyorsa ve ayrı bir durumda iletim hattında bit hatası oluşuyorsa hangi problem ve hangi mekanizma gerekir?

> [!success]- Ayrıntılı çözümü göster
> Alıcının işleme kapasitesinin aşılması **akış kontrolü (flow control)** problemidir; Stop-and-Wait veya uygun gönderme penceresi gibi mekanizmalar kullanılabilir.
>
> Bitlerin bozulması **hata tespiti/hata kurtarma** problemidir; CRC ile hata tespit edilebilir, yeniden iletim (ARQ) istenebilir.
>
> Ağın ara yönlendiricilerinde kuyrukların dolması ise **tıkanıklık (congestion)** sorunudur; akış kontrolünden farklıdır.
>

### 1.6 — Hesaplama — Çerçeve yükü

**Soru:** 1.200 baytlık paket, 14 bayt başlık ve 4 bayt son ek taşıyan bir çerçeveye yerleştiriliyor. Çerçeve toplam kaç bayt? Veri alanı oranı kaçtır?

> [!success]- Ayrıntılı çözümü göster
> Toplam boyut: \(1200+14+4=\boxed{1218\text{ bayt}}\).
>
> Veri alanı oranı: \(\eta=1200/1218\times100\approx\boxed{\%98{,}52}\).
>
> Bu hesap yalnızca verilen çerçeve başlığı ve son ekini hesaba katar; önsöz, çerçeveler arası boşluk ve diğer fiziksel yükler ayrıca ele alınabilir.
>

---

## Bölüm II — Protokol 1–3, Stop-and-Wait ve ARQ

**Kapsam:** Slayt 16–28 · **Soru sayısı:** 6

### 2.1 — Karşılaştırma — Protokol 1, 2 ve 3

**Soru:** Protokol 1, Protokol 2 ve Protokol 3 hangi varsayımlarla çalışır, her biri bir öncekinin hangi eksikliğini giderir?

> [!success]- Ayrıntılı çözümü göster
> **Protokol 1:** Hatasız kanal + sınırsız hızda alıcı varsayımı; akış veya hata kontrolü yoktur.
>
> **Protokol 2 (Stop-and-Wait):** Kanal hatasızdır fakat alıcı hızına uyulmalıdır; gönderici her çerçeveden sonra alıcının hazır olmasını/onayını bekler. **Akış kontrolü** sağlar.
>
> **Protokol 3 (PAR / Stop-and-Wait ARQ):** Kanal çerçeve veya ACK kaybedebilir; zamanlayıcı, ACK ve **1 bitlik sıra numarası** ile hata kurtarma ve kopya ayıklama eklenir.
>
> Sıralama: ideal iletişim → alıcıyı bekleme → kayıp/tekrar sorununu çözme.
>

### 2.2 — Senaryo — ACK kaybı

**Soru:** A, sıra numarası 0 olan çerçeveyi B'ye gönderir. B çerçeveyi doğru alır ve ACK yollar; ACK kaybolur. Sonraki olayları sırayla yaz.

> [!success]- Ayrıntılı çözümü göster
> 1. A, `DATA(0)` gönderir ve zamanlayıcıyı başlatır.
> 2. B, `DATA(0)` alır, üst katmana **bir kez** teslim eder; sonraki beklenen sıra numarası 1 olur; ACK yollar.
> 3. ACK kaybolur. A, `timeout` olunca `DATA(0)` tekrar gönderir.
> 4. B artık `DATA(1)` beklemektedir. Tekrar gelen `DATA(0)` **kopyadır**; üst katmana ikinci kez vermez; uygun ACK'i tekrar gönderir.
> 5. A ACK'i alırsa sıradaki yeni çerçeve olan `DATA(1)`'e geçebilir.
>
> **Ana fikir:** Güvenilirlik yalnızca tekrar göndermekle değil, **aynı veriyi iki kez teslim etmemekle** de ilgilidir.
>

### 2.3 — Kavramsal — 1 bit neden yeterli?

**Soru:** Stop-and-Wait ARQ'da sıra numaraları neden yalnızca 0 ve 1 olabilir? 3. çerçevenin sıra numarası kaç olur?

> [!success]- Ayrıntılı çözümü göster
> Stop-and-Wait'te aynı anda **en fazla bir onaysız veri çerçevesi** bulunur. Alıcının ayırt etmesi gereken iki durum vardır: ‘beklediğim yeni çerçeve’ ve ‘öncekinin tekrar gönderimi’. Bu nedenle numaralar dönüşümlü olarak **0,1,0,1,...** kullanılır (Alternating Bit Protocol).
>
> İlk çerçeve 0 ise üçüncü yeni çerçeve tekrar **0** numarasını taşır. Sıra numarası tek başına bütün tarihçeyi değil, o anki protokol durumuyla birlikte yeni/kopya ayrımını sağlar.
>

### 2.4 — Hesaplama — Stop-and-Wait verimliliği

**Soru:** Çerçeve uzunluğu 1.000 bayt, hat hızı 1 Mbps, tek yön yayılma gecikmesi 20 ms, ACK iletim süresi ihmal. Stop-and-Wait kanal kullanımı kaçtır?

> [!success]- Ayrıntılı çözümü göster
> Veri çerçevesinin iletim süresi: \(T_t=(1000\times8)/10^6=0{,}008\,s=8\,ms\).
>
> Gidiş-dönüş yayılma süresi: \(2T_p=40\,ms\).
>
> Bir çevrim yaklaşık \(T_t+2T_p=48\,ms\).
>
> Kullanım: \(U=8/48=\boxed{1/6\approx\%16{,}67}\).
>
> **Dikkat:** Bu formül, ACK iletim ve işlem süreleri ihmal edildiğinde geçerlidir.
>

### 2.5 — Hesaplama — Efektif veri hızı

**Soru:** Önceki soruda çerçevenin 800 baytı gerçek uygulama verisi, kalanı çerçeve yükü olsaydı etkili uygulama aktarım hızı kaç kbps olurdu?

> [!success]- Ayrıntılı çözümü göster
> Bir çevrimde 800 bayt = 6.400 bit uygulama verisi iletilir. Çevrim süresi 48 ms.
>
> \(R_{goodput}=6400/0{,}048=133333{,}3\,bit/s\).
>
> **Sonuç:** \(\boxed{133{,}33\,kbps}\).
>
> Kanal kullanım oranıyla uygulama faydalı veri hızı farklı ölçütlerdir.
>

### 2.6 — Senaryo — Timeout çok kısa olursa

**Soru:** Gidiş-dönüş süresi 80 ms olan bir hatta zaman aşımı 20 ms seçilirse ne tür sonuçlar doğabilir?

> [!success]- Ayrıntılı çözümü göster
> ACK henüz normal şekilde geri dönemeden zamanlayıcı dolabilir; gönderici **gereksiz yeniden iletim** yapar. Bu durum hat üzerindeki trafik yükünü artırır, alıcıda daha fazla kopya ayıklama gerektirir ve verimliliği azaltabilir.
>
> Zaman aşımı çok uzun olursa gerçek kayıpların toparlanması gecikir. Pratik protokoller süreyi ölçülen gecikmeye göre ayarlamaya çalışır.
>

---

## Bölüm III — Kayar Pencere, Piggybacking ve Pencere Yönetimi

**Kapsam:** Slayt 29–45 · **Soru sayısı:** 7

### 3.1 — Kavramsal — Gönderme ve alma penceresi

**Soru:** Gönderme penceresi ve alma penceresi neyi ifade eder? 3 çerçevelik gönderme penceresinin Stop-and-Wait'e üstünlüğü nedir?

> [!success]- Ayrıntılı çözümü göster
> **Gönderme penceresi**, henüz onay beklerken eşzamanlı yollanmasına izin verilen sıra numaralarını belirler. **Alma penceresi**, alıcının hangi sıra numaralarını kabul edip saklayabileceğini belirler.
>
> Stop-and-Wait yalnızca **1** onaysız çerçeveye izin verirken pencere 3 ise gönderici ACK beklemeden peş peşe üç çerçeve gönderebilir. Böylece uzun gidiş-dönüş gecikmelerinde hat boş kalmayabilir. Bunun bedeli tampon, sayaç ve yeniden iletim yönetiminin karmaşıklaşmasıdır.
>

### 3.2 — Kavramsal — Piggybacking

**Soru:** İki yönlü veri aktarımında ACK'i ayrı çerçeve yerine ters yöndeki veri çerçevesine ekleme işlemini açıkla. Avantajı ve riski nedir?

> [!success]- Ayrıntılı çözümü göster
> **Piggybacking**, alınan veriye ait ACK bilgisinin ters yönde gönderilecek **veri çerçevesinin başlığına eklenmesidir**. Ayrı ACK çerçevesi sayısını azaltarak başlık ve kanal kullanım yükünü düşürür.
>
> **Risk:** Ters yönde yakın zamanda veri yoksa ACK gereğinden uzun gecikir ve gönderici zaman aşımına düşebilir. Bu nedenle ACK belirli süre bekletilir; bu süre içinde ters yönde veri oluşmazsa **bağımsız ACK** gönderilir.
>

### 3.3 — Senaryo — Piggybacking karar süresi

**Soru:** A→B yönünde çerçeve ulaştı. B'nin A'ya göndereceği verinin 15 ms sonra oluşacağı tahmin ediliyor. B'nin ACK bekletme sınırı 10 ms. Ne yapılmalı?

> [!success]- Ayrıntılı çözümü göster
> ACK 10 ms içinde ters yöndeki veriyle birleştirilemiyorsa daha fazla beklenmez; **bağımsız ACK** gönderilmesi gerekir. 15 ms sonra veri oluşması ACK'i 15 ms bekletmeyi haklı çıkarmaz; belirlenmiş ACK sınırı aşılmış olur.
>
> **Ders:** Piggybacking bant genişliği kazancı ile onay gecikmesi arasında denge gerektirir.
>

### 3.4 — Hesaplama — Pencere büyüklüğü ve tam kullanım

**Soru:** 1.000 bayt çerçeve, 2 Mbps hat hızı, 12 ms tek yön yayılma gecikmesi var. ACK iletimi ihmal. Hattı sürekli meşgul edebilmek için en küçük gönderme pencere boyutu nedir?

> [!success]- Ayrıntılı çözümü göster
> \(T_t=8000/(2\times10^6)=4\,ms\).
>
> \(a=T_p/T_t=12/4=3\).
>
> İdeal tam kullanım için \(W\ge 1+2a=7\).
>
> **Sonuç:** \(\boxed{W_{min}=7}\) çerçeve.
>
> Örnek varsayım: hata yok, ACK/işleme süreleri ihmal, aynı uzunlukta çerçeveler.
>

### 3.5 — Hesaplama — Pencere ve kullanım oranı

**Soru:** Önceki hatta gönderme penceresi W=4 olursa yaklaşık en yüksek kanal kullanımı kaç olur?

> [!success]- Ayrıntılı çözümü göster
> İdeal yaklaşım: \(U=\min(1,W/(1+2a))\). Burada \(a=3\), \(W=4\).
>
> \(U=4/7=\boxed{\%57{,}14}\).
>
> Pencere 7'ye çıkarılırsa ideal modelde tam kullanıma yaklaşılır. Büyüklük tek başına gerçek ağda %100 faydalı veri garantisi değildir; başlık, kayıp, işlem ve diğer etkiler vardır.
>

### 3.6 — Kavramsal — Kümülatif ACK

**Soru:** Gönderici 0,1,2,3 çerçevelerini gönderdi. 0,1,2 sırayla alındı. 'Bir sonraki beklenen sıra numarasını' bildiren kümülatif ACK gösterimine göre alıcı hangi ACK değerini yollar?

> [!success]- Ayrıntılı çözümü göster
> Bu soruda ACK gösterimi **son doğru alınan değil, bir sonraki beklenen çerçeve numarası** olarak tanımlanmıştır. 0,1,2 ulaştıysa alıcı **3** bekler ve **ACK=3** yollar.
>
> **Çok önemli not:** Ders kitaplarındaki kimi algoritmalarda ACK alanı **son doğru alınan sıra numarasını** bildirir. O gösterimde sonuç ACK=2 olurdu. Soruda kullanılan ACK tanımını mutlaka kontrol et.
>

### 3.7 — Senaryo — Gönderme penceresinin kayması

**Soru:** 3 bitlik sıra numarası kullanılıyor (0–7); gönderme penceresi başlangıçta [3,4,5,6]. 3 ve 4 onaylanırsa yeni pencere nasıl görünür?

> [!success]- Ayrıntılı çözümü göster
> Kümülatif onaylar 3 ve 4'ü kapatınca pencere tabanı 3'ten **5**'e ilerler. W=4 korunursa etkin gönderme penceresi **[5,6,7,0]** olur.
>
> Buradaki `0`, modulo 8 sıra numarası sarılmasından kaynaklanır. Pencereyi sürekli artan tam sayı değil, döngüsel sıra uzayı üzerinde düşünmek gerekir.
>

---

## Bölüm IV — One-Bit, Go-Back-N ve Selective Repeat

**Kapsam:** Slayt 46–59 · **Soru sayısı:** 7

### 4.1 — Kavramsal — Protokol 4 ve sıra numarası

**Soru:** One-Bit Sliding Window protokolünün Stop-and-Wait ARQ ile ilişkisini açıkla. Pencere boyutu nedir?

> [!success]- Ayrıntılı çözümü göster
> One-Bit Sliding Window, iki yönlü iletişimde veri ve ACK alanlarını birlikte ele alan, **1 bit sıra numarası (0/1)** ve **bir çerçevelik gönderme penceresi** kullanabilen bir kayar pencere biçimidir.
>
> Stop-and-Wait ARQ ile aynı temel fikir bulunur: Aynı anda yalnızca bir onaysız çerçeve, yinelenen çerçeveleri sıra numarasıyla ayırt etme ve gerekirse yeniden gönderme. Çift yönlü iletişimde piggybacking ve özel eşzamanlı gönderim durumları ayrıca önemlidir.
>

### 4.2 — Senaryo — Go-Back-N kaybı

**Soru:** Gönderici 0,1,2,3,4,5 çerçevelerini yolladı; 2 kayboldu. Alıcı penceresi 1 olan Go-Back-N'de alıcı ne yapar ve zaman aşımında hangi çerçeveler yeniden gönderilir?

> [!success]- Ayrıntılı çözümü göster
> Alıcı 0 ve 1'i sırasıyla kabul eder. **2 kaybolduğundan**, 3,4,5 zamanında ulaşsa bile sıra dışıdır; Go-Back-N alıcısı bunları **üst katman için kabul etmez/bufferlamaz**. Kümülatif ACK'i en son sıralı alınana göre tekrarlar (ACK semantiğine bağlı olarak 'son alınan 1' veya 'sonraki beklenen 2').
>
> Gönderici 2'nin kaybolduğunu zaman aşımıyla anladığında **2,3,4,5** onaysız çerçevelerini yeniden gönderir.
>
> **İpucu:** 'Go-Back-N' = ilk eksik çerçeveden itibaren geri dönüp devamını tekrar gönder.
>

### 4.3 — Senaryo — Selective Repeat kaybı

**Soru:** Aynı 0–5 aktarımında yalnızca 2 kayboluyor. Selective Repeat alıcısı 3,4,5'i kabul edebiliyorsa ne olur?

> [!success]- Ayrıntılı çözümü göster
> Selective Repeat alıcısının alma penceresi kapsıyorsa **3,4,5'i tamponda saklar** ve ayrı ayrı onaylayabilir. Eksik olan **2** yeniden gönderildiğinde tamponlanan parçalarla birlikte doğru sırada üst katmana teslim edilir.
>
> Gönderici yalnızca **2 numaralı eksik çerçeveyi** yeniden iletir (başka kayıp olmadığı varsayımıyla).
>
> GBN'ye göre daha az gereksiz tekrar vardır; ancak alıcı tamponu ve çerçeve başına durum/yönetim gereksinimi daha fazladır.
>

### 4.4 — Hesaplama — GBN en büyük pencere

**Soru:** Sıra numaraları 4 bit ile tutuluyorsa Go-Back-N için en büyük gönderme penceresi kaç olabilir?

> [!success]- Ayrıntılı çözümü göster
> Sıra numarası uzayı \(N=2^m=2^4=16\).
>
> Go-Back-N için klasik üst sınır: \(W_s\le N-1=\boxed{15}\).
>
> **Neden 16 değil?** Bütün sıra uzayı aynı anda pencerede olursa eski ve yeni turdaki çerçeveler, özellikle ACK kayıpları ve numara sarma durumlarında belirsizlik yaratabilir.
>

### 4.5 — Hesaplama — SR en büyük pencere

**Soru:** 4 bit sıra numarası kullanılırsa Selective Repeat için en büyük güvenli pencere kaçtır?

> [!success]- Ayrıntılı çözümü göster
> Sıra numarası uzayı \(N=16\). Klasik Selective Repeat koşulu (gönderme ve alma pencereleri eşit olduğunda): \(W\le N/2=\boxed{8}\).
>
> **Gerekçe:** Eski ve yeni çerçeve numaralarının alıcı penceresinde karışmasını engellemek için iki pencere döneminin numara uzayları çakışmamalıdır.
>
> Daha genel güvenli koşul sıklıkla \(W_s+W_r\le N\) olarak ifade edilir; bu dersin eşit pencere varsayımında \(W\le N/2\) sonucunu verir.
>

### 4.6 — Hesaplama — Gereken bit sayısı

**Soru:** Go-Back-N'de gönderme penceresi 20 çerçeve ve Selective Repeat'te pencere 20 çerçeve olacaksa ayrı ayrı en az kaç sıra numarası biti gerekir?

> [!success]- Ayrıntılı çözümü göster
> **GBN:** \(2^m-1\ge20\Rightarrow2^m\ge21\). En küçük \(m=\boxed{5}\) (32-1=31).
>
> **SR:** \(2^{m-1}\ge20\Rightarrow2^m\ge40\). En küçük \(m=\boxed{6}\) (64/2=32).
>
> **Yaygın tuzak:** İkisi için aynı pencere sınırını kullanmak; SR'de güvenli üst sınır daha küçüktür.
>

### 4.7 — Karşılaştırma — Hangi protokol ne zaman?

**Soru:** Kayıp oranının yüksek olduğu ve iletim gecikmesinin uzun olduğu ağda GBN ile SR'nin verimlilik ve karmaşıklık farkını açıklayın.

> [!success]- Ayrıntılı çözümü göster
> Uzun gecikmede aynı anda birden çok çerçeve gönderebilen kayar pencere yaklaşımı Stop-and-Wait'ten daha verimlidir.
>
> **GBN:** Alıcı sırasız gelenleri atar; ilk eksikten sonraki onaysız çerçeveler tekrar iletilir. Uygulaması görece basittir, çok kayıp olduğunda gereksiz yeniden iletim artar.
>
> **SR:** Alıcı sırasız gelenleri tamponlar; yalnız eksik çerçeveler yeniden gönderilir. Daha az tekrar iletim ama daha fazla tampon ve karmaşık zamanlayıcı/ACK yönetimi gerektirir.
>
> Kayıp oranı yüksekse ve kaynaklar uygunsa SR avantajlı olabilir; her durumda kesin tek seçenek yoktur.
>

---

## Bölüm V — Protokol Doğrulama, FSM ve Petri Ağları

**Kapsam:** Slayt 60–69 · **Soru sayısı:** 6

### 5.1 — Kavramsal — Safety ve liveness

**Soru:** Güvenilir veri bağlantı protokolünde güvenlik (safety) ve ilerleme (liveness) özelliklerini ayırt edip örnek ver.

> [!success]- Ayrıntılı çözümü göster
> **Safety (emniyet/doğruluk):** Kötü bir olayın gerçekleşmemesi. Örn. aynı çerçevenin uygulamaya iki kez teslim edilmemesi veya hiç gönderilmeyen verinin teslim edilmemesi.
>
> **Liveness (ilerleme/canlılık):** Uygun varsayımlar altında iyi bir olayın eninde sonunda gerçekleşmesi. Örn. kayıp sınırlıysa ve bağlantı sonunda çalışıyorsa bekleyen verinin bir gün teslim edilebilmesi.
>
> Zamanlayıcı eksikliği ilerleme sorununa, sıra numarası eksikliği tekrar teslim doğruluğu sorununa neden olabilir.
>

### 5.2 — FSM — İki durumlu gönderici

**Soru:** Stop-and-Wait göndericisi için `READY` ve `WAIT_ACK` durumlarını, olayları ve geçişleri yaz.

> [!success]- Ayrıntılı çözümü göster
> **READY:** Üst katmandan yeni paket gelirse çerçeve oluştur, gönder, zamanlayıcıyı başlat, **WAIT_ACK** durumuna geç.
>
> **WAIT_ACK + doğru ACK:** Zamanlayıcıyı iptal et, sıra numarasını değiştir, **READY** durumuna geç.
>
> **WAIT_ACK + timeout:** Aynı onaysız çerçeveyi yeniden gönder, zamanlayıcıyı yeniden başlat, **WAIT_ACK** durumunda kal.
>
> **WAIT_ACK + yanlış/eski ACK:** Mevcut onaysız çerçeveyi başarıyla onaylanmış sayma.
>
> Bu soyut model gerçek uygulamadaki hata olayları ve tampon yönetimiyle genişletilebilir.
>

### 5.3 — FSM — Kilitlenme (deadlock)

**Soru:** Bir protokolde gönderici ACK bekliyor; alıcı ise veri bekliyor; göndericinin zamanlayıcısı da yok. Bu durum neden sorunludur?

> [!success]- Ayrıntılı çözümü göster
> Gönderilen çerçevenin kaybolduğunu varsayalım. Gönderici yalnız ACK, alıcı yalnız veri bekler ve iki tarafta da yeni bir olayı oluşturacak mekanizma yoksa sistem **kilitlenme (deadlock)** yaşayabilir.
>
> **Düzeltme:** Göndericiye zaman aşımı ve yeniden iletim eklenir; alıcıya kopyaları ayırt etmek için sıra numarası eklenir. Doğrulama sırasında kilitlenmeye ulaşılabilir mi diye birleşik durum grafiği incelenir.
>

### 5.4 — Petri — Yer, geçiş ve jeton

**Soru:** Petri ağında `place`, `transition`, `token` ne ifade eder? Bir geçiş ne zaman ateşlenebilir?

> [!success]- Ayrıntılı çözümü göster
> **Yer (place):** Koşul/durum veya kaynak tutma noktasıdır. **Jeton (token):** O koşulun varlığını ya da mevcut kaynak sayısını temsil eder. **Geçiş (transition):** Bir olay veya eylemdir.
>
> Basit klasik Petri ağında geçişin tüm giriş yerlerinde gerekli sayıda jeton varsa geçiş **etkinleşir**. Ateşlendiğinde girişlerden belirtilen jetonlar tüketilir, çıkış yerlerine jetonlar eklenir. Bu yolla eşzamanlılık ve kaynak paylaşımı modellenebilir.
>

### 5.5 — Petri — İşaretleme hesabı

**Soru:** P1'de 2 jeton, P2'de 0 jeton var. T geçişi P1'den 1 jeton tüketip P2'ye 1 jeton bırakıyor. T iki kez ateşlenirse işaretleme ne olur?

> [!success]- Ayrıntılı çözümü göster
> Başlangıç \(M_0=(2,0)\).
>
> Birinci ateşleme: \(M_1=(1,1)\).
>
> İkinci ateşleme: \(M_2=(0,2)\).
>
> **Sonuç:** \(\boxed{M_2=(0,2)}\). Üçüncü ateşleme **etkin değildir**; P1'de gerekli jeton bulunmamaktadır.
>

### 5.6 — Doğrulama — Ulaşılabilir durumlar

**Soru:** İki bağımsız bileşenin sırasıyla 3 ve 4 olası durumu varsa birleşik sistem en fazla kaç durum içerebilir? Her kombinasyon mutlaka erişilebilir midir?

> [!success]- Ayrıntılı çözümü göster
> Teorik Kartezyen durum çarpımı \(3\times4=\boxed{12}\) durumdur. Ancak protokol geçiş kısıtları, senkronizasyon ve başlangıç durumu nedeniyle **her kombinasyona erişmek mümkün olmayabilir**.
>
> Doğrulama analizi başlangıç durumundan gerçek geçişleri izleyerek **ulaşılabilir durumları** belirler; ulaşılamayanlar sayılırsa hatalı sonuçlara varılabilir.
>

---

## Bölüm VI — HDLC, PPP, LCP ve NCP

**Kapsam:** Slayt 70–81 · **Soru sayısı:** 7

### 6.1 — Kavramsal — HDLC çerçevesi

**Soru:** HDLC çerçevesinin temel alanlarını sırayla yaz ve görevlerini açıkla.

> [!success]- Ayrıntılı çözümü göster
> Yaygın HDLC çerçevesi: **Flag | Address | Control | Information | FCS | Flag**.
>
> `Flag`: Çerçeve sınırlarını belirtir (`01111110`). `Address`: İstasyon adres bilgisi. `Control`: Çerçeve türü ve protokol kontrol bilgileri. `Information`: Taşınan veri (çerçeve türüne bağlı). `FCS`: Hata tespiti amacıyla çerçeve kontrol dizisi (CRC tabanlı).
>
> Protokol varyantlarına göre alanların uzunlukları değişebilir.
>

### 6.2 — Hesaplama — HDLC bit stuffing

**Soru:** Veri bit dizisi `011111101111110` içinde art arda beş `1` geldikten sonra `0` ekleme kuralını uygula. Sonuçta hangi diziyi bulursun?

> [!success]- Ayrıntılı çözümü göster
> Veriyi dikkatle iki parçaya ayıralım: `0 111111 0 111111 0`. Her **beş ardışık 1** sonrasında `0` eklenir.
>
> İlk altı `1`: `11111 0 1`; ikinci altı `1`: `11111 0 1`.
>
> Çıktı: `0 1111101 0 1111101 0` = \(\boxed{\texttt{01111101011111010}}\).
>
> Alıcı çerçeve sınırları içindeki stuffing sıfırlarını çıkarıp özgün diziyi elde eder. Sınır bayrağı stuffing uygulanacak veri alanının parçası değildir.
>

### 6.3 — Kavramsal — HDLC I/S/U çerçeveleri

**Soru:** HDLC'de I, S ve U çerçevelerinin farkı nedir?

> [!success]- Ayrıntılı çözümü göster
> **I (Information):** Kullanıcı verisi taşımak ve ilgili denetim/ACK bilgilerini kullanmak için.
>
> **S (Supervisory):** Akış ve hata kontrolü, onay ve tekrar isteme gibi denetim işlevleri için.
>
> **U (Unnumbered):** Bağlantı ve kontrol yönetimi dâhil numarasız denetim işlevleri için.
>
> Ayrıntılar kullanılan HDLC moduna ve çerçeve türüne bağlıdır.
>

### 6.4 — Kavramsal — PPP'nin bileşenleri

**Soru:** PPP neden yalnızca çerçeveleme protokolü değildir? LCP ve NCP'nin görevini açıklayınız.

> [!success]- Ayrıntılı çözümü göster
> **PPP (Point-to-Point Protocol)** nokta-noktaya bağlantılarda veri bağlantı çerçevelemesi ve bağlantı yapılandırma işlevleri sunar.
>
> **LCP (Link Control Protocol):** Bağlantının kurulması, seçeneklerin müzakeresi, test edilmesi ve kapatılması için kullanılır.
>
> **NCP (Network Control Protocol family):** Bağlantı üzerinde kullanılacak ağ katmanı protokollerinin yapılandırılmasıyla ilgilenir; örneğin IPv4 için IPCP ve IPv6 için IPv6CP.
>
> **Not:** PPP'nin kendisi uçtan uca IP yönlendirme yapmaz; nokta-noktaya bağlantı düzeyinde çalışır.
>

### 6.5 — Kavramsal — PPP çerçeve alanları

**Soru:** Yaygın PPP çerçevesinin alanlarını yaz. Protokol alanı ile FCS neyi gösterir?

> [!success]- Ayrıntılı çözümü göster
> Tipik PPP çerçevesi: **Flag | Address | Control | Protocol | Information | FCS | Flag**.
>
> **Protocol:** Veri alanında hangi üst katman protokolünün veya PPP kontrol protokolünün taşındığını belirtir. **FCS:** Çerçevenin iletim sırasında bozulup bozulmadığının tespitine yardımcı olur.
>
> Asenkron seri taşımada `0x7E` bayrağı ve kaçış (`0x7D`, XOR `0x20`) gibi oktet stuffing kuralları kullanılabilir; HDLC'nin bit stuffing yöntemiyle birebir aynı işlem sanılmamalıdır.
>

### 6.6 — Senaryo — PPP yaşam döngüsü

**Soru:** PPP noktadan noktaya bağlantı kurulup IPv4 trafiği taşınacaksa LCP ve IPCP'nin hangi sırayla devreye girdiğini ve bağlantının nasıl sonlandırıldığını anlat.

> [!success]- Ayrıntılı çözümü göster
> 1. Fiziksel bağlantı kullanılabilir hâle gelir.
> 2. **LCP** bağlantı parametrelerini müzakere ederek bağlantıyı kurar; gerekiyorsa kimlik doğrulama aşaması gelir.
> 3. **IPCP (bir NCP)** IPv4'e ilişkin ağ katmanı seçeneklerini yapılandırır.
> 4. Bağlantı açıkken IP paketleri PPP çerçeveleri içinde taşınır.
> 5. İş bittiğinde **LCP** ile bağlantı düzenli biçimde kapatılabilir, fiziksel bağlantı sona erebilir.
>
> **Anahtar:** LCP bağlantıyı, NCP ilgili ağ protokolünü yapılandırır.
>

### 6.7 — Karşılaştırma — HDLC ve PPP

**Soru:** HDLC ve PPP arasında üç benzerlik ve üç temel fark yazınız.

> [!success]- Ayrıntılı çözümü göster
> **Benzerlikler:** İkisi de veri bağlantı katmanıyla ilişkilidir; veriyi çerçeveler; hata tespiti için FCS kullanabilir.
>
> **Farklar:** (1) HDLC genel amaçlı bit odaklı bir çerçeveleme/denetim protokol ailesidir; PPP IP gibi birden fazla ağ katmanı protokolünü nokta-noktaya bağlantıda taşımaya yöneliktir. (2) PPP, LCP/NCP aracılığıyla açık bağlantı ve ağ katmanı yapılandırma mekanizmaları tanımlar. (3) HDLC'nin klasik bit stuffing gösterimi ile asenkron PPP'nin byte escaping gösterimi farklıdır.
>
> **Uyarı:** PPP çerçeve yapısı HDLC-benzeri olduğundan aralarında hiçbir ilişki yok demek yanlıştır.
>

---

## Kapsamlı Formül Föyü

### Stop-and-Wait ve gecikme

- Çerçeve iletim süresi: \(T_t=\frac{L_{bit}}{R_{bit/s}}\)
- Tek yön yayılma gecikmesi: \(T_p=\frac{d}{v}\)
- Gecikme oranı: \(a=T_p/T_t\)
- ACK ve işlem süreleri ihmal edildiğinde Stop-and-Wait kullanım oranı: \(U\approx\frac{1}{1+2a}=\frac{T_t}{T_t+2T_p}\)
- \(W\) çerçevelik kayar pencerenin ideal kullanım oranı: \(U\approx \min(1,\frac{W}{1+2a})\)
- Tam kullanıma yaklaşmak için: \(W\ge 1+2a\) (ideal varsayımlar altında)
- Uygulama faydalı veri hızı: \(\text{Goodput}=\frac{\text{Teslim edilen uygulama bitleri}}{\text{Geçen süre}}\)

### Sıra numarası ve pencere boyutu

Sıra numarası \(m\) bit ise uzay \(N=2^m\) farklı değere sahiptir.

| Protokol | Klasik sınır | Açıklama |
|---|---|---|
| Stop-and-Wait ARQ | \(W_s=1\) | 1 bit sıra numarası yeterli |
| Go-Back-N | \(W_s\le2^m-1\) | Alma penceresi tipik olarak 1 |
| Selective Repeat | \(W\le2^{m-1}\) | Eşit gönderme ve alma penceresi varsayımı |

> [!warning] Sınırın gerekçesini açıkla
> Sıra numaraları döngüseldir (modulo \(2^m\)). Göndericinin eski ve yeni pencere dönemindeki çerçeveleri karıştırmaması gerekir. SR için eşit pencerede yarı uzay kuralı kullanılır.

### Çerçeve verimliliği

\[
\eta_{frame}=\frac{\text{faydalı veri baytı}}{\text{toplam çerçeve baytı}}\times100
\]

> [!note] Verimlilik türleri farklıdır
> Çerçeve içindeki faydalı veri oranı, kanal zaman kullanımı ve uygulama iyi veri hızı (goodput) aynı metrik değildir.

## Karma Deneme — 20 Soru

Bu bölümdeki soruları **çözümlere bakmadan** çöz. Her biri 5 puandır; toplam 100 puan.

1. Protokol 1'in üç varsayımını ve gerçek ağda neden kullanılamayacağını yaz.
2. Protokol 2'nin Protokol 1'e eklediği mekanizma nedir?
3. Stop-and-Wait ARQ'da ACK kaybı neden tekrar teslim tehlikesi oluşturur?
4. Bir bitlik sıra numarası kullanıldığında ilk beş yeni çerçevenin numaralarını yaz.
5. 2.000 baytlık çerçeve, 4 Mbps hat, 10 ms tek yön yayılma: Stop-and-Wait kullanımını hesapla.
6. 1.200 bayt çerçevede 1.160 bayt uygulama verisi varsa çerçeve içi veri oranını hesapla.
7. Piggybacking'in avantajı ve en büyük riski nedir?
8. Gönderme penceresi 4 olan bir protokolün Stop-and-Wait'ten üstünlüğünü açıkla.
9. 4 bit sıra numarasında GBN ve SR için klasik en büyük pencereyi hesapla.
10. GBN'de 0–6 gönderiminde 3 kaybolursa hangileri yeniden yollanır (hepsi onaysız varsay)?
11. SR'de aynı kayıpta hangi çerçeve yeniden gönderilir? Alıcı 4–6'yı nasıl işler?
12. TCP'deki kümülatif ACK ile veri bağlantı katmanındaki kümülatif ACK arasında kavramsal ilişkiyi açıkla; ACK anlamının protokole göre değişebileceğini belirt.
13. FSM'de durum ve geçiş nedir? Stop-and-Wait'ten bir örnek ver.
14. Güvenlik (safety) ve ilerleme (liveness) özelliklerine birer protokol örneği ver.
15. Petri ağında yer, jeton ve geçişi açıklayınız.
16. Petri ağı başlangıç (3,0), her ateşleme (-1,+1): iki ateşleme sonucu nedir?
17. HDLC çerçevesinin temel alanlarını sırayla yaz.
18. HDLC bit stuffing kuralını bir örnekle açıkla.
19. PPP'deki LCP ile NCP arasındaki fark nedir? IPCP'yi belirt.
20. Asenkron PPP'de escape yaklaşımı ile HDLC bit stuffing yönteminin farkını açıkla.

> [!tip] Karma denemeyi değerlendirme yöntemi
> Soruları çözdükten sonra aynı numarayla yukarıdaki 6 bölümdeki ayrıntılı cevaplarla karşılaştır. Her soruda **doğru tanım (2 puan) + doğru mekanizma (2 puan) + kritik ayrıntı / örnek (1 puan)** ölçütünü kullan. Hesaplamalarda işlem adımları ve birimler puanlamaya dahildir.

## Sınavda Yapılan 12 Yaygın Hata

- [ ] ‘Simplex’ veri akışını ters yönde hiç ACK gönderilemez diye yorumlamak.
- [ ] ‘Çerçeve’ ile ‘paket’i aynı katmanın veri birimi saymak.
- [ ] ACK kaybolunca alıcının çerçeveyi hiç almadığını varsaymak.
- [ ] Zaman aşımını ekleyip **sıra numarası** ihtiyacını atlamak.
- [ ] Akış kontrolüyle **ağ tıkanıklık kontrolünü** karıştırmak.
- [ ] \(U=1/(1+2a)\) formülünü ACK süresinin önemli olduğu sorulara doğrudan uygulamak.
- [ ] Piggybacking yüzünden ACK'i sınırsız bekletmek.
- [ ] GBN'de sıra dışı çerçevelerin tamponlandığını söylemek.
- [ ] SR'de bütün onaysız çerçevelerin yeniden gönderildiğini söylemek.
- [ ] GBN için \(2^m-1\), SR için \(2^{m-1}\) ayrımını unutmak.
- [ ] HDLC'nin bit stuffing yöntemini PPP asenkron byte escaping ile aynı sanmak.
- [ ] PPP'nin LCP ve NCP işlevlerini veya bağlantı kurulumu sırasını karıştırmak.

## Obsidian Çalışma Bağlantıları

- [[Bilgisayar_Aglari_Week_6_Obsidian_Tum_Ders_Notlari|Week 6 — Tüm Ders Notları]]
- [[Bilgisayar_Aglari_Week_5_Obsidian_Tum_Ders_Notlari|Week 5 — Çerçeveleme, Hamming ve CRC]]
- [[Bilgisayar_Aglari_Week_2_Obsidian_Tum_Ders_Notlari|Week 2 — OSI ve TCP/IP Temelleri]]

> [!summary] Son tekrar stratejisi
> Önce 6 ana bölümdeki 39 çözümlü örneği çöz, sonra 20 soruluk karma denemeyi cevap anahtarına bakmadan yap. Özellikle **ACK kaybı**, **GBN–SR karşılaştırması**, **pencere sınırı** ve **PPP yaşam döngüsü** sorularında cevabı şema veya algoritma adımlarıyla açıklamaya çalış.
