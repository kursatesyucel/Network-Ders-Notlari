---
title: "Bilgisayar Ağları ve Veri İletişimi — Week 4 — Tüm Ders Notları"
aliases:
  - "Week 4 Fiziksel Katman"
tags:
  - bilgisayar-aglari
  - yuksek-lisans
  - week-4
  - fiziksel-katman
  - modem
  - multiplexing
  - vize
  - obsidian
course: "Bilgisayar Ağları ve Veri İletişimi"
week: 4
slide_count: 68
language: tr
source: "https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_4.pdf"
---

# Week 4 — Fiziksel Katman: Telefon Ağı, Çoklama, Mobil ve Kablo İnternet

> [!info] Kaynak ve çalışma sınırı
> Ankara Üniversitesi, Dr. Refik Samet, **Week_4.pdf**, Tanenbaum *Computer Networks* 4. baskı, Bölüm 2 (s. 118–177). **Orijinal 68 PDF sayfası ve slayt numarası birebir esas alınmıştır.** Dersin modem, GSM, AMPS ve DSL rakamları tarihsel 4. baskı bağlamında yorumlanmalıdır. Teknik terimler İngilizce karşılıklarıyla verilmiştir. Açıklama, özgün slayt metninin aynen kopyası değil, Türkçe öğretim amaçlı yeniden yazımıdır.

> [!tip] Önerilen çalışma sırası
> 1–14: PSTN ve telefon sistemi; 15–29: modem, baud, QAM; 30–41: ADSL/WLL; 42–54: FDM, WDM, TDM ve anahtarlama; 55–62: hücresel ağlar; 63–68: kablo internet. Özellikle **baud–bps, DMT, T1, SONET, FDM/TDM/WDM, CDMA** hesaplamalarını tekrar et.

