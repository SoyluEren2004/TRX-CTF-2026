### **ENG**

### **Task Summary**
### Category: Web Security
### Difficulty: Easy / Medium
### Objective: Log in as "Admin" on a simple blog application to capture the flag hidden within the private comments.

### Source Code Analysis

Upon inspecting the provided app.js file, we identify that the primary goal is to authenticate as the administrator.

The admin username is defined in the configuration file as follows:

export const ADMIN_USERNAME = "StifflingFluffiness"; // 19 Characters

The vulnerability lies within the /login POST endpoint logic:
app.post("/login", (req, res) => {
    const { username } = req.body;

    // 1. CONTROL: Username length must be between 4 and 12 characters
    if (!username || username.length < 4 || username.length > 12) {
        req.session.error = "Invalid username";
        return res.redirect("/");
    }

    req.session.username = username;
    // 2. CONTROL: Input is converted to uppercase and compared with the admin username
    req.session.isAdmin = username.toUpperCase() === ADMIN_USERNAME.toUpperCase();
    res.redirect("/");
});

### The Vulnerability: Unicode Case Folding

In JavaScript, the .toUpperCase() or .toLowerCase() functions can increase the character count when transforming certain special Unicode characters (ligatures).

For instance, the German character ß is a single (1) character, but when converted to uppercase, it becomes SS (2 characters).

By leveraging this logic, we can replace parts of the string "StifflingFluffiness" with their corresponding Unicode ligatures to bypass the length constraint:

**Generated Payload**: **ﬆiﬄingﬂuﬃneß**

### Results

    Length Bypass: The .length value of our generated payload is exactly 12, allowing it to pass the initial security check easily.

    Expansion: During the backend validation, the .toUpperCase() method is applied, causing the payload to expand:

    ﬆiﬄingﬂuﬃneß -> .toUpperCase() -> STIFFLINGFLUFFINESS

This result matches ADMIN_USERNAME.toUpperCase() perfectly. By logging in with the username ﬆiﬄingﬂuﬃneß, we successfully gain admin privileges and retrieve the Flag from the private comments section of the first post.

### Remediation

To prevent this vulnerability, length validation should be performed after the string transformation (normalization/casing), not before.

// Fixed Logic:
const upperUsername = username.toUpperCase();
// Check length AFTER conversion to account for Unicode expansion
if (!upperUsername || upperUsername.length < 4 || upperUsername.length > 20) { 
    // Handle error...
}


# **TR**

# **Görev Özeti**

### Kategori: Web Güvenliği
### Zorluk: Kolay / Orta
### Hedef: Basit bir blog uygulamasına "Admin" olarak giriş yaparak gizli yorumlarda saklanan bayrağı (flag) ele geçirmek.

### Kaynak Kod Analizi

Bize sağlanan app.js dosyasını incelediğimizde, hedefin uygulamanın admin kullanıcısı olarak oturum açmak olduğunu görüyoruz.

Konfigürasyon dosyasından gelen admin kullanıcı adı şu şekilde belirlenmiş: export const ADMIN_USERNAME = "StifflingFluffiness"; // 19 Karakter

Sisteme giriş yaptığımız POST endpoint'indeki /login kontrol bloğu ise zafiyetin yattığı asıl nokta:

app.post("/login", (req, res) => {
    const { username } = req.body;

    // 1. KONTROL: Uzunluk 4 ile 12 karakter arasında olmalı
    if (!username || username.length < 4 || username.length > 12) {
        req.session.error = "Invalid username";
        return res.redirect("/");
    }

    req.session.username = username;
    // 2. KONTROL: Girdi büyük harfe çevriliyor ve admin ismiyle karşılaştırılıyor ki asıl problemde burda kontrolü önce yapmadığı için öncesinde istediğimiz gibi karakterleri küçültüp büyütüyoruz 1. kontrolden geçiyoruz sonra ise büyültüp küçültüğümüz payloadı alıyor büyük harflere çevirip bir daha kontrol ediyor ve karakter sınır kuralını atlatıyoruz böylece.
    req.session.isAdmin = username.toUpperCase() === ADMIN_USERNAME.toUpperCase();
    res.redirect("/");
});

JavaScript'teki .toUpperCase() veya .toLowerCase() fonksiyonları, bazı özel Unicode karakterlerini (ligatürleri) dönüştürürken karakter sayısını artırabilir.

Örneğin, Almanca'daki ß karakteri tek (1) karakterdir ancak büyük harfe çevrildiğinde SS (2 karakter) olur.

Bu mantığı kullanarak StifflingFluffiness kelimesini oluşturan parçaları, karşılık gelen Unicode ligatürleri ile değiştirebiliriz:

Oluşturulan Payload: ﬆiﬄingﬂuﬃneß

Sonuç

Yukarıda hazırladığımız payload'un .length değeri tam olarak 12'dir ve ilk güvenlik duvarını kolayca aşar.
Arka planda doğrulama için .toUpperCase() işlemi uygulandığında payload genişler:
ﬆiﬄingﬂuﬃneß -> .toUpperCase() -> STIFFLINGFLUFFINESS

Bu da ADMIN_USERNAME.toUpperCase() ile kusursuz bir şekilde eşleşir. Sisteme ﬆiﬄingﬂuﬃneß kullanıcı adıyla giriş yapıldığında admin yetkileri kazanılır ve birinci postun altındaki gizli yorumlarda bulunan Flag elde edilir.

### Nasıl Engellenir?
sonra değil önce yapılmalı örnek:
// Düzeltilmiş Mantık:
const upperUsername = username.toUpperCase();
if (!upperUsername || upperUsername.length < 4 || upperUsername.length > 12) { ... }

