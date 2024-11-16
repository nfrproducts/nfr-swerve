# YAGSL Example Fork with NFR Swerve Configurations

Bu repository, NFR tarafından üretilmiş swerve sürüş modüllerinin ayar dosyalarını içermektedir. Bu, kullanıcılara YAGSL altyapısını kullanarak kendi swerve sistemlerini kolayca kurma imkânı sunar. Bu README, konfigürasyon dosyalarını nasıl kullanacağınızı ve projelerinize nasıl entegre edeceğinizi adım adım açıklamaktadır.

Kendi robotunuzu yapılandırmaya başlamak için lütfen [YAGSL Configuration Guide](https://yagsl.gitbook.io/yagsl/configuring-yagsl/configuration#json-files) adresine gidin ve oradaki adımları izleyin.

Ayrıca, YAGSL kütüphanelerini incelemek için aşağıdaki bağlantılara göz atabilirsiniz:

- [YAGSL Repository](https://github.com/BroncBotz3481/YAGSL)
- [YAGSL Example Repository](https://github.com/BroncBotz3481/YAGSL-Example)

## Kurulum Adımları

1. **Repository'i Klonlama**

   ```bash
   git clone https://github.com/tunapro1234/nf-swerve-lib.git
   cd nf-swerve-lib
   ```

   Alternatif olarak, repository'i ZIP dosyası olarak da indirebilirsiniz. Bunun için GitHub sayfasından "Code" butonuna tıklayıp "Download ZIP" seçeneğini kullanabilirsiniz.

2. **Gerekli Bağımlılıkları Yükleme**

   Bu projeyi kullanabilmek için bazı bağımlılıkların kurulu olduğundan emin olun. Bağımlılıkları aşağıdaki şekilde kurabilirsiniz:

   ```bash
   ./gradlew vendordep
   ```

   Linux kullanmayan kullanıcılar, WPILib VSCode eklentisini kullanarak vendordep ekleyebilirler. Bunun için "WPILib Command Palette" (Ctrl+Shift+P veya Cmd+Shift+P) üzerinden "Manage Vendor Libraries" seçeneğini seçin ve ardından "Install new libraries (online)" veya "Install new libraries (offline)" seçeneğini kullanarak gerekli vendordep dosyalarını ekleyin. Online seçeneğinde bağlantı sorulacaktır, offline olarak eklemek için gerekli `.json` dosyasını yerel olarak indirip yükleyebilirsiniz.

   Genellikle, bu JSON dosyalarını ekledikten sonra ek bir işlem yapmanıza gerek yoktur; çünkü Gradle, projenizi derlerken bu dosyalarda belirtilen kütüphaneleri otomatik olarak indirir ve projenize dahil eder. Ancak, projenizi ilk kez derlerken veya yeni bir JSON dosyası eklediğinizde, **bilgisayarınızın internet bağlantısının aktif olduğundan emin olun**. Bu, Gradle'ın gerekli kütüphaneleri indirip projenize entegre edebilmesi için gereklidir.

   Eğer projenizi çevrimdışı bir ortamda kullanmayı planlıyorsanız, tüm gerekli kütüphanelerin önceden indirilmiş olduğundan emin olmak için projenizi en az bir kez internet bağlantısı varken derlemeniz önerilir. Bu sayede, çevrimdışı ortamlarda da projenizi sorunsuz bir şekilde derleyebilirsiniz.

3. **NFR Swerve Konfigürasyonlarının Entegrasyonu**

   - Projede yer alan `nfr-swerve/src/main/deploy/swerve` ve `nfr_navx` klasörleri altındaki konfigürasyon dosyalarını bulun.
   - Bu dosyalar, swerve modüllerinin motor ayarları, PID parametreleri gibi önceden tanımlanmış ayarları içermektedir.
   - `physicalproperties` dosyası içerisinde ise motorlar için `rampRate`, `currentLimit` ve diğer fiziksel parametreler bulunmaktadır.

4. **Kendi Robotunuza Uygun Hale Getirme**

   - Kendi robotunuza uygun ayarlamaları yapmak için [YAGSL Tuning Webpage](https://broncbotz3481.github.io/YAGSL-Example/) adresindeki adımları izleyebilirsiniz. Örneğin, navX'i pigeon sensörüyle değiştirme gibi işlemler için bu kaynağı kullanabilirsiniz.

5. **Test ve Kalibrasyon**

   - Ayarları yükledikten sonra robotunuzu test edin ve gerekirse konfigürasyonlarda ince ayar yapın.
   - Testlerinizi FRC test alanında veya simülasyon ortamında gerçekleştirebilirsiniz.
   - Diğer gerekli adımlar ve offset ayarları hakkında bilgi almak için [YAGSL Configuration Guide](https://yagsl.gitbook.io/yagsl/configuring-yagsl/configuration#json-files) adresine göz atabilirsiniz.

## Önemli Parametreler

- **Motor IDs:** Motor kimliklerinin her biri, kontrol ünitenizdeki benzersiz kimlik numaralarına uygun olmalıdır.
- **PID Ayarları:** Motor kontrolü için PID değerlerini, robotunuzun dinamiklerine uygun şekilde optimize etmeniz gerekebilir.
- **Inverted Ayarları:**
  - **`drive`** (Sürüş Motoru): Bizim konfigürasyonlarımızda sürüş motoru için `inverted` değeri `false` olarak ayarlıdır. Bu, motorun varsayılan yönde çalışacağını gösterir.
  - **`angle`** (Açısal Motor): Açısal motor için `inverted` değeri `true` olarak ayarlanmıştır. Bu, açısal motorun dönüş yönünün tersine çalışacağını belirtir. Bu ayar, açısal kontrolün doğru şekilde yapılmasını sağlar.
- **Ramp Rate (Ramp Hızı):**
  - **`drive`** (Sürüş Motoru): Sürüş motoru için `rampRate` değeri `0.25` olarak ayarlanmıştır. Bu, motor hızının ne kadar hızlı değişeceğini belirler ve ani hız değişikliklerini önleyerek robotun daha kontrollü bir şekilde ivmelenmesini sağlar.
  - **`angle`** (Açısal Motor): Açısal motor için `rampRate` değeri `0.25` olarak ayarlanmıştır. Bu, açısal motorun hızının ne kadar hızlı değişeceğini kontrol eder ve daha hassas bir dönüş sağlar.
- **Current Limit (Akım Limiti):**
  - **`drive`** (Sürüş Motoru): Sürüş motoru için `currentLimit` değeri `40` amper olarak ayarlanmıştır. Bu, motorun çekeceği maksimum akımı sınırlar ve motorun aşırı ısınmasını veya zarar görmesini önler.
  - **`angle`** (Açısal Motor): Açısal motor için `currentLimit` değeri `20` amper olarak ayarlanmıştır. Bu, açısal motorun güvenli bir şekilde çalışmasını sağlar.

## Konfigürasyon İpuçları

- **Robotunuz otonom sırasında veya başlık ayarlamaya çalışırken kontrolsüz bir şekilde dönüyor:**
  - Jiroskopu tersine çevirin.
  - Her modül için sürüş motorlarını ters çevirin (Eğer robot dönerken ön ve arka yer değiştiriyorsa).
- **Robotunuz ağırsa:**
  - SwerveMath içinde momentum hız sınırlamalarını uygulayın.
  - IMU'nun robotun merkezinde olduğundan emin olun.

## Lisans

Bu repository, Apache-2.0 lisansı altında lisanslanmıştır. Daha fazla bilgi için `LICENSE` dosyasını kontrol edin.

