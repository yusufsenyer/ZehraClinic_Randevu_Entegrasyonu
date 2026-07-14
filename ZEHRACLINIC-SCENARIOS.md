# ZehraClinic — Senaryo Bazlı Test Komutları

Bu dosya, mobil uygulamadaki her özelliği izole şekilde test etmek için hazırlanmış
`npm run scenario:...` komutlarını listeler.

## Nasıl Kullanılır

- Backend'in çalışıyor olması gerekir: `cd backend && rails server -b 0.0.0.0 -p 3000`
- Seed verisi yüklü olmalı: `rails db:seed`
- Senaryolar backend API'ye doğrudan istek atar; UI navigasyon adımları manuel test için dokümante edilmiştir.
- Tüm senaryoları listelemek için: `npm run scenario:list`
- Tek senaryo çalıştırmak için: `npm run scenario:<ad>`

## Kimlik Doğrulama

### `npm run scenario:auth-email-register`
# Email/parola ile kayıt akışını test eder, yeni kullanıcı oluşturup token alınmasını doğrular.

### `npm run scenario:auth-email-login`
# Email/parola ile giriş akışını test eder, geçerli kullanıcıyla HomeScreen'e ulaşmayı doğrular.

### `npm run scenario:auth-forgot-password`
# Şifre sıfırlama akışını test eder, Devise mailer üzerinden reset linki gönderilmesini doğrular.

### `npm run scenario:auth-google-login`
# Google OAuth ile giriş akışını simüle eder, backend /auth/google/mobile endpoint'ini doğrular.

### `npm run scenario:auth-apple-login`
# Apple OAuth ile giriş akışını simüle eder, backend /auth/apple/mobile endpoint'ini doğrular.

### `npm run scenario:onboarding-welcome-to-home`
# WelcomeScreen → Login → HomeScreen onboarding akışını test eder.

## Gebelik Takibi

### `npm run scenario:pregnancy-weekly-tracking`
# Mevcut haftanın takip verisini API üzerinden görüntüler, hafta bilgisini doğrular.

### `npm run scenario:pregnancy-measurement-add`
# Haftalık değişkenlere ölçüm ekleme akışını test eder, API POST ile kaydı doğrular.

### `npm run scenario:pregnancy-ultrasound-upload`
# Ultrason görseli yükleme endpoint'ini test eder, Active Storage/S3 entegrasyonunu doğrular.

### `npm run scenario:pregnancy-baby-mother-info`
# Bebek ve anne bilgi ekranları arası geçiş akışını test eder, API verilerini doğrular.

### `npm run scenario:pregnancy-calendar-view`
# Gebelik takvimi görüntüleme akışını dokümante eder.

## Doktor & Randevu

### `npm run scenario:doctor-list-filter`
# Uzmanlık filtresiyle doktor listeleme akışını test eder, search/specialty parametrelerini doğrular.

### `npm run scenario:doctor-detail-availability`
# Doktor detayı ve müsaitlik takvimi görüntüleme akışını test eder.

### `npm run scenario:booking-create-pending`
# Randevu oluşturma akışını test eder, pending durumuna düşüldüğünü doğrular.

### `npm run scenario:booking-doctor-approve`
# Doktorun randevuyu onaylamasını test eder, confirmed durumuna geçişi doğrular.

### `npm run scenario:booking-doctor-propose`
# Doktorun yeni saat teklif etmesini test eder, doctor_proposed durumuna geçişi doğrular.

### `npm run scenario:booking-patient-counter-offer`
# Hastanın doktor teklifine karşı teklif vermesini test eder, patient_proposed durumuna geçişi doğrular.

### `npm run scenario:booking-reject`
# Doktorun randevuyu reddetmesini test eder, rejected durumuna geçişi doğrular.

### `npm run scenario:booking-dashboard-manage`
# DoctorDashboard üzerinden randevu yönetimi ve müsaitlik ayarlarını test eder.

## Eğitim

### `npm run scenario:education-course-enroll`
# Kursa kayıt olma akışını test eder, API POST /courses/:id/enrollments ile kaydı doğrular.

### `npm run scenario:education-video-watch-progress`
# Video izleme ve ilerleme güncelleme akışını dokümante eder.

## Ödeme & Mağaza

### `npm run scenario:payment-checkout-success`
# Başarılı ödeme akışını test eder, API POST /user_payments ile ödeme başlatmayı doğrular.

### `npm run scenario:payment-checkout-failure`
# Başarısız ödeme senaryosunu dokümante eder (3D Secure hata callback, geçersiz kart).

### `npm run scenario:store-subscription-purchase`
# Paket/abonelik satın alma akışını test eder, API /packages listesini doğrular.

### `npm run scenario:invoices-view`
# Fatura listesi görüntüleme akışını test eder, API GET /invoices ile veriyi doğrular.

## Danışmanlık

### `npm run scenario:consultation-start`
# Online danışma başlatma akışını dokümante eder (şu an "Yakında" olarak işaretli).

### `npm run scenario:consultation-video-call`
# Görüntülü görüşme ekranı akışını test eder, Stream Video token endpoint'ini doğrular.

## Profil

### `npm run scenario:profile-view-edit`
# Profil görüntüleme/güncelleme akışını test eder, API /me/profile endpoint'ini doğrular.

## Hata / Uç Durumlar

### `npm run scenario:booking-unavailable-slot`
# Dolu veya müsait olmayan saat seçmeye çalışınca 409 Conflict döndüğünü doğrular.

### `npm run scenario:auth-invalid-credentials`
# Hatalı email/parola ile giriş denendiğinde 401 Unauthorized döndüğünü doğrular.

### `npm run scenario:network-offline-fallback`
# Ağ bağlantısı olmadığında uygulamanın Redux cache ile çalışmaya devam ettiğini dokümante eder.

---

## Kapsam Dışı / Not

- **AcademyLoginScreen:** Ayrı bir eğitim giriş ekranı, ana auth akışıyla aynı backend'i kullanır. Ayrı senaryo üretilmemiştir.
- **AdminFeatureFlagsScreen:** Admin paneline özel feature flag yönetimi, düzenli kullanıcı akışı değildir. Ayrı senaryo üretilmemiştir.
- **Eski/Kullanılmayan Ekranlar:** `AppointmentPaymentScreen`, `CalendarScreen` (kök), `AppointmentDoctorScreen` vb. navigasyon kaydı olmadığı için senaryo kapsamı dışındadır.
- **Deep Link / Universal Link:** Mobil uygulamada aktif deep link yapısı bulunmamaktadır (sadece web için `window.__NAV_REF__` tanımlıdır).
- **Bildirimler (Push Notification):** Uygulamada push notification altyapısı kurulu değildir.
- **Screenshot Otomasyonu:** `scripts/take-screenshot.js` web platformu için mevcuttur ancak standalone bir senaryo değildir.
