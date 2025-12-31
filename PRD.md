# BlogYapay - PRD

Kullanıcı elinde 100 den fazla gmail hesabı var, gmail ile bağlantı kuracak..otomatik blogger hesabı açacak örnek bir haber blogger ı. buna uygun tema ai ile üretilecek, ai ile içerik üretielcek (yazı resim, video vs) ve post gönderecek. günlük atacak. reklam kısmı olacak. adsense takibi vs olacak. reklam yerleşimi olacak. 

## 1. Amaç ve Vizyon
*   **Otomasyon ve Verimlilik:** Kullanıcının manuel içerik üretim yükünü hafifletip, çoklu Blogger hesaplarını tek bir merkezi panelden yönetilebilir hale getirmek.
*   **Özgün İçerik Üretimi:** Gelişmiş yapay zeka algoritmaları ile kopya içerik (duplicate content) sorununu ortadan kaldıran, SEO uyumlu ve benzersiz blog yazıları oluşturmak.
*   **Pasif Gelir Artışı:** Sürekli ve kaliteli içerik akışı sağlayarak, blog ağlarının organik trafiğini artırmak ve uzun vadeli gelir potansiyelini yükseltmek.
*   **Zaman Tasarrufu:** Araştırma, yazma ve yayınlama süreçlerini otomatize ederek kullanıcının stratejik planlamaya odaklanmasına zaman tanımak.
*   **Ölçeklenebilirlik:** Tek bir kullanıcı limiti olmasına rağmen, sınırsız sayıda blog hesabını sisteme dahil ederek dijital varlıkların ölçeklenebilirliğini sağlamak.

## 2. Hedef Kullanıcılar
*   **Niş Blog Yazarları:** Belirli konularda otorite kurmak ve birden fazla micro-niche blog yöneterek gelir elde etmek isteyen bireyler.
*   **SEO Uzmanları ve Dijital Pazarlamacılar:** Backlink stratejileri oluşturmak veya affiliate marketing için geniş bir blog ağına (PBN - Private Blog Network) ihtiyaç duyan profesyoneller.
*   **İçerik Üreticileri:** Fikir üretme aşamasında zorlanan veya yazma hızını artırmak isteyen, yapay zekadan destek bekleyen yazarlar.
*   **Girişimciler ve KOBİ Sahipleri:** Kendi markalarının bloglarını otomatik yöneterek marka bilinirliğini artırmak isteyen ancak yeterli zamanı olmayan işletme sahipleri.
*   **Passive Income (Pasif Gelir) Arayanlar:** Yatırım olarak blog ağları kuran ve bu ağların bakımını minimum seviyede tutarak kazanç sağlamayı hedefleyen bireyler.

## 3. Özellikler ve Fonksiyonlar
*   **Çoklu Hesap Yönetimi:** Farklı e-posta adreslerine sahip sınırsız sayıda Blogger hesabının API ile sisteme güvenle bağlanabilmesi ve listelenmesi.
*   **Gelişmiş İçerik Editörü:** Yapay zeka tarafından oluşturulan metinlerin, görsellerin ve etiketlerin yayınlanmadan önce incelenip düzenlenebileceği bir arayüz.
*   **Akıllı İçerik Planlayıcısı:** Belirli tarihlerde ve saatlerde otomatik yayınlanmak üzere içeriklerin kuyruğa alınabileceği (scheduling) zamanlama sistemi.
*   **SEO Optimizasyon Araçları:** Anahtar kelime yoğunluğu kontrolü, meta description oluşturma ve başlık (H1, H2, H3) analizi yapan entegre SEO asistanı.
*   **Dinamik Şablon Sistemi:** Her blog için farklı tonlama (resmi, samimi, teknik vb.) ve format (liste, rehber, haber vb.) belirleyebilen özelleştirilebilir yazı şablonları.
*   **Otomatik Görsel Entegrasyonu:** Yazı konusuyla alakalı, telifsiz stok fotoğrafların veya yapay zeka ile üretilen görsellerin içeriğe otomatik eklenmesi.
*   **Raporlama ve Analitik:** Hangi blogun ne kadar içerik ürettiği, yayın durumları ve temel istatistikleri gösteren detaylı dashboard.

