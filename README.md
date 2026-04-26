# TRX-CTF-2026
TRX CTF 2026 CHALLANGES

🇺🇸 Important Note: Regarding the Flag

The solutions and screenshots provided in this repository were obtained by running the application in a local Docker environment.

    Local Testing: The local Docker version contains placeholder flags such as TRX{fake_flag} or TRX{fake_flag_for_testing} to verify that the exploit works.

    Real Flag: To obtain the actual flag, you must spawn a remote instance on the CTF platform and apply the documented exploit steps on the live server within the provided time limit.

    Validation: Successfully capturing a "fake flag" locally confirms that the exploit logic is correct and will function correctly on the production instance.


🇹🇷 Önemli Not: Bayrak (Flag) Hakkında

Bu depoda (repository) göreceğiniz çözümler ve ekran görüntüleri, uygulamanın yerel Docker ortamında çalıştırılmasıyla elde edilmiştir.

    Yerel Test: Docker üzerinde çalışan yerel sürümde, sistemin çalıştığını teyit eden TRX{fake_flag} veya TRX{fake_flag_for_testing} gibi geçici bayraklar yer alır.

    Gerçek Bayrak: Gerçek bayrağı elde etmek için, CTF platformu üzerinden bir instance (canlı makine) başlatmalı ve size verilen kısıtlı süre içerisinde burada açıklanan adımları canlı sunucu üzerinde uygulamalısınız.

    Doğrulama: Eğer yerel ortamda "fake flag" ya da "test fake flag" alabiliyorsanız, çözüm yolunuz %100 doğrudur ve canlı sistemde de çalışacaktır.


🇺🇸 Local Lab Experience

If you would like to test these vulnerabilities live on your own machine:

    Setup: Download the project as a ZIP file or clone the repository, then use Docker to spin up your local environment in seconds.

    Free Mode: You can challenge yourself by trying to find the vulnerabilities on your own without any hints or writeups.

    Guided Mode: If you get stuck, you can follow the steps in my Writeup to learn exactly how the exploits work step-by-step.

    Note: Successfully exploiting the local environment will yield a TRX{fake_flag}. This confirms that your logic is correct and ready to be applied to a live instance.

🇹🇷 Yerel Laboratuvar Deneyimi (Local Lab Experience)

Zafiyetleri kendi bilgisayarınızda canlı olarak test etmek isterseniz:

    Kurulum: Proje dosyalarını ZIP olarak indirin veya repoyu klonlayın, ardından Docker kullanarak yerel ortamınızı saniyeler içinde ayağa kaldırın.

    Serbest Mod: İsterseniz hiçbir ipucu almadan, tamamen kendi tekniklerinizle sistemdeki açıkları bulmaya çalışabilirsiniz.

    Rehberli Mod: Takıldığınız noktalarda hazırladığım Writeup adımlarını takip ederek zafiyetlerin nasıl sömürüldüğünü (exploit) adım adım öğrenebilirsiniz.

    Not: Yerel ortamda başarılı bir sızma gerçekleştirdiğinizde TRX{fake_flag} değerini elde edeceksiniz. Bu, çözüm mantığınızın doğru olduğunu ve canlı instance üzerinde de çalışacağını teyit eder.

# Laboratuvarı başlatmak için:
docker-compose up --build
