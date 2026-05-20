# 🍔 MisYemek Platformu



MisYemek, yerel restoran ekosistemini dijital ortama taşıyan, kullanıcı odaklı, yüksek etkileşimli ve gerçek zamanlı kurye takibi sunan açık kaynaklı bir yemek sipariş ve teslimat platformudur. 

Proje, herhangi bir ağır JavaScript framework bağımlılığı taşımaksızın **saf (Vanilla) HTML5, CSS3 ve modern JavaScript (ES Modules)** standartları kullanılarak Tek Sayfa Uygulama (SPA - Single Page Application) mimarisinde geliştirilmiştir. Bulut veritabanı ve kimlik doğrulama için **Firebase**, harita ve coğrafi işlemler için ise **Leaflet.js** ve **OpenStreetMap** altyapısından yararlanılmıştır.

---

## 🚀 Öne Çıkan Özellikler

*   📍 **GPS Tabanlı Konum Algılama:** Tarayıcının Geolocation API'si üzerinden kullanıcının anlık konumu alınır.
*   📏 **Haversine Algoritması ile Mesafe Hesaplama:** Kullanıcının konumu ile platformdaki 24 restoran arasındaki mesafe Dünya'nın küresel yarıçapı temel alınarak anlık hesaplanır ve en yakın restoranlar otomatik olarak üstte listelenir.
*   🗺️ **Gerçek Zamanlı Rota Çizimi & Kurye Simülasyonu:** Leaflet Routing Machine ve Carto CDN altyapısıyla restorandan kullanıcı adresine en uygun rota çizilir ve kurye aracı rota üzerinde animasyonlu olarak hareket eder.
*   ⏱️ **Dinamik Teslimat Süresi:** Rota uzunluğuna göre teslimat süresi canlı olarak hesaplanır ve kullanıcıya dakika bazında sunulur.
*   🔐 **Firebase Kimlik Doğrulama:** E-posta/Şifre tabanlı güvenli kayıt ve giriş sistemi. Sayfa yenilemelerinde `onAuthStateChanged` ile oturum sürekliliği (Session Persistence).
*   📁 **Cloud Firestore ile Güvenli Veri Yönetimi:** NoSQL mimarisi üzerinde kullanıcı profilleri, çoklu adres tanımları, kart yönetim sistemleri ve sipariş geçmişi izole ve güvenli biçimde saklanır.
*   🛒 **Düşük Sürtünmeli Kullanıcı Akışı:** Kullanıcılar hesap oluşturmadan restoran ve menü keşfi yapabilir; kimlik doğrulama yalnızca sipariş verme aşamasında zorunlu kılınır.
*   💳 **Kullanıcı Ekosistemi & Canlı Kart Ön İzleme:** Profil düzenleme, en fazla 3 teslimat adresi kaydetme, canlı kart tasarımı ön izlemeli ödeme kartı kaydetme ve geçmiş siparişleri listeleme.
*   📊 **Restoran & Yönetici Paneli (`/admin`):** Sipariş ve restoran takibini kolaylaştıran, özel bypass (demo) ve Firestore entegrasyonuna sahip yönetim arayüzü.

---

## 🛠️ Kullanılan Teknolojiler

### Ön Yüz (Frontend)
*   **HTML5 & CSS3:** CSS Custom Properties (değişkenler), CSS Grid ve Flexbox düzen sistemleri ile tamamen duyarlı (Responsive) tasarım. Mobil, tablet ve masaüstü uyumlu.
*   **Vanilla JS (ES2022+):** Modüler dosya yapısı (`type="module"`), asenkron API yönetimi (`async/await`) ve event-driven mimari.

### Harita & Lojistik Modülleri
*   **Leaflet.js (v1.9.4):** İnteraktif harita motoru.
*   **Leaflet Routing Machine:** Rota ve mesafe hesabı.
*   **Carto Voyager CDN:** Localhost ortamında da kesintisiz çalışan harita katmanı.
*   **Flaticon CDN:** Harita üzerindeki özel kurye ve kullanıcı ikonları.

### Arka Yüz & Bulut Servisleri (BaaS)
*   **Firebase Authentication:** Kullanıcı kayıt/giriş işlemleri.
*   **Cloud Firestore:** Gerçek zamanlı NoSQL veritabanı.
*   **Font Awesome (v6.4.0):** Vektör ikon desteği.
*   **Unsplash API:** Optimize edilmiş restoran kapak ve ürün görselleri.

---

## 📁 Proje Dosya Yapısı

