tr Türkçe Açıklama
Soru Analizi

Uygulama, kullanıcıdan bir alan adı (domain) alıp whois sorgusu yapan basit bir araçtır. Arka planda Ruby (Sinatra) kullanmaktadır. Girdi kontrolü için bir Regex (Düzenli İfade) filtresi kullanılsa da, bu filtre hatalı yapılandırıldığı için tam güvenli değildir.
Zafiyetler

    Regex Multiline Bypass: Ruby'de kullanılan ^ ve $ işaretleri, tüm metni değil sadece "satırı" kontrol eder. Bu durum, %0a (new line) karakteri kullanılarak filtrenin atlatılmasına olanak tanır.

    Unsafe Command Interpolation: Kullanıcıdan gelen veri, doğrudan sistem komutunun (Open3.capture3) içine yerleştirilmiştir. Bu, Komut Enjeksiyonu (Command Injection) zafiyetine yol açar.

    SUID Privilege Escalation: Sistemde /readflag adlı dosya root yetkileriyle (SUID) çalışmaktadır. Doğru argüman verildiğinde bayrağı okumamıza izin verir.

Çözüm Adımları

    İsteği Yakala: Bir domain sorgusu yapın ve Burp Suite ile isteği yakalayın.

    Payload Hazırla: Filtreyi geçmek için %0a ekleyip yanına /readflag komutunu yazın.

    Argüman Yönetimi: Boşlukların komutu bölmemesi için %20 ve tırnak işaretlerini (%22) kullanarak istenen cümleyi ekleyin.

    Bayrağı Al: Sunucudan dönen yanıtın içinde bayrağı (flag) okuyun.

🇺🇸 English Version
Challenge Analysis

The application is a WHOIS lookup tool built with Ruby (Sinatra). While it attempts to validate input using a Regex filter, the implementation is flawed, allowing for unintended command execution.
Vulnerabilities

    Regex Multiline Bypass: In Ruby, ^ and $ symbols match the start and end of a line, rather than the entire string. Using %0a (new line) bypasses the validation.

    Command Injection: User input is directly interpolated into a shell command via Open3.capture3, enabling the execution of arbitrary system commands.

    SUID Binary: The binary /readflag has the SUID bit set, running with root privileges to read the flag if the correct passphrase is provided.

Exploitation Steps

    Intercept: Capture the lookup request using Burp Suite.

    Inject: Append %0a to the domain parameter followed by the /readflag command.

    Quote Arguments: Wrap the required passphrase in quotes (%22) and use URL encoding (%20 for spaces) to ensure the command is parsed correctly.

    Capture Flag: Read the flag from the server's response

    Vulnerable Code Snippet (Ruby):

    # The root cause of the vulnerability
post '/lookup' do
  @domain = params[:domain]
  # Problem: Regex only checks the first line
  if @domain && @domain.match?(/^[a-z.-]+$/) 
    # Problem: Direct command injection
    stdout, stderr, status = Open3.capture3("whois #{@domain}") 
    @result = stdout.empty? ? stderr : stdout
  end
end