## İçindekiler
- [[#Bölüm I — PSTN ve Telefon Sistemi (1–14)|PSTN ve telefon sistemi (1–14)]]
- [[#Bölüm II — Modemler ve Sayısal Modülasyon (15–29)|Modemler ve sayısal modülasyon (15–29)]]
- [[#Bölüm III — DSL ve Kablosuz Yerel Döngü (30–41)|DSL ve kablosuz yerel döngü (30–41)]]
- [[#Bölüm IV — Çoklama ve Anahtarlama (42–54)|Çoklama ve anahtarlama (42–54)]]
- [[#Bölüm V — Mobil Telefon Sistemleri (55–62)|Mobil telefon sistemleri (55–62)]]
- [[#Bölüm VI — Kablolu Televizyon ve Kablo İnternet (63–68)|Kablo internet (63–68)]]
- [[#Formül Föyü ve Çözümlü Hesaplamalar|Formül föyü ve hesaplamalar]]
- [[#Kavram Karşılaştırmaları|Karşılaştırma tabloları]]
- [[#Yüksek Lisans Vize Denemesi — Week 4|Deneme sınavı ve cevap anahtarı]]


---

## Bölüm I — PSTN ve Telefon Sistemi (1–14)

### Slayt 1 — Fiziksel Katman — Devam

Bu hafta fiziksel katmanın telefon ve geniş bant erişim ağlarına uygulanışını inceleriz. Temel zincir: **abone hattı → modem/DSL → santral → omurga → anahtarlama**, ardından mobil telefon ve kablolu televizyon üzerinden internet. Sunum Tanenbaum 4. baskı Bölüm 2, s.118–177 temellidir. 68 slayt bulunur.

### Slayt 2 — PSTN — Genel bakış 1/3

**PSTN (Public Switched Telephone Network)**, geleneksel anahtarlamalı telefon şebekesidir. Tarihsel olarak anlaşılır insan sesini iletmek üzere tasarlanmıştır; dolayısıyla sayısal bilgisayar verisinin iletimi için başlangıçta ideal değildi. Ses için dar frekans bandı, analog yerel döngü ve devre anahtarlama gibi kabuller bilgisayar iletişimini sınırlar. Fiber ve dijital santrallerle şebeke gelişmiştir. **PSTN, internet veya Wi-Fi değildir.**

### Slayt 3 — PSTN — Yerel kablo ile çevirmeli hat karşılaştırması 2/3

Slayt tarihsel yerel bilgisayar kablosunu `10^8–10^9 bit/s`, çevirmeli analog hattı yaklaşık `10^4 bit/s` mertebesinde karşılaştırır; hata olasılığı da belirgin farklıdır. **Bunlar güncel erişim hızları değil dönem karşılaştırmasıdır.** Temel çıkarım: telefon ağı öncelikle ses için tasarlandığından bilgisayar verisi taşımak için kodlama/modülasyon gerekir; geniş bant teknolojileri bu sınırları gevşetmiştir.

### Slayt 4 — PSTN'nin alt konuları 3/3

Beş başlık: **Telefon sisteminin yapısı; telekom düzenlemesi; yerel döngü, modem, ADSL ve kablosuz erişim; ana hatlar ve çoklama; anahtarlama**. Telefon ağı üzerinden bir görüşme kurulurken abone hattı, santraller ve yüksek kapasiteli bağlantılar birlikte çalışır. Aynı fiziksel altyapının farklı hizmetleri taşıması için çoklama kritik rol oynar.

### Slayt 5 — Telefon sisteminin yapısı 1/5 — Topoloji

Şekil üç tarihsel mimariyi karşılaştırır: (a) tam bağlı ağda her abone her diğerine doğrudan bağlanır; (b) merkezî anahtar hat sayısını azaltır; (c) iki seviyeli hiyerarşi bölgesel ölçeklenmeyi kolaylaştırır. Tam bağlı `n` uç için bağlantı sayısı `n(n−1)/2` olur. Örneğin 100 abone arasında **4.950 bağlantı** gerekir. Merkezî/hiyerarşik topolojiler kablolama maliyetini azaltır, ancak kritik düğümlerde yedeklilik önemlidir.

### Slayt 6 — Telefon sisteminin yapısı 2/5 — Çağrı yolu

Şekilde orta mesafeli bir telefon görüşmesinin geçtiği anahtarlar gösterilir. Kavramsal yol: **telefon → yerel döngü → uç santral (end office) → transit/toll ofisleri → hedef uç santral → hedef yerel döngü → hedef telefon**. Her çağrı bütün üst kademe santrallerden geçmek zorunda değildir; ortak santral bulunduğunda daha kısa yol kullanılabilir.

### Slayt 7 — Telefon sisteminin yapısı 3/5 — Local loop ve trunk

**Yerel döngü (local loop)**, abonenin telefonundan en yakın uç santrale giden geleneksel iki bakır iletkenli bağlantıdır. **Trunk**, santraller arasındaki çok sayıda çağrıyı taşıyabilen bağlantıdır. Yerel döngü ile santraller arası trunk aynı şey değildir. Toll office (uzun mesafe/transit santrali) farklı uç santralleri bağlar.

### Slayt 8 — Telefon sisteminin yapısı 4/5 — Hiyerarşi

Uç santraller doğrudan birbirine bağlı değilse çağrı daha yüksek seviyeli anahtarlama merkezlerinden geçirilir. Tarihsel kademeler **toll → primary → sectional → regional** olarak adlandırılabilir. Santraller arasında **interoffice trunks** yüksek kapasiteli bağlantılardır. Modern telefon ağları daha düz topolojiler ve IP tabanlı taşıma kullanabilir: dersin modeli tarihsel PSTN yapısını anlatır.

### Slayt 9 — Telefon sisteminin üç ana bileşeni 5/5

Slayt üç ana bileşeni sınıflandırır: **local loop** (eve/işyerine gelen abone hattı; eski modelde analog bakır çift), **trunk** (santraller arası dijital/fiber bağlantı), **switching office** (çağrının uygun hatta aktarılması). Bu üçlüyü sınavda açıklayabilmelisin. Paket anahtarlamadaki router işleviyle telefon devre anahtarı tam olarak aynı değildir.

### Slayt 10 — Telefon düzenleme yapısı — LATA, LEC, IXC, POP

ABD telekom düzenine ilişkin tarihsel şekil: **LATA** yerel erişim ve taşıma bölgesi; **LEC** yerel telefon operatörü; **IXC** bölgeler arası taşıyıcı; **POP** operatörün bağlantı/erişim noktası. Bu terimler Türk telefon şebekesinin güncel idari haritası değildir. Sınavda: yerel işletmeci, uzun mesafe işletmecisi ve ağların birbirine bağlandığı noktaları ayır.

### Slayt 11 — Yerel döngü 1/4 — Analog modem gereksinimi

Bilgisayar sayısal bit üretir, eski çevirmeli telefonun yerel döngüsü analog ses bandında çalışır. **Modem**, sayısal veriyi analog taşıyıcı üzerindeki sembollere dönüştürür (**modülasyon**); alıcıda tekrar sayısal veriyi elde eder (**demodülasyon**). Santral/omurga bölümünde analog–dijital dönüşümler olabilir. Modem yalnızca sinyalin şeklini değiştirir; veriyi internet yönlendirmesi yaparak taşımak zorunda değildir.

### Slayt 12 — Yerel döngü 2/4 — Modem, codec ve dönüşüm şekli

Orijinal şekilde bilgisayar→modem→analog abone döngüsü→santral/codec→dijital trunk→hedef yönü zinciri bulunur. **Codec**, analog sesin sayısallaştırılması ve tersine dönüşümüyle ilgilenebilir. Modem **sayısal bitleri analog taşıyıcıya bindirir**; codec analog dalgayı örnekleyip sayısal kodlara dönüştürebilir. İkisi kavramsal olarak karıştırılmamalıdır.

### Slayt 13 — Yerel döngü 3/4 — İletim bozulmaları

Üç ana sorun: **zayıflama (attenuation)** sinyal gücünün azalması; **gecikme bozulması (delay distortion)** farklı frekans bileşenlerinin farklı gecikmelerle gelmesi; **gürültü (noise)** istenmeyen ek sinyaller. Bu üçü sembol kararında hata yaratabilir. Analog hat üzerinde gerilim zamanla değiştirilerek bilgi temsil edilir.

### Slayt 14 — Yerel döngü 4/4 — Attenuation, distortion, noise

Zayıflama sıklıkla `dB/km` ile belirtilir; frekans ve hat uzunluğu önemlidir. Gecikme bozulmasında farklı Fourier bileşenleri farklı hızlarda yayılarak darbe şeklini bozar. Gürültü örnekleri: **ısıl gürültü (thermal)**, **karışma (crosstalk)**, **darbe gürültüsü (impulse)**. **Zayıflama = güç kaybı; gürültü = istenmeyen güç; distortion = dalga biçimindeki değişiklik.**

> [!question] Bölüm sonu kontrolü
> **Kendine sor:** Zayıflama, gürültü ve gecikme bozulması neden birbirinden farklıdır?


---

## Bölüm II — Modemler ve Sayısal Modülasyon (15–29)

### Slayt 15 — Modemler 1/15 — Neden taşıyıcı kullanılır?

Kare dijital darbelerin spektrumu geniştir. Telefon hattı yüksek frekansları farklı oranlarda zayıflatıp geciktirir. Bu yüzden tarihsel telefon ağında doğru akım tabanlı kare darbe yerine ses bandındaki bir **sinüzoidal taşıyıcı** modüle edilir. Amaç sayısal sembolleri hattın geçirebildiği frekans bölgesine taşımaktır.

### Slayt 16 — Modemler 2/15 — AM, FM, PM

Temel taşıyıcı `s(t)=A cos(2π f_c t+φ)` ile modellenebilir. **Genlik modülasyonu (AM/ASK)** `A` değerini; **frekans modülasyonu (FM/FSK)** `f_c` değerini; **faz modülasyonu (PM/PSK)** `φ` değerini değiştirir. Örneğin BPSK'da iki faz `0°` ve `180°` iki ikili sembolü temsil eder. Terimler taşıyıcının hangi fiziksel parametresinin değiştiğini gösterir.

### Slayt 17 — Modemler 3/15 — Dalga şekilleri

Şekilde (a) ikili bitler, (b) genlik değişimi, (c) frekans değişimi, (d) faz değişimi gösterilir. Grafik sorusunda önce sabit kalan özellikleri işaretle: ASK'da farklı genlik, FSK'da farklı salınım sıklığı, PSK'da faz atlaması. **Dalga aniden aşağı geçti diye mutlaka frekans değişmiştir** sonucunu çıkarma.

### Slayt 18 — Modemler 4/15 — Modem tanımı

**MOdulator–DEModulator**: göndericide bitleri modüle edilmiş taşıyıcı sinyale, alıcıda taşıyıcıyı bitlere dönüştüren cihaz. Tarihsel modemler örneğin saniyede 2400 sembol gönderip **sembol başına daha çok bit** kodlayarak veri hızını yükseltmeye çalışır. **2400 baud ≠ her zaman 2400 bps.**

### Slayt 19 — Modemler 5/15 — Baud ve bit/saniye

**Baud = saniyedeki sembol (signal element) sayısı**, `R_s` (sembol/s). **Bit hızı = saniyede taşınan bilgi biti**, `R_b` (bit/s). `M` eş olasılıklı ikili kodlanmış sembol noktası için ideal `k=log₂ M` bit/sembol ve `R_b=R_s log₂ M`. Örnek: 2400 baud, `M=4` → `4800 bit/s`; sembol süresi `1/2400≈416,7 μs`. Kodlama ek yükü varsa net bilgi hızı daha düşük olabilir.

### Slayt 20 — Modemler 6/15 — QPSK ve bant genişliği

**QPSK (Quadrature Phase Shift Keying)** dört ayrı faz konumu kullanır; `log₂4=2 bit/sembol` taşır. 2400 baud→4800 bit/s (ideal). **Bant genişliği**, ortamın geçirebildiği frekans aralığıdır ve Hz ölçülür. Baud (sembol/s) ve bit/s ile aynı büyüklük değildir; modülasyon biçimi bu değerleri ilişkilendirir.

### Slayt 21 — Modemler 7/15 — Sembol hızı–veri hızı

Dersin önemli özeti: **R_b=R_s×k**, burada `R_s` sembol/s, `k` faydalı bit/semboldür. QPSK 2; 16-QAM 4; 64-QAM 6 bit/sembol taşır (ideal). 1200 baud ve 16-QAM ⇒ **4800 bit/s**. **Baud hızını bit hızı olarak yazma.**

### Slayt 22 — Modemler 8/15 — QPSK, 16-QAM, 64-QAM şekli

Takımyıldız (constellation) şekli, izin verilen karmaşık düzlem sembol noktalarını gösterir. QPSK 4, 16-QAM 16, 64-QAM 64 nokta kullanır. Her nokta genlik ve fazın bir birleşimidir. Sembol noktaları birbirine yaklaştıkça aynı gürültü koşullarında yanlış algılama ihtimali artabilir.

### Slayt 23 — Modemler 9/15 — QAM ve bit/sembol

**QAM (Quadrature Amplitude Modulation)** genlik/faz bileşenlerini kullanarak daha çok sembol durumu oluşturur. `16-QAM→4 bit/sembol`, `64-QAM→6 bit/sembol`. 2400 baud ve 16-QAM ⇒ **9600 bps**. Nokta sayısı `M` arttıkça teorik bit/sembol `log₂ M` artar; pratikte daha iyi SNR gerekir.

### Slayt 24 — Modemler 10/15 — Takımyıldız ve TCM

**Constellation diagram** sembol noktalarının düzlemdeki konumlarını gösterir. Gürültü bir noktayı komşu noktanın karar bölgesine taşıyabilir. **TCM (Trellis-Coded Modulation)** kodlama yoluyla hata dayanıklılığı elde etmeyi amaçlar; aynı sembol hızında ham kod bitleriyle net kullanıcı bitlerini ayırmak önemlidir.

### Slayt 25 — Modemler 11/15 — V.32 ve V.32bis şekli

Slayttaki iki takımyıldız şekli tarihsel modem standartlarını gösterir: **V.32 ≈ 9600 bit/s**, **V.32bis ≈ 14.400 bit/s**. Gösterilen nokta sayıları kodlanmış bitleri de içerebilir; noktaların sayısını doğrudan faydalı bit hızıyla eşitleme. Daha sık nokta kümeleri daha duyarlı alıcı kararları gerektirir.

### Slayt 26 — Modemler 12/15 — V.32/V.32bis verileri

Tarihsel açıklamada V.32 için 2400 baud×4 bilgi biti/sembol=9600 bps; ek kodlama biti kullanılabilir. V.32bis 2400 baud×6 bilgi biti/sembol=14.400 bps; takımyıldızda ek kodlama durumları bulunabilir. **Ham sembol etiketleme bitleri ile kullanıcıya ulaşan net veri bitleri aynı olmak zorunda değildir.**

### Slayt 27 — Modemler 13/15 — V.34/V.34bis, sıkıştırma

Sunum V.34 için 28.800 bps, tarihsel V.34bis anlatımı için 33.600 bps örnekleri verir. Bu tip hatlarda modülasyon, kodlama ve sembol hızı birlikte değerlendirilir. **Sıkıştırma** tekrarlı verilerde uygulamanın gördüğü etkin aktarımı artırabilir, ama fiziksel bağlantının bit hızı sınırını sihirli biçimde yükseltmez. Zaten sıkıştırılmış veride kazanç düşük olabilir.

### Slayt 28 — Modemler 14/15 — Simplex, half/full-duplex

**Simplex:** yalnızca tek yön. **Half-duplex:** iki yönde ama aynı anda değil. **Full-duplex:** her iki yönde aynı anda. Full-duplex modemlerde yönler farklı frekans aralıklarıyla ayrılabilir. **Asimetrik hız** (indirme/yükleme farklı) ile **full-duplex** (aynı anda iki yön) birbirinden bağımsız özelliklerdir.

### Slayt 29 — Modemler 15/15 — V.90 ve V.92

Tarihsel V.90 yaklaşık 56 kbps indirme ve 33,6 kbps yükleme hedefler; V.92 uygun hatta 48 kbps yükleme sunabilir. Bu oranlar eski çevirmeli PSTN teknolojisine aittir. **56 kbps her hatta garantili değildir**; hat koşulları ve düzenlemeler gerçek hızı sınırlar. Telefon çağrısı geldiğinde internet oturumunun askıya alınmasına ilişkin özellikler de sunulmuştur.

> [!question] Bölüm sonu kontrolü
> **Kendine sor:** 2400 baud, 16-QAM için bit hızı kaçtır? V.32bis hızını hangi çarpma verir?


---

## Bölüm III — DSL ve Kablosuz Yerel Döngü (30–41)

### Slayt 30 — DSL 1/9 — xDSL ve asimetri

**DSL (Digital Subscriber Line)**, mevcut telefon abone bakır çiftini geniş bant veri erişiminde kullanır. `xDSL` aile adıdır; **ADSL (Asymmetric DSL)** indirme hızı genellikle yüklemeden yüksek olacak şekilde tasarlanmıştır. Hat uzunluğu, kesit, gürültü, ekler ve bakır kalitesi kapasiteyi etkiler. xDSL, aynı hız garantisine sahip tek bir ürün değildir.

### Slayt 31 — DSL 2/9 — Bant genişliği–mesafe grafiği

Şekil yerel bakır hattın uzunluğu arttıkça kullanılabilir bant genişliği/veri kapasitesinin genellikle azaldığını gösterir. Nedeni frekansa bağımlı zayıflama ve gürültü etkisidir. **Daha yüksek frekans bileşenleri genelde uzun bakır hatlarda daha zor taşınır.** Sınav grafik yorumunda x ekseni mesafe, y ekseni bant genişliğidir; sayıların tarihsel kablo koşullarına bağlı olduğunu belirt.

### Slayt 32 — DSL 3/9 — xDSL hizmetinin hedefleri

Tarihsel hedefler: mevcut kategori-3 bükümlü çift altyapısını kullanmak, normal telefon/faks hizmetini bozmamak, çevirmeli modemden hızlı olmak, sürekli açık (always on) hizmet sunmak. Böylece kullanıcı internete her seferinde telefon numarası çevirerek bağlanmak zorunda kalmaz. **Always on**, gerçek ağda hiç kesinti olmaz garantisi değildir.

### Slayt 33 — DSL 4/9 — Frekans bantlarına ayırma

Erken ADSL, yaklaşık 1,1 MHz'lik bakır hat spektrumunu **POTS (ses)**, **upstream**, **downstream** bölgelerine ayırır; buna **FDM (Frequency Division Multiplexing)** denir. Alternatif **DMT (Discrete MultiTone)** çok sayıda dar alt taşıyıcı üzerinden veri kodlar. Ses ve veri birbirinden farklı frekanslarda taşınabilir.

### Slayt 34 — DSL 5/9 — DMT şekli

Sunumdaki klasik DMT örneğinde ~1,1 MHz bölge **256 alt taşıyıcıya**, her biri yaklaşık **4312,5 Hz** genişliğe bölünür. Düşük tonlar POTS/koruma için ayrılır; geri kalanı yönlere ve kanal koşullarına göre tahsis edilir. **256×4312,5 Hz=1.104.000 Hz ≈1,1 MHz.** İdeal soyut bir kanal aralığı sayımını gerçek net bit hızına eşitleme.

### Slayt 35 — DSL 6/9 — Tarihsel ADSL hızları

Sunumun ilk nesil ADSL bağlamında verdiği üst hızlar yaklaşık **8 Mbps downstream, 1 Mbps upstream** ve bazı tarihsel paket örnekleridir. Modülasyonda QAM ve tona göre bit yükleme kullanılır. **Güncel VDSL, ADSL2+, fiber gibi hizmetlerin hızları farklıdır; slayt rakamları dersin tarihsel referansıdır.**

### Slayt 36 — DSL 7/9 — NID ve ayırıcı

**NID (Network Interface Device)** operatör abonelik hattıyla müşteri tesisatı arasındaki demarkasyon noktasıdır. **Splitter (ayırıcı)** sesin tipik 0–4 kHz POTS bölgesini üst frekans veri bölgesinden analog filtreyle ayırır: telefon/faks ses tarafına, DSL modem veri tarafına bağlanır. Splitter, IP paketi yönlendirmez.

### Slayt 37 — DSL 8/9 — ADSL ekipman şekli

Şekildeki zincir: `bakır hat → müşteri NID/splitter → telefon + DSL modem` ve karşı uçta `santral splitter → ses anahtarı + DSLAM`. **DSLAM (Digital Subscriber Line Access Multiplexer)** çok sayıda DSL abonesinin veri trafiğini toplayıp sağlayıcı ağına iletir. Abone modem ve santral tarafındaki DSLAM aynı DSL fiziksel iletim tekniğini kullanır.

### Slayt 38 — DSL 9/9 — Santral tarafı ve DSLAM

Santralde alçak frekanslı POTS bileşeni ses anahtarına ayrılır; DSL frekansları **DSLAM** üzerinden veri ağına verilir. **ADSL esas itibarıyla fiziksel katman erişim teknolojisidir**; internet oturumu ve IP servisi daha üst katmanlarda ek mekanizmalara dayanır. DSLAM, DSL abone hatlarının toplanma noktasıdır; tek başına HTTP sunucusu değildir.

### Slayt 39 — Kablosuz yerel döngü 1/3 — WLL/MMDS/LMDS

**WLL (Wireless Local Loop)**, sabit aboneyi operatör ağına bakır hat yerine radyo bağlantısıyla bağlar. **MMDS** ve **LMDS** sunumda tarihsel sabit kablosuz erişim seçenekleridir. Frekans yükseldikçe daha geniş bant potansiyeli artabilir fakat görüş hattı ve yağış/engel hassasiyeti de önemli olabilir. Sunumdaki hizmet bantları ve kapsama sayıları ülke/teknolojiye göre değişir.

### Slayt 40 — Kablosuz yerel döngü 2/3 — LMDS mimarisi

Şekil yüksek noktadaki baz istasyonunun radyo sektörlerini kapsayıp sabit kullanıcı terminallerine erişim sunmasını gösterir. Sektör anteni, müşteri terminali, taşıyıcı omurga ve abone cihazı arasındaki rol dağılımını incele. Bir sektörün kapasitesi genellikle aynı anda aktif birden fazla abonelik tarafından **paylaşılır**; her kullanıcıya aynı toplam hız verilmez.

### Slayt 41 — Kablosuz yerel döngü 3/3 — Paylaşılan kapasite

Sunum LMDS'de asimetrik hız, sektör başına paylaşılan kapasite ve aktif kullanıcı sayısı üzerinden örnek verir. **Boyut kontrolü:** Sayfalar 3×5 KB/dk ise `15.000×8/60≈2000 bit/s` ortalama gereksinimdir (ondalık KB); 3,6 Mbps kapasite varsayımıyla `3.600.000/2000≈1800` teorik kullanıcı çıkar. Slayttaki birimlerin PDF metin çıkarımında bitişmesi yanlış bir “36 Gbps” okumasına yol açabilir; bu hesap 3,6 Mbps'ye dayanır. Bu, paylaşılan ideal ortalama hesabıdır; radyo ek yüklerini kapsamaz.

> [!question] Bölüm sonu kontrolü
> **Kendine sor:** ADSL’de ses ve veri neden aynı çift üzerinde eşzamanlı taşınabilir?


---

## Bölüm IV — Çoklama ve Anahtarlama (42–54)

### Slayt 42 — Trunk ve Çoklama — FDM/TDM

**Çoklama (multiplexing)**, bir fiziksel bağlantıyı birden fazla iletişim akışının paylaşmasıdır. **FDM** bant genişliğini ayrı frekans alt bantlarına ayırır; kullanıcılar aynı zamanda farklı frekanslarda taşınır. **TDM** zamanı dilimlere böler; kullanıcılar sırayla aynı fiziksel kapasiteyi kullanır. Çoklama ve anahtarlama birbirinden farklıdır.

### Slayt 43 — FDM 1/2 — Frekans kaydırma şekli

Şekil her ses kanalının temel banttaki frekans aralığının daha yüksek, ayrı bir bant konumuna taşınmasını ve toplam spektrumda yan yana yerleştirilmesini gösterir. Birden çok sinyal **aynı anda** farklı taşıyıcı frekanslarda bulunabilir. Kanallar arası koruma bantları girişimi azaltmaya yardım eder ve spektral verimi etkiler.

### Slayt 44 — Trunk ve Çoklama 2/2 — Group, supergroup

Tarihsel telefon FDM hiyerarşisi: **12 adet 4 kHz ses kanalı = 48 kHz grup** (örnek bandı 60–108 kHz). **5 grup = 60 ses kanallı supergroup.** **5 supergroup = 300 kanallı mastergroup** (CCITT örneği); bazı Bell düzenlerinde 10 supergroup kullanılır. Hiyerarşi **farklı ses kanallarının frekansta istiflenmesi** fikrini gösterir.

### Slayt 45 — WDM 1/2 — Dalga boyu çoklaması

**WDM (Wavelength Division Multiplexing)**, optik fiber içinde farklı ışık dalga boylarını aynı anda taşır. Elektriksel FDM'nin optik benzeri olarak düşünülebilir. Farklı λ kanallarını birleştiren/söken optik filtreler ve dalga boyu seçici elemanlar kullanılır. Slayttaki kanal sayıları (8×2,5; 40×2,5; 96×10 Gb/s) tarihsel teknoloji evreleridir.

### Slayt 46 — WDM 2/2 — Combiner ve splitter şekli

Şekil dört giriş ışığının **λ₁, λ₂, λ₃, λ₄** bir **combiner** ile ortak fiberde taşınmasını ve çıkışta **splitter/filtre** ile ayrılmasını gösterir. Tek fiber içindeki sinyaller birbirini aynı dalga boyunda ezmeden farklı λ kullanır; optik güç sınırları, filtre ayrımı ve doğrusal olmayan etkiler yine önemlidir. `4 kanal×10 Gb/s=40 Gb/s` ham hat kapasitesi örneğidir.

### Slayt 47 — TDM 1 — T1 taşıyıcısı

Tarihsel **T1**: 24 ses kanalı × 8 bit/örnek × 8000 örnek/s = **1.536.000 bit/s**; her çerçeveye 1 ek çerçeveleme biti eklenirse `193 bit/çerçeve×8000 çerçeve/s=1.544.000 bit/s`. **1,544 Mbps** değerini sayılarla türetebilmelisin. T1 ve E1 aynı hızda değildir.

### Slayt 48 — TDM 2 — Delta modülasyonu

**Delta modulation**, her örnekte kesin genlik değerini göndermek yerine sinyalin bir önceki tahmine göre **artış/azalış yönünü** kodlayan yaklaşımın temel biçimidir. Bir bit artır/azalt kararı temsil edebilir. Aşırı hızlı değişimlerde eğim aşırı yüklenmesi; küçük değişimlerde granüler gürültü oluşabilir. **PCM**, genliği örnekleyip nicemler; delta modülasyonu fark/yön kodlar.

### Slayt 49 — TDM 3 — Yüksek kademeli T taşıyıcılar

Çok sayıda T1 akışı daha yüksek seviyeli T taşıyıcılara çoklanabilir; ek çerçeveleme ve hizalama bitleri bulunduğundan **çıkış hızı her zaman basitçe n×T1** olmak zorunda değildir. Slayt T1'lerin T2/T3/T4 seviyelerine toplanışını gösterir. Hangi hız standardının hangi dijital taşıyıcıya ait olduğunun aynı olmadığını bil.

### Slayt 50 — TDM 4 — SONET çerçevesi

**SONET (Synchronous Optical Network)**, fiber üzerinden senkron dijital taşıma için standart yapıdır. Şekil arka arkaya iki SONET çerçevesini gösterir; **STS-1/OC-1 ≈51,84 Mbps** temel hızdır. Tarihsel SONET çerçevesi 9 satır×90 sütun×8 bit=6480 bit ve 125 μs çerçeve süresi → 51,84 Mbps. Kullanıcı yükü daha düşüktür; üstveri/overhead ayrılır.

### Slayt 51 — TDM 5 — SONET/SDH oranları

**SDH (Synchronous Digital Hierarchy)** SONET'le ilişkili uluslararası senkron taşıma ailesidir. Örnek oranlar: SONET STS-1 51,84 Mbps, STS-3/OC-3 155,52 Mbps; SDH STM-1 155,52 Mbps, STM-4 622,08 Mbps, STM-16 2,48832 Gbps. **Hat hızı ≠ net uygulama veri hızı**: taşıma çerçevesinin yükleri ve uyarlama üst verileri bulunur.

### Slayt 52 — Devre anahtarlama — Circuit switching

**Devre anahtarlama (Circuit Switching)**, görüşme/iletişim öncesinde mantıksal veya fiziksel kaynak tahsisiyle uçtan uca bağlantı oluşturur. Kaynaklar bağlantı süresince ayrılmış olabilir; kurulum gecikmesi vardır fakat sürekli akışta öngörülebilirlik sağlanabilir. Paket anahtarlamada veri parçalar hâlinde, paylaşılan bağlantılarda taşınır. **Devre anahtarlama PSTN'yle, paket anahtarlama IP ağlarıyla tarihsel olarak ilişkilidir.**

### Slayt 53 — Mesaj anahtarlama — Store and forward

**Mesaj anahtarlama (Message Switching)**, tüm mesajın ara düğüme gelip saklanarak sonraki düğüme aktarılmasıdır. Devre kurulumu gerektirmez ama büyük mesaj ara bellek ve uzun bekleme gerektirebilir. **Paket anahtarlama** mesajı küçük parçalara bölerek farklı paketlerin ardışık bağlantılarda boru hattı gibi ilerlemesini sağlayabilir. Üç yöntemi şekil üzerinden karşılaştır.

### Slayt 54 — Paket anahtarlama — Circuit/packet karşılaştırması

Paket anahtarlamada farklı akışlar aynı hattın kapasitesini istatistiksel olarak paylaşır; kaynaklar trafik yokken boşa ayrılmış kalmayabilir. Ancak **kuyruklama, değişken gecikme, paket kaybı** görülebilir. Devre anahtarlamada kurulum sonrası tahsis edilmiş kaynaklar boş zamanlarda kullanılmayabilir. Sınavda **statistical multiplexing** ile sabit devre tahsisini ayır.

> [!question] Bölüm sonu kontrolü
> **Kendine sor:** Devre, mesaj ve paket anahtarlamada kurulum, sakla-gönder ve kaynak paylaşımı nasıl değişir?


---

## Bölüm V — Mobil Telefon Sistemleri (55–62)

### Slayt 55 — Mobil telefon sistemi — 1G, 2G, 3G

Slaytın tarihsel sınıflandırması: **1G analog ses (AMPS)**, **2G dijital ses (D-AMPS/GSM vb.)**, **3G dijital ses ve veri (IMT-2000 ailesi)**. Bu tablo günümüzün 4G LTE ve 5G sistemlerini kapsamıyor; sunum tarihsel teknolojik gelişimi öğretir. Her nesil tek bir ülke standardı değildir.

### Slayt 56 — AMPS — Hücresel frekans yeniden kullanımı

**Hücresel ağ** bölgeyi küçük kapsama alanlarına ayırır. Komşu hücrelerde aynı frekansları kullanmak girişim yaratabileceğinden frekanslar uygun örüntüyle yeniden kullanılır (**frequency reuse**). Hücrelerin küçültülmesi kapasiteyi artırabilir; fakat daha çok baz istasyonu ve daha sık geçiş (**handoff**) gerekir. Gerçekte hücreler düzgün altıgen olmak zorunda değildir.

### Slayt 57 — AMPS kanal kategorileri

Sunumdaki **832 kanal** tarihsel AMPS örneğinde kategoriler: **control** baz→mobil sistem kontrolü; **paging** baz→mobil gelen çağrıyı haber verme; **access** iki yönlü çağrı kurulumu/kanal isteği; **data/traffic** iki yönlü ses/faks/veri taşıma. Sınavda kontrol trafiği ile kullanıcı trafiğini ayır; bu sayı güncel mobil frekans tahsislerini göstermez.

### Slayt 58 — D-AMPS — TDMA ile çoklu kullanıcı

**D-AMPS (Digital Advanced Mobile Phone System)**, dijital hücresel sistemde aynı RF kanalını zaman dilimlerine bölerek birden fazla kullanıcının paylaşabilmesine örnektir. Slayt 3 veya 6 kullanıcının zaman dilimleriyle dağıtılmasını gösterir. Önemli kavram **TDMA (Time Division Multiple Access)**; burada baz istasyonu kullanıcıların zamanını koordine eder.

### Slayt 59 — GSM — Frekans kanalları ve 8 zaman yuvası

**GSM (Global System for Mobile Communications)**, tarihsel 2G dijital mobil iletişim standardıdır. Slayt örneğinde 124 frekans kanalı ve her taşıyıcıda **8 zaman yuvası** içeren TDMA yapısı gösterilir. Bu sayılar belirli GSM bandı/dağılımı örneğidir; tüm GSM bantlarında her zaman 124 kanal bulunduğu anlamına gelmez. Ayrıca baz/terminal arasında frekans ayrımı kullanılabilir.

### Slayt 60 — GSM 2 — Çerçeve yapısı

Slayt GSM'nin zaman dilimi, TDMA çerçevesi ve daha üst **multiframe** yapısını gösterir. Tek bir taşıyıcıda farklı kullanıcıların patlamaları (bursts) belirlenmiş zaman aralıklarında yerleştirilir. Slot, frame ve multiframe farklı zaman ölçekleridir; çizimde bir zaman yuvasını tam bir frekans kanalı olarak yorumlama.

### Slayt 61 — CDMA — Çip dizileri ve korelasyon

**CDMA (Code Division Multiple Access)**, kullanıcıların farklı yayma kodlarıyla aynı zaman/frekans kaynaklarını paylaşmasına dayanır. Her bit, kodun **chip** değerleriyle yayılır; alıcı istenen kullanıcının koduyla korelasyon yaparak sinyali ayırabilir. İdeal ortogonal dizilerde farklı kullanıcıların çapraz korelasyonu sıfıra yakın/0 olabilir. **Chip rate**, ham kullanıcı bit hızından yüksektir; PN/ortogonallik, senkronizasyon ve güç kontrolü önemlidir.

### Slayt 62 — 3G — IMT-2000 hizmetleri

Slayt 3G/IMT-2000 için kaliteli ses, mesajlaşma, multimedya, internet erişimi gereksinimlerini listeler. 3G'nin tarihsel yeniliği yalnızca ses değil paket veri ve daha yüksek kapasitedir. **Bu beklentiler dönemsel 3G hedeflerini gösterir; 5G standardı olarak yorumlanmamalıdır.**

> [!question] Bölüm sonu kontrolü
> **Kendine sor:** GSM’de TDMA ile CDMA’da kod ayrımını örnekle açıkla.


---

## Bölüm VI — Kablolu Televizyon ve Kablo İnternet (63–68)

### Slayt 63 — Kablolu televizyon — Başlıklar

Sunum son bölümde beş konuyu sıralar: **Community Antenna Television**, **Internet over Cable**, **Spectrum Allocation**, **Cable Modems**, **ADSL versus Cable**. Kablolu televizyonun geleneksel tek yönlü yayın altyapısından iki yönlü geniş bant erişime evrilmesi ana hikâyedir. ADSL bakır telefon hattı, kablo internet çoğunlukla HFC/coax dağıtımı kullanır.

### Slayt 64 — Community Antenna Television — İlk CATV

**CATV**, tarihsel olarak uzak/engelli bölgelerde TV yayınlarını ortak yüksek antenle alıp koaksiyel kablo üzerinden abonelere dağıtma modelinden doğdu. İlk sistemler ağırlıklı **tek yönlü (downstream broadcast)** idi. İnternet hizmeti sunmak için upstream yolunun ve çift yönlü erişim donanımlarının eklenmesi gerekir.

### Slayt 65 — Internet over Cable 1 — Kablo TV erişimi

Şekilde kablo TV ağının çok sayıda aboneye dallanarak ulaştığı yapı bulunur. Modern kablo internetin temel fikri **paylaşılan dağıtım ortamında** veri iletmektir; merkezde kablo operatörü sistemi, saha dağıtım elemanları ve evlerde kablo modem bulunur. **Bir komşunun trafiği her durumda seni eşit etkiler** gibi kesin hükümler doğru değildir; segmentasyon, planlama ve QoS önemlidir.

### Slayt 66 — Internet over Cable 2 — Telefon erişimiyle karşılaştırma

Şekil geleneksel telefon erişiminde eve gelen **ayrı yerel abone çiftleri** ile kablo TV'nin **paylaşılan dağıtım ağını** karşılaştırır. DSL abone bakır hattı santrale kadar bireysel olabilir; santral sonrası ağ kaynakları elbette paylaşılır. Kablo ağında mahalle/servis grubundaki fiziksel erişim spektrumu kullanıcılar arasında paylaşılıyor olabilir. **DSL tamamen özel, kablo tamamen paylaşımsız** gibi ezberler yanlıştır.

### Slayt 67 — Kablo erişiminde spektrum tahsisi

Kablo TV sistemi, frekans spektrumunu TV yayınları, **upstream** ve **downstream** veri trafiği için ayrı bantlara ayırabilir. Şekilde bantlar tarihsel Kuzey Amerika uygulamasını temsil eder. FDM ile ayrılan frekanslar ve kanal üzerindeki çoklu kullanıcı erişimi farklı kavramlardır. Gerçek DOCSIS sürümlerinde daha farklı spektrum planları kullanılabilir.

### Slayt 68 — Kablo modemler — Yukarı/aşağı kanalları

Kablo modem, operatörün koaksiyel/HFC erişim şebekesinde dijital veri taşımak için ilgili modülasyon ve MAC mekanizmalarını kullanır; merkez tarafta tipik olarak **CMTS (Cable Modem Termination System)** veya güncel dağıtık erişim bileşenleri bulunur. **Downstream** ağ→abone, **upstream** abone→ağdır; yönlerin kanal genişliği/modülasyonu farklı olabilir. Sunumdaki ayrıntılar tarihsel Kuzey Amerika kanal planlarıdır; **DOCSIS nesillerinin güncel tüm limitleri değildir.**

> [!question] Bölüm sonu kontrolü
> **Kendine sor:** DSL ile kablo internette abone hattı ve paylaşılan erişim alanı nasıl farklılaşır?


---

## Formül Föyü ve Çözümlü Hesaplamalar

### 1. Sembol hızı ve bit hızı

$$R_b=R_s\log_2 M$$

- $R_s$: sembol hızı (**baud** = sembol/s).
- $M$: modülasyonda kullanılan ayrı sembol durumu sayısı.
- $R_b$: ideal ham bit hızı (bit/s). Net kullanıcı hızı kodlama/çerçeveleme yüklerinden etkilenebilir.

> [!example] Örnek A — QPSK
> 2400 baud ve dört farklı faz için $M=4$, $\log_2 4=2$. Dolayısıyla $R_b=2400\times2=\boxed{4800\ \text{bit/s}}$.

> [!example] Örnek B — 16-QAM
> 2400 baud, $M=16$: sembol başına 4 bit. $R_b=2400\times4=\boxed{9600\ \text{bit/s}}$.

> [!example] Örnek C — 64-QAM
> 3000 baud, $M=64$: $\log_2 64=6$. $R_b=3000\times6=\boxed{18\ 000\ \text{bit/s}}$.

> [!example] Örnek D — Sembol süresi
> 2400 baud: $T_s=1/2400\ \text{s}=0{,}00041667\ \text{s}=\boxed{416{,}67\ \mu\text{s}}$.

### 2. Frekans bölüşümü ve ADSL

DMT örneği: 256 alt kanal, her biri 4312,5 Hz:

$$256\times4312{,}5=1\ 104\ 000\ \text{Hz}\approx1{,}1\ \text{MHz}.$$

> [!warning] Frekans aralığı, doğrudan net kullanıcı bit hızı değildir
> DMT'nin her tonu farklı sayıda bit kodlayabilir; gürültü, kodlama ve kullanılmayan bantlar net hızı etkiler.

### 3. T1 — Neden 1,544 Mbps?

$24$ ses kanalı × $8$ bit/örnek × $8000$ örnek/s = $1\ 536\ 000$ bit/s ses yükü.

Her örnekleme periyodunda bir çerçeveleme biti varsa:

$$R=(24\times8+1)\times8000=193\times8000=\boxed{1{,}544\ \text{Mbps}}.$$

> [!example] T1 çerçeve süresi
> $1/8000\ \text{s}=125\ \mu\text{s}$. Her T1 çerçevesi 193 bittir.

### 4. SONET STS-1

9 satır × 90 sütun × 8 bit = 6480 bit/çerçeve.

$$R=6480/(125\ \mu\text{s})=\boxed{51{,}84\ \text{Mbps}}.$$

Üç STS-1 eşdeğer hız: $3\times51{,}84=\boxed{155{,}52\ \text{Mbps}}$ (OC-3/STM-1 nominal hat hızı).

### 5. FDM telefon grubu

12 ses kanalı × 4 kHz = **48 kHz**. 60–108 kHz bandında 12 ses kanalı örneği. Beş grup × 12 = **60 kanal** (supergroup).

### 6. WDM kapasitesi

8 dalga boyu, her biri 2,5 Gbps ⇒ $8\times2{,}5=\boxed{20\ \text{Gbps}}$ nominal toplam.

96 dalga boyu, her biri 10 Gbps ⇒ $96\times10=\boxed{960\ \text{Gbps}}$ nominal toplam. Bu, üst katman net kapasitesi garantisi değildir.

### 7. LMDS ortalama talep

Abone, 1 dakikada üç adet 5 KB sayfa indiriyorsa ve 1 KB = 1000 B alınırsa:

$$R=\frac{3\times5\times1000\times8}{60}=\boxed{2000\ \text{bit/s}}.$$

Paylaşılan teorik kanal $3{,}6$ Mbps ise en fazla $3\ 600\ 000/2000=1800$ eşzamanlı ortalama kullanıcı gereksinimi karşılanabilir (protokol yükü, radyo kanal koşulu ve ani trafik yok varsayımıyla).

### 8. Desibel ile zayıflama

Güç kaybı için $L_{dB}=10\log_{10}(P_{in}/P_{out})$. Örneğin giriş gücü 100 mW ve çıkış 10 mW ise kayıp $10\log_{10}(10)=\boxed{10\ dB}$. Genlik oranı için uygun koşullarda $20\log_{10}$ kullanılır; **güç ve genlik dB hesaplarını karıştırma**.

---

## Kavram Karşılaştırmaları

### Modülasyon ve erişim

| Kavram | Anlamı | Sınavda ayrım |
|---|---|---|
| ASK | Genliği değiştirir | Fiziksel taşıyıcı parametresi |
| FSK | Frekansı değiştirir | Taşıyıcı seçimi |
| PSK | Fazı değiştirir | Faz konumu |
| QAM | Genlik/faz birleşimiyle çoklu sembol | $\log_2 M$ bit/sembol |
| Baud | Sembol/s | bps değildir |
| bps | Bit/s | Baud × bit/sembol |
| Full-duplex | Aynı anda iki yönde | Asimetrik hızla aynı kavram değildir |

### Çoklama türleri

| Yöntem | Ayırdığı kaynak | Tipik örnek |
|---|---|---|
| FDM | Frekans bantları | Klasik telefon taşıyıcıları, DSL spektrum bölüşümü |
| TDM | Zaman yuvaları | T1, GSM TDMA |
| WDM | Optik dalga boyları | Fiber omurga |
| CDMA | Yayma kodları | Kod bölüşümlü hücresel erişim |

> [!warning] Önemli ayrım
> **CDMA, WDM ile aynı şey değildir**: optik taşıyıcıların dalga boylarına ayrılması ile radyo sinyallerinin kodlara göre ayrılması farklı mekanizmalardır.

### Telefon/kablo erişimi

| Özellik | PSTN çevirmeli modem | ADSL | Kablo internet |
|---|---|---|---|
| Tarihsel ortam | Ses bandında analog abone çifti | Bakır telefon abone çifti | Koaksiyel/HFC erişimi |
| Tipik ayrım | Ses bandı modem sinyali | Ses + up/down tonları | Up/down kablo spektrumu |
| Merkez cihazı | Santral/modem bankası | DSLAM | CMTS / kablo erişim sistemi |
| Paylaşım | Devre bazlı | Erişim çifti aboneye ait, sonraki ağ ortak | Paylaşılan dağıtım segmenti |

### Anahtarlama türleri

| Devre anahtarlama | Mesaj anahtarlama | Paket anahtarlama |
|---|---|---|
| Önceden bağlantı/kaynak tahsisi olabilir | Mesaj tümüyle saklanır/aktarılır | Paket bazlı sakla-gönder |
| Ayrılan kapasite boş kalabilir | Büyük ara bellek ve bekleme | İstatistiksel kaynak paylaşımı |
| Çağrı kurulumu gerekir | Bütün mesaj beklenebilir | Kuyruk ve paket kaybı mümkündür |

### Mobil sistemler

| Nesil/teknoloji | Temel fikir |
|---|---|
| 1G / AMPS | Analog hücresel ses; frekans yeniden kullanımı |
| 2G / D-AMPS | Dijital ses ve TDMA tabanlı kanal paylaşımı |
| 2G / GSM | Frekans + zaman yuvaları, 8-slot örneği |
| CDMA | Kod bölüşümü, chip dizileri ve korelasyon |
| 3G / IMT-2000 | Dijital ses, veri ve multimedya |

---

## Yüksek Lisans Vize Denemesi — Week 4

> [!important] Önerilen kullanım
> Önce soruları cevap anahtarına bakmadan çöz. Toplam **100 puan**; önerilen süre **90 dakika**. Sorular, dersin özgün slaytlarına göre hazırlanmış çalışma sorularıdır, resmî sınav değildir.

### Sorular

1. **(5 p)** PSTN'deki local loop, trunk ve switching office kavramlarını tanımlayınız. Çağrı yolunu açıklayınız.
2. **(5 p)** Zayıflama, gecikme bozulması ve gürültünün farklarını örneklerle açıklayınız.
3. **(8 p)** Baud–bps ayrımını yapınız. 3000 baud'da QPSK, 16-QAM ve 64-QAM için ideal hızları hesaplayınız.
4. **(5 p)** ASK, FSK, PSK, QAM yöntemlerini karşılaştırınız. Takımyıldız noktaları neden SNR ihtiyacını etkiler?
5. **(5 p)** Çevirmeli modem, ADSL ve kablo modem arasındaki erişim farkını yazınız.
6. **(6 p)** DMT'nin çalışma prensibini ve ADSL'de POTS/upstream/downstream ayrımını açıklayınız.
7. **(6 p)** ADSL splitter, NID ve DSLAM'ın görevlerini belirtiniz.
8. **(6 p)** FDM, TDM ve WDM'yi kıyaslayınız. Her birine bir örnek veriniz.
9. **(8 p)** T1 hızının neden 1,544 Mbps olduğunu adım adım türetiniz.
10. **(6 p)** SONET STS-1'in 51,84 Mbps hızını 9×90 çerçeve ve 125 μs üzerinden hesaplayınız.
11. **(6 p)** Devre, mesaj ve paket anahtarlamayı kurulum, kaynak paylaşımı ve kuyruklama bakımından açıklayınız.
12. **(6 p)** AMPS'te hücresel frekans yeniden kullanımını; küçük hücrelerin yarar ve maliyetlerini anlatınız.
13. **(7 p)** GSM TDMA ile CDMA'nın çoklu erişim mantığını ve chip kavramını açıklayınız.
14. **(6 p)** CATV'nin tek yönlü TV dağıtımından çift yönlü internet erişimine dönüşümünü açıklayınız.
15. **(7 p)** DSL ile kablo internetin son kilometre topolojilerini karşılaştırınız; paylaşımlı kapasiteyi tartışınız.
16. **(8 p)** Bir WDM fiberde 40 dalga boyu × 2,5 Gbps ve ayrı örnekte 96 dalga boyu × 10 Gbps için toplam nominal kapasiteyi bulunuz.

### Cevap anahtarı ve puanlama ölçütleri

1. **Local loop** abone–uç santral bakır bağlantısı, **trunk** santraller arası yüksek kapasiteli bağlantı, **switching office** çağrıyı uygun bağlantıya aktarır. Örnek yol abone→yerel santral→transit anahtar→hedef santral→abone.
2. **Zayıflama:** sinyal gücü düşer. **Gecikme bozulması:** frekans bileşenleri farklı gecikir. **Gürültü:** dışardan istenmeyen sinyal eklenir (thermal/crosstalk/impulse).
3. Baud sembol/s, bps bit/s. $R_b=R_s\log_2M$. 3000 baud QPSK **6000 bps**; 16-QAM **12.000 bps**; 64-QAM **18.000 bps**.
4. ASK genlik, FSK frekans, PSK faz; QAM genlik ve faz bileşenleri. Daha çok nokta → karar bölgeleri yakınlaşabilir → aynı hata oranı için daha iyi SNR gerekir.
5. Çevirmeli modem ses bandında analog taşıyıcı; ADSL bakır telefon hattındaki geniş bant tonları; kablo modem koaksiyel/HFC üzerinde paylaşılan frekans spektrumu kullanır.
6. DMT, farklı dar alt taşıyıcılara bölerek uygun modülasyonu seçer; POTS alt ses bandında, upstream/downstream veri frekansları ayrılmıştır.
7. NID demarkasyon, splitter ses/veri filtreleme, DSLAM santralde çok sayıda DSL abonesinin veri trafiğini toplama işlevidir.
8. FDM frekans, TDM zaman, WDM ışık dalga boyu. Örnekler telefon grubu/T1/fiber omurga.
9. $(24\times8+1)\times8000=193\times8000=\boxed{1.544.000}$ bit/s.
10. $9\times90\times8=6480$ bit; $6480/(125\times10^{-6})=\boxed{51.840.000}$ bit/s.
11. Devre önceden bağlantı ve olası ayrılmış kaynak; mesaj anahtarlamada bütün mesaj ara düğümde tutulur; paket anahtarlama küçük birimler ve istatistiksel paylaşım kullanır, kuyruk/paket kaybı olabilir.
12. Yakın hücreler girişimden kaçınmak için frekansları koordineli kullanır. Küçük hücre kapasiteyi artırabilir ama baz istasyonu sayısı ve handoff yükü artar.
13. GSM TDMA'da kullanıcılar zaman dilimlerine ayrılır. CDMA kullanıcı verisini özel chip koduyla yayar; alıcı ilgili kodla korelasyon yapar.
14. CATV ilk başta tek yönlü yayın dağıtır; internet için dönüş (upstream) kanalı, kablo modem ve uygun erişim operatörü altyapısı eklenir.
15. DSL bakır abone çiftinde santrale kadar bireysel olabilir; kablo internet fiziksel dağıtım segmentini paylaşır; her ikisi de erişimden sonra ortak ağ kullanır. Performans yük, sinyal ve segmentasyonla değişir.
16. $40\times2{,}5=\boxed{100\ \text{Gbps}}$; $96\times10=\boxed{960\ \text{Gbps}}$ nominal toplam.

---

## Haftalık Çalışma Kontrol Listesi

- [ ] Slayt 1–14: PSTN, local loop/trunk, üç hat bozulması
- [ ] Slayt 15–29: ASK/FSK/PSK/QAM, baud–bit/s, modem hızları
- [ ] Slayt 30–41: xDSL, DMT, splitter/DSLAM, WLL
- [ ] Slayt 42–54: FDM/TDM/WDM, T1, SONET, üç anahtarlama türü
- [ ] Slayt 55–62: AMPS, GSM TDMA, CDMA, 3G
- [ ] Slayt 63–68: CATV, kablo internet, spektrum paylaşımı
- [ ] 16 soruluk 100 puanlık denemeyi çöz ve cevapları karşılaştır

## Kaynaklar ve Obsidian Bağlantıları

- [Ankara Üniversitesi — Week 4 orijinal PDF](https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_4.pdf)
- [Ankara Üniversitesi — Lisans dersleri ve materyal dizini](https://android.eng.ankara.edu.tr/lisans-dersleri/blm332-veri-iletisimi-ve-aglar-blm3032-bilgisayar-aglari/)
- [Ankara Üniversitesi — Chapter 2 örnek problemler](https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Chapter-2.pdf)
- [[Bilgisayar_Aglari_Week_3_Obsidian_Tum_Ders_Notlari|Week 3: Fiziksel Katman — Fourier, Nyquist, Shannon ve iletim ortamları]]
- [[Bilgisayar_Aglari_Week_2_Obsidian_Tum_Ders_Notlari|Week 2: OSI/TCP-IP ve ağ standartları]]
