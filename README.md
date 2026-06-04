<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:e94560&height=200&section=header&text=NeuroRace&fontSize=60&fontColor=fff&animation=twinkling&fontAlignY=38&desc=AI%20cars%20that%20learn%20to%20race%20through%20evolution&descSize=16&descAlignY=58&descColor=ccc" width="100%"/>

<div align="center">

[![Language](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Three.js](https://img.shields.io/badge/Three.js-r128-black?style=for-the-badge&logo=threedotjs&logoColor=white)](https://threejs.org/)
[![WebGL](https://img.shields.io/badge/WebGL-990000?style=for-the-badge&logo=webgl&logoColor=white)](https://www.khronos.org/webgl/)
[![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)](LICENSE)
![Visitor](https://visitor-badge.laobi.icu/badge?page_id=Sayicon.NeuroRace)

**Kendi kendini evrimleştiren yapay zekâ araçlarının 3B yarış simülasyonu.**

**[▶ Canlı Demo](https://sayicon.github.io/NeuroRace/NeuroRace.html)**

</div>

---

## Proje Hakkında

NeuroRace, **nöral ağ** ve **genetik algoritma** kombinasyonu olan *nöroevülüsyon* (neuroevolution) tekniğini görselleştiren, tarayıcıda çalışan tek sayfalık bir simülasyondur. 15 araç, her nesilde daha iyi sürüş öğrenerek birbirleriyle yarışır — hiçbir kural elle yazılmaz, her şey evrim yoluyla öğrenilir.

---

## Nasıl Çalışır?

### Sensör Sistemi

```mermaid
graph TD
    CAR["🚗 ARAÇ"]

    CAR -->|"Sol-90°"| S1["📡 Sensor 1\nSol duvar mesafesi"]
    CAR -->|"Sol-45°"| S2["📡 Sensor 2\nSol çapraz mesafesi"]
    CAR -->|"Ön 0°"| S3["📡 Sensor 3\nÖn duvar mesafesi"]
    CAR -->|"Sağ-45°"| S4["📡 Sensor 4\nSağ çapraz mesafesi"]
    CAR -->|"Sağ-90°"| S5["📡 Sensor 5\nSağ duvar mesafesi"]

    S1 & S2 & S3 & S4 & S5 -->|"0.0 – 1.0 normalize"| NN["🧠 Nöral Ağ"]
```

### Nöral Ağ Mimarisi

```mermaid
flowchart LR
    subgraph INPUT["Giriş — 5 nöron"]
        S["Sol-90 · Sol-45 · Ön · Sağ-45 · Sağ-90\nDuvar mesafeleri  →  0.0 – 1.0"]
    end
    subgraph HIDDEN["Gizli — 6 nöron · tanh"]
        H["H1 · H2 · H3 · H4 · H5 · H6"]
    end
    subgraph OUTPUT["Çıkış — 2 nöron · tanh"]
        O["Direksiyon · Gaz\n−1.0 ile +1.0 arası"]
    end

    INPUT -->|"30 ağırlık\nweightsIH"| HIDDEN
    HIDDEN -->|"12 ağırlık\nweightsHO"| OUTPUT
```

### Genetik Algoritma (Evrim Döngüsü)

```mermaid
flowchart TD
    A["🎲 Nesil Başlangıcı\n15 araç — rastgele ağlar"] --> B["🏁 Simülasyon\n1200 frame"]
    B --> C{"Araç hayatta mı?"}
    C -- Hayır --> D["💀 Elendi"]
    C -- Evet --> E["✅ Checkpoint +1"]
    E --> B
    B --> F["🏆 Nesil Sonu\nEn iyi araçlar seçildi"]
    F --> G["🔀 Çaprazlama\nEbeveyn ağırlıkları birleştirildi"]
    G --> H["🧬 Mutasyon\n%15 rastgele ağırlık değişimi"]
    H --> A

    style A fill:#1a1a2e,color:#fff
    style F fill:#e94560,color:#fff
    style H fill:#0f3460,color:#fff
```

### Pist Döngüsü

Her 10 nesilde bir pist değişir:

| # | Pist Tipi | Özellik |
|:-:|-----------|---------|
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
git clone https://github.com/Sayicon/NeuroRace.git
cd NeuroRace
start NeuroRace.html        # Windows
open NeuroRace.html         # macOS
xdg-open NeuroRace.html     # Linux
```

> Modern bir tarayıcı (Chrome, Firefox, Edge) yeterlidir.

---

## Yapılandırma

```javascript
const CONFIG = {
    carCount: 15,         // Aynı anda yarışan araç sayısı
    sensorCount: 5,       // Sensör sayısı
    mutationRate: 0.15,   // Mutasyon oranı (%15)
    generationTime: 1200, // Nesil uzunluğu (frame)
    maxSpeed: 1.5,        // Maksimum hız
    trackWidth: 18,       // Pist genişliği
    rayLength: 70,        // Sensör menzili
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

Neuroevolution, nöral ağ ağırlıklarını **gradyan tabanlı geri yayılım (backpropagation) kullanmadan**, yalnızca evrimsel baskı ile optimize eden bir yaklaşımdır. Etiketli veri seti gerektirmez; hayatta kalma yeterliliği yeterlidir.

NeuroRace'de bu prensip doğrudan uygulanır: araçlar sürmeyi kimseye sormadan, sadece hayatta kalmaya çalışarak öğrenir.

---

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-Sayicon-181717?style=for-the-badge&logo=github)](https://github.com/Sayicon)

</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:e94560,100:1a1a2e&height=120&section=footer" width="100%"/>
