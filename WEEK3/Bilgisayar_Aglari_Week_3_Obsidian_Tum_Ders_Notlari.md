---
title: "Bilgisayar Ağları ve Veri İletişimi — Week 3 — Tüm Ders Notları"
aliases:
  - "Week 3 Fiziksel Katman"
tags:
  - bilgisayar-aglari
  - yuksek-lisans
  - week-3
  - fiziksel-katman
  - vize
  - obsidian
course: "Bilgisayar Ağları ve Veri İletişimi"
week: 3
slide_count: 63
language: tr
source: "https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_3.pdf"
---

# Week 3 — Fiziksel Katman (Physical Layer)

> [!info] Kaynak ve kapsam
> Ankara Üniversitesi, Dr. Refik Samet, **Week_3.pdf**; Tanenbaum, *Computer Networks*, 4. baskı, Bölüm 2, s. 85–118 temel alınmıştır. **63 slaydın tamamı sırasıyla** ele alınmıştır. İngilizce teknik terimler ilk kullanımda parantez içinde verilmiştir. Kitabın tarihsel teknoloji verileri güncel mühendislik limitleriyle karıştırılmamalıdır.

> [!tip] Bu dosyayı nasıl çalışmalısın?
> İlk olarak 1–21 arası sinyaller ve matematik bölümünü öğren; **Nyquist, SNR, Shannon** formüllerini ezberlemek yerine hangi koşulda kullanıldıklarını anlamalısın. Ardından 22–37 iletim ortamları; 38–52 kablosuz dalgalar; 53–63 uydular bölümünü bitir. Her bölümün altındaki kontrol sorularını çözüp dosyanın sonundaki cevap anahtarıyla karşılaştır.

