🔐 Access Control & Privilege Escalation – Pentest Master Checklist (Tek Sayfa)

Amaç:
“Bu endpoint’te yetki var mı?” değil
“Bu endpoint’te yetki gerçekten kontrol ediliyor mu?” sorusunu sordurmak.

1️⃣ Authentication & Session Kontrolleri

🎯 Yetkilendirme testine girmeden önce session güvenilir mi?

 Login olmadan /admin, /my-account, /api/* erişimi var mı?

 Login → session cookie yenileniyor mu?

 Session kullanıcı + role bağlı mı?

 Logout sonrası eski cookie hâlâ çalışıyor mu?

 Aynı cookie farklı kullanıcıda geçerli mi?

Cookie: session=ABC123


⚠️ Session sadece “login oldu mu?” diye bakıyorsa → Access control çöker

2️⃣ Vertical Access Control (Dikey Yetki)

🎯 Normal kullanıcı admin fonksiyonlarına ulaşabiliyor mu?

 /admin, /administrator, /manage, /dashboard

 UI’da gizli ama URL ile erişilebilir mi?

 “403 Forbidden” yerine işlem gerçekleşiyor mu?

 Silme / rol değiştirme gibi işlemler kontrolsüz mü?

GET /admin/delete?username=carlos


📌 PDF Örneği:
Unprotected admin functionality lab’leri 

Portswigger Access Control

3️⃣ Security by Obscurity (Gizleyerek Güvenlik)

🎯 “URL gizli” ama kontrol yok mu?

 Admin link JS içinde mi?

 isAdmin=false sadece UI mı gizliyor?

 View-source ile admin URL çıkıyor mu?

if (isAdmin) {
  link = "/admin-hfvc11";
}


⚠️ JavaScript ≠ Access Control

4️⃣ Parameter-Based Access Control

🎯 Yetki parametreyle mi belirleniyor?

 URL parametreleri (?admin=true, ?role=2)

 Cookie (role=user, admin=false)

 JSON body ("roleid":2)

 Hidden field / form value

{
  "email": "test@test.com",
  "roleid": 2
}


📌 PDF Lab:
User role controlled by request parameter 

Portswigger Access Control

5️⃣ Horizontal Privilege Escalation (IDOR)

🎯 Başka kullanıcının verisine erişebiliyor musun?

 id, userId, accountId, orderId

 Başka kullanıcıya ait data dönüyor mu?

/my-account?id=wiener
/my-account?id=carlos


📌 PDF Lab:
User ID controlled by request parameter 

Portswigger Access Control

6️⃣ GUID / Unpredictable ID ≠ Güvenlik

🎯 GUID kullanılmış ama kontrol var mı?

 GUID’ler linklerde / yorumlarda sızıyor mu?

 API response’ta başka kullanıcı ID’si var mı?

 Redirect (302) body’sinde veri sızıyor mu?

/my-account?id=a80a3f91-49ca-48ff-88da-fde7f95a1821


⚠️ GUID ≠ Authorization

7️⃣ Horizontal ➜ Vertical Escalation

🎯 Ele geçirilen kullanıcı admin mi?

 API key sızıyor mu?

 Password reset endpoint’i var mı?

 Rol değişikliği yapılabiliyor mu?

 Admin panel erişimi açılıyor mu?

📌 PDF’de zincirleme örnekler var 

Portswigger Access Control

8️⃣ Method-Based Access Control

🎯 Yetki sadece HTTP metoduna mı bağlı?

 POST engelli ama GET çalışıyor mu?

 PUT / DELETE denenmiş mi?

 POSTX, TRACE gibi metodlar farklı mı davranıyor?

POST  /admin-roles  → 401
GET   /admin-roles  → 200


📌 PDF Lab:
Method-based access control can be circumvented 

Portswigger Access Control

9️⃣ URL Override & Routing Bypass

🎯 Front-end filtre var ama backend?

 X-Original-URL

 X-Rewrite-URL

 Case sensitivity (/ADMIN)

 Trailing slash (/admin/)

 Suffix (;, .json)

POST /login
X-Original-URL: /admin/delete?username=carlos


📌 PDF Lab:
URL-based access control can be circumvented 

Portswigger Access Control

🔟 Multi-Step Process

🎯 Sadece ilk adım mı korunuyor?

 Ara adımlar yetkili mi?

 Final request direkt çağrılabiliyor mu?

POST /confirm-action


📌 PDF Lab:
Multi-step process with no access control 

Portswigger Access Control

1️⃣1️⃣ Referer-Based Access Control

🎯 Referer’a güveniliyor mu?

 Referer yokken ne oluyor?

 Fake Referer ile bypass mümkün mü?

Referer: https://site.com/admin


📌 PDF Lab:
Referer-based access control 

Portswigger Access Control

🛡️ Secure Design – Savunma Checklist

 URL gizleme tek başına kullanılmamış

 Default deny uygulanmış

 Her endpoint server-side kontrol yapıyor

 Rol kontrolü merkezi

 Client verisine güvenilmiyor

 Tüm metodlar aynı şekilde kontrol ediliyor

🎯 Kısaca Akılda Kalsın
