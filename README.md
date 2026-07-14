# ZehraClinic

Gebelik takibi, doktor randevu yönetimi ve online danışmanlık sunan bir sağlık teknolojileri platformu.

## Proje Mimarisi

```
ZehraClinic/
├── backend/           # Ana API sunucusu (Ruby on Rails 8)
├── mobile-app/        # Ana mobil uygulama (React Native / Expo)
└── ZehraClinic-Randevu-Sistemi/  # Randevu modülü prototipi
```

---

## Backend

Ruby on Rails 8 API sunucusu. Gebelik takibi, randevu yönetimi, eğitim içerikleri ve ödeme işlemlerini yönetir.

### Teknolojiler

| Teknoloji | Bilgi |
|---|---|
| Ruby | 3.3.0 |
| Rails | ~> 8.0.2 (API mode) |
| Veritabanı | PostgreSQL |
| Auth | Devise + Devise-JWT (JTIMatcher) |
| OAuth | Google, Apple (OmniAuth) |
| Yetkilendirme | Pundit + Rolify |
| State Machine | AASM |
| Dosya Yükleme | Active Storage + AWS S3 |
| Cache / Queue | Solid Cable, Solid Queue, Solid Cache |
| Deploy | Kamal (Docker) |

### Kurulum

```bash
cd backend
bundle install
cp config/database.example.yml config/database.yml
rails db:create db:migrate db:seed
rails server -b 0.0.0.0 -p 3000
```

### API Endpoints

#### Kimlik Doğrulama
- `POST /api/v1/auth/login` — Giriş
- `POST /api/v1/auth/register` — Kayıt
- `POST /api/v1/auth/forgot_password` — Şifre sıfırlama
- `POST /api/v1/auth/google/mobile` — Google ile giriş
- `POST /api/v1/auth/apple/mobile` — Apple ile giriş

#### Kullanıcı Profili
- `GET /api/v1/users/me` — Profil bilgisi
- `GET /api/v1/me/profile` — Detaylı profil
- `GET /api/v1/me/pregnancies` — Gebeliklerim
- `GET /api/v1/me/measurements` — Ölçümlerim
- `GET /api/v1/me/subscriptions` — Aboneliklerim
- `GET /api/v1/me/orders` — Siparişlerim

#### Gebelik Takibi
- `GET /api/v1/pregnancies/:id` — Gebelik detayı
- `GET /api/v1/trackings/current` — Mevcut hafta takibi
- `GET /api/v1/trackings/:week` — Haftalık takip
- `POST /api/v1/trackings/:week/variables/measurements` — Ölçüm ekleme
- `GET /api/v1/trackings/:week/mother_information` — Anne bilgisi
- `GET /api/v1/trackings/:week/baby_informations` — Bebek bilgisi
- `POST /api/v1/trackings/:week/ultrasound_images` — Ultrason yükleme

#### Doktor & Randevu
- `GET /api/v1/doctors` — Doktor listesi
- `GET /api/v1/doctors/:id` — Doktor detayı
- `GET /api/v1/doctors/:id/availability?date=YYYY-MM-DD` — Müsait saatler
- `GET /api/v1/doctors/:id/availability/range?start=...&end=...` — Tarih aralığı müsaitlik
- `GET /api/v1/doctors/:id/templates` — Haftalık çalışma şablonu
- `PUT /api/v1/doctors/:id/templates` — Şablon güncelleme
- `GET /api/v1/doctors/:id/exceptions` — İzin/tatil günleri
- `POST /api/v1/doctors/:id/exceptions` — İzin ekleme
- `DELETE /api/v1/doctors/:id/exceptions/:id` — İzin silme
- `POST /api/v1/bookings` — Randevu oluşturma
- `PATCH /api/v1/bookings/:id/approve` — Doktor onayı
- `PATCH /api/v1/bookings/:id/propose` — Doktor yeni teklif
- `PATCH /api/v1/bookings/:id/respond` — Hasta cevabı
- `PATCH /api/v1/bookings/:id/reject` — Doktor reddi

#### Eğitim
- `GET /api/v1/courses` — Kurs listesi
- `POST /api/v1/courses/:id/enrollments` — Kursa kayıt
- `PATCH /api/v1/course_enrollments/update_progress` — İlerleme güncelleme
- `GET /api/v1/videos/:id/progress` — Video izleme durumu

