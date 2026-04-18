# 13 Standalone Release Plan

## Amaç

`satis/` klasörünü bağımsız bir paket reposu olarak yayınlamadan önce teknik, dokümantasyon ve test altyapısını ürün seviyesine getirmek.

Bu plan sadece `satis/` için geçerlidir. Kök projedeki (example app) dosyalar kapsam dışıdır.

## Kapsam

- Paket sınırlarını netleştirme
- Dokümantasyon ve LLM bağlam dosyalarını güncelleme
- Test altyapısını bağımsızlaştırma (Testbench)
- Gereksiz/kullanılmayan kod taraması
- Release checklist ve kabul kriterleri

## Karar

**Önerilen yol: Testbench'e geçiş.**

Gerekçe:

- Paket testlerini host app bağımlılığından kurtarır.
- CI/CD pipeline'ı bağımsız çalışır.
- Satış sonrası bakım ve sürüm güvenilirliği artar.

## Durum Notu (Güncel)

- [x] Faz 5.5 / İlk Dalga tamamlandı (Billing gateways -> `src/Billing/Gateways`).
- [x] Faz 5.5 / İkinci Dalga kısmi tamamlandı:
  - [x] `Services/Billing/*` klasörleme
  - [x] `Services/Documentation/*` klasörleme
  - [x] `Services/Support/*` (`SupportTicketService`)
  - [x] `Services/Sync/*` (`GitHubTagSyncService`)
  - [x] `Services/Licensing/*` (`LicenseService`)
  - [x] `Services/Repository/*` (`ComposerRepositoryService`)
- [x] Event domain klasörleme:
  - [x] `Events/Billing/*`
  - [x] `Events/Support/*`
- [x] Faz 5.5 ikinci dalga son namespace/refactor kontrolleri tamamlandı.
- [x] Faz 4 (Testbench migration) tamamlandı:
  - [x] `composer.json` içinde `orchestra/testbench` dev bağımlılığı eklendi
  - [x] `tests/Pest.php` ve `tests/TestCase.php` iskeleti eklendi
  - [x] Test fixture model/factory/migration iskeleti eklendi
  - [x] Standalone koşum denemesi yapıldı (`vendor/bin/pest -c phpunit.xml.dist`)
  - [x] Tüm feature testlerin Testbench fixture modelleriyle çalışması doğrulandı
  - [x] Standalone koşum kırıkları giderildi:
    - [x] test app key / auth defaults
    - [x] Livewire/Filament + Blade Icons provider wiring
    - [x] host route bağımlı notification testleri için test route bootstrap (`profile.support.view`, `password.reset`)
    - [x] medya/notification/password reset tabloları için test migration'ları
  - [x] Standalone repo içinde doğrudan test koşumu tamamen yeşil doğrulandı (`72 passed`)
- [x] Faz 5 (kod sağlığı / dead reference sweep) tamamlandı:
  - [x] taşınan service/event namespace referansları için legacy yol taraması temiz
  - [x] `auth.providers.admins.model` bağımlılığı kaldırma hedefi doğrulandı
  - [x] monorepo + standalone test koşumları yeşil
- [x] Faz 6 release gate doğrulandı:
  - [x] Pint format koşumu tamam
  - [x] package feature testleri (monorepo) yeşil
  - [x] standalone Testbench suite yeşil
  - [x] kritik smoke akışları (webhook/support/repository) hedefli testlerle doğrulandı

## Konfigürasyon Notu (Yeni)

- [x] Support admin recipient modeli artık konfigüre edilebilir:
  - `satishub.support.admin_model`
  - fallback sırası: `support.admin_model` -> `satishub.user_model`
  - dedicated `Admin` modeli olmayan kurulumlarda `User` modeli ile çalışmayı destekler.

## Faz 1 - Paket Sınırı ve Temizlik

### Yapılacaklar

1. `satis/` dışında hiçbir dosyayı release kapsamına almama.
2. Paket içi gereksiz dosya temizliği (`.DS_Store` gibi).
3. `composer.json` package metadata kontrolü:
   - `name`, `description`, `type`, `autoload`, `extra.laravel.providers`.
4. `CHANGELOG.md` formatını semver release akışına uygun hale getirme.

### Çıktı

- Paket kökü release'e hazır temiz bir dosya ağacı.

## Faz 2 - Dokümantasyon Senkronizasyonu

### Yapılacaklar

1. `README.md` path'lerini standalone repo formatına çevirme:
   - `satis/docs/...` yerine `docs/...`.
2. `docs/` içindeki dosyalarda standalone path normalizasyonu (`src/...`, `routes/...`, `config/...`).
3. Yeni event yüzeyini dokümana net ekleme:
   - `OrderCreated`
   - `LicenseCreated`
   - `WebhookEventProcessed`
   - `SupportTicketOpened`
   - `SupportTicketReplied`
4. `order_link_resolvers` extension point anlatımını örnekle sabitleme.

