---
title: "Bilgisayar Ağları ve Veri İletişimi — Week 6 — Tüm Ders Notları"
aliases:
  - "Week 6 Veri Bağlantı Katmanı Devamı"
  - "Week 6 Sliding Window HDLC PPP"
tags:
  - bilgisayar-aglari
  - yuksek-lisans
  - week-6
  - veri-baglanti-katmani
  - stop-and-wait
  - sliding-window
  - go-back-n
  - selective-repeat
  - hdlc
  - ppp
  - vize
  - obsidian
course: "Bilgisayar Ağları ve Veri İletişimi"
week: 6
slide_count: 81
language: tr
source: "https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_6.pdf"
---

# Week 6 — Veri Bağlantı Katmanı (Devam): ARQ, Kayar Pencere, Doğrulama, HDLC ve PPP

> [!info] Kaynak ve kapsam
> Ankara Üniversitesi, **Dr. Refik Samet — Week_6.pdf**, Andrew S. Tanenbaum, *Computer Networks*, 4. baskı, **Bölüm 3, s. 211–246**. Kaynak sunum **81 slayt** içerir. Bu notlarda **81 slaydın hepsi aynı sırayla**, Türkçe anlatılır; İngilizce teknik terimler gerektiğinde parantez içinde verilir. Anlatımlar slaytlardaki kavramların açıklanmış ve gerektiğinde düzeltilmiş hâlidir; slaytların sözcüğü sözcüğüne çevirisi değildir.

> [!warning] Teknik ve tarihsel ayrımlar
> Slayttaki *simplex* protokollerde **veri tek yönde**, ACK/denetim iletileri ters yönde akabilir. **Stop-and-Wait protokol 2**, hatasız kanalda akış denetimini öğretir; **protokol 3**, kayıp/bozulma ve tekrarları ele alır. **PPP**, kendi başına TCP gibi güvenilir yeniden iletim sağlamaz; FCS ile **hata tespiti** yapar. Sunumdaki çevirmeli modem bağlantısı anlatımları tarihsel örneklerdir.

> [!tip] Çalışma rotası
> Önce ACK kaybını çöz → 1 bitlik sıra numarasını anla → gönderme/alma penceresini çiz → **Go-Back-N ve Selective Repeat** farkını elle simüle et → son olarak FSM/Petri, HDLC ve PPP çerçevelerini öğren.

