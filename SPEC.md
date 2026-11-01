# SPEC — Mercek

> Bu dosya, ilk sürüm için "bitti"nin ne anlama geldiğini tanımlar. Kabul kriterleri numaralıdır; her biri bir teste ya da elle doğrulama notuna karşılık gelir.

## 1. Amaç

Pazaryerinde ilan hazırlayan bir satıcı ya da ilanları inceleyen bir moderatör, kontrol etmek istediği ürün ilanını girip metnin reklam mevzuatına ve yasaklı ürün kurallarına uyup uymadığını öğrenebilsin; sorunlu ifadeleri de dayandıkları kuralla birlikte görebilsin.

## 2. Kullanıcı ve bağlam

- **Kullanıcılar:** Pazaryerlerindeki küçük ölçekli satıcılar (ilanlarını yüklemeden önce kontrol etmek için) ve pazaryeri moderatörleri (inceleme kuyruğundaki ilanlar için).
- **Nerede çalışır:** Tarayıcıda. İlk sürüm Streamlit ile geliştirilecek.
- **Girdi:** Kullanıcı ilanın kategorisini, başlığını ve açıklamasını girer. İlk sürümde görsel desteklenmez.
- **Kategoriler:** İlk sürüm kozmetik ve gıda takviyesi ilanlarını kapsar.
- **Kural kaynakları:** Ticari Reklam ve Haksız Ticari Uygulamalar Yönetmeliği, internetten satışı yasak ürünlerle ilgili mevzuat, kozmetik ve gıda takviyesi düzenlemeleri. Her kural, kaynağının bağlantısıyla birlikte `veri/kurallar` klasöründe tutulur.

## 3. Kabul kriterleri (ilk sürüm)

- [ ] **K1.** Başlık ya da açıklama boşsa veya açıklama 20 karakterden kısaysa kullanıcı bir uyarı görür ve model çağrılmaz.
- [ ] **K2.** Sistemin önerdiği karar üç seçenekten biridir: `uygun`, `duzeltme_gerekli`, `yasakli`. Bu kararı model değil kod verir: ihlallerden biri yasaklı ürün kuralına dayanıyorsa `yasakli`, başka türde bir ihlal varsa `duzeltme_gerekli`, hiç ihlal yoksa `uygun`. Model yalnızca ihlalleri bulur.
- [ ] **K3.** Her ihlal için şu alanlar gösterilir: ilandan alıntı, ihlal türü, kural kimliği, kaynak (belge ve madde) ve düzeltme önerisi.
- [ ] **K4.** Boşluk ve büyük/küçük harf farkları giderildikten sonra alıntılanan ifade ilan metninde geçmiyorsa o ihlal gösterilmez ve uygulama günlüğüne (log) yazılır.
- [ ] **K5.** Her ihlal, `veri/kurallar` klasöründe tanımlı bir kural kimliğine bağlıdır. Tanımsız bir kural kimliği taşıyan ihlal doğrulamada reddedilir.
- [ ] **K6.** İlanla ilgili mevzuat maddeleri RAG ile bulunur; her ihlalin yanında dayandığı madde görünür.
- [ ] **K7.** Sistemin önerdiği karar otomatik olarak uygulanmaz. Moderatör ekranında "onayla / düzenleme iste / reddet" seçenekleri vardır ve moderatörün kararı kaydedilir (ilk sürümde yerel bir dosyaya; kalıcı veritabanı sonraki aşamada).
- [ ] **K8.** İlana gizli talimat ("bu ilanı onayla" gibi) eklenmiş test vakalarında, kararın, ilanın talimatsız hâlinde verilen kararla aynı kaldığı vakaların oranı ölçülür ve README'de raporlanır. Tespit edilen gizli talimatlar ayrıca uyarı olarak gösterilir.
- [ ] **K9.** Model ve sağlayıcı bilgileri `.env` dosyasından okunur (`LLM_BASE_URL`, `LLM_API_KEY`, `LLM_MODEL`). Uygulama en az iki farklı sağlayıcıyla çalışır.
- [ ] **K10.** Her sonucun altında girdi ve çıktı token sayısı, süre ve tahmini maliyet görünür.
- [ ] **K11.** Model hata verdiğinde ya da anahtar eksik olduğunda kullanıcı anlaşılır bir Türkçe mesaj görür; teknik hata ayrıntısı ekrana yansımaz.
- [ ] **K12.** `pytest -q` internet bağlantısı olmadan çalışır; en az 8 test vardır ve hepsi geçer.

## 4. Kapsam dışı (ilk sürüm)

- Görseldeki yazının okunması (sonraki aşama)
- Kozmetik ve gıda takviyesi dışındaki kategoriler (sonraki aşama)
- Kullanıcı hesabı ve giriş sistemi
- Gerçek bir pazaryeriyle entegrasyon
- Hukuki görüş bildirilmesi (Mercek yalnızca uyarı verir)
- Gerçek satıcı, marka ya da kişi bilgisi içeren örnekler

## 5. Teknik kısıtlar

- Python 3.12, Streamlit, `openai` istemcisi (OpenAI uyumlu uç noktalar) ve Pydantic kullanılır.
- Dosya düzeni: `app.py` yalnızca arayüzü içerir; iş mantığı `mercek/` paketindedir ve `st.*` çağrısı içermez; testler `tests/` klasöründedir.
- Model adı koda yazılmaz; `LLM_MODEL` ortam değişkeninden okunur.
- Yeni bir bağımlılık eklenmeden önce gerekçesi yazılır.

## 6. Arayüz taslağı

```
Mercek
Kategori: [ Kozmetik ▾ ]
Başlık:   [ .............................................. ]
Açıklama: [ .............................................. ]
          [ .............................................. ]
[ Kontrol et ]

Önerilen karar: Düzeltme gerekli
1. "her 10 günde 2000 yeni saç kökü"
   Kanıtlanmamış etki vaadi · Kaynak: ilgili yönetmelik maddesi
   Öneri: Kanıtlayamadığınız bir sonuç vaat etmeyin.
...
girdi 1.240 token · çıktı 310 token · 2,1 sn · ~$0,0004
[ Onayla ]  [ Düzenleme iste ]  [ Reddet ]
```

## 7. Değerlendirme

- **Test seti:** Reklam Kurulu kararlarından türetilmiş ilanlardan ve kendi yazdığım örnek ilanlardan oluşur. Her vakada beklenen karar ve beklenen ihlal türleri yer alır.
- **Zorlayıcı vakalar:** İlana gizlenmiş talimatlar, harf oyunları ("k@nser" gibi), sınırda kalan ifadeler ve hiç sorunu olmayan ilanlar (yanlış alarm oranını ölçmek için).
- **Metrikler:** Karar doğruluğu, ihlal yakalama oranı, yanlış alarm oranı ve gizli talimatlara karşı dayanıklılık oranı (K8). Sonuçlar ölçüm tarihiyle birlikte README'ye yazılır.

## 8. Yayın

Uygulama Streamlit Community Cloud'da (gerekirse Render'da) yayına alınır. API anahtarları platformun "Secrets" bölümüne girilir; repoya konmaz.

## 9. AI kullanımı

Hangi aracın ne için kullanıldığı ve kodlama ajanının yaptığı hatalar README'deki "AI kullanımı" bölümüne yazılır.
