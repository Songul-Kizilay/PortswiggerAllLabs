🔐 Access Control & Privilege Escalation
Ayrıntılı Türkçe Pentest Checklist
1️⃣ Authentication & Session Temeli

Yetkilendirme testine geçmeden önce mutlaka kontrol et

 Login olmadan erişilebilen hassas endpoint var mı?

 Session cookie login sonrası değişiyor mu?

 Cookie sadece kullanıcıya mı bağlı, yoksa role de bağlı mı?

 Logout sonrası eski cookie ile istek atılabiliyor mu?

 Aynı session cookie farklı kullanıcıda çalışıyor mu?

🎯 Amaç: Access control hatası mı, session bug’ı mı ayırt etmek

2️⃣ Vertical Access Control (Dikey Yetki Kontrolü)

Normal kullanıcı admin işlemi yapabiliyor mu?

🔍 URL Bazlı Kontroller

 /admin, /administrator, /manage, /panel, /dashboard

 /admin/deleteUser

 /admin/users

 /admin/config

Test:

Direkt tarayıcıdan gir

Burp Repeater’dan isteği gönder

Farklı kullanıcı cookie’siyle dene

🔍 UI Gizli ama Backend Açık mı?

 Admin linki UI’da yok ama URL çalışıyor mu?

 “Yetkiniz yok” mesajı geliyor mu yoksa işlem gerçekleşiyor mu?

 Sadece link gizlenmiş mi, gerçek kontrol yok mu?

🔍 Admin URL Leak Kontrolleri

 robots.txt

 JS dosyaları

 HTML source

 API response’ları

if (isAdmin) {
  link = "/admin-panel-x92a";
}


⚠️ JS herkese görünür → gerçek kontrol değildir

3️⃣ Security by Obscurity (Gizleyerek Koruma)

 Admin URL rastgele ama kontrol yok mu?

 URL brute-force veya leak ile bulunabiliyor mu?

 URL bilindiğinde herkes erişebiliyor mu?

🎯 Kural: URL gizlemek ≠ access control

4️⃣ Parameter-Based Access Control

Rol / yetki kullanıcı tarafından değiştirilebiliyor mu?

🔍 Query String

 ?admin=true

 ?role=1

 ?isAdmin=1

🔍 Cookie

 role=user

 admin=false

 access=0

🔍 Hidden Field / Form

 Profil update request’lerinde rol alanı var mı?

 JSON body içinde role, isAdmin, userType bulunuyor mu?

📌 Test:

Değeri değiştir

Request’i tekrar gönder

Yetki artıyor mu?

5️⃣ Platform / Framework Kaynaklı Bypass’lar
🔍 URL Override Header’ları

 X-Original-URL

 X-Rewrite-URL

 X-Forwarded-Path

POST /
X-Original-URL: /admin/deleteUser


🎯 Frontend kontrol var, backend override’a güveniyor olabilir

🔍 HTTP Method Manipülasyonu

 POST engelli ama GET çalışıyor mu?

 PUT / DELETE deneniyor mu?

 OPTIONS bilgi sızdırıyor mu?

 Geçersiz metodlar (POSTX, TRACE) farklı davranıyor mu?

📌 Burp Repeater → Change request method

6️⃣ URL Matching & Routing Hataları

Backend ve access control aynı endpoint’i mi görüyor?

 /ADMIN/deleteUser

 /admin/DeleteUser

 /admin/deleteUser/

 /admin/deleteUser.anything

 /admin/deleteUser;

⚠️ Spring (özellikle <5.3) suffix pattern açık olabilir

7️⃣ Horizontal Privilege Escalation (Yatay Yetki Aşımı)

Başka kullanıcının verisine erişim

🔍 IDOR Kontrolleri

 id=1 → id=2

 userId=

 accountId=

 orderId=

/my-account?id=123


📌 Değiştir → başka kullanıcı geliyor mu?

8️⃣ GUID / Unpredictable ID Senaryoları

Tahmin edilemez ama sızıyor mu?

 GUID’ler yorumlarda, mesajlarda, profil linklerinde var mı?

 API response’larında başka kullanıcı ID’si geliyor mu?

 Redirect response body’si temiz mi?

⚠️ 302 dönse bile response BODY mutlaka kontrol et

9️⃣ Horizontal ➜ Vertical Escalation

Yatay açık admin’e götürüyor mu?

 Başka kullanıcı admin mi?

 Admin hesabında:

Parola reset

API key

Rol değişikliği

Admin panel linki var mı?

🎯 Yatay açık → admin compromise = tam yetki

🔟 Multi-Step Process Zafiyetleri

Adımların hepsi korunuyor mu?

 Step 1 korunuyor

 Step 2 korunuyor

 Step 3 direkt çağrılabiliyor mu?

📌 Final request’i tek başına gönder

POST /confirm-action

1️⃣1️⃣ Referer-Based Access Control

Referer’a güvenilmiş mi?

 Referer header yokken çalışmıyor mu?

 Fake Referer ekleyince çalışıyor mu?

Referer: https://site.com/admin


⚠️ Referer tamamen attacker kontrolünde

1️⃣2️⃣ Location-Based Access Control

 IP bazlı kısıt var mı?

 VPN ile bypass oluyor mu?

 Sadece client-side geo mu?

🛡️ Güvenli Tasarım Kontrolü (Defensive Checklist)

 URL gizleme tek başına kullanılmamış

 Default deny uygulanmış

 Tüm endpoint’lerde server-side kontrol var

 Merkezi access control mekanizması var

 Her endpoint için açık rol tanımı var

 Düzenli access control testleri yapılıyor