#### Ödeme
- `POST /api/v1/user_payments` — Ödeme oluşturma

### Önemli Modeller

| Model | Açıklama |
|---|---|
| User | Kullanıcı (Devise-JWT, OAuth, rol yönetimi) |
| Profile | Kullanıcı profili, risk skoru |
| Pregnancy | Gebelik kaydı, LMP, tahmini doğum |
| Tracking | Haftalık takip (1-42 hafta) |
| Measurement | Ölçüm değerleri |
| Course / CourseModule / Video | Eğitim içerikleri |
| Doctor | Doktor bilgisi, uzmanlık |
| Booking | Randevu (status machine ile) |
| DoctorAvailabilityTemplate | Haftalık çalışma şablonu |
| DoctorException | İzin/tatil günleri |
| Payment / Invoice | Ödeme ve faturalama |

---

## Mobile App

React Native (Expo) ile geliştirilmiş mobil uygulama.

### Teknolojiler

| Teknoloji | Bilgi |
|---|---|
| Framework | React Native 0.81 / Expo ~54 |
| Navigasyon | React Navigation (Stack + Bottom Tabs) |
| State Yönetimi | Redux Toolkit |
| HTTP | Axios |
| Auth | Expo Auth Session (Google/Apple) |
| UI | TailwindCSS, Linear Gradient |

### Kurulum

```bash
cd mobile-app
npm install
npx expo start -p 4000
```

### Ekranlar

#### Ana Akış
- **WelcomeScreen** — Karşılama ekranı
- **LoginScreen** — Giriş/kayıt
- **HomeScreen** — Ana sayfa, hızlı işlemler
- **ProfileScreen** — Kullanıcı profili

#### Gebelik Takibi
- **WeeklyTrackingScreen** — Haftalık takip
- **WeeklyBabyMomScreen** — Bebek & anne durumu
- **BabyInfoScreen / MotherInfoScreen** — Detaylı bilgiler
- **CalendarScreen** — Gebelik takvimi

#### Eğitim
- **EducationScreen / EducationDetailScreen** — Kurslar
- **VideoEducationScreen** — Video izleme

#### Randevu
- **DoctorListScreen** — Doktor listesi (uzmanlık filtresi)
- **DoctorDetailScreen** — Doktor detayı + takvim + saat seçimi
- **CheckoutScreen** — Randevu oluşturma
- **AppointmentsScreen** — Randevularım
- **DoctorDashboardScreen** — Doktor paneli (randevu yönetimi + müsaitlik ayarları)

#### Diğer
- **StoreScreen** — Paket/abonelik mağazası
- **ConsultationScreen** — Online danışma
- **CallScreen** — Görüntülü görüşme
- **InvoicesScreen** — Faturalar

---

## Randevu Sistemi (Prototip)

`ZehraClinic-Randevu-Sistemi/` klasörü, ana projeye entegre edilmek üzere geliştirilmiş bağımsız bir randevu prototipidir. Mevcut `backend/` ve `mobile-app/` içindeki randevu modülü bu prototipin olgunlaştırılmış halidir.

### Randevu Akışı

```
Hasta randevu alır → pending
    ↓
Doktor değerlendirir
    ├── Kabul eder → confirmed
    ├── Yeni teklif yapar → doctor_proposed
    │   └── Hasta yanıtlar
    │       ├── Kabul → confirmed
    │       ├── Red → rejected
    │       └── Karşı teklif → patient_proposed → (döngü)
    └── Reddeder → rejected
```

### Müsaitlik Yönetimi

- **Haftalık şablon:** Her doktor için 7 günlük çalışma saatleri
- **İstisnalar:** İzin, tatil günleri (tarih bazlı)
- **Engellenen saatler:** Onaylanmış randevular otomatik kapatılır
- Default: Hafta içi 09:00-17:00

---

## Geliştirme

### Test

```bash
cd backend && rails test
```

### Seed Verisi

```bash
cd backend && rails db:seed
```

8 doktor oluşturur, her biri için haftalık çalışma şablonlarını (Pzt-Cum 09:00-17:00) yükler.

### Environment Variables

Gerekli başlıca değişkenler:

```
DATABASE_URL=postgresql://...
GOOGLE_CLIENT_ID=...
APPLE_CLIENT_ID=...
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
S3_BUCKET=...
```

---

## Lisans

Tüm hakları saklıdır. © ZehraClinic