## İçindekiler
- [[#Bölüm I — Fiziksel Katman ve Sinyaller (1–21)|I. Fiziksel Katman, Fourier ve kapasite (1–21)]]
- [[#Bölüm II — Kılavuzlu Ortamlar (22–37)|II. Bakır ve fiber ortamlar (22–37)]]
- [[#Bölüm III — Kablosuz İletim ve Spektrum (38–52)|III. Kablosuz ortamlar (38–52)]]
- [[#Bölüm IV — Uydular (53–63)|IV. Uydu iletişimi (53–63)]]
- [[#Formül Föyü|Formül föyü]]
- [[#Çözümlü Sorular|Çözümlü sorular]]
- [[#Kavram Karşılaştırmaları ve Sınavda Sık Yapılan Hatalar|Kavram karşılaştırmaları]]
- [[#Deneme Soruları ve Cevap Anahtarı|Deneme ve cevap anahtarı]]


---

## Bölüm I — Fiziksel Katman ve Sinyaller (1–21)

### Slayt 1 — Giriş — Fiziksel Katman

Sunum, Tanenbaum *Computer Networks* 4. baskı, Bölüm 2’nin Fiziksel Katman (Physical Layer) konusunu işler. Amaç, bitlerin gerçek bir iletim ortamında nasıl temsil edildiğini, bir kanalın ne kadar veri taşıyabileceğini ve farklı iletim teknolojilerinin güçlü/zayıf yanlarını öğrenmektir. **Bu hafta 63 slayttır.**

### Slayt 2 — Fiziksel katmanın amacı

Fiziksel katman (Physical Layer), bir cihazdan diğerine **ham bit akışını** (raw bit stream) taşır. `0` ve `1` değerleri doğrudan kablonun içinde hareket etmez; gerilim, akım, ışık veya elektromanyetik dalga gibi fiziksel büyüklükler tarafından temsil edilir. Veri bağlantı katmanı çerçevelerle, fiziksel katman bit ve sinyallerle ilgilenir.

### Slayt 3 — Fiziksel katmanın işlevleri — 1

Tasarım soruları: **1 hangi gerilim seviyesi 0/1 olarak yorumlanır? 2 bit süresi kaç nanosaniyedir? 3 iletişim eşzamanlı olarak iki yönde yapılabilir mi?** Bit hızı `R` bit/s ise bit süresi `T_b=1/R` saniyedir. Örneğin 100 Mbps için 10 ns/bit. Simplex tek yönlü, half-duplex sırayla iki yönlü, full-duplex aynı anda iki yönlü iletimdir.

> [!example] Bit süresi örneği
> `R=10 Mbps` için `T_b=1/(10×10⁶)=100 ns`. Hız iki katına çıkarsa bit süresi yarıya düşer.

### Slayt 4 — Fiziksel katmanın işlevleri — 2

Bağlantı nasıl kurulur ve sonlandırılır? Konnektörde kaç pin vardır ve pinlerin görevleri nelerdir? Elektriksel, mekanik, işlevsel ve zamanlama (timing) arayüzleri bu katmanın kapsamındadır. İletim ortamı, modelde fiziksel katmanın altında düşünülse de mühendislik açısından fiziksel katmanla birlikte incelenir.

### Slayt 5 — Veri iletişiminin kuramsal temeli — 1

İletişim kablosunda gerilim veya akım zamanla değiştirilerek bilgi taşınır. Bir sinyal `g(t)` biçiminde modellenebilir: `t` zaman, `g(t)` gerilim/sinyal büyüklüğüdür. Matematiksel modelleme sinyallerin bozulması, bant genişliği ve taşıma kapasitesi konusunda öngörü sağlar.

### Slayt 6 — Veri iletişiminin kuramsal temeli — 2

Üç temel konu: **Fourier analizi**, **bant genişliği sınırlı sinyaller**, **kanalın azami veri hızı**. Önce sinyalin frekans bileşenlerini görür, sonra ortamın hangi bileşenleri geçirdiğini inceler, en sonunda Nyquist ve Shannon sınırlarını hesaplarız.

### Slayt 7 — Fourier analizi — 1

Her periyodik sinyal, uygun koşullar altında sinüs ve kosinüs bileşenlerinin toplamıyla ifade edilebilir. Dijital sinyalin ani geçişleri çok sayıda yüksek frekanslı harmonik gerektirir; iletişim ortamı yüksek frekansları zayıflatırsa alıcıdaki kare darbeler yuvarlaklaşır.

### Slayt 8 — Fourier analizi — 2

Periyot `T` ve temel frekans `f=1/T` olmak üzere sunumdaki ifade:

$$g(t)=\frac{c}{2}+\sum_{n=1}^{\infty} a_n\sin(2\pi nft)+\sum_{n=1}^{\infty} b_n\cos(2\pi nft).$$

`a_n`, `b_n` katsayıları n’inci harmonik bileşeni belirler; `c/2` ortalama (DC) bileşendir. Frekanslar `f,2f,3f,...` biçiminde yükselir.

### Slayt 9 — Fourier analizi — 3

Fourier katsayıları biliniyorsa sinüs/kosinüs toplamı ile zaman alanındaki sinyal yeniden kurulabilir. Tek seferlik sınırlı süreli bir bit örüntüsü bile analiz amacıyla `T` periyotla sonsuz tekrar ediyormuş gibi modellenebilir. **Bu matematiksel model, gerçek iletimin mutlaka periyodik olduğu anlamına gelmez.**

### Slayt 10 — Fourier analizi — 4

Bir periyottaki katsayılar:

$$a_n=\frac{2}{T}\int_0^T g(t)\sin(2\pi nft)\,dt,\quad b_n=\frac{2}{T}\int_0^T g(t)\cos(2\pi nft)\,dt,$$
$$c=\frac{2}{T}\int_0^T g(t)\,dt.$$

Sınavda çoğunlukla bu ifadelerin *ne anlama geldiği* ve harmoniklerin frekans/şekil ilişkisi önemlidir. Katsayıları hesaplamak için dalga biçiminin ve zaman aralıklarının eksiksiz verilmesi gerekir.

> [!question] Bölüm içi kontrol
> 1. Fourier’de temel frekans nedir?
> 2. Yüksek harmonikler neden dikdörtgen darbe biçimi için önemlidir?

### Slayt 11 — Bant genişliği sınırlı sinyaller — 1

Sunumun örneği ASCII `b` karakterinin 8 bitlik gösterimidir: `01100010`. Bitlerin bir sinyalde 0/1 gerilim seviyeleriyle gösterildiği bir dikdörtgen dalga çizilir. Ani geçişleri kusursuz oluşturmak teorik olarak sonsuz sayıda harmonik gerektirir.

### Slayt 12 — Bant genişliği sınırlı sinyaller — 2

`01100010` örüntüsünün Fourier katsayıları slaytta özel olarak türetilmiştir. Katsayılar darbenin nerede başlayıp bittiğine bağlıdır. Bu nedenle farklı bit örüntülerinin spektrumları farklı olur. Ana fikir: **Aynı bit hızında bile dalga şekli frekans bileşenlerinin dağılımını etkiler.**

### Slayt 13 — Bant genişliği sınırlı sinyaller — 3

Slayttaki (a) asıl kare dalga ve harmonik spektrumudur; (b) yalnızca bir harmonik, (c) iki harmonik ile yaklaşımı gösterir. Az harmonik tutulduğunda sinyal yumuşar ve dik köşeleri kaybolur. Bu şekli grafik üzerinden açıklayabilmelisin.

### Slayt 14 — Bant genişliği sınırlı sinyaller — 4

Dört ve sekiz harmonik kullanılarak yaklaşım geliştirilir. Harmonik sayısı artırıldığında orijinal kare darbeye daha çok yaklaşılır, ancak keskin geçişlerin çevresinde salınımlar görülebilir. Bu davranış Fourier yaklaşımının doğal bir özelliğidir; kanalın ideal, sınırsız bantlı olmadığı unutulmamalıdır.

### Slayt 15 — Bant genişliği sınırlı sinyaller — 5

İlgili harmonik genliğini belirleyen büyüklük `\sqrt{a_n^2+b_n^2}` şeklindedir. Sunum bunu harmonik RMS genlik grafiğiyle ilişkilendirir; teknik olarak sinüzoidal bir harmonik için RMS genliği ayrıca `1/\sqrt 2` faktörü içerir. Güç/enerji genliğin karesiyle ilişkilidir; **genlik ile enerji eşit değildir**.

### Slayt 16 — Bant genişliği sınırlı sinyaller — 6

Gerçek kanal bazı frekansları az, bazılarını çok zayıflatır. **Bant genişliği (Bandwidth)**, bu bağlamda kayda değer ölçüde iletilebilen frekans aralığıdır ve birimi Hz’dir. Kesim frekansının (cutoff frequency) ötesindeki bileşenler zayıflar. Sinyal geçişleri bozulur ve sembolleri ayırt etmek zorlaşabilir. Bant genişliği Hz ile veri hızı bit/s **aynı ölçü değildir**.

### Slayt 17 — Bant genişliği sınırlı sinyaller — 7

8 bitlik bir örüntü `b` bit/s hızla yollanırsa örüntünün periyodu `T=8/b` saniye, temel frekansı `f=b/8` Hz olur. 3 kHz üst kesim varsayılırsa geçebilecek en yüksek harmonik sayısı yaklaşık `3000/(b/8)=24000/b` olur. Bu **özel 8 bitlik tekrar eden örüntü örneği** genel veri hızı sınırı değildir.

> [!warning] Özel örnek ile genel sınırı karıştırma
> `24000/b` formülü sekiz bitin periyodik tekrarına ve **3 kHz kesim** varsayımına aittir; Nyquist veya Shannon teoreminin yerine geçmez.

### Slayt 18 — Bant genişliği sınırlı sinyaller — 8

Sunumdaki 3 kHz örneğinde hız arttıkça korunabilen harmonik azalır:

| Bit hızı | 8-bit süre | İlk harmonik | Yaklaşık harmonik sayısı |
|---:|---:|---:|---:|
| 300 bit/s | 26,67 ms | 37,5 Hz | 80 |
| 1.200 bit/s | 6,67 ms | 150 Hz | 20 |
| 2.400 bit/s | 3,33 ms | 300 Hz | 10 |
| 9.600 bit/s | 0,833 ms | 1.200 Hz | 2 |
| 19.200 bit/s | 0,417 ms | 2.400 Hz | 1 |
| 38.400 bit/s | 0,208 ms | 4.800 Hz | 0 |

**Yorum:** Kanal bant genişliği sabitse bit hızı yükseldikçe dalga biçimini korumak zorlaşır.

> [!question] Bölüm içi kontrol
> 1. 3 kHz kesimli 8 bit örneğinde bit hızı artarsa kaç harmonik kalır?
> 2. Nyquist ile bu örneğin temel farkı nedir?

### Slayt 19 — Kanalın azami veri hızı — Nyquist

İdeal **gürültüsüz**, bant genişliği `H` Hz ve `V` farklı sinyal düzeyi olan taban bant kanalında Nyquist hız üst sınırı:

$$R_{\max}=2H\log_2 V \quad (\text{bit/s}).$$

Örnek: `H=3.000 Hz`, `V=2` ise `R=2×3000×1=6000 bit/s`. `V=4` ise `12.000 bit/s`. `2H` işaret/sembol değişim hızına ilişkin temel sınırdır; sembol başına `log₂ V` bit taşınır.

> [!important] Nyquist koşulları
> Gürültüsüz bant sınırlı ideal kanal; `V`, birbirinden ayrılabilir sinyal düzeyi sayısıdır. Modülasyon türü ve kanal modeli değişirse bu sınırın yorumunu dikkatle yap.

### Slayt 20 — Sinyal–gürültü oranı ve desibel

**SNR (Signal-to-Noise Ratio)**, sinyal gücü `S` ile gürültü gücü `N` oranıdır. Güç oranının desibel karşılığı:

$$\mathrm{SNR}_{dB}=10\log_{10}(S/N).$$

Örnekler: `S/N=10 → 10 dB`, `100 → 20 dB`, `1000 → 30 dB`. **Ters dönüşüm:** `S/N=10^{(dB/10)}`. Desibel logaritmiktir: 30 dB, doğrusal güç oranı olarak 30 değil 1000’dir.

### Slayt 21 — Kanalın azami veri hızı — Shannon

**Gürültülü kanal** için Shannon kapasite sınırı:

$$C=H\log_2(1+S/N) \quad (\text{bit/s}).$$

`H`: Hz cinsinden bant genişliği; `S/N`: **doğrusal** güç oranı. Örnek: `H=3000 Hz`, `SNR=30 dB` ise `S/N=1000`; `C≈3000×log₂(1001)≈29.901 bit/s` (yaklaşık 30 kbps). **30 dB değerini formüle 30 olarak doğrudan koymak hatadır.** Shannon bilgi kuramsal sınırdır, pratik kullanım için hata düzeltme/modülasyon ve donanım koşulları da belirleyicidir.

> [!danger] Klasik sınav hatası
> `SNR=30 dB` verilmişse `S/N=10^(30/10)=1000` alınır. Formülde 30 **kullanılmaz**.

> [!question] Bölüm içi kontrol
> 1. Nyquist denkleminin varsayımı nedir?
> 2. 30 dB kaç doğrusal SNR’dir?
> 3. Shannon’da bant genişliği hangi birimdedir?


---

## Bölüm II — Kılavuzlu Ortamlar (22–37)

### Slayt 22 — İletim ortamlarına genel bakış

İletim ortamları ikiye ayrılır: **kılavuzlu (guided)** — bükümlü çift, koaksiyel, fiber gibi fiziksel yol boyunca ilerleyen sinyaller; **kılavuzsuz (unguided)** — radyo, mikrodalga, kızılötesi, lazer ve uydu iletişimi gibi uzayda yayılan elektromanyetik dalgalar. Karşılaştırma eksenleri: bant genişliği, zayıflama, girişim, maliyet, erişim, kurulum/bakım, mesafe ve gecikme.

### Slayt 23 — Kılavuzlu iletim ortamları

Sunumdaki dört başlık: manyetik ortam, bükümlü çift, koaksiyel kablo, fiber optik. **Manyetik ortamın** listede yer alması şaşırtıcıdır; kitap onu yüksek miktarda verinin fiziksel taşıma ile nakledilmesi perspektifinden inceler. Manyetik bant ağ kablosu değildir.

### Slayt 24 — Manyetik ortam

Manyetik teyp/bant ve çıkarılabilir depolama ortamları fiziksel olarak nakledilerek büyük veri miktarlarının taşınmasını sağlayabilir. Çok yüksek toplam veri miktarı ve düşük maliyet/bit sunabilir; ancak **gecikmesi çok yüksektir** ve etkileşimli iletişim için uygun değildir. Örnek: 100 TB veriyi kargo ile taşımak, çok yavaş internet bağlantısından hızlı toplam aktarım anlamına gelebilir. Bu yöntem günümüzde toplu veri taşıma/air-gapped transfer bağlamında düşünülür.

### Slayt 25 — Bükümlü çift — Yapı

Bükümlü çift (Twisted Pair), yalıtılmış iki bakır telin birbirine dolanmasından oluşur. Burulma, diferansiyel iletim ve dengeli hat özellikleri sayesinde dış elektromanyetik girişimin ve tel çiftleri arasındaki çapraz konuşmanın (crosstalk) etkisini azaltmaya yardım eder. Sunum eski **Cat 3** ve **Cat 5** UTP örneklerini, daha sık burulma görselleriyle karşılaştırır; kategori performansı yalnızca burulma sayısıyla tanımlanmaz.

### Slayt 26 — Bükümlü çift — Özellikler

Analog veya sayısal sinyaller taşınabilir. Performans kablonun kategorisine, kalitesine, uzunluğa, girişime ve kullanılan fiziksel katman standardına bağlıdır. Sunumdaki “kilometrelerce” anlatımı tarihsel telefon hatları için anlamlıdır; **standart bakır Ethernet kanalını kilometrelerce uzatabileceğin anlamına gelmez**. Güncel yapılandırılmış kablolamada bakır Ethernet kanal uzunluğu tipik olarak 100 m ile sınırlıdır (standarda göre değişebilir).

### Slayt 27 — Koaksiyel kablo

Koaksiyel kablo (Coaxial Cable): merkezde bakır iletken, çevresinde yalıtkan dielektrik, örgülü/folyo dış iletken ve koruyucu kılıf bulunur. Dış iletken ekranlama sağlayarak girişime karşı koruma sunar. Kullanım: kablo TV, geniş bant erişim, RF sistemleri. Bant genişliği ve zayıflama kalite, uzunluk, frekans ve ekranlama gibi etkenlere bağlıdır.

### Slayt 28 — Fiber optik — Işığın iletilmesi

Optik iletim sisteminin üç bileşeni **ışık kaynağı, fiber ortam ve algılayıcıdır**. Işık, çekirdek (core) ve kaplama (cladding) arasındaki kırılma indisi farkı sayesinde dalga kılavuzunda tutulur. Sunum bunu toplam iç yansıma (total internal reflection) şemasıyla anlatır. Gerçekte özellikle tek modlu fiberde dalga optiği modeli daha doğru açıklama sunar.

### Slayt 29 — Fiber optik — Tek mod ve çok mod

**Çok modlu fiber (Multimode)** çok sayıda yayılım moduna izin verir; modsal yayılma nedeniyle uzak mesafede bant genişliği/mesafe sınırlaması olabilir. **Tek modlu fiber (Singlemode)** bir modun iletimini destekleyerek uzun mesafe ve yüksek hızda avantaj sağlar. Sunumdaki “50 Gbps / 100 km” rakamı dönemine özgü bir örnektir; bugünün evrensel üst sınırı değildir.

> [!question] Bölüm içi kontrol
> 1. Singlemode ile multimode farkını açıkla.
> 2. Toplam iç yansıma kavramının rolü nedir?

### Slayt 30 — Fiberde ışık zayıflaması — 1

Zayıflama (Attenuation), optik gücün mesafe boyunca düşmesidir. Pozitif kayıp değeri için:

$$L_{dB}=10\log_{10}(P_{giriş}/P_{çıkış}).$$

Örnek: Güç 10 mW’den 1 mW’ye düşerse `L=10log10(10)=10 dB` kayıp. Uzun hatlarda **dB/km** birimi kullanılır. dB güç oranıdır; dBm ise 1 mW referanslı mutlak güç düzeyidir.

> [!example] Fiber güç kaybı
> `P_giriş=20 mW`, `P_çıkış=5 mW` ise `10log10(20/5)=6,02 dB` kayıp.

### Slayt 31 — Fiberde ışık zayıflaması — 2

Grafik, zayıflamanın dalga boyuna bağlı olduğunu gösterir. Üç tarihsel çalışma bölgesi yaklaşık **850 nm, 1310 nm ve 1550 nm** çevresindedir. 1310 nm ve 1550 nm bölgeleri düşük zayıflama açısından önemlidir. Eğrideki su emiliminden kaynaklanan tepe ve diğer özellikler fiber türüne ve üretim teknolojisine göre değişir.

### Slayt 32 — Fiberde ışık zayıflaması — 3

Slayt 0,85 μm, 1,30 μm ve 1,55 μm çevresindeki optik iletişim bantlarını vurgular. `1 μm=1000 nm`, bu nedenle `1,55 μm=1550 nm`. 1310 ve 1550 nm’de km başına kayıp genellikle daha düşüktür. Sunumdaki “25.000–30.000 GHz” ifadesi bant genişliği mertebesine ilişkin tarihsel bağlamdır; her cihazın bu kapasiteyi kullandığını göstermez.

### Slayt 33 — Fiber kablolar — Katmanları

Fiberin **çekirdeği (core)** ışığı taşır; **kaplama (cladding)** ışığın yönlendirilmesine yardım eder; **koruyucu kılıf (jacket)** mekanik koruma sağlar. Sunum tek modlu çekirdeği yaklaşık 8–10 μm, çok modlu örneği yaklaşık 50 μm olarak gösterir. Fiber çekirdeği, klasik bakır koaksiyel kablonun iç iletkeniyle karıştırılmamalıdır.

### Slayt 34 — Fiber kablolar — Birleştirme yöntemleri

Üç yöntem: **konnektör** (tak–çıkar, kolay bakım, ilave kayıp), **mekanik ek** (uçların mekanik hizalanması), **füzyon ek** (uçların eritilerek birleştirilmesi; iyi yapıldığında düşük ek kaybı). Sunumdaki yüzde kayıp değerleri eski/örnek verilerdir; modern konnektör/füzyon uygulamalarının sabit performans değerleri olarak ezberlenmemelidir. Optik güç bütçesi hesaplanırken her konnektör/ek için kayıp payı ayrılır.

### Slayt 35 — Fiber kablolar — LED, lazer ve fotodiyot

Göndericide LED veya yarı iletken lazer; alıcıda fotodiyot kullanılır. Genel karşılaştırma: LED daha geniş spektrumlu/düşük maliyetli, lazer daha dar spektrumlu/yüksek hız ve uzun mesafeye uygun olabilir. Hız/mesafe kaynağın yanında fiber, optik alıcı ve sistem tasarımına da bağlıdır. Sunumdaki 1 ns fotodiyot → 1 Gbps ifadesi eski cihaz örneğidir, güncel evrensel sınır değildir.

### Slayt 36 — Fiber optik ağlar — Aktif tekrarlayıcı halka

Şekilde fiber halka ve **aktif tekrarlayıcılar (active repeaters)** bulunur. Tekrarlayıcı optik sinyali algılar, gerekirse elektriksel olarak yeniden şekillendirir ve yeniden optik sinyale dönüştürür. Bu, uzun yol boyunca sinyal kalitesini korumaya yardım eder; ancak aktif cihazlar enerji ve bakım gerektirir.

### Slayt 37 — Fiber optik ağlar — Pasif yıldız

Pasif yıldız (Passive Star) bağlantısında optik ışık, merkezi pasif birleştirici (coupler) aracılığıyla birden fazla çıkışa dağıtılabilir. Pasif eleman elektrik beslemesi gerektirmez; ancak gelen optik güç çıkışlara paylaşıldığı için bölüşme kaybı ortaya çıkar. Bu şema, **aktif paket anahtarlayan Ethernet switch ile aynı şey değildir**.

> [!question] Bölüm içi kontrol
> 1. Fiberin çekirdek, kaplama ve jacket katmanlarını karşılaştır.
> 2. Pasif yıldız ile aktif tekrarlayıcı arasındaki fark nedir?


---

## Bölüm III — Kablosuz İletim ve Spektrum (38–52)

### Slayt 38 — Kablosuz iletim — Giriş

Mobil kullanıcıların sürekli erişim ihtiyacı kablosuz iletimi öne çıkarır. Slaytta “gelecek fiber ve kablosuzdur” şeklinde tarihsel öngörü bulunur. Bugün birçok omurga ve sabit erişimde fiber, son kullanıcı ve hareketli uçlarda radyo yaygındır; yine de bakır ve diğer ortamlar tamamen ortadan kalkmış değildir.

### Slayt 39 — Kablosuz iletim — Dalga, frekans, dalga boyu

Hızlanan/ivmelenen elektrik yükleri elektromanyetik dalga oluşturabilir. **Frekans (f)** bir saniyedeki salınım sayısıdır (Hz); **dalga boyu (λ)** ardışık tepe noktaları arasındaki mesafedir (m). Boşlukta ilişki:

$$c=f\lambda,\quad c\approx3\times10^8\,m/s.$$

Örnek: 2,4 GHz için `λ≈0,125 m = 12,5 cm`. Ortamın kırılma indisine göre dalga yayılma hızı değişebilir.

> [!example] Wi-Fi dalga boyu
> `f=5 GHz` için `λ=3×10⁸/(5×10⁹)=0,06 m=6 cm`.

### Slayt 40 — Kablosuz iletim — Anten

Uygun antenler elektromanyetik sinyallerin verimli yayılmasını ve algılanmasını sağlar. Antenin boyutu, tasarımı, frekans aralığı, kazancı, polarizasyonu ve yönlülüğü önemlidir. Yüksek frekans = daha kısa dalga boyu olduğundan anten boyutları da genellikle küçülebilir; anten boyutu her durumda tam bir dalga boyuna eşit değildir.

### Slayt 41 — Kablosuz iletim — Teknoloji başlıkları

Sunum sırasıyla elektromanyetik spektrum, radyo, mikrodalga, kızılötesi ve milimetre dalgaları, serbest uzay optik iletişimi başlıklarını sunar. Her birinin menzil, doğrultu, engel penetrasyonu, girişim ve atmosferik koşullara duyarlılık özellikleri farklıdır.

### Slayt 42 — Elektromanyetik spektrum — Genel şekil

Elektromanyetik spektrumda radyo, mikrodalga, kızılötesi, görünür ışık, ultraviyole, X ışını ve gama ışınları bulunur. Frekans yükseldikçe boşlukta dalga boyu kısalır. Şekil radyo/TV, uydu, karasal mikrodalga ve fiber optiğin farklı frekans bölgelerini gösterir. **Tek başına taşıyıcı frekans yüksek olması veri hızını garanti etmez**; kullanılabilen bant genişliği ve SNR önemlidir.

### Slayt 43 — Elektromanyetik spektrum — Modülasyon

Bilgi, taşıyıcı dalganın **genliğini (AM)**, **frekansını (FM)** veya **fazını (PM)** değiştirmekle kodlanabilir. Gerçek dijital iletişimde ASK/FSK/PSK/QAM gibi yöntemler kullanılır. Slayt yüksek enerjili UV/X/gama bölgelerinin güvenlik ve yayılım açısından iletişim için pratik olmadığını belirtir; bu, güvenli kullanımın standart ve güç limitlerine bağlı olduğu gerçeğini değiştirmez.

### Slayt 44 — Elektromanyetik spektrum — Spektral verimlilik

Kanalın taşıyabileceği bit hızı kullanılabilir bant genişliği ve **spektral verimlilik (bit/s/Hz)** ile ilişkilidir: yaklaşık `R≈ηB` (η spektral verimlilik). Örnek `B=20 MHz`, `η=4 bit/s/Hz` ise idealize edilmiş `R≈80 Mbps`. Bu, protokol ek yüklerini ve gerçek kanal koşullarını içermez.

### Slayt 45 — Elektromanyetik spektrum — Frekans/dalga boyu bandı

`f=c/λ` bağıntısında küçük bir dalga boyu aralığı `Δλ` için frekans aralığı yaklaşık:

$$|\Delta f|\approx\frac{c|\Delta\lambda|}{\lambda^2}.$$

İşaret türevde eksidir (`df/dλ=-c/λ²`), fakat **bant genişliği pozitif büyüklük** olduğu için mutlak değer alınır. Merkez dalga boyu küçüldükçe aynı Δλ daha büyük frekans aralığına karşılık gelir.

### Slayt 46 — Elektromanyetik spektrum — 1,30 μm örneği

Sunumdaki örnek: `λ=1,30×10^-6 m`, `Δλ=0,17×10^-6 m`, `c≈3×10^8 m/s`.

$$\Delta f\approx\frac{3\times10^8\times0.17\times10^{-6}}{(1.30\times10^{-6})^2}\approx3.02\times10^{13}\,Hz=30.2\,THz.$$

`8 bit/s/Hz` varsayılırsa `R≈30 THz×8≈240 Tbit/s`. **Bu potansiyel bant genişliği hesabıdır; tek bir gerçek optik bağlantının garantili veri hızı değildir.**

> [!tip] Birim kontrolü
> `c [m/s] × Δλ [m] / λ² [m²] = 1/s = Hz`. Sonuç `10¹² Hz` mertebesindeyse THz’ye çevir.

### Slayt 47 — Elektromanyetik spektrum — Yayılı spektrum

**Frekans atlamalı yayılı spektrum (FHSS):** Taşıyıcı frekans önceden belirlenmiş dizilimle değiştirilir. **Doğrudan dizilimli yayılı spektrum (DSSS):** Veri daha hızlı bir çip dizisiyle yayılarak geniş spektrumda taşınır. Amaçlar arasında girişime dayanıklılık ve spektrum paylaşımı vardır. “Dar bantta yüksek güç/Hz” ile “geniş banda yayılan sinyal” farklı tasarım yaklaşımlarıdır.

### Slayt 48 — Radyo iletimi

Radyo dalgaları uygulama ve frekansına göre geniş alanlara yayılabilir; kimi bantlarda engelleri aşabilir. Şekilde iki tarihsel yayılım biçimi vardır: **yer dalgası (ground wave)** VLF/LF/MF frekanslarında yeryüzünü takip edebilir; **gök dalgası (sky wave)** HF bandında iyonosfer tarafından kırılarak/yansıtılarak uzak mesafelere ulaşabilir. Her radyo dalgası aynı şekilde yayılmaz.

### Slayt 49 — Mikrodalga iletimi

Mikrodalgalar çoğunlukla **görüş hattı (line of sight)** gerektiren yönlü antenlerle kullanılır. Yer eğriliği, binalar, engeller, yağış ve atmosferik koşullar bağlantıyı etkileyebilir. Uzun mesafede ara istasyonlar/tekrarlayıcılar gerekebilir. Kullanım alanları karasal omurga, mobil ağlar ve bazı yayın/satellit bağlantılarıdır.

### Slayt 50 — Elektromanyetik spektrumun düzenlenmesi

Radyo spektrumu sınırlı ortak kaynaktır; lisanslama, tahsis ve güç sınırlamaları düzenleyici kurumlarca yürütülür. Sunum ABD’de tarihsel **ISM bantlarını** örnekler: 902–928 MHz, 2,400–2,4835 GHz ve 5,735–5,860 GHz. **Bantların lisans koşulları ülkeden ülkeye ve zamanla değişebilir**; bu rakamlar Türkiye için güncel düzenleyici uygunluk garantisi değildir.

### Slayt 51 — Kızılötesi ve milimetre dalgaları

Kızılötesi (Infrared) kısa mesafe, genellikle doğrultu/ortam koşullarına duyarlı haberleşmede kullanılabilir; klasik TV uzaktan kumandaları buna örnektir. Milimetre dalgaları (mmWave) ise genel olarak 30–300 GHz mertebesindedir ve modern kablosuz haberleşmede de kullanılır; **IR ile mmWave aynı fiziksel frekans bandı değildir**. Duvar penetrasyonu ve engel hassasiyeti dalga boyuna bağlıdır.

### Slayt 52 — Serbest uzay optik iletişimi

Binalar arasında lazer kullanılarak serbest uzay optik bağlantısı (Free-Space Optics) oluşturulabilir. Görüş hattı, ışın hizalaması, atmosferik türbülans, sis ve hava olayları önemlidir. Şekil, yükselen sıcak hava nedeniyle ışının saparak fotodedektörü kaçırabileceğini gösterir. FSO, cam fiber değildir; ışık açık havada ilerler.

> [!question] Bölüm içi kontrol
> 1. 2,4 GHz dalga boyunu hesapla.
> 2. Gizli düğüm sorunu fiziksel katmandan ziyade hangi ağ alanıyla ilişkilidir?
> 3. FSO hangi faktörlerden etkilenir?


---

## Bölüm IV — Uydular (53–63)

### Slayt 53 — Haberleşme uyduları — Transponder

Bir haberleşme uydusu, kabaca gökyüzünde çalışan mikrodalga aktarıcı gibi düşünülebilir. **Transponder**, bir frekans bandındaki uplink sinyalini alır, uygun işlem/yükseltme yapar ve genellikle farklı downlink frekansında yeniden yayar. Uplink Dünya→uydu, downlink uydu→Dünya yönüdür.

### Slayt 54 — Haberleşme uyduları — Bent-pipe

Uydu sinyalini kendisi IP paketi seviyesinde yönlendirmeden alıp başka frekansla yeniden ileten saydam aktarıcı gibi çalışabilir; bu **bent-pipe** yaklaşımıdır. Uydu ışınları (beams) geniş coğrafyayı veya daha dar spot bölgeleri kapsayabilir. Modern işleyici uydular daha gelişmiş dijital yeniden üretim/yönlendirme de yapabilir.

### Slayt 55 — Haberleşme uyduları — Yörünge yüksekliği

Kepler’in üçüncü yasasına göre aynı merkez gövde etrafındaki dairesel yörüngeler için `T²∝r³`, dolayısıyla `T∝r^(3/2)`; `r` Dünya merkezinden uzaklıktır. Yükseklik arttıkça yörünge periyodu uzar. Yaklaşık GEO irtifası **35.786 km**’dir. Jeostasyoner yörüngenin periyodu yıldız günüdür (~23 saat 56 dakika), gündelik anlatımda 24 saat denir.

> [!warning] Jeostasyoner ≠ her jeosenkron
> Jeostasyoner yörüngenin ayrıca ekvator düzleminde, dairesel ve Dünya’nın dönüş yönüyle uyumlu olması gerekir.

### Slayt 56 — Haberleşme uyduları — Türler

Üç sınıf: **GEO (geostationary/geosynchronous)** çok yüksek irtifa, geniş kapsama ve yüksek yayılma gecikmesi; **MEO** orta irtifa, GNSS gibi sistemlerde kullanılır; **LEO** alçak irtifa, düşük yayılma gecikmesi fakat hareketli uydu/çoklu uydu ihtiyacı. Ayrıca uyduların fiberle karşılaştırılmasında kapsama, kapasite, gecikme ve maliyet ele alınır.

### Slayt 57 — Haberleşme uyduları — Yörünge grafiği

Slayt tarihsel sayılar verir: GEO ~35.000 km ve ~270 ms gidiş-dönüş; MEO ~10.000 km için ~35–85 ms; LEO için ~1–7 ms gibi *örnek uydu bağlantısı ölçüleri*. **Bunlar tüm sistemlerin uçtan uca RTT değerleri değildir**; yer istasyonu, yönlendirme, çoklu sıçrama ve işleme gecikmeleri eklenir. GEO bir yer→uydu→yer geçişi için yalnız yayılma ~239 ms civarındadır; tam geri dönüş trafiği bunun iki katını aşabilir.

### Slayt 58 — Haberleşme uyduları — Frekans bantları

Sunumun tarihsel uplink/downlink örnekleri:

| Bant | Downlink | Uplink | Öne çıkan kısıt |
|---|---:|---:|---|
| L | ~1,5 GHz | ~1,6 GHz | Sınırlı spektrum |
| S | ~1,9 GHz | ~2,2 GHz | Sınırlı spektrum |
| C | ~4 GHz | ~6 GHz | Karasal girişim |
| Ku | ~11 GHz | ~14 GHz | Yağmur zayıflaması |
| Ka | ~20 GHz | ~30 GHz | Yağmur ve ekipman gereksinimleri |

**Bu değerler temsilî tarihsel örneklerdir**, her uydu ağına uygulanacak kesin frekans tahsisi değildir.

### Slayt 59 — Haberleşme uyduları — VSAT hub

**VSAT (Very Small Aperture Terminal)** küçük çanaklı uydu yer terminalidir. Slayt, merkezdeki **hub** istasyonu ile bağlı uç terminallerin uydudan haberleştiği yıldız benzeri bir mimariyi gösterir. Uç→uydu→hub→uydu→diğer uç gibi iki uydu geçişi gerekebilir; bu durum gecikmeyi artırabilir.

### Slayt 60 — MEO uyduları

MEO, LEO ile GEO arasında kalan orta Dünya yörüngeleridir; GNSS sistemleri (GPS, Galileo vb.) bunun yaygın örneklerindendir. Slayttaki “MEO telekomünikasyonda kullanılmıyor” bilgisi ve belirli GPS uydu sayısı eski tarihli bilgi olarak değerlendirilmelidir; **günümüzde MEO haberleşme takımyıldızları da vardır**. MEO hem kapsama hem gecikme açısından ara bir çözüm sunabilir.

### Slayt 61 — LEO uyduları — Iridium

**Iridium**, LEO iletişim uydularını birden fazla yörünge düzlemine dağıtan tarihsel bir sistem örneğidir. Şekilde altı yörünge düzlemi ve Dünya üzerinde hareket eden hücre kapsaması gösterilir. LEO uyduları gökyüzünde sabit kalmadığından kullanıcı bağlantısı, takip, handover ve birden fazla uydunun koordinasyonu önemlidir. Slayttaki hücre sayısı mimari/tarihsel örnektir.

### Slayt 62 — Globalstar — Uzayda ve yerde aktarma

Şekil iki yaklaşımı karşılaştırır: **uzayda aktarma (inter-satellite relay)** ve **yerde aktarma (ground relay/bent-pipe)**. Uzay bağlantıları bazı yolları yer istasyonu bağımlılığından kurtarabilir; bent-pipe sistemlerde trafik çoğunlukla yer istasyonlarına indirilerek yönlendirilir. Bunun gecikme, kapsam ve altyapı gereksinimlerine etkisi olabilir.

### Slayt 63 — Teledesic — Tarihsel proje

Teledesic, geniş bant uydu interneti sağlamayı hedefleyen tarihsel bir projeydi. Slaytta hedeflenen uplink ~100 Mbps, downlink ~720 Mbps gibi **planlanan** değerler yer alır. **Bu değerler faal bir güncel Teledesic ağına ait ölçülmüş hızlar değildir**; proje ticari küresel ağ olarak hayata geçirilmedi. Sınavda bu örneği, 2000’ler başının geniş bant LEO tasarımları bağlamında yorumla.

> [!question] Bölüm içi kontrol
> 1. GEO–MEO–LEO farkları?
> 2. Uplink/downlink farkı?
> 3. Teledesic rakamları neden güncel kapasite değildir?


---

## Formül Föyü

| Konu | Formül | Birimler / dikkat |
|---|---|---|
| Bit süresi | $T_b=1/R_b$ | $R_b$: bit/s; $T_b$: saniye |
| Frekans–periyot | $f=1/T$ | $f$: Hz |
| 8-bit örüntü periyodu | $T=8/b$ | Tekrarlanan 8 bit örneğinde |
| Nyquist | $R_{max}=2H\log_2V$ | Gürültüsüz, $H$: Hz, $V$: düzey sayısı |
| SNR (dB) | $10\log_{10}(S/N)$ | Güç oranı; boyutsuz |
| dB → doğrusal SNR | $S/N=10^{\mathrm{SNR}_{dB}/10}$ | Shannon öncesinde çevir |
| Shannon | $C=H\log_2(1+S/N)$ | Gürültülü kanalın teorik kapasitesi |
| Optik güç kaybı | $L=10\log_{10}(P_{in}/P_{out})$ | Pozitif dB kayıp |
| Işık/dalga | $c=f\lambda$ | Vakumda $c\approx3\times10^8$ m/s |
| Dalga boyu bandı | $|\Delta f|\approx c|\Delta\lambda|/\lambda^2$ | Dar bant yaklaşımı |
| Spektral verim | $R\approx \eta B$ | $\eta$: bit/s/Hz; $B$: Hz |
| Yörünge periyodu | $T^2\propto r^3$ | $r$ Dünya merkezinden uzaklık |
| Yayılma gecikmesi | $t=d/v$ | $v$ ortamda yayılma hızı |

> [!warning] **Bant genişliği** iki ayrı anlamda karşına çıkabilir: spektral genişlik **Hz**, veri aktarım hızı **bit/s**. Formülde ne sorulduğunu ve birimi kontrol et.

## Çözümlü Sorular

### Örnek 1 — Nyquist (4 seviyeli kanal)

**Soru:** Gürültüsüz kanalın bant genişliği 4 kHz, sinyal düzeyi 4. Azami veri hızı?

$$R_{max}=2\times4000\times\log_2 4=2\times4000\times2=\boxed{16000\ \text{bit/s}}.$$

**Yorum:** Aynı H için düzey sayısının 2’den 4’e yükselmesi sembol başına 1 yerine 2 bit sağlar. Gürültü varsa düzeyleri ayırt etmek zorlaşabilir.

### Örnek 2 — dB dönüşümü

**Soru:** 20 dB SNR doğrusal oran olarak kaçtır?

$$S/N=10^{20/10}=\boxed{100}.$$

### Örnek 3 — Shannon

**Soru:** $H=3$ kHz, SNR = 20 dB için teorik kapasite nedir?

Önce $S/N=100$ bulunur. Ardından:

$$C=3000\log_2(101)\approx\boxed{19.975\ \text{bit/s}}.$$

> [!tip] Hesap makinesinde yalnız `log` bulunuyorsa $\log_2x=\log_{10}x/\log_{10}2$.

### Örnek 4 — Nyquist ve Shannon birlikte

**Soru:** $H=4$ kHz, $V=8$, SNR = 30 dB. Hangi teorik sınır daha düşüktür?

- Nyquist: $2\cdot4000\cdot\log_2 8=24000$ bit/s.
- Shannon: $4000\log_2(1001)\approx39869$ bit/s.
- **Bu belirli 8-düzeyli düzen için** iki sınırdan küçüğü **24 kbit/s** olur. Shannon tüm olası ideal kodlama/modülasyon yaklaşımları için kanal üst sınırını verir; Nyquist hesaplaması seçilen düzey sayısıyla sınırlıdır.

### Örnek 5 — 8 bitlik desenin harmonikleri

**Soru:** 3 kHz kesimli tarihsel telefon kanalı örneğinde `b=4800 bit/s`. Kaç harmonik kalabilir?

$$T=8/4800=1{,}667\ \text{ms},\qquad f=1/T=600\ \text{Hz}.$$

$$N\approx3000/600=\boxed{5\ \text{harmonik}}.$$

### Örnek 6 — Optik güç kaybı

**Soru:** Fiberin giriş gücü 8 mW, çıkış gücü 2 mW.

$$L=10\log_{10}(8/2)=10\log_{10}(4)\approx\boxed{6{,}02\ \text{dB}}.$$

### Örnek 7 — dB/km ile toplam kayıp

**Soru:** Fiber 20 km uzunluğunda, zayıflama katsayısı 0,25 dB/km. Konektör ve ekler hariç toplam kayıp?

$$L=20\times0{,}25=\boxed{5\ \text{dB}}.$$

### Örnek 8 — Dalga boyu

**Soru:** 2,4 GHz elektromanyetik dalganın boşluktaki dalga boyu?

$$\lambda=\frac{3\times10^8}{2{,}4\times10^9}=\boxed{0{,}125\ \text{m}=12{,}5\ \text{cm}}.$$

### Örnek 9 — Optik bant

**Soru:** $\lambda=1{,}30\ \mu m$, $\Delta\lambda=0{,}17\ \mu m$. Yaklaşık frekans genişliği?

$$\Delta f\approx\frac{(3\times10^8)(0{,}17\times10^{-6})}{(1{,}30\times10^{-6})^2}\approx\boxed{30{,}2\ \text{THz}}.$$

### Örnek 10 — Uydu yayılma gecikmesi

**Soru:** Yer istasyonu–GEO uydu–ikinci yer istasyonu yolu, her dikey bacak yaklaşık 35.786 km olsun. İşleme gecikmelerini ihmal et.

$$d=2\times35786\ \text{km}=71572\ \text{km}.$$

$$t\approx\frac{71.572.000}{3\times10^8}=\boxed{0{,}239\ \text{s}=239\ \text{ms}}.$$

**Dikkat:** Bu, **tek yönlü iki bacaklı bir uzay aktarımının** idealize yayılma süresidir; tam gidiş–dönüş (RTT) için aynı tür yolun dönüşü de gerekir. Ayrıca yer geometrisi eğik olduğunda mesafe uzayabilir.

---

## Kavram Karşılaştırmaları ve Sınavda Sık Yapılan Hatalar

| Karışan kavramlar | Doğru ayrım |
|---|---|
| Fiziksel katman / veri bağlantı | Bitler ve sinyaller / çerçeveler ve MAC |
| Bandwidth (Hz) / bit rate (bit/s) | Frekans genişliği / saniyedeki bit sayısı |
| Nyquist / Shannon | İdeal gürültüsüz ve düzey sayısına bağlı sınır / gürültülü kanal bilgi kapasitesi |
| SNR (dB) / S/N (doğrusal) | Logaritmik / güç oranı; Shannon doğrusal değer kullanır |
| Frekans / dalga boyu | Sabit yayılma hızında ters orantılı |
| Fiber / koaksiyel | Işık kılavuzu / elektriksel iletken yapı |
| Tek mod / çok mod | Tek baskın mod / çoklu yayılım modları |
| TC / PMD | Fiziksel aktarım uyarlaması / fiziksel sinyal özellikleri — ATM (önceki hafta) |
| GEO / MEO / LEO | Yüksek / orta / alçak uydu yörünge sınıfları |
| Uplink / downlink | Yer→uydu / uydu→yer |
| Transponder / IP router | Genellikle RF sinyalini tekrarlar / IP paketini yönlendirir |
| Bantların tarihi / güncel teknoloji | Eski slayttaki hızlar veya tahsisler bugünkü evrensel değer değildir |

> [!danger] En yaygın 7 hata
> 1. `30 dB` değerini Shannon'da `S/N=30` olarak kullanmak.
> 2. `log₂(1+S/N)` ifadesinde `+1` kısmını unutmak.
> 3. Hz ile bit/s birimlerini eşitlemek.
> 4. Optik zayıflamada çıkış/giriş oranını ters çevirip işaret yorumunu açıklamamak.
> 5. Fiberin mutlaka elektromanyetik girişime tabi olduğunu sanmak (bakır kadar değil; fiber optik sinyal taşır).
> 6. Uydu gecikmesini yalnız irtifa / ışık hızı olarak alıp ikinci bacağı unutmak.
> 7. Sunumun 2003 dönemi telekomünikasyon örneklerini güncel teknoloji limitleri olarak kabul etmek.

## Deneme Soruları ve Cevap Anahtarı

### Sorular (Önce çöz)

1. (5 puan) Fiziksel katmanın altı ayrı tasarım işlevini örnekleyiniz.
2. (5 puan) Fourier serisi sinyal iletişiminde neden önemlidir? Harmonik sayısı azalırsa sinyal nasıl değişir?
3. (10 puan) `H=5 kHz`, `V=16` için Nyquist sınırını bulunuz.
4. (10 puan) `H=4 kHz`, `SNR=20 dB` için Shannon kapasitesini hesaplayınız.
5. (5 puan) `10 dB`, `20 dB`, `40 dB` değerlerini doğrusal SNR’ye dönüştürünüz.
6. (10 puan) `b=9600 bit/s` ile tekrarlanan 8 bitlik desenin periyodunu, temel frekansını ve 3 kHz kesimde geçen yaklaşık harmonik sayısını bulunuz.
7. (10 puan) Bükümlü çift, koaksiyel ve fiber optiği iletim yapısı, girişim ve kullanım açısından karşılaştırınız.
8. (10 puan) 1310/1550 nm bölgelerinin neden önemli olduğunu ve dB/km kavramını açıklayınız.
9. (10 puan) `P_in=20 mW`, `P_out=1 mW` için optik kaybı hesaplayınız.
10. (10 puan) `f=6 GHz` için dalga boyunu hesaplayınız ve mikrodalganın görüş hattı ihtiyacını açıklayınız.
11. (10 puan) GEO, MEO ve LEO'yu kapsama ve gecikme açısından karşılaştırınız.
12. (5 puan) Uplink, downlink, transponder, bent-pipe, VSAT kavramlarını tanımlayınız.

### Cevap anahtarı

1. 0/1 sinyal seviyeleri, bit süresi, iki yönlü eşzamanlılık, bağlantı kurulumu, sonlandırma, konnektör/pin ve zamanlama özellikleri.
2. Sinyali temel ve yüksek harmoniklere ayırır; sınırlı harmonikle keskin darbeler yuvarlaklaşır ve bozulum artar.
3. $R=2\times5000\times\log_2 16=\boxed{40000\ \mathrm{bit/s}}$.
4. $S/N=100$; $C=4000\log_2(101)\approx\boxed{26633\ \mathrm{bit/s}}$.
5. Sırasıyla $10,100,10000$.
6. $T=8/9600=0{,}8333$ ms; $f=1200$ Hz; $N\approx3000/1200=2{,}5$; **tam harmonik sayılırsa yalnız 1. ve 2. harmonik** geçer.
7. Bükümlü çift dengeli bakır çift; koaksiyel dış ekranlı bakır iletken; fiber cam optik iletim. Fiber elektromanyetik girişime çok daha dayanıklı, genellikle uzun mesafe ve yüksek kapasiteye uygun; sistem seçimi koşullara bağlı.
8. Bu dalga boylarında fiber zayıflaması düşüktür. dB/km birim uzunluk başına logaritmik güç kaybını anlatır.
9. $10\log_{10}20\approx\boxed{13{,}01\ \mathrm{dB}}$.
10. $\lambda=3\times10^8/(6\times10^9)=\boxed{0{,}05\ \mathrm{m}=5\ \mathrm{cm}}$. Görüş hattını binalar/yer eğriliği engelleyebilir.
11. GEO geniş kapsama/yüksek gecikme; LEO daha dar uydu başına kapsama/daha düşük yayılma gecikmesi ve sık handover; MEO ara özellikler.
12. Uplink yer→uydu, downlink uydu→yer, transponder RF frekans çevirerek yeniden ileten bileşen, bent-pipe saydam aktarma, VSAT küçük antenli yer terminalidir.

---

## Hızlı Son Tekrar — 20 Kart

- **Fiziksel katman?** Ham bit akışı/sinyalleme.
- **Bit süresi?** $1/R_b$.
- **Fourier?** Periyodik sinyalin sinüs/kosinüs bileşenlerine ayrılması.
- **Harmonik?** Temel frekansın tam sayı katı.
- **Bant genişliği (fiziksel)?** Hz.
- **Nyquist?** $2H\log_2 V$.
- **Shannon?** $H\log_2(1+S/N)$.
- **SNR dB?** $10\log_{10}(S/N)$.
- **30 dB doğrusal SNR?** 1000.
- **Kılavuzlu ortam?** Bakır/fiber vb.
- **Kılavuzsuz ortam?** Hava/boşlukta elektromanyetik iletim.
- **Fiber çekirdek?** Işığın yol aldığı yapı.
- **Fiber kaplama?** Kırılma indisiyle ışığı yönlendirmeye yardım eder.
- **1310 ve 1550 nm?** Düşük zayıflama bölgeleri.
- **Işık dalga bağıntısı?** $c=f\lambda$.
- **2,4 GHz için $\lambda$?** ~12,5 cm.
- **FHSS?** Frekans atlama.
- **DSSS?** Doğrudan dizilimle yayma.
- **GEO?** ~35.786 km, uzun gecikme.
- **VSAT?** Küçük çanaklı uydu yer terminali.

## Kaynaklar

- [Orijinal Week 3 ders PDF'i](https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_3.pdf)
- [Ankara Üniversitesi ders materyalleri](https://android.eng.ankara.edu.tr/lisans-dersleri/blm332-veri-iletisimi-ve-aglar-blm3032-bilgisayar-aglari/)
- Andrew S. Tanenbaum, *Computer Networks*, 4th ed., Chapter 2, s. 85–118.

> [!success] Tamamlanma kontrol listesi
> - [ ] Slayt 1–21: Fourier, Nyquist ve Shannon
> - [ ] Slayt 22–37: Kılavuzlu iletim ortamları
> - [ ] Slayt 38–52: Kablosuz iletişim
> - [ ] Slayt 53–63: Haberleşme uyduları
> - [ ] 10 çözümlü örneği cevaba bakmadan çözdüm
> - [ ] 100 puanlık deneme sorularını tamamladım
