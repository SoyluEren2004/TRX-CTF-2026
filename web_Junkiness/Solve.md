CTF Writeup: Junkiness (Web / Prototype Pollution)
🇹🇷 Türkçe Özet

Bu soruda, bir Node.js uygulamasının register ve login mekanizmalarındaki güvenli olmayan nesne işleme mantığı istismar edilmiştir.

    Dizi Manipülasyonu: username[] kullanılarak karakter sınırı kontrolü atlatılmıştır.

    Prototype Pollution: Kayıt sırasında __proto__ üzerinden sisteme isAdmin: true yetkisi enjekte edilmiştir.

    Auth Bypass: Giriş panelinde var olmayan bir kullanıcı üzerinden prototip zinciri tetiklenmiş ve Admin yetkisiyle içeri girilmiştir.

🇺🇸 English Summary

The "Junkiness" challenge involved exploiting unsafe object merging in a Node.js authentication system.

    Type Confusion: Bypassed length checks using array notation (username[]).

    Prototype Pollution: Injected isAdmin: true into the global Object.prototype during the registration process.

    Logic Bypass: Leveraged the prototype chain by logging in with a non-existent user, forcing the app to inherit the "Admin" status from the polluted prototype.