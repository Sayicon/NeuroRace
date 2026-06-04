<div align="center">

# NeuroRace

**Kendi kendini evrimleştiren yapay zekâ araçlarının 3B yarış simülasyonu.**

[![Language](https://img.shields.io/badge/language-HTML%2FJS-F7DF1E?style=for-the-badge&logo=javascript)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Three.js](https://img.shields.io/badge/Three.js-r128-black?style=for-the-badge&logo=three.js)](https://threejs.org/)
[![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)](LICENSE)

</div>

---

## Proje Hakkında

NeuroRace, **nöral ağ** ve **genetik algoritma** kombinasyonu olan *nöroevülüsyon* (neuroevolution) tekniğini görselleştiren, tarayıcıda çalışan tek sayfalık bir simülasyondur. 15 araç, her nesilde daha iyi sürüş öğrenerek birbirleriyle yarışır — hiçbir kural elle yazılmaz, her şey evrim yoluyla öğrenilir.

**[Canlı Demo](https://sayicon.github.io/NeuroRace/NeuroRace.html)**

---

## Nasıl Çalışır?

### Sensörler

Her araç, 5 yönde uzanan **mesafe sensörüne** sahiptir:

```
        On (0 derece)
         |
Sol-45  —+— Sag-45
        /|\
       / | \
  Sol-90  Sag-90
```

Sensörler, duvara olan uzaklığı 0–1 arasında normalize ederek nöral ağa iletir.

### Nöral Ağ Mimarisi

```
Giris Katmani   Gizli Katman   Cikis Katmani
  (5 noron)       (6 noron)      (2 noron)
     [S1]           [H1]
     [S2]           [H2]          [Direksiyon]
     [S3]  ───────► [H3]  ──────► [Gaz]
     [S4]           [H4]
     [S5]           [H5]
                    [H6]

Aktivasyon: tanh   Aktivasyon: tanh
```

| Katman | Boyut | Aktivasyon |
|--------|-------|-----------|
| Giris  | 5     | —         |
| Gizli  | 6     | tanh      |
| Cikis  | 2     | tanh      |

Çıkış değerleri **[-1, 1]** aralığında direksiyon açısı ve gaz değeri olarak yorumlanır.

### Genetik Algoritma (Evrim)

Her nesil 1200 frame sürer. Nesil sonunda:

1. **Seçilim** — En fazla checkpoint geçen araçlar hayatta kalır
2. **Çaprazlama** — İki ebeveyn ağın ağırlıkları birleştirilir
3. **Mutasyon** — %15 olasılıkla ağırlıklar rastgele değiştirilir

```
Nesil N:    [Arac1★] [Arac2] [Arac3★] ...
                  ↘       ↙
Nesil N+1: [Cocuk] --mutasyon--> [Cocuk']
```

### Pist Döngüsü

Her 10 nesilde bir pist değişir:

| # | Pist Tipi | Özellik |
|---|-----------|---------|
| 1 | Organik | Kıvrımlı doğal yollar |
| 2 | Oval | Yüksek hız düzlüğü |
| 3 | Yıldız | Teknik keskin virajlar |
| 4 | Karmaşık | Çok katmanlı eğriler |

---

## Kullanılan Teknolojiler

| Teknoloji | Amaç |
|-----------|------|
| [Three.js r128](https://threejs.org/) | 3B grafik, kamera, ışıklandırma |
| WebGL | Donanım hızlandırmalı render |
| Canvas API | Nöral ağ görselleştirmesi |
| Tailwind CSS | Responsive dashboard arayüzü |
| Vanilla JS | Fizik, genetik algoritma, simülasyon döngüsü |

---

## Kurulum ve Çalıştırma

Herhangi bir kurulum veya sunucu gerekmez — tek dosya:

```bash
# Klonla
git clone https://github.com/Sayicon/NeuroRace.git
cd NeuroRace

# Tarayicida ac
start NeuroRace.html        # Windows
open NeuroRace.html         # macOS
xdg-open NeuroRace.html     # Linux
```

> Modern bir tarayıcı (Chrome, Firefox, Edge) yeterlidir.

---

## Yapılandırma

`NeuroRace.html` içindeki `CONFIG` nesnesini düzenleyerek simülasyonu özelleştirebilirsiniz:

```javascript
const CONFIG = {
    carCount: 15,         // Ayni anda yarisan arac sayisi
    sensorCount: 5,       // Sensor sayisi
    mutationRate: 0.15,   // Mutasyon orani (%15)
    generationTime: 1200, // Nesil uzunlugu (frame)
    maxSpeed: 1.5,        // Maksimum hiz
    trackWidth: 18,       // Pist genisligi
    rayLength: 70,        // Sensor menzili
};
```

---

## Özellikler

- Gerçek zamanlı 3B render — Three.js ile WebGL destekli
- Nöral ağ görselleştirmesi — Ağırlıkları canlı gösteren bağlantı grafiği
- Simülasyon hız kontrolü — Hızlı nesil geçişi için x1–x10 hız
- Performans tablosu — Her araç için nesil, checkpoint ve sıralama bilgisi
- Pause / Resume desteği

---

## Neuroevolution Nedir?

Neuroevolution, nöral ağ ağırlıklarını **gradyan tabanlı geri yayılım (backpropagation) kullanmadan**, yalnızca evrimsel baskı ile optimize eden bir yaklaşımdır. Etiketli veri seti gerektirmez; hayatta kalma ve çoğalma yeterliliği tanımlayıcıdır.

NeuroRace'de bu prensip doğrudan uygulanır: araçlar sürmeyi kimseye sormadan, sadece hayatta kalmaya çalışarak öğrenir.

---

<div align="center">

*42 Kocaeli öğrencisi tarafından geliştirildi.*

[![GitHub](https://img.shields.io/badge/GitHub-Sayicon-181717?style=flat-square&logo=github)](https://github.com/Sayicon)

</div>