```
misyemek/
├── .vscode/                 # Editor yapılandırma ayarları
├── admin/                   # Yönetici Paneli
│   ├── css/
│   │   └── style.css        # Yönetici paneli özel CSS dosyası
│   ├── js/
│   │   └── main.js          # Yönetici paneli veri akışı ve Firestore JavaScript dosyası
│   ├── dashboard.html       # Yönetici paneli ana kontrol ekranı
│   └── index.html           # Yönetici paneli giriş arayüzü
├── index.html               # Müşteri paneli (Ana Uygulama - SPA)
└── README.md                # Proje tanıtım ve kurulum dokümanı
```

---

## 🛢️ Veritabanı Şeması (Firestore)

Cloud Firestore üzerinde kullanıcı verileri `users/{userId}` belgesi altında aşağıdaki şemaya uygun olarak hiyerarşik NoSQL yapısında tutulur:

```json
{
  "firstname": "Ahmet",
  "lastname": "Yılmaz",
  "email": "ahmet@example.com",
  "phone": "5551234567",
  "birthdate": "1995-05-20",
  "addresses": [
    {
      "id": "1716215160000",
      "title": "Ev",
      "detail": "Çankaya Mh. Atatürk Cd. No:12 D:4 Ankara"
    }
  ],
  "cards": [
    {
      "id": "1716215260000",
      "holder": "AHMET YILMAZ",
      "number": "**** **** **** 4321",
      "expiry": "12/28"
    }
  ],
  "orders": [
    {
      "id": "MS-98246",
      "restaurant": "Burger Station",
      "date": "20.05.2026 18:00",
      "items": "2x Cheese Burger, 1x Patates Cipsi",
      "total": 480.00
    }
  ],
  "orderCount": 1
}
```

Firestore güvenlik kuralları (Rules), kullanıcıların yalnızca kendi kimlik bilgileriyle eşleşen belgelere erişmesine izin verecek şekilde konfigüre edilmiştir:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

---

## ⚙️ Kurulum ve Çalıştırma

Uygulama statik dosyalardan oluştuğu için yerel bilgisayarınızda çalıştırmak oldukça basittir. Ancak **Firebase SDK (ES Modules)** ve **Leaflet/Carto CDN** entegrasyonlarının CORS politikalarına takılmadan sağlıklı çalışabilmesi için yerel bir HTTP sunucusu üzerinden çalıştırılması zorunludur.

### Adım 1: Depoyu Klonlayın
```bash
git clone https://github.com/tah4mis/misyemek.git
cd misyemek
```

### Adım 2: Yerel Sunucu Başlatın (Önerilen Yöntemler)

*   **VS Code Live Server (En Pratik):**
    1. Proje klasörünü VS Code ile açın.
    2. Sağ alttaki **"Go Live"** butonuna tıklayın.
    3. Tarayıcınızda otomatik olarak `http://127.0.0.1:5500` adresinde uygulama açılacaktır.

*   **Python ile Basit Sunucu:**
    ```bash
    # Python 3+
    python -m http.server 8000
    ```
    Tarayıcınızdan `http://localhost:8000` adresine gidin.

*   **Node.js http-server ile:**
    ```bash
    npm install -g http-server
    http-server -p 8080
    ```
    Tarayıcınızdan `http://localhost:8080` adresine gidin.

### Adım 3: Yönetici Paneline Erişim
Yönetici paneline erişmek için tarayıcıda adres satırının sonuna `/admin` ekleyin (Örn: `http://localhost:8000/admin`).
*   **Demo Yönetici Giriş Bilgileri:**
    *   **E-posta:** `admin@misyemek.com`
    *   **Şifre:** `admin123`

---

## 🎯 Projenin Mevcut Durumu (TRL 6–7)

*   **Arayüz & Responsive Tasarım:** %100 (Tamamlandı)
*   **Konum & GPS Harita Entegrasyonu:** %100 (Tamamlandı)
*   **Firebase Authentication & Firestore:** %100 (Tamamlandı)
*   **Sepet & Sipariş Akışı:** %100 (Tamamlandı)
*   **Kullanıcı Profili & Adres/Kart Yönetimi:** %100 (Tamamlandı)
*   **Gerçek Zamanlı Kurye Takip Simülasyonu:** %100 (Tamamlandı)
*   **Entegrasyon ve Kararlılık Testleri:** %70 (Devam Ediyor)

*Gelecek planlamasında gerçek bir ödeme altyapısı (Stripe/İyzico) entegrasyonu ve dinamik bir restoran paneli yer almaktadır.*

---

## 📄 Lisans

Bu proje **MIT Lisansı** ile lisanslanmıştır. Daha fazla bilgi için `LICENSE` dosyasına göz atabilirsiniz.
