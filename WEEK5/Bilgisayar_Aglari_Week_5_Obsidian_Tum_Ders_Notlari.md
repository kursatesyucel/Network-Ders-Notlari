---
title: "Bilgisayar Ağları ve Veri İletişimi — Week 5 — Tüm Ders Notları"
aliases:
  - "Week 5 Veri Bağlantı Katmanı"
tags:
  - bilgisayar-aglari
  - yuksek-lisans
  - week-5
  - veri-baglanti-katmani
  - framing
  - hamming
  - crc
  - vize
  - obsidian
course: "Bilgisayar Ağları ve Veri İletişimi"
week: 5
slide_count: 60
language: tr
source: "https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_5.pdf"
---

# Week 5 — Veri Bağlantı Katmanı: Çerçeveleme, Akış Kontrolü, Hamming ve CRC

> [!info] Kaynak ve kapsam
> Ankara Üniversitesi, **Dr. Refik Samet — Week_5.pdf**, *Computer Networks*, Andrew S. Tanenbaum, 4. baskı, **Bölüm 3, s. 183–211**. **60 slaydın tamamı sırasıyla** açıklanmıştır. Türkçe anlatım için özgün İngilizce başlık ve terimler parantez içinde verilmiştir. Sunumdaki tarihsel/genelleyici ifadeler gerektiğinde teknik notlarla güncellenmiştir.

> [!warning] Kaynak düzeltmesi — Slayt 29
> Sunumda bit-stuffing bayrağı **`0111110`** (7 bit) olarak yazılmış görünüyor; **standart HDLC bayrağı `01111110` (8 bit)** olduğundan notlarda doğru biçim kullanılır. Sınavda hem beş `1` sonrası sıfır doldurma kuralını hem de sekiz bitlik bayrağı doğru yaz.

> [!tip] Nasıl çalışmalı?
> İlk önce hizmet türü ve çerçevelemeyi çalış. Ardından Hamming uzaklığı, hata tespit/düzeltme sınırları ve CRC uzun bölme hesaplamalarını **kalemle çözüp kontrol et**. Soru çözerken *katman-2 yerel hata yönetimi* ile *TCP'nin uçtan uca güvenilirliği*ni karıştırma.