## İçindekiler
- [[#Bölüm I — Temel Veri Bağlantı Protokolleri İçin Varsayımlar (1–15)|1–15: Varsayımlar]]
- [[#Bölüm II — Protokol 1, 2 ve 3 (16–28)|16–28: Üç temel protokol]]
- [[#Bölüm III — Kayar Pencere ve Piggybacking (29–45)|29–45: Sliding window]]
- [[#Bölüm IV — Protokol 4, 5 ve 6 (46–59)|46–59: One-bit, GBN, SR]]
- [[#Bölüm V — Protokol Doğrulama (60–69)|60–69: FSM ve Petri]]
- [[#Bölüm VI — HDLC ve PPP (70–81)|70–81: HDLC/PPP]]
- [[#Kritik Kavram Karşılaştırmaları|Kavram tabloları]]
- [[#Formül Föyü|Formüller]]
- [[#Çözümlü Sorular|Çözümlü sorular]]
- [[#Week 6 Yüksek Lisans Vize Denemesi — 100 Puan|Vize denemesi]]
- [[#Deneme Cevap Anahtarı ve Puanlama|Cevap anahtarı]]
- [[#Obsidian Aktif Hatırlama Kartları|Hatırlama kartları]]
- [[#Çalışma Kontrol Listesi|Kontrol listesi]]


---

## Bölüm I — Temel Veri Bağlantı Protokolleri İçin Varsayımlar (1–15)

### Temel fikir

Bu bölümde “çerçeveyi gönderdim” ile “üst katmana **yalnız bir kez ve doğru sırada teslim ettim**” arasındaki fark öğrenilir. ACK kaybı, zaman aşımı ve kopya ayıklama temel senaryolardır.

### Slayt 1 — Veri bağlantı katmanına devam

Bu haftanın odak noktası hata tespit kodları değil, **hata sonrası iletim davranışı** ve protokolün doğru çalışmasıdır. Temel protokoller, kayar pencere, biçimsel doğrulama, HDLC ve PPP ele alınır.

### Slayt 2 — Üç temel protokol ve 1. varsayım

Fiziksel katman, veri bağlantı katmanı ve ağ katmanı **mesaj aktararak haberleşen bağımsız süreçler** gibi modellenir. Amaç gerçek işletim sistemi yerleşimini zorunlu kılmak değil, sorumlulukları ayırmaktır.

### Slayt 3 — Katman süreçlerinin farklı yerleşimleri

Fiziksel ve veri bağlantı katmanları bir ağ arayüzü işlemcisinde, ağ katmanı ana CPU’da çalışabilir; başka uygulamalar da mümkündür. Mantıksal katman ayrımı donanım bileşenlerinin mutlaka ayrı olması demek değildir.

### Slayt 4 — 2. varsayım: A’dan B’ye sürekli veri

A makinesi B’ye uzun bir veri akışı göndermek ister ve ağ katmanında her zaman gönderilecek paket vardır. Buna **doygun kaynak (saturated source)** varsayımı denebilir. İlk protokollerde veri akışı tek yönlüdür.

### Slayt 5 — 3. varsayım: makineler çökmüyor

Başlangıç algoritmaları cihazların çöküp yeniden başlamasını ele almaz. Ağ katmanından veri bağlantıya verilen paket **yük olarak** işlenir; üst katmanın iç başlığı veri bağlantı için içerik olarak kabul edilir.

### Slayt 6 — Paketin çerçeveye kapsüllenmesi

Veri bağlantı katmanı ağ katmanı paketine **başlık ve uygun son ek** ekleyerek çerçeve (frame) oluşturur. Başlıkta sıralama/ACK bilgileri, sonda hata tespit bilgisi bulunabilir. `frame = header + packet + trailer`.

### Slayt 7 — Fiziksel katman prosedürleri

`to_physical_layer` çerçeve gönderir, `from_physical_layer` çerçeve alır. Örnek modelde FCS/checksum oluşturma donanımla yapılabilir. Her protokolde bu işlemlerin fiziksel olarak özel bir işlemci tarafından yapılması şart değildir.

### Slayt 8 — Olay bekleme yordamı

`wait_for_event(&event)` ile alıcı/gönderici **olay güdümlü** davranır: `frame_arrival` sağlam çerçeve, `cksum_err` hata tespit edilen çerçeve, `timeout` zaman aşımı gibi. Böylece sürekli döngü yerine olaylara göre durum değişir.

### Slayt 9 — Temel veri türleri

`boolean` doğru/yanlış, `seq_nr` çerçeveyi ayırmak için sıra numarası, `packet` üst katmanla alışveriş edilen veri birimidir. `MAX_SEQ`, kullanılabilecek en büyük sıra numarasını tanımlar.

### Slayt 10 — Çerçeve başlığı alanları

Örnek `frame` yapısında `kind` (veri/kontrol), `seq` (gönderilenin numarası), `ack` (onay bilgisi) ve `info` (payload) bulunur. Bu alanlar **eş zamanlı olarak her fiziksel protokolde aynı boyuttadır** şeklinde yorumlanmamalıdır.

> [!example] Slayt 10 için örnek / şekil okuma
> **Örnek çerçeve:** `kind=data, seq=0, ack=1, info=paketA`. `seq` gönderilen çerçeve numarası; `ack` karşı yönden beklenen/onanmış bilgi için kullanılır. Sözde kodlarda ACK alanının tam anlamını protokol tanımından kontrol et.

### Slayt 11 — Protokol bildirimleri — protocol.h

C-benzeri sözde koddaki ortak yapı ve yordamların ilk görünümü. `from_network_layer`, `to_network_layer`, `to_physical_layer`, `wait_for_event` ve sıra numarası tipleri farklı protokollerin ortak arayüzünü tanımlar.

### Slayt 12 — Protokol bildirimlerinin devamı

Tanımların tekrarlanmasının amacı ilerideki protokol 1–6 kodunu **aynı olay modeli** içinde karşılaştırabilmektir. Alıcıya teslim, göndericiden yeni paket çekme ve zamanlayıcı başlatma ayrı işlemlerdir.

### Slayt 13 — Güvenilmez kanal varsayımı

Kanal zaman zaman çerçeveyi tamamen kaybedebilir. Gönderici **zamanlayıcı (timer)** başlatır; zamanında uygun geri dönüş gelmezse zaman aşımı olayı oluşturulur. Zaman aşımı, kaybın kesin kanıtı değil bir belirtidir.

### Slayt 14 — Zamanlayıcı işlemleri

`start_timer`, `stop_timer` veri çerçevelerinin yeniden iletimini yönetir; `start_ack_timer`, `stop_ack_timer` geciktirilen ACK için kullanılabilir. **ACK zamanlayıcısı** ve **yeniden iletim zamanlayıcısı** aynı görevde değildir.

### Slayt 15 — Üç protokolün listesi

Protokol 1: **sınırsız simplex** (hatasız, alıcı anında hazır). Protokol 2: **simplex Stop-and-Wait** (hatasız, alıcı hızı sınırlı). Protokol 3: **gürültülü kanal simplex** (kayıp/bozulma, ACK, timeout, sıra numarası).


---

## Bölüm II — Protokol 1, 2 ve 3 (16–28)

> [!important] Protokol 1/2/3 sınav ayrımı
> **P1:** Hatasız, sınırsız hız/tampon, ACK gereksiz. **P2:** Hatasız ama yavaş alıcı; Stop-and-Wait akış kontrolü. **P3:** Gürültülü kanal; ACK + zamanlayıcı + 0/1 sıra numarasıyla güvenilirlik.

### Slayt 16 — Protokol 1 — sınırsız simplex

Veri yalnız A→B gider; her iki uç hazırdır, işleme süresi yok sayılır, tampon sınırsızdır ve kanal çerçeve kaybetmez. Bu koşullar gerçekçi olmadığından kitap bunu **ütopya protokolü** olarak sunar.

### Slayt 17 — Protokol 1 — sözde kodun işleyişi

Gönderici `from_network_layer` ile paketi alır, `to_physical_layer` ile yollar ve hemen devam eder. Alıcı `wait_for_event(frame_arrival)` sonrası veriyi ağ katmanına verir. **ACK, sıra numarası ve akış kontrolü yoktur.**

### Slayt 18 — Protokol 2 — Stop-and-Wait varsayımları

Alıcı artık sınırsız hızda işleyemez; kanal hâlâ hatasız varsayılır. Göndericinin alıcıyı aşırı yüklemesini önlemek için **bir çerçeve gönder, onayı bekle** yaklaşımı benimsenir.

### Slayt 19 — Protokol 2 — basit akış denetimi

Gönderici bir veri çerçevesi yollar; alıcı işleyip sonraki çerçeveye hazır olduğunda geri bildirim verir. Gönderici bu işaretten önce yeni veri göndermez. ACK burada esasen **akış kontrolü** için kullanılır; kanal hata yapmıyor varsayılır.

### Slayt 20 — Protokol 2 — neden beklemek gerekir?

Alıcının `from_physical_layer + to_network_layer` işlemleri Δt sürüyorsa gönderici sınırsız hızla gönderemez. **Stop-and-Wait**, alıcı yeni çerçeveye hazır olmadan üzerine yazılmasını engeller. Verimlilik bekleme nedeniyle düşük olabilir.

> [!example] Slayt 20 için örnek / şekil okuma
> **Zaman çizelgesi:** `A: DATA0 → B; B: kabul et, ACK ← A; A: DATA1 → B`. Burada bekleme alıcının korunması içindir; hata kurtarma henüz eklenmedi.

### Slayt 21 — Protokol 3 — gürültülü kanal

Artık veri çerçeveleri bozulabilir veya kaybolabilir. Kontrol toplamı/FCS bozulmayı yüksek olasılıkla yakalar; sonlu hata tespit kodlarında **tüm olası hataları tespit etme garantisi yoktur**.

### Slayt 22 — Zaman aşımı ve yeniden iletim

Gönderici çerçeveyi yollar, alıcı doğruysa ACK gönderir. Çerçeve bozulursa atılır; gönderici ACK gelmeyince zaman aşımından sonra tekrar yollar. Bu **ARQ** mantığının başlangıcıdır.

### Slayt 23 — Salt yeniden iletimin ölümcül açığı

ACK’nin kaybolması hâlinde veri zaten kabul edilmiş olsa bile gönderici tekrar yollar. Alıcı tekrar gelen veriyi yeni sanırsa **aynı paket ağ katmanına iki kez teslim edilir**. Çözüm için kopyayı ayırma gerekir.

### Slayt 24 — ACK kaybı senaryosu

1) Veri B’ye doğru ulaşır. 2) B ağ katmanına teslim eder ve ACK yollar. 3) ACK kaybolur. 4) A timeout sonrası aynı veriyi tekrar gönderir. **ACK kaybı = veri kaybı değildir**.

### Slayt 25 — Sıra numarası ile kopyayı ayıklama

Alıcı, çerçeve başlığındaki **seq** değerini izler. Aynı sıra numarası beklenmeyen şekilde tekrar gelirse veriyi ağ katmanına tekrar teslim etmez; uygun onayla göndericinin ilerlemesine yardım edebilir.

> [!example] Slayt 25 için örnek / şekil okuma
> **ACK kaybı çizelgesi:** `A --DATA(0)--> B`, `B --ACK(0)-X`, `A --timeout-->`, `A --DATA(0) tekrar--> B`. B ikinci DATA(0)’ı **yeniden ağ katmanına teslim etmez**. ACK kaybını veri kaybıyla karıştırma.

### Slayt 26 — PAR/ARQ şeması

Olumlu onay + yeniden iletim (Positive Acknowledgement with Retransmission) kullanılan algoritmada gönderici **ACK gelene kadar** aynı çerçeve için sorumluluk taşır. Şekilde doğru onay, veri kaybı ve tekrar durumları görülür.

### Slayt 27 — Protokol 3 — şema devamı

Zaman aşımı, veri çerçevesi ya da ACK kaybında yeniden iletimi tetikler. **Sıra numarası** alıcıdaki yinelemeleri filtreler. Böylece bir çerçeve birden fazla kez fiziksel iletilse de üst katmana bir kez teslim edilebilir.

### Slayt 28 — Protokol 3 — ARQ tanımı

PAR ve ARQ, olumlu onay ve gerektiğinde yeniden gönderme mantığıdır. Veri **simplex** akarken ACK geri döner. İki uç sıra numarasına ilişkin durumu bekleme süreleri boyunca hafızada tutar.


---

## Bölüm III — Kayar Pencere ve Piggybacking (29–45)

> [!important] Kritik kavram
> **Gönderme penceresi**, onay beklerken birden fazla çerçeve göndermeyi sağlar. **Piggybacking**, karşı yönde giden veri çerçevesinin içine ACK koyar. Aynı şey değildir.

### Slayt 29 — Kayar pencereye giriş

İlk protokollerde veri tek yönde gider; pratikte iki yönlü veri aktarımı gerekir. **Kayar pencere protokolleri**, sıralama, onay ve birden fazla çerçevenin hatta bulunmasını aynı çerçevede ele alır.

### Slayt 30 — Çift kanal yaklaşımı

Tam çift yönlü veri için iki bağımsız simplex fiziksel devre kullanılabilir; ileri veri ve ters onaylar taşınır. Ancak ayrı kanalların kapasitelerinin kullanım oranı yetersiz kalabilir.

### Slayt 31 — Ayrı ters kanalın verimsizliği

Yalnız ACK taşıyan ters yönün kapasitesinin önemli kısmı boşa gidebilir. Bu, hem veri hem kontrol trafiğini aynı çift yönlü iletim mekanizmasında düzenleme fikrini güçlendirir.

### Slayt 32 — Aynı devrede iki yönlü veri

Alternatif olarak aynı çift yönlü bağlantıda A↔B veri iletimi yapılır. Kontrol çerçeveleri veri trafiğiyle karışık iletilebilir; çerçevenin türü başlıktaki alanla belirlenir.

### Slayt 33 — Veri ve ACK çerçevelerinin ayırt edilmesi

İki yönlü hatta veri ve salt onay çerçeveleri birlikte bulunabilir. **kind** alanı, gelen çerçevenin payload taşıyıp taşımadığını gösterir; bir ACK ayrı bir çerçevede de taşınabilir.

### Slayt 34 — Piggybacking — geciktirme fikri

Alıcı ACK’yi hemen ayrı çerçevede göndermek yerine yakın zamanda karşı yöne veri gönderecekse **kısa süre** bekleyebilir. Amaç ACK’yi karşı yöne giden veri çerçevesinin başlığına eklemektir.

### Slayt 35 — Piggybacking — ACK alanı

ACK, geri dönüşte giden bir **veri çerçevesinin ack alanına** eklenir. Ayrı onay çerçevesi gönderme maliyetini azaltır; tek çerçevede veri + onay taşınır.

> [!example] Slayt 35 için örnek / şekil okuma
> ```mermaid
> sequenceDiagram
>  participant A
>  participant B
>  A->>B: DATA(seq=0)
>  B->>A: DATA(seq=5, ACK=0)
>  Note over A,B: B kendi verisiyle A için ACK taşır
> ```

### Slayt 36 — Piggybacking — bant genişliği avantajı

Ayrı ACK çerçevesi kendi başlığı ve FCS’sini gerektirirken gömülü ACK birkaç bitlik alanla taşınabilir. Daha az kontrol çerçevesi, daha az kesme ve tampon kullanımı anlamına gelebilir.

### Slayt 37 — Piggybacking — ne kadar beklenir?

ACK eklemek için veri beklemek verimi artırabilir; ancak bekleme fazla uzarsa gönderici timeout olur. **ACK gecikme sınırı**, yeniden iletim zaman aşımından uygun biçimde kısa seçilmelidir.

### Slayt 38 — Piggybacking — fazla gecikmenin riski

Alıcı geri yöndeki verinin geleceğini önceden bilemez. Onay zamanında çıkmazsa kaynak yeniden iletim yapar; ACK’yi çerçeveye bindirme kazancı ortadan kalkabilir.

### Slayt 39 — Piggybacking — ACK zamanlayıcısı

Kısa süre içinde karşı yön verisi gelirse ACK bindirilir; gelmezse **bağımsız ACK** gönderilir. `ack_timer`, bu esnekliği mümkün kılar.

### Slayt 40 — Üç çift yönlü kayar pencere protokolü

Kayar pencere ailesi güvenilirliği farklı maliyetlerle sağlar. Protokol 4, 5 ve 6’nın karşılaştırması verim, alıcı tamponu ve yeniden iletim miktarı üzerinden yapılır.

### Slayt 41 — Protokol 4/5/6 listesi

Protokol 4: **One-Bit Sliding Window**. Protokol 5: **Go-Back-N**. Protokol 6: **Selective Repeat**. Üçünü aynı anda **protokol adı / alıcı pencere / yeniden gönderme** açısından öğren.

### Slayt 42 — Sıra numarası bit sayısı

n bitlik sıra numarası alanı `0..2^n−1` aralığını kapsar. Örneğin n=3 için 0–7 kullanılır. Stop-and-Wait’in bir bitlik sıra numaraları **0 ve 1** şeklindedir.

> [!example] Slayt 42 için örnek / şekil okuma
> **Hızlı hesap:** `n=3` bit → sıra numaraları `0,1,2,3,4,5,6,7`; ardından tekrar `0` gelir (modulo 8). 3 bit **8 farklı değer** kodlar; bunların tümünün aynı anda gönderme penceresinde bulunması her ARQ türünde güvenli değildir.

### Slayt 43 — Gönderme ve alma pencereleri

Gönderme penceresi, **henüz izinli** ve/veya onay bekleyen çerçeve numaralarını temsil eder. Alma penceresi alıcının kabul edebileceği numaraları gösterir. Pencere ilerledikçe yeni sıra numaraları kullanılabilir.

### Slayt 44 — Pencere boyutları farklı olabilir

Gönderici/alıcı pencere sınırları veya boyutları aynı olmak zorunda değildir. Protokolün izin verdiği şekilde sabit ya da değişken olabilir; bunu belirleyen hata işleme ve tampon stratejisidir.

### Slayt 45 — Pencere boyutu 1’in dört aşaması

Şekil 3-bit sıra alanına rağmen pencere boyutu 1 olan örneği gösterir: (a) başlangıç, (b) ilk gönderim, (c) alıcıya ulaşma, (d) ACK sonrası pencerenin kayması. **Sıra alanı büyüklüğü ≠ pencere boyutu**.

> [!example] Slayt 45 için örnek / şekil okuma
> **Pencerenin ilerleyişi:** Başlangıç `[0]` gönderme hakkı → `DATA(0)` yola çıktı → B `DATA(0)` kabul etti → A `ACK(0)` alınca gönderme penceresi `[1]` olur. 3 bitlik sıra uzayı kullanılması burada **W=1** olmasını değiştirmez.


---

## Bölüm IV — Protokol 4, 5 ve 6 (46–59)

> [!important] GBN–SR farkı
> **Go-Back-N:** Alıcı yalnız sıradakini alır, kayıptan sonra birkaç çerçeve yeniden gönderilebilir. **Selective Repeat:** Alıcı sıra dışı çerçeveleri tamponlar, yalnız eksikler yeniden gönderilir.

### Slayt 46 — Protokol 4 — one-bit sliding window

İki yönlü veri ve onay taşınabilir; sıra numarası 0/1’dir. Tek çerçevelik pencere, veri ve ACK’yi aynı çerçevede taşıyabilme mantığını gösterir.

### Slayt 47 — Protokol 4 — akış ve numara mantığı

Her uç kendi göndereceği sıra numarasını ve karşıdan beklediği numarayı takip eder. ACK ve veri eş zamanlı iki yönden geldiğinde bile sıra durumlarının karıştırılmaması gerekir.

### Slayt 48 — Protokol 4 — normal ve anormal senaryo

Şekilde `(seq, ack, paket numarası)` gösterimiyle normal akış ve eş zamanlı başlangıç gibi istisnai durumlar karşılaştırılır. Yıldız, ağ katmanına kabul edilen paketi gösterir; protokol tekrar teslimi engellemelidir.

### Slayt 49 — Protokol 5 — Go-Back-N’e giriş

**Pipelining (boru hattı)** göndericinin birden fazla çerçeveyi ACK beklemeden göndermesidir. **GBN alıcı penceresi 1** olduğundan beklenmeyen sıra numaralı sonraki çerçeveler kabul edilmez; eksik olandan itibaren yeniden gönderim gerekir.

> [!example] Slayt 49 için örnek / şekil okuma
> **Senaryo:** 0,1,2,3 gönderildi; 1 kayboldu. GBN alıcısı 0’ı teslim eder, 2 ve 3’ü sıra dışı diye atar. Gönderici timeout sonrası **1,2,3** yeniden yollar. SR alıcısı 2 ve 3’ü bellekte tutabilir ve **yalnız 1** yeniden istenebilir.

### Slayt 50 — Go-Back-N — algoritma şeması 1

Gönderici bir pencere dolusu çerçeveyi yollayabilir. `ack_expected` en eski onay bekleyeni, `next_frame_to_send` sıradaki gönderim numarasını temsil eder. Gönderileceklerin sayısı pencere boyutuyla sınırlıdır.

### Slayt 51 — Go-Back-N — algoritma şeması 2

Çerçeve gelişinde beklenen sıra ile eşleşiyorsa veri teslim edilir ve ACK oluşturulur. **Kümülatif ACK** daha önceki tüm ardışık çerçeveleri kapsayabilir; tek tek kayıp olmayan her çerçeve için ayrı onay gerekmez.

### Slayt 52 — Go-Back-N — algoritma şeması 3

Timeout olduğunda gönderici en eski bekleyen çerçeveden başlayarak **henüz onaylanmamış çerçeveleri yeniden gönderir**. Sadece hata olan çerçeveyi tekrar göndermekten farklıdır.

### Slayt 53 — Go-Back-N — algoritma şeması 4

Pencere büyüdükçe bağlantı doluluğu ve verim artabilir; ancak kayıp sonrası birden çok geçerli çerçevenin tekrar iletilmesi bant genişliğini harcayabilir. GBN, düşük hata oranında uygulanması nispeten kolaydır.

### Slayt 54 — Go-Back-N — zamanlayıcıların yazılımla benzetimi

Slayttaki çizim, her onay bekleyen çerçeveye ilişkin sürelerin **tek bir yazılım zaman kuyruğu** üzerinden takip edilebileceğini anlatır. Gerçek uygulama her çerçeveye ayrı donanım sayacı gerektirmez.

> [!example] Slayt 54 için örnek / şekil okuma
> **Önemli ayrıntı:** Zamanlayıcı uygulaması donanım yapısı değil, protokol davranışının korunmasıyla değerlendirilir. Tek donanım timer, sıralı yazılım olay listesi veya başka düzenler kullanılabilir.

### Slayt 55 — Protokol 6 — Selective Repeat’e giriş

Selective Repeat (seçici tekrar) yalnız **kaybolan veya bozulmuş çerçeveleri** yeniden ister/gönderir. Alıcı pencere boyutu 1’den büyük olabilir; **sırası dışında gelen geçerli çerçeveler tamponda tutulabilir**.

### Slayt 56 — Selective Repeat — algoritma şeması 1

Alıcı gelen çerçevenin **alma penceresinde** olup olmadığına bakar. Pencere içindeki doğru fakat sıradan bağımsız çerçeveleri tamponlayabilir; beklenen eksik çerçeve gelmeden üst katmana sıralı teslimat yapmayabilir.

### Slayt 57 — Selective Repeat — algoritma şeması 2

Gönderici kayıpları zamanlayıcılar ve uygun denetim geri bildirimleriyle izler. GBN’den farklı olarak tüm ardışık pencereyi geri göndermek yerine **belirli çerçeveyi** yeniler.

### Slayt 58 — Selective Repeat — algoritma şeması 3

Tamponlama ek bellek ve karmaşıklık gerektirir. Bu maliyet, özellikle uzun gecikmeli/yüksek hata olasılıklı bağlantılarda gereksiz yeniden iletimleri azaltarak karşılanabilir.

### Slayt 59 — Selective Repeat — pencere boyutu ve sıra sarma

Şekilde sıra numarası uzayına göre **7 ve 4** büyüklüğünde pencere örnekleri vardır. m bitlik numaralamada güvenli SR pencere boyutu genellikle **2^(m−1)** değerini aşmamalıdır; aksi hâlde eski/yeni çerçeveler karışabilir.

> [!example] Slayt 59 için örnek / şekil okuma
> **Sıra numarası güvenliği:** m=3 → `N=8`; GBN için azami tipik gönderici penceresi `N−1=7`; SR için güvenli azami pencere `N/2=4` (standart sıra numaralı SR varsayımlarında). `W=7` SR için güvenli değildir; eski ve yeni seq değerleri karışabilir.


---

## Bölüm V — Protokol Doğrulama (60–69)

### Slayt 60 — Protokol doğrulamaya giriş

Sözel “çalışıyor gibi” yeterli değildir. **Biçimsel doğrulama (formal verification)** ile protokolün kilitlenme, yanlış teslim, tekrar ve ilerleme durumları incelenir. Ders iki yöntem sunar: **sonlu durum makinesi** ve **Petri ağı**.

### Slayt 61 — Sonlu durum makinesi — durum kavramı

Protokol makinesi her an belirli bir durumdadır. Durum program sayacı, değişkenler, sıra numaraları ve beklemelerle belirlenir. Toplam n ikili bitin temsil ettiği ham durum kombinasyonu en fazla **2^n** olabilir; bütün kombinasyonların ulaşılabilir olması gerekmez.

### Slayt 62 — Sistem durumlarının birleştirilmesi

Protokol 3 için basitleştirilmiş model: gönderici 2 durum, alıcı 2 durum, kanal 4 durum → **2×2×4 =16** birleşik durum. Bu sayım modelleme varsayımlarına dayanır; gerçek kanalın olası durumları daha fazladır.

> [!example] Slayt 62 için örnek / şekil okuma
> **Hesap:** `S∈{0,1}`, `R∈{0,1}`, `C∈{0,1,ACK,boş}` → `2×2×4=16`. **Not:** 16 birleşik durumun tamamı zorunlu olarak erişilebilir değildir; *reachability* incelenmelidir.

### Slayt 63 — FSM — olaylarla durum geçişleri

Çerçeve gönderimi, gelişi, ACK, timeout ve kayıp **geçiş (transition)** oluşturabilir. Her durumdan sıfır veya daha fazla geçiş çıkar. Durum çizgileri protokolün mümkün davranışlarını açıklar.

### Slayt 64 — Ulaşılabilirlik analizi

Başlangıç durumundan hangi durumlara gidilebildiği **reachability** ile incelenir. Hiç erişilemeyen durumlar gereksiz olabilir; erişilebilen bir kilitlenme durumu protokolün tasarım kusurunu ortaya koyabilir.

### Slayt 65 — Protokol 3 için birleşik durum diyagramı

Orijinal şekil `SRC` ile etiketlenen birleşik durumlar gösterir: S gönderen durumu, R alıcı durumu, C kanaldaki durum (`0`, `1`, `ACK`, `-`). Bu durumların bazısı kayıp/iletim olaylarıyla erişilir. Şemadaki üçlü gösterimi bir **durum kodlaması** olarak oku.

> [!example] Slayt 65 için örnek / şekil okuma
> **Şekil okuma:** `SRC` üçlüsünde ilk değer gönderici, ikinci alıcı, üçüncü kanal durumu. Kanal `-` boş anlamına gelebilir. Zaman aşımı veya kayıp gibi geçişler çizimde durumlar arası yönlü oklarla ifade edilir.

### Slayt 66 — Petri ağları — place, token, transition

Petri ağında **yer (place)** durum/koşulu, **jeton (token)** mevcut işaretlemeyi, **geçiş (transition)** olayı temsil eder. Geçişe gelen yaylar girişleri, çıkan yaylar sonuçları gösterir.

### Slayt 67 — Petri ağlarında geçişin etkinleşmesi

Bir geçiş, gerekli tüm giriş yerlerinde yeterli token varsa **etkin (enabled)** olur. Tetiklendiğinde girişlerden token tüketilir, çıkışlara token üretilir. Birden fazla geçiş etkinse farklı yürütüm yolları oluşabilir.

### Slayt 68 — Protokol 3 için Petri ağı

Şekil, protokol 3’ün gönderici, alıcı ve kanal koşullarını Petri ağı biçiminde modeller. FSM, sistemin bütüncül durumlarını; Petri ağı ise **eşzamanlı/paralel olay ilişkilerini** görünür kılmakta kullanışlıdır.

### Slayt 69 — Petri ağının cebirsel gösterimi

Her geçiş bir kural gibi yazılabilir: `giriş_yerleri → çıkış_yerleri`. Bu, geçişin hangi işaretlemelerde mümkün olduğunu ve tetiklenince token dağılımının nasıl değiştiğini gösterir.

> [!example] Slayt 69 için örnek / şekil okuma
> **Petri kural örneği:** `A + B → C` geçişi, A ve B’de token varken tetiklenebilir; A ve B’den birer token tüketip C’ye token üretir. Çoklu giriş koşulları eşzamanlılık kısıtlarını modeller.


---

## Bölüm VI — HDLC ve PPP (70–81)

### Slayt 70 — Örnek veri bağlantı protokolleri

**SDLC, HDLC, ADCCP, LAPB** gibi bit yönelimli bağlantı protokolleri vardır. Bayrak desenleri çerçeve sınırını belirler; **bit stuffing** veri içinde yanlış bayrak algılamayı engeller.

### Slayt 71 — HDLC ve internet bağlantı protokollerine giriş

**HDLC** klasik bit yönelimli çerçeveleme protokolüdür. **PPP** ise tarihsel olarak internetin nokta-noktaya bağlantılarında önemli rol oynamıştır. Temel çerçeveleme felsefelerini karşılaştır.

### Slayt 72 — HDLC çerçeve yapısı

Tipik çerçeve: `Flag | Address | Control | Information | FCS | Flag`. Bayrak genellikle **01111110**; veri saydamlığı için beş ardışık 1’den sonra 0 ekleme uygulanabilir. FCS çoğunlukla CRC temellidir.

> [!example] Slayt 72 için örnek / şekil okuma
> ```text
> FLAG  | ADDRESS | CONTROL | INFORMATION | FCS | FLAG
> 01111110                                         01111110
> ```
> Veri içinde **beş art arda 1** görülürse gönderici `0` doldurabilir; alıcı bunun doldurma biti olduğunu anlayıp çıkarır. Flag’in kendisi doldurulmaz.

### Slayt 73 — HDLC kontrol alanının türleri

**I** (Information): kullanıcı verisi ve sıra/ACK bilgisi; **S** (Supervisory): akış/hata kontrolüne ilişkin denetim; **U** (Unnumbered): bağlantı yönetimi gibi işlevler. `P/F` Poll/Final bitidir; slayttaki `Seq`, `Next`, `Type` alanlarını bu çerçevede yorumla.

### Slayt 74 — İnternette veri bağlantı katmanı — nokta-noktaya

İnternet host, router ve bağlantı altyapısından oluşur. Ders, geniş alan omurgalarında kullanılan **noktadan noktaya hatların** katman-2 çerçeveleme gereksinimine odaklanır.

### Slayt 75 — PPP’nin kullanım alanları — tarihsel

Router–router kiralık hatlar ve çevirmeli modemle kullanıcı–router bağlantısı örnek gösterilir. Bu örnekler kaynak kitabın dönemini yansıtır; **PPP sadece eski modemde çalışır** sonucuna varma.

### Slayt 76 — Ev bilgisayarı örneği

Şekilde ev bilgisayarının modem/telefon bağlantısıyla ISP’nin yönlendiricisine erişmesi gösterilir. IP paketi, nokta-noktaya veri bağlantı çerçevesi içerisinde taşınabilir. Bu, ağ katmanı IP ile bağlantı katmanı PPP’nin görev ayrımıdır.

### Slayt 77 — PPP’nin temel işlevleri — çerçeveleme

PPP çerçeve sınırlarını ayırır, FCS ile hata tespiti yapar, farklı ağ katmanı protokollerini kapsülleyebilir, kimlik doğrulama ve bağlantı seçeneklerinin görüşülmesini destekleyebilir. **PPP hata tespiti yapar; otomatik güvenilir ARQ sağlaması temel özelliği değildir.**

### Slayt 78 — PPP’nin LCP ve NCP bileşenleri

**LCP (Link Control Protocol)** bağlantıyı kurar, seçenek görüşür, test eder ve kapatır. **NCP (Network Control Protocol)** ilgili ağ katmanı protokolüne özgü yapılandırmayı görüşür; IP için IPCP örnektir.

> [!example] Slayt 78 için örnek / şekil okuma
> **LCP mi, NCP mi?** `LCP = fiziksel/nokta-noktaya link seçenekleri`, `NCP = IPv4/IPv6 gibi ağ protokolüne özgü seçenekler`. Örneğin PPP bağlantısı müzakere edildikten sonra IPCP ile IPv4 seçenekleri görüşülebilir.

### Slayt 79 — PPP çerçeve alanları

Tipik PPP: `Flag | Address | Control | Protocol | Information | FCS | Flag`. Standart adres alanı **0xFF** (sekiz tane 1), kontrol alanı **0x03** (00000011) olabilir. `Protocol`, payload’un hangi protokolü içerdiğini belirtir; bazı alanlar görüşmeyle sıkıştırılabilir.

> [!example] Slayt 79 için örnek / şekil okuma
> **PPP örnek başlığı:** `7E | FF | 03 | 00 21 | IP verisi | FCS | 7E` (`0x0021` IPv4 Protocol alanının yaygın değeridir). FCS, çerçevede hata tespitini sağlar. `FF` doğru standart adres baytı 8 bit `11111111`dir; kaynak PDF metin çıkarımında bir `1` düşmüş olabilir.

### Slayt 80 — PPP bağlantı yaşam döngüsü

Şematik evreler: **Dead → Establish → Authenticate (isteğe/kuruluma bağlı) → Network → Open → Terminate → Dead**. LCP müzakeresi, gerekirse kimlik doğrulama, NCP yapılandırması ve veri alışverişi sonrasında bağlantı kapatılır. Her PPP oturumunda kimlik doğrulama zorunlu değildir.

> [!example] Slayt 80 için örnek / şekil okuma
> ```mermaid
> flowchart LR
>  D[Dead] --> E[Establish - LCP]
>  E --> A[Authenticate - gerekirse]
>  A --> N[Network - NCP]
>  N --> O[Open - veri]
>  O --> T[Terminate]
>  T --> D
> ```
> Kimlik doğrulama uygulanmayan bir bağlantıda ilgili aşama atlanabilir.

### Slayt 81 — PPP LCP çerçeve türleri

LCP; **Configure-Request/Ack/Nak/Reject**, **Terminate-Request/Ack**, **Code-Reject**, **Protocol-Reject**, **Echo-Request/Reply**, **Discard-Request** gibi mesajlarla seçenek görüşmesi, bağlantı kapatma ve hat denetimi yapabilir. Kaynak slayttaki initiator/responder gösterimini bu kontrol akışıyla ilişkilendir.


---

## Kritik Kavram Karşılaştırmaları

### Stop-and-Wait, Go-Back-N, Selective Repeat

| Özellik | Stop-and-Wait ARQ | Go-Back-N (GBN) | Selective Repeat (SR) |
|---|---|---|---|
| Aynı anda onay bekleyen | 1 | Birden fazla | Birden fazla |
| Alıcı penceresi | 1 | 1 | Birden fazla olabilir |
| Sıra dışı geçerli çerçeve | Kabul edilmez | Atılır | Tamponlanabilir |
| Kayıp sonrası tekrar | Aynı çerçeve | Kayıptan itibaren onaysızlar | Yalnız eksik olan(lar) |
| Alıcı belleği | Az | Az | Daha fazla |
| Karmaşıklık | Düşük | Orta | Daha yüksek |
| Verim | Uzun gecikmede düşük | İyi; kayıpta düşebilir | Özellikle kayıplı hatlarda iyi |

> [!warning] Terim ayrımı
> **Go-Back-N** ve **Selective Repeat**, bu haftanın **veri bağlantı katmanı** protokolleri olarak modellenir. TCP'nin ACK ve tekrar mekanizmaları benzer fikirler içerir ama TCP bu slaytlardaki GBN/SR algoritmalarının birebir aynısı değildir.

### Hata tespiti, hata kurtarma ve akış kontrolü

- **CRC/FCS:** Hatanın oluşup oluşmadığını kontrol eder; kayıp çerçeveyi kendi başına yeniden getirmez.
- **ACK:** Alıcıdan gelen teslim/alım bildirimi. ACK kaybolabilir.
- **Timeout:** Zamanında beklenen ilerleme yoksa yeniden işlem başlatılmasına yol açar; her timeout fiziksel veri kaybını kanıtlamaz.
- **Sıra numarası:** Tekrar gelen çerçeveyi yeni çerçeveden ayırır.
- **Akış kontrolü (Flow Control):** Alıcının kapasitesinin aşılmasını önlemeye çalışır.
- **Tıkanıklık kontrolü (Congestion Control):** Ağın taşıyamayacağı trafik miktarını yönetir; **aynı şey değildir**.

### HDLC / PPP karşılaştırması

| HDLC | PPP |
|---|---|
| Bit yönelimli çerçeveleme ailesi | Nokta-noktaya bağlantı protokolü |
| Bayrak, adres, kontrol, veri, FCS | Bayrak, adres, kontrol, protocol, veri, FCS |
| I/S/U kontrol çerçeveleri | LCP ve NCP ile bağlantı/ağ protokolü yapılandırma |
| Bit stuffing önemlidir | Bayt/bit saydamlığı varyantları bağlantıya göre değişebilir |
| Geniş protokol ailesi ve varyantlar | Çoklu ağ katmanı protokolünü kapsülleyebilir |
| Bazı kipler sıralama/ACK içerebilir | Temel PPP, yeniden iletimle güvenilir teslimat garantisi vermez |

### Pencere sayıları: iki kritik eşitsizlik

m bit sıra numarası ve `N=2^m` ise, burada incelenen klasik ARQ koşullarında:

- **GBN:** `W_sender ≤ N−1`; alıcı penceresi genellikle `1`.
- **SR:** Simetrik pencere için `W ≤ N/2` yeterli klasik güvenlik sınırıdır.

Bunlar yalnız ezberlenecek formüller değildir: Sıra numaraları dolup başa sarınca yeni bir çerçevenin, hâlâ dolaşımda olabilecek **eski çerçeveyle karıştırılmasını** önlemeyi amaçlar.

### Kümülatif ACK örneği

Gönderici sıra numarası `0,1,2,3` gönderdi. Alıcı `0,1,2`’yi ardışık aldı. Kullanılan ACK tanımına bağlı olarak onay “2’ye kadar hepsi tamam” veya “sonraki beklenen 3” şeklinde ifade edilebilir. **Protokolün ACK numarasının semantiğini açıkça belirtmeden** `ACK=3` veya `ACK=2` ifadesini evrensel sanma.

---

## Formül Föyü

### 1. Sıra numarası uzayı

$$N=2^m,\qquad 	ext{MAX\_SEQ}=2^m-1$$

Örnek: `m=3` → `N=8`, `MAX_SEQ=7`.

### 2. GBN gönderme penceresi (klasik sınır)

$$W_{GBN}\le 2^m-1$$

### 3. Selective Repeat penceresi (klasik güvenli sınır)

$$W_{SR}\le 2^{m-1}$$

### 4. Bir çerçeveyi bağlantıya verme süresi

$$T_{tx}=\frac{L}{R}$$

L: bit cinsinden çerçeve boyu, R: bit/s aktarım hızı.

### 5. Stop-and-Wait verimlilik yaklaşımı

İhmal edilebilir ACK iletim süresi ve işlem gecikmesi altında:

$$\eta_{SW}\approx\frac{T_{tx}}{T_{tx}+2T_{prop}}=\frac{1}{1+2a},\qquad a=\frac{T_{prop}}{T_{tx}}$$

ACK çerçevesi uzunluğu ihmal edilemiyorsa uygun `T_ACK` ve diğer gecikmeleri paydaya ekle.

### 6. Pencereli gönderimde yaklaşık verim

Kayıpsız, sabit RTT ve ek yüklerin ihmal edildiği basitleştirilmiş modelde:

$$\eta_W\approx\min\left(1,\frac{W}{1+2a}\right)$$

### 7. RTT ve bant genişliği–gecikme çarpımı

$$RTT\approx2T_{prop},\qquad BDP=R\cdot RTT$$

Pencereyi doğru seçerken boru hattında kaç bit/çerçeve bulunabildiğine bakılır.

---

## Çözümlü Sorular

### Örnek 1 — ACK kaybolduğunda ne olur?

**Verilen:** 0 numaralı veri B’ye ulaştı, B ağ katmanına teslim etti, ACK(0) kayboldu.

1. A, ACK gelmediği için timeout olur.
2. A, `DATA(0)` çerçevesini yeniden yollar.
3. B’nin beklediği numara artık 1’dir; yeniden gelen 0 **kopyadır**.
4. B, 0’ı tekrar ağ katmanına vermeden uygun onay gönderir.

**Sonuç:** Sıra numarası, **iki kez alınma** ile **iki kez üst katmana teslim edilme** arasındaki farkı yaratır.

### Örnek 2 — 3 bitlik numarayla pencereler

`m=3 → N=2³=8`.

- GBN için klasik azami gönderici penceresi `8−1=7`.
- SR için klasik güvenli simetrik pencere `8/2=4`.

**Sonuç:** “3 bit → 8 farklı değer → 8 gönderme penceresi” yorumu yanlıştır.

### Örnek 3 — 4 bitlik numarayla pencereler

`m=4 → N=16`.

GBN azami `15`, SR güvenli simetrik pencere azami `8`.

### Örnek 4 — Go-Back-N kayıp senaryosu

**Gönderilen:** `0, 1, 2, 3, 4`; `2` kayboldu.

- B, 0 ve 1’i kabul eder.
- B, 3 ve 4’ü beklenen 2 olmadığı için atar.
- A, en eski onaysız 2’ye ait zamanlayıcı dolduğunda `2,3,4` tekrar yollar (bu sade senaryoda).
- **Yeniden iletilen çerçeve sayısı: 3.**

### Örnek 5 — Selective Repeat kayıp senaryosu

Aynı `0,1,2,3,4` akışında 2 kaybolursa:

- B, 0 ve 1’i teslim eder.
- B, 3 ve 4’ü alma penceresinde ise tamponlar ve uygun ACK’lerle bildirir.
- A yalnız 2’yi yeniden iletir.
- B, 2’yi alınca `2,3,4`’ü ağ katmanına sıralı teslim edebilir.
- **Yeniden iletilen çerçeve sayısı: 1.**

### Örnek 6 — Stop-and-Wait verim hesabı

**Verilen:** 1000 bayt veri çerçevesi, 1 Mbps hat, tek yön yayılma `20 ms`, ACK iletim süresi ihmal.

`L = 8000 bit`; `Ttx = 8000/1.000.000 = 8 ms`. `RTT≈40 ms`.

$$\eta=\frac{8}{8+40}=\frac16\approx\%16,67$$

**Neden düşük?** Gönderici 8 ms veri yollar, 40 ms civarında iletim yapılmadan ACK/RTT bekler.

### Örnek 7 — Pencere boyutu ve kullanım

Örnek 6’da `a=20/8=2,5` ve `1+2a=6`.

- `W=1`: yaklaşık `%16,67`.
- `W=3`: yaklaşık `%50`.
- `W=6`: basitleştirilmiş modelde yaklaşık `%100`.

**Yorum:** Gönderici 6 çerçevelik pencereyle bağlantı boş kalmadan veri gönderebilir; pratikte ek yük ve kayıplar bu değeri azaltır.

### Örnek 8 — ACK piggybacking

A→B veri geldi, B’nin A’ya göndereceği kendi verisi 2 ms sonra hazır olacak; ACK gecikme sınırı 5 ms.

B, 2 ms bekleyip `DATA + ACK` gönderirse ayrı ACK çerçevesi maliyeti azalır. Ancak B’nin verisi 8 ms sonra gelseydi, 5 ms dolunca **bağımsız ACK** göndermek gerekir.

### Örnek 9 — FSM durum sayısı

Gönderici 2, alıcı 2, kanal 4 ayrık durumda → `2×2×4=16` birleşik durum.

**Dikkat:** Ulaşılabilir durumların sayısı en fazla 16 olabilir; bazı bileşimler fiziksel olarak mümkün olmayabilir.

### Örnek 10 — Petri ağı geçişi

Başlangıçta A’da 1, B’de 1, C’de 0 token var. `A+B→C` geçişi etkinse tetiklendiğinde A=0, B=0, C=1 olur. C’de token bulunması **sonuç koşulunun gerçekleştiğini** temsil eder.

### Örnek 11 — HDLC bit stuffing

Veri bitleri: `01111110`. Veri alanında bayrak gibi görünmesini önlemek için beş ardışık 1’den sonra 0 eklenir:

`0 11111 0 1 0` → **`011111010`** (doldurma işaretleyici gösterimi). 

> [!warning] Hesabı dikkatle kontrol et
> Gerçek ham veri `0 111111 0` yani `01111110` ise beş `1` sonrası `0` eklenmiş çıktı **`011111010`** olur: `0 + 11111 + 0(stuffed) + 1 + 0`. Alıcı stuffed 0’ı silince `01111110` geri elde edilir.

### Örnek 12 — PPP çerçeve alanları

`7E | FF | 03 | 00 21 | IP datagramı | FCS | 7E`.

- `7E`: sınır bayrağı
- `FF`: adres alanı
- `03`: numarasız kip kontrol alanı
- `00 21`: IPv4’ün PPP Protocol alanı
- `FCS`: hata tespiti

**LCP** bağlantıyı kurar/test eder; **NCP** ağ katmanına özgü seçenekleri görüşür.

---

## Week 6 Yüksek Lisans Vize Denemesi — 100 Puan

> [!note] Sınav yöntemi
> İlk turda cevap anahtarına bakmadan çöz. **Çoktan seçmeli 5×2=10**, **kısa açıklama 8×5=40**, **analiz 5×10=50** puan. Toplam **18 soru / 100 puan**.

### Bölüm A — Çoktan Seçmeli (10 Puan)

1. (2p) Gürültülü kanalda ACK kaybı nedeniyle çerçevenin ikinci kez üst katmana teslim edilmesini hangi bilgi engeller? A) CRC tek başına B) Sıra numarası C) IP TTL D) PMD
2. (2p) GBN alıcı penceresi klasik modelde kaçtır? A) 0 B) 1 C) N/2 D) N
3. (2p) m=3 için klasik güvenli SR simetrik pencere üst sınırı nedir? A) 2 B) 4 C) 7 D) 8
4. (2p) PPP bağlantı seçenekleri için kullanılan protokol hangisidir? A) LCP B) ICMP C) ARP D) SMTP
5. (2p) HDLC I-çerçevesi için en uygun tanım? A) Sadece fiziksel senkron B) Kullanıcı verisi ve kontrol alanları C) Ethernet MAC adresi D) IP yönlendirme

### Bölüm B — Kısa Cevap (40 Puan)

6. (5p) Üç temel protokolü (1/2/3) varsayımları, hata koşulları ve ACK açısından karşılaştır.
7. (5p) Veri bağlantı protokolündeki `kind`, `seq`, `ack`, `info`, `checksum` alanlarını açıkla.
8. (5p) ACK kaybında neden timeout ve yeniden iletim tek başına yeterli değildir? Bir zaman çizelgesiyle açıkla.
9. (5p) Piggybacking nedir? Bekleme süresi fazla uzadığında hangi problem oluşur?
10. (5p) Sıra numarası uzayı ile pencere boyutu aynı mıdır? `m=4` üzerinden örnekle.
11. (5p) GBN ile SR’yi alıcı penceresi, tampon kullanımı ve yeniden iletim açısından karşılaştır.
12. (5p) FSM’de durum, geçiş, başlangıç durumu ve ulaşılabilirlik kavramlarını tanımla.
13. (5p) HDLC I/S/U çerçeveleri ile PPP LCP/NCP’nin görevlerini özetle.

### Bölüm C — Analiz ve Hesaplama (50 Puan)

14. (10p) `DATA(0)` B’ye doğru ulaştı, ACK kayboldu. Zaman aşımından başlayarak A ve B’deki işlemleri, üst katmana neden ikinci teslim yapılmadığını açıkla.
15. (10p) Gönderici `0,1,2,3,4,5` gönderiyor, çerçeve 2 kayboluyor. GBN ve SR için alıcının 3,4,5 karşısındaki davranışını ve hangi çerçevelerin tekrar gönderileceğini karşılaştır (tümünün pencereye sığdığını varsay).
16. (10p) `m=4` için sıra numarası uzayı, GBN azami gönderme penceresi ve SR azami güvenli simetrik pencereyi hesapla. Bu sınırların neden gerekli olduğunu açıkla.
17. (10p) Çerçeve 1000 bayt, hat 1 Mbps, tek yön yayılma 20 ms, ACK süresi ihmal. Stop-and-Wait kullanımını hesapla. W=3 ve W=6 için yaklaşık pencereli kullanım oranlarını bul ve yorumla.
18. (10p) Bir PPP bağlantısını Dead→Establish→Authenticate→Network→Open→Terminate yaşam döngüsü boyunca anlat. LCP, NCP, FCS ve Protocol alanının görevini belirt. PPP’nin doğrudan güvenilir teslimat garantisi verip vermediğini açıkla.

---

## Deneme Cevap Anahtarı ve Puanlama

### Çoktan seçmeli

**1-B, 2-B, 3-B, 4-A, 5-B.**

### Kısa cevap — tam puan için

6. P1 hatasız ve sınırsız; P2 hatasız ama alıcı yavaş, Stop-and-Wait; P3 gürültülü, sıra numarası+ACK+timeout.
7. `kind`: veri/kontrol türü; `seq`: gönderilen sıra; `ack`: alım/onay bilgisi; `info`: ağ katmanı paketi; `checksum/FCS`: hata tespiti.
8. Veri kabul edilip ACK kaybolunca gönderici tekrar gönderir; `seq` olmadan alıcı veriyi iki kez teslim eder. Tekrar gelen `seq` ayıklanır.
9. ACK karşı yöne giden veri çerçevesine eklenir; uzun gecikme timeout ve gereksiz tekrar doğurur; ACK timer gerekir.
10. Hayır. m=4, N=16; klasik GBN W≤15, SR W≤8. Pencere numara uzayının tümünü güvenle kaplayamaz.
11. GBN alıcı penceresi 1, sıradan sapmalar atılır, kayıptan sonrası tekrar; SR sırasız doğruları tamponlar, eksik yeniden iletilir.
12. Durum değişken bileşimi, geçiş olay kaynaklı değişim, başlangıç durum kodu, ulaşılabilirlik ise başlangıçtan gidilebilen durumların incelenmesi.
13. HDLC I veri/sıra; S denetim; U numarasız/bağlantı; PPP LCP bağlantı müzakere, NCP ağ katmanı ayarları.

### Analiz — adım adım

14. B `DATA(0)`’ı teslim edip ACK yollar; ACK kaybolur. A timeout ile tekrar `DATA(0)` gönderir; B beklediği `1` olduğundan **kopya** algılar, üst katmana ikinci kez teslim etmez, uygun ACK gönderir.
15. GBN: B 0,1’i teslim eder; 3,4,5’i atar; A gerekli timeout sonrası **2,3,4,5** yeniden yollar. SR: B 3,4,5’i pencere içindeyse tamponlar, yalnız **2** yeniden iletilir, ardından veri sıralı teslim edilir.
16. `N=2^4=16`; GBN `Wmax=15`, SR `Wmax=8`. Sıra numaralarının tekrar kullanımı sırasında eski ve yeni verinin karışmasını engellemek gerekir.
17. `Ttx=1000×8/10^6=8ms`; `RTT≈40ms`; `a=2.5`; `ηSW=1/(1+5)=1/6≈%16,67`. `W=3` → %50; `W=6` → yaklaşık %100. Bu değerler kayıp ve ek yükler yok sayılarak hesaplanır.
18. Dead: hat kapalı; Establish: LCP seçenek görüşür; Authenticate: gerekliyse kimlik doğrulama; Network: NCP ağ katmanını yapılandırır; Open: kullanıcı trafiği; Terminate: kapanış. `Protocol` payload içeriğini ayırt eder; FCS hata tespit eder; **PPP tek başına yeniden iletimli güvenilir veri teslimi garanti etmez**.

---

## Obsidian Aktif Hatırlama Kartları

> [!question]- Stop-and-Wait’te ACK neden beklenir?
> Alıcının işlemesini aşmamak ve (gürültülü sürümde) gönderilen çerçevenin kabulü üzerine ilerlemek için.

> [!question]- ACK kaybı hangi özel hataya yol açabilir?
> Sıra numarası yoksa yeniden iletilen **aynı verinin iki kez ağ katmanına teslim edilmesine**.

> [!question]- GBN ile SR arasındaki en kısa fark nedir?
> GBN sıra dışı çerçeveleri atıp kayıptan sonraki onaysız çerçeveleri tekrar gönderir; SR sıra dışı doğru çerçeveleri tamponlayıp yalnız eksikleri yeniler.

> [!question]- 3 bitlik sıra numarası için GBN/SR en büyük pencereler?
> GBN = 7; SR = 4 (klasik güvenli sınırlar).

> [!question]- Piggybacking neden yapılır?
> ACK’yi karşı yön veri çerçevesine ekleyerek ayrı kontrol çerçevelerinin maliyetini azaltmak için.

> [!question]- ARQ ve CRC aynı şey mi?
> Hayır: CRC hatayı **tespit eder**; ARQ yeniden iletimle **kurtarmaya çalışır**.

> [!question]- FSM ve Petri Net farkı?
> FSM tekil durum/geçişleri modeller; Petri Net eşzamanlı olayları ve kaynakların koşullarını tokenlarla gösterir.

> [!question]- HDLC bayrağı nedir?
> `01111110`. Veri içindeki aynı desenin sahte bayrak sayılmaması için bit stuffing uygulanabilir.

> [!question]- PPP’de LCP ve NCP farkı?
> LCP bağlantı kurma/test/müzakere/kapama; NCP ilgili ağ katmanı protokolünün seçenekleri.

> [!question]- PPP bağlantı durumları?
> Dead → Establish → Authenticate (gerekirse) → Network → Open → Terminate.

---

## Çalışma Kontrol Listesi

- [ ] Protokol 1, 2, 3 arasındaki varsayım farklarını açıklayabiliyorum.
- [ ] ACK kaybı ve sıra numarası gereğini çizerek anlatabiliyorum.
- [ ] Timeout ile veri kaybını eşitlemiyorum.
- [ ] Piggybacking ve ACK timer davranışını açıklayabiliyorum.
- [ ] `m` bit üzerinden sıra numarası alanını ve pencere sınırlarını hesaplayabiliyorum.
- [ ] GBN ve SR için 2 numaralı çerçevenin kaybı senaryosunu çözebiliyorum.
- [ ] Stop-and-Wait verimini hesaplayabiliyorum.
- [ ] FSM, reachability ve Petri token geçişlerini açıklayabiliyorum.
- [ ] HDLC I/S/U türlerini ve bit stuffing'i biliyorum.
- [ ] PPP çerçevesi, LCP, NCP ve bağlantı evrelerini açıklayabiliyorum.
- [ ] 18 soruluk deneme sınavını cevap anahtarına bakmadan çözdüm.

---

## Kaynakça ve Haftalar Arası Bağlantılar

- **Birincil kaynak:** [Ankara Üniversitesi 826501 — Week 6 PDF](https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/Week_6.pdf), Dr. Refik Samet, 81 sayfa.
- **Ders izlencesi:** [826501 Computer Networks and Data Communication](https://android.eng.ankara.edu.tr/wp-content/uploads/sites/656/2017/09/826501_Syllabus.pdf).
- **Kitap:** Andrew S. Tanenbaum, *Computer Networks*, 4. baskı, Bölüm 3, s. 211–246.
- Önceki hafta: [[Bilgisayar_Aglari_Week_5_Obsidian_Tum_Ders_Notlari|Week 5 — Hamming ve CRC]].
- Referans modelleri: [[Bilgisayar_Aglari_Week_2_Obsidian_Tum_Ders_Notlari|Week 2 — OSI / TCP-IP]].

> [!success] Week 6 tamamlandı
> 81 slayt, üç temel protokol, kayar pencere, GBN, SR, FSM/Petri, HDLC ve PPP aynı çalışma notunda sırasıyla işlendi.