### Çıktı

- README + docs birbiriyle tutarlı, standalone repo odaklı.

## Faz 3 - LLM Dosyaları Güncelleme

### Yapılacaklar

1. `llms.txt` güncelle:
   - yeni event'ler
   - support-create attachment akışı
   - provider order link resolver uzatma noktası
2. `llms-full.txt` güncelle:
   - event lifecycle
   - yeni risk/invariant notları
   - testbench stratejisi ve test komutları
3. `docs/12-ai-context-files.md` ile tam hizalama.

### Çıktı

- AI context dosyaları gerçek kod durumunu yansıtır.

## Faz 4 - Testbench Geçişi (Önerilen)

### Yapılacaklar

1. Dev bağımlılıkları ekleme:
   - `orchestra/testbench`
   - gerekirse `orchestra/testbench-core`
2. Paket test bootstrap'ı oluşturma:
   - `satis/tests/Pest.php`
   - `satis/tests/TestCase.php` (Testbench tabanlı)
3. Test ortamında gerekli model/stub setup:
   - user/admin model binding
   - auth provider config
   - media/test disk config
4. Host app bağımlı testleri Testbench'e taşıma:
   - `App\Models\User` bağımlılıklarını fixture model veya test app model ile değiştirme.
5. CI komutlarını standalone hale getirme.

### Çıktı

- Paket testleri tek başına ayağa kalkar ve çalışır.

## Faz 5 - Kod Sağlığı ve Dead Code Taraması

### Yapılacaklar

1. Kullanılmayan class/import/metot taraması.
2. Public API olmayan yardımcı sınıfların erişim seviyelerini gözden geçirme.
3. Event dispatch noktalarının gerçek iş akışıyla birebir uyumunu kontrol etme.
4. Provider fallback ve hata dayanımı gözden geçirme.

### Çıktı

- Gereksiz kod minimize edilmiş, bakım maliyeti düşük kod tabanı.

## Faz 5.5 - Sınıf/Klasör Gruplama Refaktörü

### Hedef

Aynı domain içindeki sınıfları daha okunabilir ve sürdürülebilir klasör yapısına taşımak.

### İlk Dalga (Low Risk)

1. Billing gateway sınıflarını tek klasörde toplama:
   - `src/Billing/StripeCheckoutGateway.php` -> `src/Billing/Gateways/StripeCheckoutGateway.php`
   - `src/Billing/PolarCheckoutGateway.php` -> `src/Billing/Gateways/PolarCheckoutGateway.php`
   - `src/Billing/LemonSqueezyCheckoutGateway.php` -> `src/Billing/Gateways/LemonSqueezyCheckoutGateway.php`
2. Namespace güncellemesi:
   - `Afsakar\Satishub\Billing\Gateways\...`
3. Config referanslarını güncelleme:
   - `config/satishub.php` içindeki `gateway` class referansları.

### İkinci Dalga (Orta Risk)

1. `src/Services` altında domain alt klasörleme:
   - `Services/Billing/*`
   - `Services/Support/*`
   - `Services/Documentation/*`
2. Event sınıflarını domain altına ayırma (opsiyonel):
   - `Events/Billing/*`, `Events/Support/*`

### Refaktör Kuralları

- Public contract'i kırmamak için önce import ve config güncelle, sonra dosya taşı.
- Her dalga sonrası Pint + hedef test seti zorunlu.
- Class rename/namespace change adımlarını changelog'a not et.

## Faz 6 - Release Gate

### Minimum Geçiş Kriterleri

1. Kod stili:

```bash
vendor/bin/pint --format agent
```

2. Paket testleri:

```bash
php artisan test --compact satis/tests/Feature
```

3. Kritik akış smoke testleri:
- webhook -> order -> license
- support ticket open/reply (+ attachment)
- repository auth + dist link

4. Doküman tutarlılığı:
- README, docs, llms dosyaları senkron.

## Riskler ve Azaltma

- **Risk:** Testbench geçişinde auth/model wiring kırılması.
  - **Azaltma:** Önce küçük bir test dosyasıyla POC, sonra kademeli migration.

- **Risk:** Provider API farklı payload varyasyonları.
  - **Azaltma:** Provider-specific fixture testlerini artırma.

- **Risk:** Release sırasında docs-code drift.
  - **Azaltma:** Release öncesi docs checklist zorunlu adımı.

## Önerilen Uygulama Sırası

1. Faz 1 + Faz 2
2. Faz 3
3. Faz 4 (Testbench)
4. Faz 5
5. Faz 6 (release gate + tag)

## Kabul Kriteri (Done Definition)

- `satis/` bağımsız repo olarak clone edilip kurulabiliyor.
- Test suite host app olmadan koşabiliyor.
- Yeni event yüzeyi dokümante ve test ile doğrulanmış.
- LLM dosyaları mevcut davranışla uyumlu.
- Release checklist tam geçiyor.
