# 🔐 Access Control & Privilege Escalation Checklist

Bu doküman, web uygulamalarında **Access Control zafiyetlerini** ve **Privilege Escalation** senaryolarını tespit etmek için hazırlanmış **tek sayfalık, pratik bir pentest checklist**tir.  
PortSwigger lab’leri, bug bounty hedefleri ve gerçek pentest senaryoları için uygundur.

---

## 1️⃣ Authentication & Session
- [ ] Login olmadan hassas endpoint erişimi var mı?
- [ ] Login sonrası session cookie yenileniyor mu?
- [ ] Session kullanıcı + role bağlı mı?
- [ ] Logout sonrası eski cookie çalışıyor mu?
- [ ] Aynı session cookie farklı kullanıcıda geçerli mi?

---

## 2️⃣ Vertical Access Control (Dikey)
- [ ] Admin URL’ler (`/admin`, `/administrator`, `/manage`, `/dashboard`)
- [ ] UI’da gizli ama URL ile erişilebilir mi?
- [ ] “Yetkiniz yok” yerine işlem gerçekleşiyor mu?
- [ ] `robots.txt`, JS, HTML source admin URL sızdırıyor mu?

```js
if (isAdmin) {
  link = "/admin-panel-x92a";
}
⚠️ JavaScript gizleme, access control değildir.

3️⃣ Security by Obscurity

 Admin URL rastgele ama gerçek kontrol yok mu?

 URL bilindiğinde herkes erişebiliyor mu?

 URL leak / brute-force ile bulunabiliyor mu?

4️⃣ Parameter-Based Access Control

 Query string (?admin=true, ?role=1, ?isAdmin=1)

 Cookie (role=user, admin=false)

 Request body / form (role, isAdmin, userType)

 Parametre değişince yetki artıyor mu?

5️⃣ Platform / Framework Bypass

 URL override header’ları (X-Original-URL, X-Rewrite-URL)

 POST engelli ama GET / PUT / DELETE çalışıyor mu?

 Geçersiz metodlar (POSTX, TRACE) farklı davranıyor mu?

POST /
X-Original-URL: /admin/deleteUser

6️⃣ URL Matching & Routing Hataları

 Büyük/küçük harf farkı (/ADMIN/deleteUser)

 Trailing slash (/admin/deleteUser/)

 Suffix ekleme (.anything, ;)

⚠️ Spring (< 5.3) suffix pattern zafiyetleri yaygındır.

7️⃣ Horizontal Privilege Escalation (IDOR)

 id, userId, accountId, orderId değiştirilebiliyor mu?

 Başka kullanıcı verisi geliyor mu?

/my-account?id=123 → /my-account?id=456

8️⃣ GUID / Unpredictable ID

 GUID’ler yorumlarda, mesajlarda, linklerde sızıyor mu?

 API response’larında başka kullanıcı ID’si var mı?

 Redirect (302) body’sinde veri sızıntısı var mı?

9️⃣ Horizontal ➜ Vertical Escalation

 Ele geçirilen kullanıcı admin mi?

 Admin hesabında:

 Parola reset

 API key

 Rol değişikliği

 Admin panel erişimi

🔟 Multi-Step Process

 Ara adımlar korunuyor mu?

 Final request direkt çağrılabiliyor mu?

POST /confirm-action

1️⃣1️⃣ Referer-Based Access Control

 Referer yokken işlem engelleniyor mu?

 Fake Referer ile bypass mümkün mü?

Referer: https://site.com/admin

1️⃣2️⃣ Location-Based Access Control

 IP / ülke bazlı kısıt var mı?

 VPN / proxy ile bypass edilebiliyor mu?

 Kontrol client-side mı?

🛡️ Secure Design (Defensive Checklist)

 URL gizleme tek başına kullanılmamış

 Default deny uygulanmış

 Tüm endpoint’lerde server-side access control var

 Merkezi yetkilendirme mekanizması mevcut

 Her endpoint için açık rol tanımı yapılmış

 Düzenli access control testleri uygulanıyor