## 4. Teknik Gereksinimler
*   **Blogger API Entegrasyonu:** Google Blogger API v3 ile tam uyumlu çalışarak post oluşturma, güncelleme, silme ve medya yükleme işlemlerini gerçekleştirebilecek altyapı.
*   **Yapay Zeka API Bağlantısı:** İçerik üretimi için OpenAI (GPT-4), Claude veya Anthropic gibi LLM sağlayıcılarıyla güvenli ve hızlı iletişim kuran modüller.
*   **Veritabanı Yönetimi:** Kullanıcı bilgileri, blog hesap tokenları, içerik taslakları ve yayın planlarını saklayacak, güvenli ve ölçeklenebilir bir veritabanı (örn. PostgreSQL veya MongoDB).
*   **Güvenlik Protokolleri:** API anahtarlarının ve kullanıcı giriş bilgilerinin şifrelendiği (Encryption at Rest), OAuth 2.0 ile yetkilendirme işlemlerinin yapıldığı güvenli bir sistem.
*   **Asenkron İşlem Kuyruğu:** Arka planda içerik üretimi ve yayınlama işlemlerinin sunucuyu yormadan gerçekleştirilmesi için Redis veya BullMQ gibi kuyruk sistemleri.
*   **Hata Yönetimi ve Loglama:** API hatalarında (rate limit, bağlantı kesintisi) sistemi çökertmeden otomatik yeniden deneme (retry) mekanizmaları ve detaylı loglama.

## 5. Kullanıcı Arayüzü (UI/UX)
*   **Dashboard (Genel Bakış):** Tüm blogların durumunu, sıradaki yayınları ve son aktiviteleri tek ekranda özetleyen, temiz ve anlaşılır bir panel.
*   **Düzenleyici (Editor):** Sürükle-bırak özellikli, görsel önizlemeli (WYSIWYG) ve yapay zeka önerilerinin (örn: "Metni uzat", "Daha basitleştir") yan panelde göründüğü modern bir editör.
*   **Responsive Tasarım:** Masaüstü, tablet ve mobil cihazlarda kusursuz çalışacak, kullanıcı deneyimini bozmayan esnek yapı.
*   **Karanlık/Aydınlık Mod:** Kullanıcının göz yorgunluğunu önlemek adına tercihe göre değiştirilebilir tema seçenekleri.
*   **İntuitif Akış:** Hesap eklemeden yayınlamaya kadar olan sürecin en az adımla tamamlanabileceği, karmaşık menülerden arındırılmış sade bir navigasyon.
*   **Gerçek Zamanlı Bildirimler:** İçerik yayını başladığında, tamamlandığında veya bir hata oluştuğunda kullanıcıyı bilgilendiren toast mesajları ve bildirimler.

## 6. Başarı Metrikleri
*   **İçerik Üretim Hızı:** Kullanıcının günde kaç adet özgün blog yazısı üretebildiği ve sistemin bu süreyi ne kadar kısalttığı.
*   **Yayın Başarısı:** Planlanan içeriklerin % kaçı belirlenen saatte hatasız bir şekilde yayınlanabildiği (Uptime).
*   **SEO Performansı:** Üretilen içeriklerin Google arama sonuçlarındaki (SERP) sıralama değişimleri ve organik trafik artışı.
*   **Kullanıcı Retention (Bağlılık):** Kullanıcının platformu ne sıklıkla kullandığı ve günlük aktif kullanım süresi.
*   **Hata Oranı:** API bağlantılarında veya içerik üretim süreçlerinde yaşanan teknik aksaklık sayısının minimumda tutulması.

