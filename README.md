# Mercek

Pazaryeri ilanlarını yayına çıkmadan önce reklam mevzuatına ve yasaklı ürün kurallarına göre kontrol eden asistan.

**Canlı demo:** henüz yok · **Video:** henüz yok · **Durum:** geliştirme aşamasında

## Problem ve kullanıcı

Pazaryerlerine her gün çok sayıda yeni ürün ilanı yükleniyor. Bunların bir kısmında kanıtı olmayan sağlık vaatleri, "%100 garanti" gibi yanıltıcı ifadeler ya da internetten satışı yasak ürünler bulunuyor. Ticaret Bakanlığı Reklam Kurulu'nun Temmuz 2026'da duyurulan toplantısında incelenen 149 dosyanın 118'i mevzuata aykırı bulundu ([kaynak](https://ticaret.gov.tr/haberler/ticaret-bakanligi-reklam-kurulu-371-toplantisini-gerceklestirdi-kurul-yaniltici-reklamlara-karsi-mucadelesini-kararlilikla-surduruyor)).

Mercek iki tür kullanıcı için tasarlanıyor:

- **Satıcı:** İlanını yüklemeden önce kontrol ettirir ve sorunlu ifadeleri düzeltir.
- **Moderatör:** Şüpheli ilanları dayandıkları kuralla birlikte görür; kararı kendisi verir.

İlk sürüm kozmetik ve gıda takviyesi ilanlarına odaklanıyor.

## Nasıl çalışacak

1. Kullanıcı ilanın kategorisini, başlığını ve açıklamasını girer.
2. Sistem, mevzuat metinleri içinden ilanla ilgili maddeleri bulur (RAG).
3. Model, ilanı bu maddelere ve yasaklı ürün listesine göre değerlendirir.
4. Sonuç bir şemaya göre doğrulanır: her ihlal gerçek bir kurala bağlı olmalı, alıntılanan ifade ilanda birebir geçmeli.
5. Sistemin önerdiği karar (uygun / düzeltme gerekli / yasaklı) bulunan ihlallerden kodla türetilir. Sorunlu ifadeler, ilgili kural ve düzeltme önerisiyle birlikte gösterilir. Son kararı moderatör verir.

Ayrıntılar: [SPEC.md](SPEC.md) · [Kullanım senaryosu](docs/kullanim_senaryosu.md)

## Kurulum ve çalıştırma

İlk çalışan sürümle birlikte eklenecek.

## Değerlendirme

Henüz sonuç yok. Plan: Reklam Kurulu kararlarından ve kendi hazırladığım örnek ilanlardan oluşan bir test seti. Karar doğruluğu, ihlal yakalama oranı ve yanlış alarm oranı, ölçüm tarihiyle birlikte burada paylaşılacak.

## Maliyet

Henüz ölçülmedi. İstek başına ve aylık maliyet tablosu ilk sürümden sonra eklenecek.

## Güvenlik ve yönetişim

- Sistem öneri üretir; hiçbir ilanı kendi başına yasaklamaz.
- İlan metni modele talimat olarak değil, incelenecek içerik olarak verilir. İlana gizlenmiş talimatlar ayrıca işaretlenir.
- API anahtarları yalnızca ortam değişkenlerinde tutulur, repoda yer almaz.
- Örnek ilanlarda gerçek satıcı ya da kişi bilgisi kullanılmaz.
- Mercek hukuki görüş bildirmez; uyarıları bilgilendirme amaçlıdır.

## AI kullanımı

Bu bölüm proje ilerledikçe güncellenecek.

- Proje fikrinin değerlendirilmesi ve bu repodaki ilk belgelerin (README, SPEC.md, AGENTS.md, docs/kullanim_senaryosu.md) taslakları Claude (Anthropic) ile birlikte hazırlandı.

## Yol haritası

- [ ] İlk sürüm: metin tabanlı kontrol, kaynak gösterme, moderatör onayı
- [ ] Test seti ve GitHub Actions'ta otomatik değerlendirme
- [ ] Ücretsiz bir platformda yayın
- [ ] Sonraki aşama: görseldeki yazının okunması, elektronik ürünlerde zorunlu bilgi (CE işareti vb.) kontrolü

## Lisans

[MIT](LICENSE). `AGENTS.md`, ders reposundaki ([sektor-kampuste-git](https://github.com/sektor-kampuste-git/sektor-kampuste-git), MIT) şablondan uyarlanmıştır.
