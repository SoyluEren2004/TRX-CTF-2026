CTF Writeup: Junkiness (Web / Prototype Pollution)
🇹🇷 Türkçe Özet

Bu soruda, bir Node.js uygulamasının register ve login mekanizmalarındaki güvenli olmayan nesne işleme mantığı istismar edilmiştir.

    Dizi Manipülasyonu: username[] kullanılarak karakter sınırı kontrolü atlatılmıştır.

    Prototype Pollution: Kayıt sırasında __proto__ üzerinden sisteme isAdmin: true yetkisi enjekte edilmiştir.

    Auth Bypass: Giriş panelinde var olmayan bir kullanıcı üzerinden prototip zinciri tetiklenmiş ve Admin yetkisiyle içeri girilmiştir.

Çözüm Adımları
    İsteği Yakalama (Intercepting): İlk olarak web sitesinde rastgele bir kayıt (register) denemesi yapın ve giden POST /register isteğini Burp Suite aracılığıyla     yakalayın.

    Payload Enjeksiyonu: Yakalanan isteği Repeater modülüne gönderin. İstek gövdesini (body), Prototip Kirlenmesi (Prototype Pollution) payload'ını içerecek şekilde değiştirin (Örneğin: Uzunluk kontrolünü atlatmak için username[] ve özellik enjeksiyonu için __proto__ kullanın).

    Yetki Atlatma (Auth Bypass): Sunucu "Başarılı" mesajı verdikten sonra, zehirlediğiniz parametreleri kullanarak giriş yapın. Global nesne artık kirlendiği için uygulama, giriş yaptığınızda size yönetici (admin) yetkisi verecektir.

🇺🇸 English Summary

The "Junkiness" challenge involved exploiting unsafe object merging in a Node.js authentication system.

    Type Confusion: Bypassed length checks using array notation (username[]).

    Prototype Pollution: Injected isAdmin: true into the global Object.prototype during the registration process.

    Logic Bypass: Leveraged the prototype chain by logging in with a non-existent user, forcing the app to inherit the "Admin" status from the polluted prototype.

🇺🇸 Steps to Reproduce (Exploitation Flow)

    Intercepting the Request: First, perform a random registration attempt on the website and intercept the outgoing POST /register request using Burp Suite.

    Injecting the Payload: Send the intercepted request to the Repeater module. Modify the body to include the Prototype Pollution payload (e.g., using username[] for length bypass and __proto__ for property injection).

    Authentication Bypass: Once the server responds with a "Success" message, use the poisoned parameters to perform a login. Since the global object is now polluted, the application will grant administrative access upon login.


POST /register HTTP/1.1
Host: localhost:3000
Content-Type: application/x-www-form-urlencoded

username[]= __ proto __ &password[isAdmin]=true&password[password]=1

POST /login HTTP/1.1
Host: localhost:3000
Content-Type: application/x-www-form-urlencoded

username=password&password=1

🇹🇷 Türkçe:
Eğer payload başarılı bir şekilde işlendiyse ve sisteme giriş yapabiliyorsanız, (exploit) sürecimiz tamamlanmış demektir. Bu aşamadan sonra tarayıcı üzerinden giriş paneline dönüp bilgilerinizle giriş yapabilir ve "Free Flag" butonuna tıklayarak bayrağı elde edebilirsiniz.

🇺🇸 English:
If the payload has been processed successfully and you are able to log in, the exploitation process is complete. At this stage, you can return to the login panel via your browser, sign in with your credentials, and capture the flag by clicking the "Free Flag" button.
