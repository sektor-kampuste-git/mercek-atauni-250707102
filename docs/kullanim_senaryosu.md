# Kullanım senaryosu özeti — Mercek

## 1. Problem

- **Kim yapıyor:** Pazaryeri moderatörleri yeni ilanları inceliyor. Satıcılar ilanlarını kendileri yazıyor ve çoğu zaman reklam mevzuatını bilmiyor.
- **Bugün ne oluyor (varsayımım):** Kurala aykırı ilanlar ya yayına girdikten sonra şikâyetle fark ediliyor ya da reddedilen satıcı nedenini tam anlamadan ilanı yeniden gönderiyor. Bunu henüz bir kaynakla doğrulamadım; satıcılarla ya da moderatörlerle konuşarak doğrulamak istiyorum.
- **Neden şimdi:** Ticaret Bakanlığı Reklam Kurulu yanıltıcı reklamlara yönelik denetimleri sürdürüyor. Temmuz 2026'da duyurulan bir toplantıda incelenen 149 dosyanın 118'i mevzuata aykırı bulundu ([kaynak](https://ticaret.gov.tr/haberler/ticaret-bakanligi-reklam-kurulu-371-toplantisini-gerceklestirdi-kurul-yaniltici-reklamlara-karsi-mucadelesini-kararlilikla-surduruyor)).
- **Henüz bilmediğim:** Bir moderatörün ilan başına harcadığı süre ve günlük ilan sayısı. Güvenilir bir kaynak bulduğumda bu bölümü güncelleyeceğim.

## 2. Çözüm

Satıcı ilanı girer → sistem sorunlu ifadeleri, dayandıkları kural ve düzeltme önerisiyle gösterir → moderatör onaylar, düzenleme ister ya da reddeder.

- **Modelin işi:** İlandaki iddiaları yorumlamak ve ilgili mevzuat maddesiyle eşleştirmek.
- **Kodun işi (model gerekmez):** Boş ya da kısa girdi kontrolü, yasaklı ürün listesinde arama, alıntının ilanda geçip geçmediğinin ve kural kimliğinin doğrulanması, önerilen kararın bulunan ihlallerden türetilmesi.
- **Çıktı:** Tipli (Pydantic). Alanlar: karar, ihlaller (alıntı, ihlal türü, kural kimliği, kaynak, öneri) ve gizli talimat uyarısı.
- **Kapsam:** İlk sürüm kozmetik ve gıda takviyesi ilanlarına odaklanıyor.

## 3. Veri

- **Kaynaklar:** Ticari Reklam ve Haksız Ticari Uygulamalar Yönetmeliği ve ilgili mevzuat (mevzuat.gov.tr), Reklam Kurulu karar duyuruları (ticaret.gov.tr), internetten satışı yasak ürünlere ilişkin düzenlemeler, kozmetik ve gıda takviyesi ürünlerine ilişkin düzenlemeler.
- **Örnek ilanlar:** Kendim yazacağım; gerçek satıcı ya da kişi bilgisi içermeyecek.
- **Kişisel veri:** İlk sürümde işlenmiyor.

## 4. Model ve mimari

- **RAG:** Mevzuat metinleri maddelere bölünüp aranır; bulunan maddeler modele bağlam olarak verilir.
- **Sağlayıcıdan bağımsız istemci:** Gemini (ücretsiz katman) ve dersin OpenRouter anahtarı.
- **Model kademesi:** İlk değerlendirme için ucuz bir model; daha güçlü bir modelin yalnızca belirsiz vakalarda kullanılması düşünülüyor (henüz denenmedi).

## 5. Maliyet ve getiri

Henüz hesaplanmadı. İlk sürümden sonra dersin `roi_hesapla.py` betiğiyle doldurulacak.

## 6. Değerlendirme

- Reklam Kurulu kararlarından türetilmiş ve kendi yazdığım ilanlardan oluşan bir test seti.
- Ölçüler: karar doğruluğu, ihlal yakalama oranı, yanlış alarm oranı.
- Kabul eşiği: İlk ölçümden sonra belirlenecek.

## 7. Risk, yönetişim ve hukuk

- **AB Yapay Zeka Yasası:** İlk değerlendirmeme göre yüksek riskli bir kullanım değil. Kullanıcıya yapay zekayla etkileşimde olduğu söylenecek (şeffaflık). Gerekçe model kartında ayrıntılandırılacak.
- **KVKK:** İlk sürümde kişisel veri işlenmiyor.
- **İnsan onayı:** Hiçbir ilan sistem tarafından otomatik olarak yasaklanmaz; karar moderatöre aittir.
- **Prompt injection:** İlan metni içerik olarak işlenir; ilana gizlenmiş talimatlar ayrıca işaretlenir.
- **Sınır:** Mercek hukuki görüş bildirmez.

## 8. Sahiplik ve yol haritası

- **Ürün ve teknik sahip:** İrem Altun
- **Takvim:** Ders takvimine göre şekillenecek.

## 9. Prototip

Henüz yok.