## İçindekiler
- [[#Bölüm I — Veri Bağlantı Katmanının Tasarımı (1–11)|Bölüm I — Veri Bağlantı Katmanının Tasarımı (1–11)]]
- [[#Bölüm II — Ağ Katmanına Sunulan Hizmetler (12–19)|Bölüm II — Ağ Katmanına Sunulan Hizmetler (12–19)]]
- [[#Bölüm III — Çerçeveleme Teknikleri (20–31)|Bölüm III — Çerçeveleme Teknikleri (20–31)]]
- [[#Bölüm IV — Hata Kontrolü ve Akış Kontrolü (32–34)|Bölüm IV — Hata Kontrolü ve Akış Kontrolü (32–34)]]
- [[#Bölüm V — Hata Tespiti ve Hata Düzeltme Kodları (35–50)|Bölüm V — Hata Tespiti ve Hata Düzeltme Kodları (35–50)]]
- [[#Bölüm VI — Hata Tespit Kodları ve CRC (51–60)|Bölüm VI — Hata Tespit Kodları ve CRC (51–60)]]
- [[#Formül Föyü|Formül föyü]]
- [[#Çözümlü Hesaplama ve Algoritma Örnekleri|Çözümlü örnekler]]
- [[#Yüksek Lisans Vize Denemesi — Week 5|100 puanlık deneme ve cevap anahtarı]]
- [[#Çalışma Kontrol Listesi|Kontrol listesi]]


---

## Bölüm I — Veri Bağlantı Katmanının Tasarımı (1–11)

### Slayt 1 — Veri Bağlantı Katmanına Giriş

Week 5, Tanenbaum *Computer Networks* 4. baskı Bölüm 3, s.183–211 üzerine kuruludur. Konumuz fiziksel katmanın ham bit iletiminden **veri bağlantı (data link) katmanındaki çerçeveleme ve hata yönetimine** geçiştir. Veri bağlantı katmanının hizmeti çoğunlukla **komşu düğümler** arasında gerçekleştirilir; TCP gibi tüm internet boyunca uçtan uca güvenilirlik ile karıştırılmamalıdır.

### Slayt 2 — Fiziksel katmandan veri bağlantı katmanına

Fiziksel katman sinyalleri kullanarak ham bitleri taşır. Veri bağlantı katmanı, üstteki ağ katmanına daha düzenli bir aktarım hizmeti sunmaya çalışır: **çerçeve sınırlarını** belirler, uygun protokollerde hataları saptar veya giderir ve alıcı kapasitesini dikkate alır. “Hatasız görünme” hedefi her veri bağlantı protokolünün mutlak kayıpsız teslimat sağladığı anlamına gelmez.

### Slayt 3 — Veri bağlantı katmanının amacı — 1/5

Katman 2 tasarımının temel problemi, **iki komşu makine arasında güvenilir ve verimli haberleşme algoritmaları** oluşturmaktır. Tek tek bitleri iletmek tek başına yeterli değildir; çerçevelerin sınırı, doğruluğu ve zamanlaması gibi problemler çözülmelidir.

### Slayt 4 — Neden ek protokoller gerekir? — 2/5

“A veriyi kabloya koyar, B alır” yeterli değildir: fiziksel hatlarda gürültü, girişim, zayıflama ve senkronizasyon kayıpları oluşabilir. Bir bit ters dönebilir, bitler kaybolabilir veya eklenebilir; çerçeve sınırını yanlış anlamak daha büyük bir veri kaymasına yol açabilir.

### Slayt 5 — Sınırlı kapasite ve yayılma gecikmesi — 3/5

Göndericinin veri üretme hızı ve alıcının işleme hızı sonludur. **Yayılma gecikmesi (propagation delay)** sıfır değildir; gönderilen bir bitin karşıya ulaşması zaman alır. Yüksek hız, tek başına sıfır gecikme anlamına gelmez. Bir protokol verimliliğini belirlemek için aktarım süresi, yayılma süresi ve kontrol çerçeveleri birlikte düşünülmelidir.

### Slayt 6 — Protokol tasarımında gerçek dünya — 4/5

Veri bağlantı protokolleri bit hatalarını, çerçeve kaybını, sınırlı tamponu, zaman aşımını ve ACK gecikmesini göz önüne alır. İletişimin teorik hattı ile protokolün sağladığı *hizmet* aynı şey değildir: fiziksel ortam bozulsa bile uygun kontrol teknikleri daha düzenli veri alışverişine katkı sağlar.

### Slayt 7 — Haftanın öğrenme hedefleri — 5/5

Dört amaç: (1) tasarım problemleri ve hizmet türleri; (2) hata türleri ile tespit/düzeltme kodları; (3) giderek karmaşıklaşan veri bağlantı protokollerinin mantığı; (4) protokol modelleme ve doğruluk düşüncesi. Bu 60 slaytta ağırlık **çerçeveleme ve kodlama** üzerindedir; ayrıntılı ARQ protokollerini sonraki içerikle karıştırma.

### Slayt 8 — Tasarım konuları — 1/4

Üç temel işlev: (i) ağ katmanına iyi tanımlanmış hizmet arayüzü sunmak, (ii) iletim hatalarıyla ilgilenmek, (iii) hızlı göndericinin yavaş alıcının tamponunu taşırmasını engellemek. Sonuncusu **akış kontrolü (flow control)** olup ağ tıkanıklığıyla (congestion) aynı problem değildir.

### Slayt 9 — Paketin çerçeve içine alınması — 2/4

Veri bağlantı katmanı üstteki ağ katmanından **paket (packet)** alır ve bunu **çerçeve (frame)** veri alanına yerleştirir. Yaygın gösterim: `Frame = Header | Payload (network packet) | Trailer`. Başlık adres/denetim bilgilerini, son ek kimi protokollerde FCS/CRC gibi hata tespit bilgisini taşıyabilir. Her protokolde son ek zorunlu değildir.

> [!important] Sınavda dikkat
> **Kapsülleme:** `IP paketi → [Çerçeve başlığı | IP paketi | Çerçeve son eki]`. Bağlantı katmanı yerel link sınırlarında işlem yapar.

### Slayt 10 — Paket ve çerçeve ilişkisi — 3/4

Şekil yorumu: Ağ katmanındaki paket, alt katmanda çerçevenin yükü olarak taşınır. Yerel bağlantı değiştiğinde çerçeveleme de değişebilir; IP paketi farklı hoplarda Ethernet, PPP veya Wi‑Fi çerçevelerine kapsüllenebilir. **Paket ≠ çerçeve.** Komşu düğümlerin çerçeveyi değiştirerek iletmesi IP’nin uçtan uca paket adreslemesiyle çelişmez.

### Slayt 11 — Dört tasarım alt konusu — 4/4

Haftanın omurgası: **(1) Ağ katmanına sunulan hizmetler, (2) Çerçeveleme (framing), (3) Hata kontrolü (error control), (4) Akış kontrolü (flow control).** Bu sıra sınavda “veri bağlantı katmanının görevleri” sorusunu yanıtlamak için uygundur.


---

## Bölüm II — Ağ Katmanına Sunulan Hizmetler (12–19)

### Slayt 12 — Ağ katmanına sunulan hizmet — 1/8

Bir komşu düğümdeki ağ katmanından diğer komşu düğümün ağ katmanına bilgi taşınmasını düşün. İki tarafta veri bağlantı katmanları fiziksel bağlantı aracılığıyla hizmet sağlar. Bu aktarımın kalitesi ve garantileri protokolün sunulan hizmet türüne bağlıdır.

### Slayt 13 — Sanal akran iletişimi ve gerçek yol — 2/8

Şekilde katmanların **mantıksal eş katman iletişimi** ile gerçek veri akış yolu karşılaştırılır. Mantıksal olarak kaynak ve hedef ağ katmanları konuşuyor gibi görünür; gerçek veri `Network→Data Link→Physical→Physical→Data Link→Network` sırasını izler. “Doğrudan eş katman iletişimi” fiziksel olarak ortadaki katmanların atlandığı anlamına gelmez.

### Slayt 14 — Üç hizmet türü — 3/8

**1. Onaysız bağlantısız hizmet (unacknowledged connectionless)**; **2. Onaylı bağlantısız hizmet (acknowledged connectionless)**; **3. Onaylı bağlantı odaklı hizmet (acknowledged connection-oriented)**. İki bağımsız eksen vardır: bağlantı kurulup kurulmadığı ve çerçevenin alındığının onaylanıp onaylanmadığı.

> [!important] Sınavda dikkat
> **Önemli:** TCP bağlantısız/bağlantılı sınıflandırması taşıma katmanına, buradaki üç hizmet tipi veri bağlantı katmanına ilişkindir. Aynı kelimeler farklı katmanlarda kullanılabilir.

### Slayt 15 — Onaysız bağlantısız — 4/8

Önceden mantıksal bağlantı kurulmaz; çerçeveler bağımsız gönderilir ve bağlantı katmanında tek tek ACK beklenmez. Bozulan/kaybolan çerçeve için bu hizmet düzeyinde yeniden iletim yapılmaz. Klasik Ethernet bu hizmet yaklaşımına örnek verilebilir; yüksek katmanlar kendi güvenilirlik mekanizmalarını kullanabilir.

### Slayt 16 — Onaylı bağlantısız — 5/8

Önceden bağlantı kurmadan **her çerçeveye ayrı alındı onayı** verilebilir. ACK gelmezse ilgili protokol çerçeveyi yeniden gönderebilir. Gürültülü kablosuz bağlantılarda yerel yeniden iletim faydalıdır. **Bağlantısız olmak, onaysız olmak zorunda değildir.**

### Slayt 17 — Onaylı bağlantı odaklı — 6/8

Önce mantıksal bağlantı kurulur; çerçeveler sıra numarasıyla izlenir. Modelde hedef, her çerçevenin bir kez ve sırayla teslimidir. Kaybolma ve yinelenme sorunlarını gidermek için bağlantı durumu, sıra numarası, ACK ve yeniden iletim birlikte tasarlanmalıdır.

### Slayt 18 — Bağlantı odaklı hizmetin aşamaları — 7/8

Üç aşama vardır: **bağlantı kurma → çerçeve iletimi → bağlantı sonlandırma.** İletim bittikten sonra bağlantıyla ilişkili durum bilgisi kaldırılır. Bu üçlü, Week 2’deki ATM sanal devre aşamalarını hatırlatabilir; ancak ATM ile belirli bir bağlantı katmanı protokolü aynı şey değildir.

### Slayt 19 — Protokolün konumu — 8/8

Şekilde, yönlendiricilerin noktadan noktaya hatlarla bağlandığı WAN örneği görülür. Her fiziksel link için uçlarda veri bağlantı protokolü bulunabilir. Bir yönlendirici, gelen link çerçevesini sonlandırıp ağ katmanı paketini diğer link için yeni bir çerçeveyle gönderebilir.


---

## Bölüm III — Çerçeveleme Teknikleri (20–31)

### Slayt 20 — Çerçeveleme — 1/12

Fiziksel katman bir **bit akışı (bit stream)** taşır; alıcı hangi bitlerin tek bir veri birimine ait olduğunu bilmek zorundadır. Hata, kayıp veya bit eklenmesi mümkün olduğundan **çerçeve sınırının belirlenmesi** gerekir. Bu işleme çerçeveleme denir.

### Slayt 21 — Çerçeveleme ve hata denetimi — 2/12

Bit akışı çerçevelere bölünür. Bir hata denetim değeri (checksum/FCS) hesaplanıp ilgili çerçeveye eklenebilir. Alıcı aynı denetimi tekrarlayarak tutarsızlık varsa hata saptar. **Denetim değerinin tutması, bütün olası hataların kesinlikle tespit edildiği anlamına gelmez**; yönteme bağlı olarak bazı hata desenleri kaçabilir.

### Slayt 22 — Dört çerçeveleme tekniği — 3/12

(1) **Karakter sayısı (character count)**; (2) **bayrak baytı ve bayt doldurma (byte stuffing)**; (3) **bayrak bit dizisi ve bit doldurma (bit stuffing)**; (4) **fiziksel katman kodlama ihlalleri (coding violations)**. Önemli kıyas: uzunluk alanına bağımlılık, özel ayırıcıya bağımlılık veya fiziksel kodlamanın kullanılmayan sembollerinden yararlanma.

> [!important] Sınavda dikkat
> **Sınav sorusu:** Dört framing yöntemini ve her birinin avantaj/sınırlamasını tablo hâlinde açıklayabilmelisin.

### Slayt 23 — Karakter sayısı — 4/12

Çerçeve başlığındaki uzunluk/sayı alanı, çerçevenin kaç karakter/bayt içerdiğini belirtir. Alıcı sayaca göre sonraki sınırı bulur. **Dezavantaj:** Uzunluk alanı bozulursa alıcı yanlış sınırda aramaya devam edebilir; sonraki çerçeveler de yanlış yorumlanır. Modern uzunluk alanlarının tümü bu basit yöntemin korumasız hâli gibi düşünülmemelidir.

### Slayt 24 — Karakter sayısı örneği — 5/12

Şekilde uzunlukları 5, 5, 8 ve 8 olan dört çerçeve gösterilir. Bir uzunluk alanı hata nedeniyle farklı değere dönüşürse alıcı eksik veya fazla veri okuyabilir ve eşzamanlamayı kaybedebilir. Örnek: `5|ABCD` yerine `7|ABCD...` okunduğunda sonraki çerçevenin başı veri sanılabilir.

### Slayt 25 — Bayrak baytları — 6/12

Çerçeve başına/sonuna özel **FLAG** baytları konur. Alıcı senkronizasyon kaybetse bile sıradaki bayrağı arayarak yeniden sınır bulabilir. İki ardışık FLAG bazı protokollerde bir çerçevenin bitişini ve sonrakinin başlangıcını ifade edebilir. Bayrak değeri veride de geçebileceği için ek çözüm gerekir.

### Slayt 26 — Bayt doldurma şeması — 7/12

Şekilde `FLAG | veri | FLAG` çerçevesi ve **ESC (escape)** karakteriyle kaçış örnekleri yer alır. Yaygın soyut kural: yük içindeki `FLAG → ESC FLAG`, yük içindeki `ESC → ESC ESC` yapılır. Alıcı kaçışı kaldırır ve özgün veriyi geri kazanır. Kullanılan gerçek kaçış kuralı protokole göre değişebilir.

### Slayt 27 — Bayt doldurma neden gerekli? — 8/12

İkili veri içerisinde tesadüfen bayrak baytı oluşabilir. Kaçış olmadan alıcı bunu çerçeve sonu sanır. Gönderici, veri içindeki özel bayttan önce ESC ekleyerek **saydamlık (transparency)** sağlar. Örnek soyut yük `A FLAG ESC B` iken iletilen yük `A ESC FLAG ESC ESC B` olabilir.

### Slayt 28 — Bayt tabanlı çerçeveleme sınırlaması — 9/12

Dersin tarihsel sunumu, 8 bitlik karakter koduna yakın tasarımın rastgele uzunlukta bit dizilerinde elverişsiz olabileceğini vurgular. **Düzeltme:** Unicode tek başına “her zaman 16 bitlik karakter” değildir; UTF‑8/UTF‑16/UTF‑32 gibi farklı kodlamalar vardır. Modern byte-oriented protokoller ikili veri taşımaya yine uygundur; konu, bu klasik çerçeveleme yaklaşımının karakter birimine bağımlılığıdır.

### Slayt 29 — Bit doldurma — 10/12

Bit temelli çerçevelemede yaygın **bayrak dizisi `01111110` (8 bit)** kullanılır. Gönderici veri bölümünde art arda **beş adet `1`** gördüğünde otomatik **`0` ekler**. Alıcı veri içinde beş `1` sonrasına yerleştirilmiş `0` bitini çıkarır. **Kaynak slaytta bayrak `0111110` (7 bit) biçiminde yazılmış; standart HDLC bayrağı `01111110` olduğundan bu bir yazım hatasıdır.**

> [!important] Sınavda dikkat
> **Sınavda hata yapma:** “5 tane bir gelince **her zaman araya 0 konur**” kuralı yük bitleri içindir, bayrak ayırıcının içine 0 yerleştirilmez.

### Slayt 30 — Bit doldurma örneği — 11/12

Orijinal veri: `01111110` (yükte rastlanan örüntü). Bit doldurma: `011111010` (beş `1`in ardından `0` eklendi). Hat üstü gösterim: `FLAG | 011111010 | FLAG` (`FLAG=01111110`). Alıcı yükte beş `1`den sonraki doldurma `0`ını silerek `01111110` verisini geri elde eder. Sınırlar içindeki veriyi bayraktan ayıran temel mantık budur.

### Slayt 31 — Fiziksel kodlama ihlalleri — 12/12

Bazı fiziksel hat kodları veri için kullanılmayan sembol/işaret kombinasyonlarına sahiptir. Bunlar özel çerçeve sınır işareti olarak değerlendirilebilir. Slayttaki örnekte `1=high-low`, `0=low-high` ise `high-high` veya `low-low` normal veri için kullanılmayan durumlar olabilir. **Her kodlama tekniğinde böyle özel sembol bulunmaz.**


---

## Bölüm IV — Hata Kontrolü ve Akış Kontrolü (32–34)

### Slayt 32 — Hata kontrolü

Güvenilir çerçeve hizmeti sunan protokoller, alıcıdan geri bildirim alabilir. **ACK** başarılı alımı, **NAK** hata veya kabul edilmeme durumunu bildirebilir. Zaman aşımı sonrasında yeniden iletim yapılabilir; fakat ACK kaybı yüzünden aynı veri tekrar gelebilir. Bu nedenle sıra numaraları/yinelenen çerçeve yönetimi de önemli olabilir.

### Slayt 33 — Akış kontrolü — 1/2

Hızlı gönderici çerçeveleri, alıcı işleyebileceğinden hızlı üretirse **alıcı tamponu taşabilir**. Akış kontrolünün hedefi, göndericiyi alıcının kabul kapasitesine uydurmaktır. Tıkanıklık kontrolü ise ağdaki kuyruk ve darboğazlarla ilgilenir. Soruların farkını kişi/altyapı diye düşün: alıcı = flow, ağın içi = congestion.

### Slayt 34 — Akış kontrolü yöntemleri — 2/2

**Geri bildirim tabanlı (feedback-based):** alıcı göndermeye devam izni/pencere bilgisi verir. **Hız tabanlı (rate-based):** protokol belirli iletim sınırı tanımlar; alıcıdan sürekli geri bildirim gerekmez. Kaynak kitap klasik veri bağlantı katmanı bağlamında hız tabanlı yaklaşımın kullanılmadığını belirtir; bunu günümüz bütün ağ teknolojilerini kapsayan evrensel yasa gibi yorumlama.

> [!important] Sınavda dikkat
> **Düzeltme:** Kaynak slayt hız tabanlı kontrolün data link katmanında hiç kullanılmadığını söylüyor. Bu, tarihsel ders modeli hakkında bir ifadedir; tüm modern bağlantı protokolleri için evrensel yasak değildir.


---

## Bölüm V — Hata Tespiti ve Hata Düzeltme Kodları (35–50)

### Slayt 35 — Hata tespiti ve düzeltme — 1/5

Sunum tarihsel telefon altyapısını hatırlatır: dijital santraller ve santraller arası bağlantılar, analog bakır yerel döngülerden farklı hata koşullarına sahip olabilir. Hata stratejisi yalnızca protokole değil, fiziksel ortamın hata karakteristiğine de bağlıdır.

### Slayt 36 — Hata tespiti ve düzeltme — 2/5

Fiber, bakır ve kablosuz ortamların hata olasılıkları farklı olabilir. Radyo kanallarında girişim, sönümlenme ve hareketlilik nedeniyle hata olasılığı yükselir. Bu, her kablosuz bağlantının her fiber bağlantıdan her durumda kötü olduğu anlamına gelmez; **kanal koşulları ve hata dağılımı** belirleyicidir.

### Slayt 37 — Patlamalı hatalar — 3/5

Tek bit hatasında tek konum değişir. **Patlamalı hata (burst error)** bir aralık içinde birden çok bitin etkilenmesidir; aradaki her bitin bozulması gerekmez. Örneğin `00000000 → 01101000` gibi. Patlamalı hatalar birden fazla ilgili biti bozabileceği için düzeltmesi zorlaşabilir. **Interleaving** gibi teknikler hataları farklı kod sözcüklerine dağıtabilir.

### Slayt 38 — Düzeltme ve tespit kodları — 4/5

**Hata tespit kodu (error-detecting):** eklenen fazlalık, alıcının bozulmayı belirlemesini sağlar; çoğu yöntemde bozuk bitin hangisi olduğunu göstermez. **Hata düzeltme kodu (error-correcting):** yeterli fazlalık sayesinde alıcı bazı hata desenlerinde özgün veriyi tahmin edip düzeltebilir. Bunlar birbirinden farklı garantilere sahiptir.

### Slayt 39 — Hangi kanalda hangi strateji? — 5/5

Nadir hata görülen ortamda **tespit + yeniden iletim (ARQ)** düşük ek yük sağlayabilir. Hata yoğunluğu veya yeniden iletim maliyetinin yüksek olduğu ortamda **ileri hata düzeltme (FEC)** yararlı olabilir. En iyi yaklaşım gecikme, hata oranı, gecikme bütçesi ve ek yük şartlarına göre belirlenir; modern sistemlerde karma stratejiler kullanılır.

### Slayt 40 — Kod sözcüğü, veri ve denetim bitleri — 1/11

Bir kod sözcüğünde (codeword) **m veri biti**, **r denetim/fazlalık biti** bulunur; **n=m+r**. Örnek m=4, r=3 ise n=7. Kodun bütün `2^n` bit dizilerini geçerli olarak kullanmaması, hatalı dizileri algılama/düzeltme için boşluk sağlar.

> [!important] Sınavda dikkat
> **Notasyon:** `m` = bilgi, `r` = kontrol, `n` = toplam kod sözcüğü boyu. Bu harfleri karıştırma.

### Slayt 41 — XOR ile iki kod sözcüğünün farkı — 2/11

İki eşit uzunluklu kod sözcüğünü **XOR** yap; sonuçtaki `1` sayısı birbirlerinden farklı oldukları konum sayısıdır. Özgün örnek: `10001001 XOR 10110001 = 00111000`; üç `1` vardır, **Hamming uzaklığı 3**. XOR: aynı bit `0`, farklı bit `1` üretir.

### Slayt 42 — Hamming uzaklığının tanımı — 3/11

**Hamming uzaklığı** `d(x,y)`, iki eşit uzunluklu dizinin farklı bit konumları sayısıdır. Örnek `10110` ve `10011`: XOR `00101` → uzaklık **2**. Bir yasal kod sözcüğünü diğerine dönüştürmek için en az o kadar bitin değişmesi gerekir.

### Slayt 43 — Kodun minimum uzaklığı — 4/11

Bir kodun **minimum uzaklığı `d_min`**, bütün farklı geçerli kod sözcüğü çiftleri arasında en küçük Hamming uzaklığıdır. `m` veri bitine karşılık `2^m` mesaj bulunurken, yalnızca belirlenmiş `2^m` kod sözcüğü seçilebilir. **Minimum uzaklık** tek bir kod sözcüğü çiftinin değil, tüm kod ailesinin hata dayanıklılığını belirler.

### Slayt 44 — Hata tespit gücü — 5/11

Minimum uzaklık `d_min` olan kod, **en fazla `d_min−1` bit hatasını kesin olarak tespit edebilir**. Çünkü daha az bit değişimiyle bir geçerli kod sözcüğünden başka bir geçerli kod sözcüğüne geçilemez. `d_min=4` ise en fazla 3 bit hatası kesin tespit edilebilir. Bu, belirli hata konfigürasyonlarının daha çok hatayla da yakalanamayacağı anlamına gelmez.

> [!important] Sınavda dikkat
> **Kural:** `d_min ≥ e+1` ise en çok `e` bit hata kesin tespit edilir.

### Slayt 45 — Hata düzeltme gücü ve eşlik — 6/11

En fazla `t` hata düzeltmek için **`d_min ≥ 2t+1`** gerekir; dolayısıyla `t=floor((d_min−1)/2)`. **Eşlik (parity)** biti toplam `1`lerin sayısını çift veya tek yapar. Tek parity bitli klasik kodda `d_min=2` olup tüm tek-bit hatalarını tespit eder ama genel olarak düzeltmez.

### Slayt 46 — Çift/tek eşlik örneği — 7/11

Veri `1011010` içindeki `1` sayısı **4**tür. **Çift eşlik:** sona `0` → `10110100`; **tek eşlik:** sona `1` → `10110101`. Tek bir bit dönerse eşlik kontrolü bozulur. İki bit birlikte döndüğünde eşlik yine doğru görünebilir; parity her çift sayıda hatayı kesin tespit etmez.

### Slayt 47 — Uzaklığı 5 olan kod örneği — 8/11

Slayt dört geçerli 10-bit kod sözcüğü verir: `0000000000`, `0000011111`, `1111100000`, `1111111111`. Bunların minimum uzaklığı **5**tir. Bu kod en çok **2 hata düzeltebilir**; çünkü `floor((5−1)/2)=2`. Üç hata gerçekleşirse en yakın geçerli sözcük yanlış seçilebilir. İki hataya kadar hata düzeltme garantisi vardır.

### Slayt 48 — Tek bit hata düzeltme için sayım — 9/11

`m` veri ve `r` kontrol biti varsa `n=m+r`. Her geçerli sözcük çevresindeki `n` adet tek-bit bozulmuş sözcük + kendisi **n+1** desen kaplar. `2^m` yasal sözcüğün bu desenleri çakışmamalıdır. Bu nedenle **`2^m(n+1) ≤ 2^n`** koşulu elde edilir.

### Slayt 49 — Hamming denetim biti alt sınırı — 10/11

Önceki koşulda `n=m+r` yerine yaz: **`m+r+1 ≤ 2^r`**. Örneğin `m=4` için `r=3`: `4+3+1=8 ≤ 8` sağlanır; klasik (7,4) Hamming kodunun temel alt sınırıdır. `m=8` için `r=4`: `8+4+1=13 ≤ 16`; r=3 için `12 ≤ 8` yanlış, dolayısıyla en az 4 denetim biti gerekir.

> [!important] Sınavda dikkat
> **Ezber değil mantık:** `2^m` farklı yasal sözcüğün her biri için kendisi + `n` adet tek-bit hata deseni ayrılır; bu bölgeler çakışmaz.

### Slayt 50 — Patlamalı hata ve Hamming kodu — 11/11

Şekil, blok verisinin **bitler birbirine karıştırılarak (interleaving)** aktarılmasını gösterir. Art arda gelen birkaç hata, yeniden düzenleme sonrası farklı kod sözcüklerinde tek-bit hatalara dönüşebilir ve her sözcüğün tek-bit hata düzeltme kapasitesinden yararlanılır. Bu her uzunluktaki patlamayı otomatik düzeltmez; karıştırma derinliği ve kod yapısı önemlidir.


---

## Bölüm VI — Hata Tespit Kodları ve CRC (51–60)

### Slayt 51 — Hata tespit kodlarına giriş — 1/10

Kablosuz kanallarda FEC kullanımı yaygındır; düşük hata olasılıklı linklerde ise hata tespiti ve yeniden iletim ekonomik olabilir. FEC ile ARQ **alternatif veya birlikte kullanılan araçlardır**; uygulama ihtiyacına göre hibrit yöntemler de bulunur.

### Slayt 52 — Hata tespiti ek yükü örneği — 2/10

Sunumun varsayımsal kanalı: bit hata olasılığı `10^-6`, blok boyutu 1000 bit. Tek hata düzeltme için yaklaşık 10 kontrol biti/blok gerekiyorsa **1.000.000 veri biti için 1000 blok × 10 = 10.000 kontrol biti** gerekir. Yalnız tespit için bir parity biti/blok eklendiğinde **1.000 kontrol biti** yeterli başlangıç ek yüküdür. Bu örnek, blok başına hata yapısına bağlıdır.

### Slayt 53 — Tespit + yeniden iletim maliyeti — 3/10

1 milyon veri biti 1000 adet 1000 bitlik bloktur. Bit hata olasılığı `10^-6` ise blok başına en az bir hata olasılığı yaklaşık `1−(1−10^-6)^1000≈0,0009995`, dolayısıyla **yaklaşık 1 blok** yeniden gönderilir. Parity ek yükü: 1000 bit. Yeniden gönderilen blok: yaklaşık 1001 bit. Toplam: **yaklaşık 2001 bit**. FEC örneğinde 10.000 bit. Bu, *öğretici yaklaşık karşılaştırma*dır; ACK ve diğer protokol ek yüklerini içermez.

### Slayt 54 — CRC ve polinom gösterimi — 4/10

**Çevrimsel Artıklık Denetimi (Cyclic Redundancy Check — CRC)** yaygın hata tespit yöntemidir. Bit dizisi GF(2) polinomu olarak yazılır. `110001` → `x^5+x^4+1`. Soldaki bit en yüksek dereceli katsayıya, sağdaki bit sabit terime karşılık gelir. “Checksum” genel sözcüktür; CRC hesaplaması özel olarak polinom bölümüne dayanır.

### Slayt 55 — Mod 2 aritmetik ve XOR — 5/10

CRC işlemlerinde **mod 2 toplama ve çıkarma = XOR**; elde/ödünç yoktur. Bit çiftleri `0⊕0=0`, `0⊕1=1`, `1⊕0=1`, `1⊕1=0`. Örnek `1011 XOR 1100 = 0111`. Uzun bölme sırasında bölenle hizalı XOR uygulanır; klasik tamsayı bölmesi değildir.

### Slayt 56 — CRC üreteç polinomu — 6/10

Gönderici ve alıcı **aynı üreteç polinomu `G(x)`** üzerinde anlaşır. Derecesi `r` ise mesaja `r` sıfır eklenir, mod 2 bölümünün **`r` bitlik kalanı** kontrol alanı olarak yazılır. Gönderilen bit dizisi `T(x)`, `G(x)`e tam bölünecek şekilde seçilir. Alıcı yeniden böler; **sıfır olmayan kalan hata tespitidir**, sıfır kalan mutlak hatasızlık kanıtı değildir.

> [!important] Sınavda dikkat
> **CRC hatırlatma:** `r`, üreteç polinomunun derecesidir; örneğin `10011` beş bit fakat derecesi **4**tür.

### Slayt 57 — CRC algoritmasının üç adımı — 7/10

(1) `M` mesajının sonuna `r` adet 0 ekle → `M·2^r` (`x^r M(x)`). (2) Elde edilen diziyi `G` ile mod 2 böl, kalanı `R` bul. (3) Gönderilecek diziyi `T=M·2^r XOR R` olarak oluştur; işlem eşdeğer olarak `M | R` şeklindedir. Alıcı `T mod G = 0` kontrol eder.

### Slayt 58 — CRC uzun bölme örneği — 8/10

Kaynak örnek `M=1101011011`, üreteç `G(x)=x^4+x+1` → bit gösterimi **`10011`**, `r=4`. Önce `1101011011 0000` oluşturulur. Mod 2 bölme kalanı **`1110`** olur. Gönderilen kod sözcüğü **`T=11010110111110`**. Alıcı bu diziyi `10011` ile böldüğünde kalan `0000` bulmalıdır. Ayrıntılı adım sırası aşağıdaki çözümlü sorudadır.

> [!important] Sınavda dikkat
> **Slayt 58’in sonucu:** `1101011011 → 11010110111110` (`G=10011`). Bölme kalanı **1110**.

### Slayt 59 — Hata polinomu ve patlamalı hata — 9/10

İletimdeki bozulmayı **`E(x)` hata polinomu** ile temsil edebiliriz. Alıcı `T(x)+E(x)` alır; burada toplama XOR’dur. `E(x)` içindeki 1’ler bozulan bit konumlarını gösterir. Patlamalı hata, ilk ve son bozulmuş bit arasında kalan bir aralıkla ifade edilir; aradaki bitlerin hepsinin bozulması gerekmez.

### Slayt 60 — CRC hangi hataları yakalar? — 10/10

Gönderilen `T(x)` üretece tam bölünür. Alıcı hata sonrası `T(x)+E(x)`yi böler; kalan `E(x) mod G(x)`e eşittir. Eğer hata polinomu **`G(x)`in katı** ise kalan sıfır olur ve hata **tespit edilemeyebilir**; diğer durumlarda tespit edilir. Bu nedenle “CRC bütün hataları kesin yakalar” demek yanlıştır.


---

## Dört Çerçeveleme Tekniğinin Karşılaştırılması

| Teknik | Sınırın anlaşılması | Temel problem / çözüm |
|---|---|---|
| Karakter sayısı | Başlıktaki uzunluk alanı | Uzunluk bozulursa eşzamanlama kayabilir |
| Bayrak baytı + byte stuffing | Özel bayt ile sınır | Yükte FLAG ve ESC geçerse kaçış uygulanır |
| Bayrak bit dizisi + bit stuffing | `01111110` ile sınır | Veri içindeki beş ardışık `1` sonrasına `0` eklenir |
| Fiziksel kodlama ihlalleri | Kullanılmayan fiziksel sembol | Uygun fazlalığa sahip hat kodu gerekir |

## Hizmet, Hata ve Katman Ayrımları

| Karıştırılan kavramlar | Kesin fark |
|---|---|
| Frame / packet | Frame veri bağlantı, packet ağ katmanı veri birimidir |
| Bit stuffing / byte stuffing | İlki tek bit ekler; ikincisi özel bayttan önce ESC ekler |
| Error detection / correction | Tespit hata olduğunu anlar; düzeltme belirli hataları kendisi giderir |
| Flow / congestion | Flow alıcının kapasitesi, congestion ağ içindeki yük ve kuyruklarla ilgilidir |
| CRC / FEC | CRC genellikle tespit; FEC uygun kodla düzeltme sağlar |
| Link reliability / TCP reliability | Birincisi tek bağlantı, ikincisi uçtan uca taşıma katmanı hizmeti olabilir |
| ACK / şifreleme | ACK alındı onayıdır; gizlilik ya da kriptografik güvence değildir |
| Hamming distance / Hamming code | Birincisi ölçü; ikincisi ölçüden yararlanan kod ailesidir |

---

## Formül Föyü

### Kod sözcüğü ve Hamming uzaklığı

$$n=m+r$$

$$d_H(x,y)=\operatorname{wt}(x\oplus y)$$

Burada `wt`, bit dizisindeki `1` sayısıdır.

Bir kodun minimum uzaklığı `d_min` ise:

$$\text{Kesin tespit edilebilir en çok hata}=d_{min}-1$$

$$\text{Kesin düzeltilebilir en çok hata}=\left\lfloor\frac{d_{min}-1}{2}\right\rfloor$$

`m` bilgi bitli sözcükleri tek-bit hataya karşı düzeltecek kodun denetim bitleri için gerekli sayma koşulu:

$$2^m(m+r+1)\le 2^{m+r}$$

$$\boxed{m+r+1\le 2^r}$$

> [!warning] Gerekli koşul yeterli koşul değildir
> `m+r+1 ≤ 2^r` sayımı, gerekli alt sınırı verir. Uygun kod inşasının kendisi ayrıca gösterilmelidir.

### CRC polinomu

$$G(x)=x^4+x+1\quad\Longleftrightarrow\quad G=10011$$

$$T(x)=x^r M(x)+R(x)\quad(\text{GF}(2)\text{ üzerinde})$$

$$R(x)= x^r M(x)\bmod G(x)$$

Alıcı:

$$T'(x)\bmod G(x)\ne 0\Rightarrow\text{hata tespit edildi}$$

**Ters önerme geçersizdir:** `kalan = 0` her zaman veri hatasız demek değildir.

---

## Çözümlü Hesaplama ve Algoritma Örnekleri

### Örnek 1 — XOR ve Hamming uzaklığı

İki kod sözcüğü `10001001` ve `10110001` olsun.

```text
  10001001
⊕ 10110001
----------
  00111000
```

Sonuçta üç adet `1` olduğundan **Hamming uzaklığı = 3**.

### Örnek 2 — Hata tespit/düzeltme kapasitesi

Minimum uzaklığı `d_min=5` olan kod:

- En fazla **4** bit hatasını kesin **tespit** eder.
- En fazla `floor((5−1)/2)=2` bit hatasını kesin **düzeltir**.

`d_min=2` olan tek parity kodu ise 1 hatayı tespit eder fakat garantili hata düzeltme yapmaz.

### Örnek 3 — En az kaç denetim biti?

`m=8` mesaj biti için tek-bit düzeltme hedefleniyor.

- `r=3` için `8+3+1=12`, `2^3=8` → koşul sağlanmıyor.
- `r=4` için `8+4+1=13`, `2^4=16` → gerekli koşul sağlanıyor.

**En az `r=4` denetim biti** gerekir.

### Örnek 4 — Çift ve tek parity

`1011010` dizisinde dört adet `1` bulunur:

- Çift parity: `1011010` + `0` = **`10110100`**
- Tek parity: `1011010` + `1` = **`10110101`**

İki bit aynı anda ters döndüğünde eşlik kontrolünün hatayı yakalayamayabileceğini unutma.

### Örnek 5 — Byte stuffing

Soyut protokol: `FLAG=F`, `ESC=E` ve gönderici veri içindeki her özel sembolü `E` ile kaçırıyor olsun.

```text
Orijinal veri:     A F B E C
Hat üzerindeki:    A E F B E E C
Alıcı destuffing:  A F B E C
```

Gerçek bayt değerleri ve kaçış kuralı protokole bağlıdır. Bu şema **saydamlık** mantığını gösterir.

### Örnek 6 — Bit stuffing

Beş ardışık `1`den sonra veri bölümüne bir `0` ekle:

```text
Veri (önce):       01111110
Veri (stuffed):    011111010
Çerçeve:          01111110 | 011111010 | 01111110
Alıcının verisi:   01111110
```

Orijinal 8-bit bayrak, veri alanındaki doldurma sayesinde sınırla karışmaz.

### Örnek 7 — Kaynak slayttaki CRC hesabı

- Mesaj `M = 1101011011`
- Üreteç `G = 10011` (`G(x)=x^4+x+1`)
- Derece `r = 4`
- Sıfır eklenmiş mesaj `11010110110000`
- Mod 2 bölme kalanı `R = 1110`
- Gönderilecek sözcük `T = 11010110111110`

```text
M       1101011011
M·x^4   11010110110000
G       10011
R       1110
T       11010110111110
```

Doğrulama: `T mod 10011 = 0000`. Dikkat: Bit dizisindeki tüm işlemler **XOR temellidir**, tamsayı çıkarma değildir.

### Örnek 8 — Alternatif küçük CRC hesabı

`M=1101`, `G=1011` (`r=3`) olsun. Başlangıç `1101000`; mod 2 bölüm kalanı `001`, gönderilen `1101001` olur. Kontrol: `1101001 mod 1011 = 000`.

### Örnek 9 — 1 Mbit için FEC vs. yeniden iletim

Sunumdaki varsayımlarla:

- 1.000.000 veri biti, 1.000 bitlik 1.000 blok demektir.
- Blok başına 10 kontrol bitli düzeltme kodu → **10.000** ek bit.
- Blok başına 1 parity biti → **1.000** ek bit.
- Ortalama yaklaşık bir 1001-bitlik blok yeniden iletilirse → yaklaşık **2.001** bit toplam ek yük.

**Yorum:** Hesap özel varsayıma bağlıdır; yüksek hata oranlarında yeniden iletim verimsizleşebilir.

### Örnek 10 — Yeniden iletimin sınırı

`ACK` kaybolursa gönderen aynı çerçeveyi tekrar iletebilir. Alıcı **sıra numarası** tutmuyorsa aynı veriyi iki kez üst katmana aktarabilir. Dolayısıyla “ACK kullandık, tamamen güvenilir olduk” demek tek başına yeterli değildir.

---

## Sınavda Sık Karıştırılan Noktalar

> [!danger] Kritik teknik yanlışlar
> - “Her veri bağlantı protokolü hatasız teslimat garantisi verir” **yanlış**; hizmet türüne bağlıdır.
> - “Bit stuffing bayrağı 0111110” **kaynak slaytında yazım hatasıdır**; yaygın HDLC flag `01111110`.
> - “CRC bütün hata türlerini mutlaka yakalar” **yanlış**; `E(x)` üretecin katıysa hata kaçabilir.
> - “Parity ile hangi bitin bozulduğunu bulurum” **yanlış**; tek parity biti tipik olarak sadece hata tespit eder.
> - “Hamming uzaklığı 5 ise 5 hata düzeltilir” **yanlış**; 2 hata düzeltilir, 4 hata tespit edilir.
> - “ACK güvenlik/şifreleme sağlar” **yanlış**; ACK alındı bilgisidir.
> - “Frame ile IP packet aynı PDU’dur” **yanlış**.

---

## Yüksek Lisans Vize Denemesi — Week 5

**Toplam: 100 puan · Önerilen süre: 90 dakika.** Soruları önce cevap anahtarına bakmadan çöz.

### Bölüm A — Kısa kavram soruları (5 × 4 = 20 puan)

1. **(4 puan)** Data link katmanının ağ katmanına sunduğu dört temel tasarım işlevini açıklayınız.
2. **(4 puan)** Onaysız bağlantısız, onaylı bağlantısız ve onaylı bağlantı odaklı hizmetlerin farklarını belirtiniz.
3. **(4 puan)** Frame ile packet arasındaki farkı, bir router üzerinden aktarım örneğiyle açıklayınız.
4. **(4 puan)** Akış kontrolü ve tıkanıklık kontrolünün farklı amaçlarını açıklayınız.
5. **(4 puan)** Hata tespit ve hata düzeltme kodlarının işlev farkını yazınız.

### Bölüm B — Mekanizma ve karşılaştırma (4 × 10 = 40 puan)

6. **(10 puan)** Dört çerçeveleme yöntemini açıklayınız. Karakter sayısının bozulması neden ciddi senkronizasyon hatası yaratabilir?
7. **(10 puan)** Bayt doldurma ve bit doldurma tekniklerini örnekle karşılaştırınız. `01111110` bayrağının veri ile karışması nasıl önlenir?
8. **(10 puan)** Hamming uzaklığını tanımlayınız. `d_min=5` için tespit ve düzeltme sınırını türetiniz. Tek parity bitinin özelliklerini açıklayınız.
9. **(10 puan)** CRC'de XOR'un rolünü, üreteç polinomunun nasıl kullanıldığını ve bazı hataların neden tespit edilemeyebildiğini açıklayınız.

### Bölüm C — Sayısal ve analitik sorular (4 × 10 = 40 puan)

10. **(10 puan)** `10001001` ve `10110001` kod sözcüklerinin Hamming uzaklığını XOR ile bulunuz.
11. **(10 puan)** 8 bilgi biti için tek hata düzeltmeye yönelik en az kaç denetim biti gerektiğini `m+r+1 ≤ 2^r` ile gösteriniz.
12. **(10 puan)** `M=1101011011`, `G=10011` için CRC kalanı ve gönderilecek kod sözcüğünü bulunuz. Neden dört sıfır eklediğinizi açıklayınız.
13. **(10 puan)** 1 Mbit verinin 1000-bit bloklarla iletildiği ve bit hata oranının `10^-6` olduğu varsayımsal örnekte, blok başına 10 denetim bitli FEC ile blok başına 1 parity bitli tespit + ortalama bir yeniden iletimin ek yükünü karşılaştırınız.

### Cevap anahtarı — Kısa kontrol

| Soru | Beklenen cevap / puanlama anahtarı |
|---|---|
| 1 | Üst katmana hizmet, framing, error control, flow control; her biri 1 puan |
| 2 | Bağlantı kurulması ve ACK bulunması eksenlerini doğru ayırma; üç hizmeti açıklama |
| 3 | Frame katman 2, packet katman 3; komşu linkte yeni framing uygulanabilir |
| 4 | Alıcı kapasitesi vs. ağdaki kuyruk/darboğaz; örnekle açıklama |
| 5 | Detection yalnız hata olduğunu anlar, correction belirli hatayı onarır; redundans |
| 6 | Uzunluk, byte stuffing, bit stuffing, kodlama ihlali; yanlış uzunluk senkronizasyonu bozabilir |
| 7 | `FLAG`, `ESC`; beş `1` sonrası `0`; alıcı destuff eder; örnekler |
| 8 | `d_H=wt(XOR)`, `d_min=5` için tespit 4 ve düzeltme 2; parity `d_min=2` |
| 9 | GF(2) bölüm, XOR, sıfır olmayan kalan; `E(x)` üreteç katıysa hata kaçabilir |
| 10 | `00111000`, Hamming uzaklığı **3** |
| 11 | `r=3` başarısız, `r=4` başarılı → **4 denetim biti** |
| 12 | `r=4`, kalan **1110**, sonuç **11010110111110** |
| 13 | FEC **10.000 bit**; tespit+yeniden iletim yaklaşık **2.001 bit** (varsayımlar belirtilmeli) |

---

## Çalışma Kontrol Listesi

- [ ] Çerçevenin neden gerektiğini ve dört temel tasarım işlevini açıklayabiliyorum.
- [ ] Üç veri bağlantı hizmetini doğru karşılaştırabiliyorum.
- [ ] Frame–packet ve link–end-to-end farkını biliyorum.
- [ ] Dört framing yönteminin avantaj ve zayıflıklarını sayabiliyorum.
- [ ] FLAG/ESC byte stuffing örneği kurabiliyorum.
- [ ] HDLC bayrağı `01111110` ve beş `1` sonrası `0` kuralını uygulayabiliyorum.
- [ ] ACK/NAK, flow ve congestion kavramlarını ayırabiliyorum.
- [ ] XOR ile Hamming uzaklığını hesaplıyorum.
- [ ] `d_min−1` tespit ve `floor((d_min−1)/2)` düzeltme formüllerini kullanıyorum.
- [ ] `m+r+1 ≤ 2^r` denetim biti sınırını türetebiliyorum.
- [ ] Verilen veriye even/odd parity biti ekleyebiliyorum.
- [ ] CRC bit dizisini polinom olarak gösterebiliyorum.
- [ ] CRC uzun bölmeyi XOR ile yapabiliyorum.
- [ ] CRC'nin sıfır kalanı neden mutlak doğruluk garantisi olmadığını açıklayabiliyorum.
- [ ] Week 5 deneme sınavında en az 80/100 hedefliyorum.

---

## İlgili Obsidian Notları

- [[Bilgisayar_Aglari_Week_4_Obsidian_Tum_Ders_Notlari|Week 4 — Fiziksel katman / önceki konu]]
- [[Bilgisayar_Aglari_Week_3_Obsidian_Tum_Ders_Notlari|Week 3 — İletim ortamları]]
- [[Bilgisayar_Aglari_Week_2_Obsidian_Tum_Ders_Notlari|Week 2 — OSI ve TCP/IP temel kavramlar]]

**Asıl kaynak:** [Ankara Üniversitesi — Week_5.pdf](https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_5.pdf)