## 7. Zaman Çizelgesi (Aylık Plan)
*   **1. Ay - Analiz ve Tasarım:** İhtiyaçların netleştirilmesi, UI/UX tasarımlarının (Figma) tamamlanması ve veritabanı mimarisinin oluşturulması.
*   **2. Ay - Çekirdek Geliştirme (Backend):** Blogger API entegrasyonunun sağlanması, kullanıcı yetkilendirme sistemi (Auth) ve veritabanı kurulumunun bitirilmesi.
*   **3. Ay - Yapay Zeka Entegrasyonu:** LLM servislerinin bağlanması, içerik üretim motorunun kodlanması ve ilk taslakların oluşturulması.
*   **4. Ay - Frontend Geliştirme:** Kullanıcı arayüzünün kodlanması, backend ile bağlantının sağlanması ve düzenleyicinin (editor) geliştirilmesi.
*   **5. Ay - Test ve Optimizasyon:** Beta testleri, hata (bug) düzeltmeleri, performans iyileştirmeleri ve güvenlik testlerinin yapılması.
*   **6. Ay - Lansman ve İzleme:** Resmi yayın, kullanıcı geri bildirimlerinin toplanması ve gerekli güncellemelerin yapılması.

## 8. Riskler ve Çözümler
*   **Blogger API Kısıtlamaları:** Google'ın günlük API isteği limitini aşma riski. *Çözüm: Akıllı istek yönetimi ve çoklu API hesabı rotasyonu.*
*   **Spam Algılanması:** Çok fazla ve hızlı içerik üretilerek blogların Google tarafından spam olarak işaretlenmesi. *Çözüm: İnsan taklidini yapan doğal yayın aralıkları (randomize scheduling) ve yüksek kalite kontrolü.*
*   **Yapay Zeka Halüsinasyonu:** AI'nın yanlış veya uydurma bilgiler üretmesi. *Çözüm: İçerik yayınından önce kullanıcı onayı mekanizması ve gerçek kaynak doğrulama araçları.*
*   **Hesap Güvenliği:** Kullanıcıların Blogger hesaplarının yetkisiz erişime açık olması. *Çözüm: Şifreleri hiçbir şekilde saklamama, sadece güvenli OAuth 2.0 token kullanımı.*
*   **Teknik Bakım Maliyeti:** Sunucu ve API kullanım maliyetlerinin zamanla artması. *Çözüm: Verimli kod yapısı ve ölçeklenebilir bulut altyapısı (AWS/Google Cloud) kullanımı.*

## 🔧 Seçilen Tech Stack

### Frontend
- **Next.js (App Router)** (Orta)
  - ✅ React tabanlı olduğu için modern UI kitleri ile uyumlu, Sunucu taraflı render (SSR) sayesinde SEO dostu blog sayfaları oluşturur, API Routes ile backend ihtiyacını tek projede karşılayabilir

### Backend / AI Orchestration
- **Python (FastAPI)** (Orta)
  - ✅ Yapay zeka kütüphaneleri (LangChain, OpenAI SDK) ile en entegre çalışan dildir, Asenkron yapısı sayesinde AI cevaplarını bekleme süreçlerinde verimlidir, Tip güvenliği ve otomatik dokümantasyon sağlar

### Veritabanı
- **Supabase (PostgreSQL)** (Kolay)
  - ✅ Açık kaynaklı ve Firebase alternatifi olarak hızlı geliştirme imkanı sunar, İçeriklerin (blog yazıları) yapılandırılmış veri olarak saklanması için idealdir, Entegre yetkilendirme ve depolama (storage) hizmetleri verir

### AI / LLM
- **OpenAI API (GPT-4o)** (Kolay)
  - ✅ Yaratıcı ve blog odaklı uzun metin üretiminde en iyi model kalitesine sahiptir, Kullanımı kolay ve stabil API yanıtları verir, Geniş dil desteği ile Türkçe içerik üretimi başarılıdır
- **Anthropic API (Claude 3.5 Sonnet)** (Kolay)
  - ✅ Uzun bağlam pencereleri (context window) sayesinde detaylı blog postları üretir, Güvenlik ve doğruluk oranları yüksektir

### DevOps
- **Coolify** (Orta)
  - ✅ Self-hosted ve open-source bir PaaS olarak Vercel/Heroku alternatifi sunar, Kendi VPS'iniz üzerinde uygulamanızı ve veritabanını yönetmenizi sağlar, Tek tıkla Docker konteynerları ve reverse proxy kurulumu yapar

