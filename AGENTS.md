# AGENTS.md — bu repoda çalışan kodlama ajanları için kurallar

## Proje
- Mercek: pazaryeri ilanlarını reklam mevzuatına ve yasaklı ürün kurallarına göre kontrol eden asistan
- Python 3.12 · Streamlit · `openai` istemcisi (OpenAI uyumlu, sağlayıcıdan bağımsız) · Pydantic
- Dosya düzeni: `app.py` (yalnızca arayüz) · `mercek/` (iş mantığı, `st.*` YOK) · `tests/` (ağsız) · `veri/` · `evals/`
- Spec: `SPEC.md`. Kabul kriterleri K1, K2… diye numaralıdır; bir kriteri karşılayan her değişiklik ilgili testi de ekler.

## Yapılacaklar
- Değişiklikten önce planı 3-5 madde halinde yaz, onay bekle
- Her kabul kriteri için en az bir test yaz; `pytest -q` yeşil olmadan "bitti" deme
- Model çağrılarını `mercek/llm.py` içinde topla; testlerde taklit et (mock)
- İlan metnini modele her zaman içerik olarak ver, sistem talimatıyla karıştırma
- Hata mesajları Türkçe ve anlaşılır olsun; traceback arayüze sızmasın
- Fonksiyonlara kısa Türkçe docstring yaz
- Küçük, açıklayıcı commit'ler at: `feat: K3 ihlal kartı`, `test: K4 alıntı doğrulaması`

## Yapılmayacaklar
- `.env`, `secrets.toml`, API anahtarı ya da token'ı hiçbir dosyaya yazma, log'lama, commit'leme
- Sormadan yeni bağımlılık ekleme
- `SPEC.md`'deki kapsam dışı maddeleri yapma
- Testi geçirmek için testi silme ya da atlama (`skip`)
- Model adını koda yazma; `LLM_MODEL` ortam değişkeninden oku
- Gerçek kişi, satıcı ya da marka bilgisi içeren örnek veri üretme

## Bittiğinde
- `pytest -q` çıktısını göster
- README'deki "AI kullanımı" bölümüne ne yaptığını 3 maddeyle yaz
- Değiştirdiğin dosyaları listele
