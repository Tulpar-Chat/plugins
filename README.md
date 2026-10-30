# Tulpar Eklenti Kaydı

Bu depo yalnızca `registry.json` tutar: hangi eklentiler var, son sürümleri ne
ve paketleri nereden inecek. **Eklenti kodu burada durmaz**, her eklenti kendi
deposunda yaşar.

Tulpar sunucuları bu dosyayı ham (raw) adresinden çeker ve bir saat
önbelleklerler.

## Yeni eklenti eklemek

`registry.json` içindeki `plugins` dizisine bir girdi ekleyin:

```json
{
  "id": "dev.ornek.eklenti",
  "name": "Örnek Eklenti",
  "description": "Bir cümlelik açıklama",
  "author": { "name": "Adınız", "url": "https://..." },
  "channel": "community",
  "license": "MIT",
  "latest": "1.0.0",
  "versions": [
    {
      "version": "1.0.0",
      "url": "https://github.com/<sahip>/<repo>/releases/download/v1.0.0/plugin.zip",
      "sha256": "...",
      "minTulparVersion": "1.0.0"
    }
  ]
}
```

## Kanallar

Bütün eklentiler bu tek kayıtta durur; `channel` hangisinin kime görüneceğini
belirler.

| Kanal | Anlamı | Marketplace'te |
|---|---|---|
| `official` | Tulpar ekibinin yaptığı ya da onayladığı eklentiler | Varsayılan listede |
| `community` | Diğer herkes, tek bir topluluk için yazılanlar dahil | Yalnızca aramada |

`channel` yoksa ya da Tulpar'ın tanımadığı bir değer taşıyorsa girdi `community`
sayılır; yeni bir eklenti yanlışlıkla öne çıkmaz. `official` kanalına geçiş Tulpar
ekibinin kararıdır.

`license` eklentinin kendi lisansıdır (SPDX kimliği, ör. `MIT`, `Apache-2.0`).
Tulpar çekirdeği FSL-1.1-ALv2 ile lisanslıdır; eklenti lisansı bundan bağımsızdır.
Belirtilmezse marketplace "Lisans belirtilmemiş" gösterir.

`featured: true` ileride marketplace'teki öne çıkanlar satırı için ayrılmıştır.

## Kurallar

Bunlara uymayan sürüm girdileri sunucu tarafında **sessizce elenir**:

- `url` mutlaka `https://` ile başlar
- `sha256` paketin gerçek özeti olmalıdır (64 karakter)
- `version` semver olmalıdır (`1.0.0`)
- `id` ters alan adı biçiminde olmalı ve **paketin içindeki `plugin.json` ile
  birebir aynı** olmalıdır; sunucu ikisini karşılaştırır, uyuşmazsa kurulumu
  iptal eder

Checksum zorunlu, çünkü kayıt ile paket ayrı yerlerde duruyor: indirilen şeyin
kayıtta yazan şey olduğunu doğrulamanın başka yolu yok.

## Sürüm güncellemek

1. Eklenti deposunda yeni etiketi atın, release iş akışı paketi ve `sha256`'yı
   üretir
2. Sürüm notlarındaki JSON girdisini kopyalayıp buradaki `versions` dizisinin
   başına ekleyin
3. `latest` alanını yeni sürüme çekin

Eski sürüm girdilerini silmeyin; belirli bir sürümü kurmak isteyen olabilir.

## Sözleşme

Manifest şeması, alan tipleri, kancalar ve kurulum akışının tamamı Tulpar
deposundaki `docs/plugin-api.md` içinde.
