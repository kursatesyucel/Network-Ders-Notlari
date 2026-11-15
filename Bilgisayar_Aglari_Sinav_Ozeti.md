# Bilgisayar Ağları — Tek Sayfalık Sınav Özeti

**Ankara Üniversitesi · BLM332 / BLM3032 · Ara Sınav**  
**Kapsam:** Hibrit Model, Nyquist–Shannon, Bayt Doldurma, Hamming Kodu

## 1. Hibrit Model — 20 Puan

**Katmanları yukarıdan aşağıya ezberle:**

| Katman | Temel görev | Örnek |
|---|---|---|
| 5. Uygulama | Uygulamalara ağ hizmeti sağlar | HTTP, DNS |
| 4. Taşıma | Uçtan uca uygulama iletişimi | TCP, UDP |
| 3. Ağ | IP adresleme ve yönlendirme | IP, Router |
| 2. Veri Bağı | Çerçeveleme, hata tespiti | Ethernet, PPP |
| 1. Fiziksel | Bitlerin sinyallerle iletilmesi | Kablo, fiber |

**Veri Bağı Katmanı protokolleri:**

- **Ethernet:** Kablolu yerel ağ iletişimi.
- **PPP:** Noktadan noktaya bağlantı.
- **HDLC:** Bit odaklı çerçeveleme ve hata tespiti.

> **Ezber:** IP → Ağ, MAC → Veri Bağı, TCP/UDP → Taşıma.

## 2. Nyquist ve Shannon — 25 Puan

**Nyquist — Gürültüsüz kanal**

$$
\boxed{R_{\max}=2B\log_2 L}
$$

**Shannon — Gürültülü kanal**

$$
\boxed{C=B\log_2(1+\mathrm{SNR})}
$$

**Desibel dönüşümü**

$$
\boxed{\mathrm{SNR}=10^{\mathrm{SNR}_{dB}/10}}
$$

| Bilmen gereken | Değer |
|---|---:|
| 1 kHz | 1.000 Hz |
| $\log_2 2$ | 1 |
| $\log_2 4$ | 2 |
| $\log_2 8$ | 3 |
| 10 dB | SNR = 10 |
| 20 dB | SNR = 100 |
| 30 dB | SNR = 1.000 |

**Örnek — Gürültüsüz 10 kHz, ikili sinyal:**

$$
R=2\times10.000\times\log_2 2=\boxed{20\text{ kbps}}
$$

**Örnek — Gürültülü 10 kHz, 20 dB:**

$$
C=10.000\log_2(101)\approx\boxed{66{,}58\text{ kbps}}
$$

> **Dikkat:** 66,58 kbps, Shannon kapasitesidir. Aynı kanalda iki seviyeli sinyal için Nyquist üst sınırı 20 kbps'dir. Belirli sinyal seviyesi ve gürültü koşulu birlikte verildiyse her iki sınırı değerlendir.

## 3. Bayt Doldurma — 15 Puan

| Gönderilen | Orijinal |
|---|---|
| `ESC ESC` | `ESC` |
| `ESC FLAG` | `FLAG` |
| Normal harf | Değişmez |

**Çözüm sırası:** Önce baştaki ve sondaki `FLAG` karakterlerini kaldır. Sonra `ESC` karakterlerini soldan sağa, ardından gelen karakterle eşleştir.

**Örnek gönderilen çerçeve:**

```text
FLAG A ESC ESC B ESC FLAG C FLAG
```

**Orijinal veri:**

```text
A ESC B FLAG C
```

> **Önemli:** `ESC ESC ESC FLAG` → `ESC FLAG`.

## 4. Hamming Kodu — 40 Puan

### A) Kontrol biti sayısı

$$
\boxed{2^r\geq m+r+1}
$$

10 veri biti için **$r=4$**, toplam **14 bit** gönderilir.

### B) Kontrol bitlerinin konumları

$$
\boxed{1,\ 2,\ 4,\ 8}
$$

| Kontrol | Kontrol ettiği konumlar |
|---|---|
| P1 | 1, 3, 5, 7, 9, 11, 13 |
| P2 | 2, 3, 6, 7, 10, 11, 14 |
| P4 | 4, 5, 6, 7, 12, 13, 14 |
| P8 | 8, 9, 10, 11, 12, 13, 14 |

### C) Tek eşlik (Odd Parity)

| Kontrol biti dışındaki 1 sayısı | Kontrol biti |
|---|---|
| Çift | **P = 1** |
| Tek | **P = 0** |

Amaç, **kontrol biti dahil gruptaki toplam 1 sayısını tek yapmak**.

### D) Hata bulma ve düzeltme

1. Alınan kodun kontrol gruplarını tek eşlik kuralına göre tekrar denetle.
2. Hatalı grupların ağırlıklarını topla (**sendrom**).
3. Bulduğun konumdaki biti tersine çevir (`0 ↔ 1`).
4. Kontrol bitlerini (1, 2, 4, 8. konumlar) çıkar ve orijinal mesajı elde et.

**Örnek:** P1, P2 ve P8 hatalıysa:

$$
S=1+2+8=\boxed{11}
$$

Hata **11. bitte** bulunur.

> **Not:** Hamming konumları, çalışmamızdaki gibi **soldan sağa 1–14** numaralandırılarak verilmiştir. Tek bitlik hata varsayımıyla kullanılır.

---

## Sınavda En Çok Dikkat Edeceğin 5 Şey

1. Hibrit Modelde **Taşıma, Ağ Katmanının üstündedir**.
2. Nyquist'te **$L$ sinyal seviyesi sayısıdır**.
3. Shannon'da **dB değerini önce doğrusal SNR'ye çevir**.
4. Bayt doldurmada **`ESC ESC ESC FLAG` → `ESC FLAG`**.
5. Hamming'de **tek eşlik için toplam 1 sayısı tek** olmalıdır.

**Çalışma önceliği:** Nyquist–Shannon işlemlerini sonuca kadar götürmek ve Hamming'de P1, P2, P4, P8 bitlerini hatasız hesaplamak. Bu iki konu sınavın **65 puanını** oluşturuyor.
