# Observability Stack

Bu proje, Docker Compose ile yerel ortamda log, metric ve trace toplamak/izlemek icin hazir bir observability ortami kurar.

## Icerik

- Seq (merkezi loglama)
- Jaeger (trace goruntuleme)
- Prometheus (metric toplama)
- Grafana (dashboard)
- Elasticsearch/Kibana tanimlari dosyada mevcut, ancak su an yorum satirinda (pasif)

## Gereksinimler

- Docker Desktop (Windows icin)
- Docker Compose v2

Bu compose dosyasi asagidaki harici agin var oldugunu bekler:

- `observability-network`

Eger yoksa bir kere olusturun:

```bash
docker network create observability-network
```

## Hizli Baslangic

Proje kok dizininde calistirin:

```bash
docker compose up -d
```

Durumu kontrol edin:

```bash
docker compose ps
```

Log izleyin:

```bash
docker compose logs -f
```

## Erisim Adresleri

- Seq: http://localhost:5341
- Jaeger UI: http://localhost:16686
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000

Grafana varsayilan giris bilgileri:

- Kullanici: `admin`
- Sifre: `admin`

## Prometheus Hedefi

`prometheus/prometheus.yml` dosyasinda su anda tek hedef bulunur:

- `host.docker.internal:5000` (`/metrics` endpoint)

Bu, host makinede calisan bir .NET API icin ayarlanmistir. Uygulamaniz farkli portta calisiyorsa hedefi guncelleyin.

## Durdurma ve Temizlik

Servisleri durdurmak icin:

```bash
docker compose down
```

Tum verileri (volume) silerek sifirdan baslatmak icin:

```bash
docker compose down -v
```

## Notlar

- Retention Policy (Veri Saklama Süresi): Seq'e çok fazla log akmaya başlarsa disk dolabilir. Seq arayüzüne girdikten sonra Settings -> Retention kısmından örneğin "30 günden eski logları sil" gibi bir kural eklemeyi unutma.
- Compose dosyasinda veri kaliciligi icin volume tanimlari kullanilir.
- Prometheus retention suresi `7d` olarak ayarlanmistir.
- Elasticsearch ve Kibana acilmak istenirse `docker-compose.yml` icindeki ilgili bolumlerin yorumlarini kaldirabilirsiniz.
