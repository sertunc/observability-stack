# Observability Stack

Bu repo, Docker Compose ile yerel ortamda observability kurulumu yapmak icin iki farkli secenek sunar:

- Seq + Jaeger + Prometheus + Grafana
- SigNoz

## Gereksinimler

- Docker Desktop (Windows)
- Docker Compose v2

## Secenek 1: Seq + Jaeger + Prometheus + Grafana

Compose dosyasi: `seq-jaeger-promethus-grafana/docker-compose.yml`

### Hazirlik

Bu stack, harici `observability-network` aginin var olmasini bekler.

```bash
docker network create observability-network
```

### Calistirma

```bash
cd seq-jaeger-promethus-grafana
docker compose up -d
```

Durumu kontrol:

```bash
docker compose ps
```

Loglar:

```bash
docker compose logs -f
```

### Erisim Adresleri

| Servis                  | URL                        |
|-------------------------|----------------------------|
| Seq                     | http://localhost:5341      |
| Jaeger UI               | http://localhost:16686     |
| Prometheus              | http://localhost:9090      |
| Grafana                 | http://localhost:3000      |
| OpenSearch              | http://localhost:9200      |
| OpenSearch Dashboards   | http://localhost:5601      |

Grafana varsayilan giris:

- Kullanici: `admin`
- Sifre: `admin`

### Prometheus Hedefi

`seq-jaeger-promethus-grafana/prometheus/prometheus.yml` dosyasinda varsayilan hedef:

- `host.docker.internal:5000` (`/metrics` endpoint)

Host makinede calisan .NET API icin ayarlidir. Uygulamaniz farkli portta ise hedefi guncelleyin.

### Durdurma

```bash
docker compose down
```

Tum volume'leri silerek sifirdan baslatma:

```bash
docker compose down -v
```

### Servisler

| Servis                  | Amac                                      |
|-------------------------|-------------------------------------------|
| Seq                     | Merkezi log toplama ve sorgulama          |
| OpenSearch              | Log ve arama motoru (Elasticsearch uyumlu)|
| OpenSearch Dashboards   | OpenSearch icin gorsel UI (Kibana uyumlu) |
| Jaeger                  | Distributed tracing                       |
| Prometheus              | Metrik toplama                            |
| Grafana                 | Metrik ve log gorsellestime               |

### Notlar

- Seq'e yuksek log akisi varsa disk hizla dolabilir. Seq UI icinde Settings -> Retention altindan kural ekleyin.
- Prometheus retention suresi compose icinde `7d` olarak ayarlanmistir.
- OpenSearch guvenlik eklentisi yerel test icin devre disi birakilmistir (`DISABLE_SECURITY_PLUGIN=true`). Uretim ortaminda etkinlestirin.
- Elasticsearch/Kibana bolumleri compose dosyasinda yorum satirindadir; gerekirse acabilirsiniz.

## Secenek 2: SigNoz

Compose dosyasi: `signoz/deploy/docker/docker-compose.yaml`

### Calistirma

```bash
cd signoz/deploy/docker
docker compose up -d
```

Durumu kontrol:

```bash
docker compose ps
```

Loglar:

```bash
docker compose logs -f
```

### Erisim Adresleri

- SigNoz UI: http://localhost:8080
- OTLP gRPC: localhost:4317
- OTLP HTTP: localhost:4318

### Durdurma

```bash
docker compose down
```

Tum volume'leri silerek sifirdan baslatma:

```bash
docker compose down -v
```

## Hangi Stack'i Secmeliyim?

- Hizli ve parcali bir kurulum istiyorsaniz: Seq + Jaeger + Prometheus + Grafana
- Tek bir UI'dan log/metric/trace yonetimi istiyorsaniz: SigNoz
